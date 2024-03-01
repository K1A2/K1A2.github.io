---
title: '[OSM] OSM(Open Street Map) 데이터로 지도 및 길찾기 서비스 구현하기'
categories: [개발, backend]
tags: [web, backend, data, map, maptile, router, routing, osm, openstreetmap]
---

# 개요
서비스를 개발하다보면 길을 찾아주거나, 지도를 보여주는 등 지도와 관련된 기능이 필요할때가 있습니다. 
하지만 규모가 작은 개발자는 지도라는 단어만 봐도 막막하죠.. 
직접 구현하자니 지도 데이터를 구할 방법은 모르겠고, 구글, 네이버와 같은 대기업에서 제공하는 API를 사용하자니 요청당 요금이 부과되거나 일일 사용량 제한이 있는등 여러 어려움이 있습니다. 
그래서 저는 **OSM(Open Street Map) 지도 데이터**와 여러 **오픈소스 프로젝트**를 이용하여 직접 구현하기로 결정했습니다.


# OSM?

OSM은 **Open Street Map**의 줄임말로, 누구나 지도에 데이터를 추가하고 수정할 수 있는 오픈소스 프로젝트입니다. 
네이버, 구글과 같은 기업에서 지도 이미지나 경로 등 지도 데이터를 이용한 결과물만 제공하는게 아닌 **지도 데이터 그 자체를 제공**한다는 점이 가장 큰 차이점입니다. 
그래서 OSM 데이터를 이용해서 다양한 서비스 구현이 가능한것입니다.  
하지만 오픈소스 프로젝트 특성상 데이터 편집에 참여하는 사람 수가 적으면 변경된 정보가 늦게 업데이트 된다는 단점이 있습니다. 
특히 한국은 OSM 참여가 저조한 지역으로, 전철 경로나 도로 등 기반시설 정보도 없는 경우가 꽤 있습니다. 
물론 개발자가 직접 데이터를 추가해서 업데이트하면 되긴 하지만, 귀찮은 작업이긴 합니다.

OSM에 대해서 더 자세히 알아보고 싶다면 [OSM 공식 사이트](https://osm.kr/){:target="_blank"}를 참고해보세요.


# 구현하기

OSM 데이터로 지도 타일 서버와 길찾기 서버를 구축하는 방법에 대해 알아봅시다.

## 지도 타일서버 구축하기

### 타일서버란?

네이버지도, 구글지도 등 지도 앱을 서보면 지도가 일정한 정사각형 단위로 잘려서 로딩되는걸 볼 수 있습니다.

![tile](/assets/img/2024-02-29-backend-map/tile_example.webp){: width="400px"}
_사각형으로 잘려서 로딩되는걸 볼 수 있다_

이때 로드되는 단위를 **타일**이라고 부르고, 이러한 타일 이미지를 요청한 범위에 따라 생성해주는 서버를 **타일서버**라고 부릅니다. 
타일서버를 직접 구현하는것은 어렵습니다. 사용자가 현재 보고있는 위치, 줌 레벨, 화면에 표시되는 범위를 계산해서 적절한 범위의 데이터를 텍스트, 아이콘을 렌더링 헤야합니다. 
말로만 설명해도 복잡하죠. 
다행히 OSM 데이터를 이용하여 몇가지 설정만 해주면 자동으로 타일서버를 빌드해주는 오픈소스 프로젝트가 있습니다.

### 구현하기

세 가지의 오픈소스 프로젝트를 사용합니다.
1. [**OSM 한국지역 데이터**](https://download.geofabrik.de/asia/south-korea.html){:target="_blank"}: OSM 지도 데이터 중 한국 지역만 제공되는 데이터
2. [**openmaptiles**](https://github.com/openmaptiles/openmaptiles){:target="_blank"}: OSM 데이터를 이용하여 타일 정보를 생성할 수 있게 해주는 오픈소스 프로젝트 - BSD3 라이선스
3. [**tileserver-gl**](https://github.com/maptiler/tileserver-gl){:target="_blank"}: 타일 데이터를 이용해 타일서버를 배포할 수 있게 해주는 오픈소스 프로젝트 - BSD2 라이선스


#### 1. 지도 데이터 다운로드

우선 OSM 데이터를 다운받아야 합니다. GeoFabrik에서 [남한 데이터](https://download.geofabrik.de/asia/south-korea.html){:target="_blank"}만 다운받을 수 있습니다. 
최대 대륙 단위로 지도 데이터 다운로드가 가능합니다.    
남한 지도 데이터를 다운받으면 **south-korea-latest.osm.pbf**라는 파일이 다운로드 될 것입니다. pbf 파일은 지도 원본 데이터 입니다. 
이 원본 데이터를 이용해서 장소의 이름, 아이콘, 지도 범례 정보를 포함하는 타일 데이터로 변환하는 과정이 필요합니다. 
pbf 파일을 타일 정보 파일인 mbtiles로 변환시켜봅시다.

#### 2. mbtiles 생성

다운받은 pbf 파일은 mbtiles로 변환해주어야 합니다. 
다행히 몇가지 명령어만 입력하면 자동으로 변환해주는 [openmaptiles](https://github.com/openmaptiles/openmaptiles){:target="_blank"}라는 오픈소스 프로젝트가 존재합니다. 
이 프로젝트를 이용하면 타일로 변환할때 지도에 어떤 정보를 포함할지, 어떤 스타일로 지도를 생성할지까지 커스텀이 가능합니다. 
어떤 스타일이 있는지 궁금하다면 openmaptiles에서 [스타일 목록](https://github.com/openmaptiles/openmaptiles?tab=readme-ov-file#styles){:target="_blank"}을 읽어보시면 됩니다. 
OSM에서 정의한 스타일 스키마를 커스텀해서 나만의 스타일을 만드는것도 가능합니다. 
일단은 디폴트 스타일로 지도 타일 생성을 진행하겠습니다.  

우선 시작하기 전 **도커**와 **도커 컴포즈**가 설치되어 있어야 합니다. 
글을 작성하는 시점에서 도커는 **1.12.3 이상**, 컴포즈는 **1.7.1 이상**이어야 합니다. 
만약 버전이 맞지 않거나 설치가 되어있지 않다면 우선 설치하고 아래 과정을 진행해야 합니다. 
이미 설치되어 있거나 설치를 완료했다면 아래 명령어를 따라하시면 됩니다. 

1. 레포지토리를 clone 해서 로컬 저장소에 다운로드한다.
```bash
git clone https://github.com/openmaptiles/openmaptiles.git
```
2. make 명령어를 이용해 사전빌드를 진행한다.
```bash
# clone 한 폴더 안으로 이동
cd openmaptiles
make
```
3. 데이터베이스 컨테이너를 실행시킨다.
```bash
make start-db
```
4. 외부 데이터를 가져온다.
```bash
make import-data
```
5. 다운받았던 pbf 파일을 ./data 폴더에 옮긴다. data 폴더가 없다면 만들고 옮겨도 된다.
```bash
mkdir data
mv *.pbf ./data
```
6. pbf 파일에서 데이터를 가져온다.
```bash
make import-osm
```
7. OSM에서 정의한 wikidata에 맞게 다중 언어로 레이블을 생성한다.
```bash
make import-wikidata
```
8. 최종적으로 타일을 연산하고 빌드한다.
```bash
make import-sql
make generate-bbox-file
make generate-tiles-pg
```
9. **(선택)** 스타일 파일을 빌드한다. 스타일 파일은 지도의 스타일을 정의해둔 파일로, 이후 타일 서버로 호스팅 할때 필요하다. 벡터타일을 렌더링 할때 필요하다.
```bash
make build-style
```

data 폴더에서 **south-korea.mbtiles** 라는 파일이 생성되었다면 성공입니다. 
이제 생성된 mbtiles 파일을 **tileserver-gl**을 이용하여 타일 서버를 호스팅하면 끝납니다.

**추가) 타일 변환 과정에서 에러가 발생할 때**  
import-osm 등 데이터 변환 과정에서 오류가 발생한다면 **quickstart**를 이용해 보세요. 
이 방법을 사용할경우 mbtiles 파일이 data/ 안에 생성됩니다.
```bash
./quickstart.sh asia/south-korea
```

#### 3. 타일서버 구축하기

타일서버를 구축하는 방법은 [tileserver-gl](https://github.com/maptiler/tileserver-gl){:target="_blank"}을 이용하면 정말 쉽습니다. 
tileserver-gl은 노드, 도커 등 다양한 방법으로 제공되지만, 이번에는 앞서 사용한 도커를 이용해서 구축해 보겠습니다.

1. 도커 볼륨으로 사용할 공간에 앞서 만든 mbtiles 파일을 복사한다.
2. tileserver-gl을 실행한다.
```bash
docker run --rm -it -v /{1번에서 타일 파일을 복사한 경로}:/data -p 8080:8080 maptiler/tileserver-gl
```

끝났습니다. 지도 타일서버를 이렇게 간단하게 구현할 수 있게 해준 openmaptiles와 maptiler 프로젝트에게 다시한번 감사하게 되네요.

### 결과물

localhost:8080으로 접속하면 아래와 같은 화면이 보일것입니다.
![tileserver-gl](/assets/img/2024-02-29-backend-map/tileserver.webp){: width="500px"}

이 화면이 보였다면 타일서버 구축이 완료된겁니다. 
테스트로 서울 남서쪽 타일을 하나 요청해 봅시다. 
```
http://localhost:8080/styles/basic-preview/512/11/1745/793.png
```
요청 결과로 남서쪽 타일 이미지가 오는걸 볼 수 있습니다.  
![tile](/assets/img/2024-02-29-backend-map/tile-example.webp){: width="300px"}

하지만 지도를 보다보면 뭔가 이상한걸 느낄수 있습니다. 건물이 단 하나도 보이지 않는것이죠. 
이 문제는 MAX_ZOOM의 기본 설정이 7이기 때문입니다. 
7이 최대 확대 가능한 레벨이라고 지정해서 그보다 더 확대했을때 정보는 렌더링 하지 않는것이죠.  
  
MAX_ZOOM의 값을 더 큰 값으로 수정하고 다시한번 mbtiles를 생성하는 과정을 반복하시면 됩니다. 
```
...
# Which zooms to generate with   make generate-tiles-pg
MIN_ZOOM=0
MAX_ZOOM=14
...
```

아래 이미지들은 MAX_ZOOM=18로 지정하고 실행한 결과입니다. 
도로, 건물 등 세세한 정보들이 표기됩니다.

![tile](/assets/img/2024-02-29-backend-map/level_18_tile1.webp){: width="300px"}  
![tile](/assets/img/2024-02-29-backend-map/level_18_tile2.webp){: width="300px"}

권장은 최대 14이며, 그보다 큰 줌 레벨은 일반적으로 필요 없다고 설명하고 있습니다. 
실제로 이슈를 보다보면 줌레벨 22로 지정했다가 5000일 걸린다는 메세지를 봤다고 올라와 있습니다.


## 라우팅 서버 구축하기

### 라우팅 서버란?

라우팅 서버란 몇가지 체크포인트를 제공하면 체크포인트를 모두 지나는 최적의 경로를 일려주는 길찾기 서버입니다. 
타일서버와 마찬가지로 직접 구현하면 매우 어려운 작업입니다. 
정말 다행이도 OSM 데이터를 이용하여 명령어 몇번이면 라우팅 서버를 구축할 수 있게 해주는 오픈소스 프로젝트가 있습니다.

### 구현하기

두 가지의 오픈소스 프로젝트를 사용합니다. 

1. [**OSM 한국지역 데이터**](https://download.geofabrik.de/asia/south-korea.html){:target="_blank"}: OSM 지도 데이터 중 한국 지역만 제공되는 데이터
2. [**osrm-backend**](https://github.com/Project-OSRM/osrm-backend/){:target="_blank"}: OSM 데이터를 이용하여 라우팅 서버를 구축할 수 있게 해주는 오픈소스 프로젝트- BSD2 라이선스

#### 1. 지도 데이터 다운로드

이번에도 OSM 데이터를 다운받아야 합니다. 타일서버 구축할때 이미 받았다면 이 과정은 스킵하셔도 됩니다. 
아직 받지 않았다면 GeoFabrik에서 [남한 데이터](https://download.geofabrik.de/asia/south-korea.html){:target="_blank"}를 다운받으시면 됩니다. 
남한 지도 데이터를 다운받으면 **south-korea-latest.osm.pbf**라는 지도 원본 데이터가 다운로드 됩니다. 

#### 2. 라우팅 서버 구축

[**osrm-backend**](https://github.com/Project-OSRM/osrm-backend/){:target="_blank"}의 readme만 따라하면 바로 구축 가능합니다.
1. osrm-backend 도커 볼륨으로 사용할 공간에 앞서 다운받은 pbf 파일을 복사한다.
2. pbf 파일을 전처리 해준다. 중간에 **-p /opt/car.lua** 플래그는 자동차가 다닐 수 있는 길만 가져온다는 뜻이다. bicycle은 자전거, foot은 도보 경로만 가져온다.
```bash
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-extract  -p /opt/car.lua /data/south-korea-latest.osm.pbf || echo "osrm-extract failed"
```
3. 두 전처리 명령어도 실행한다.
```bash
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-partition /data/south-korea-latest.osrm || echo "osrm-partition failed"
docker run -t -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-customize /data/south-korea-latest.osrm || echo "osrm-customize failed"
```
4. 라우팅 서버를 실행한다.
```bash
docker run -t -i -p 5000:5000 -v "${PWD}:/data" ghcr.io/project-osrm/osrm-backend osrm-routed --algorithm mld /data/south-korea-latest.osrm
```

라우팅 서버 구축도 끝났습니다. 테스트로 광화문 -> 서울역 -> 전쟁기념관 경로를 출력해 봅시다. 요청 url은 다음과 같습니다.
```
http://localhost:5001/route/v1/driving/126.977,37.575;126.972,37.5548;126.9772,37.5348?steps=true&overview=full&annotations=true
```

응답으로 뭔가 정말 긴 json형식의 문자열이 옵니다.
```json
{
   "code":"Ok",
   "routes":[
      {
         "geometry":"wzidFwe_fWbHG|DCrDEv@?vDLN@J@L?Z@P@H@hABrBA~B?rEKl@Ah@?F?h@AzGBv@BF?R@b@@b@LzA^ZHB@x@RfElAVFbB`@HB~Bp@b@LVLXL^`@HHFXTj@`@l@XTXR^Hn@Pd@HPFb@\\dAv@`DnCnA`Ah@^DBDDJDLFTJLDn@LJ@p@Jj@?v@Pp@f@VJTDr@?F?V?L?f@ANANIHKJQBKL_@`@]nBIxAD|AVxB\\~Cb@t@NrCb@lDj@\\@H@l@?d@?dC?pBEl@EhHKxBMHAb@E\\EDAVGxAS~B_@f@Ib@Ij@K`Ce@b@Ip@KZGzCk@xCe@jCi@bEq@|Cg@n@K|B]\\GZGv@E\\Bf@FVFVD`@VDKFMLYPa@Xq@j@gAb@s@d@w@N_@Fc@Bk@KsF",
         "legs":[
            {
               "steps":[
                  {
                     "geometry":"wzidFwe_fWbHG|DCrDEv@?vDLN@J@L?Z@P@H@hABrBA~B?rEKl@Ah@?F?h@AzGBv@BF?R@b@@b@LzA^ZHB@x@RfElAVFbB`@HB~Bp@b@LVLXL^`@HHFXTj@`@l@XTXR^Hn@Pd@HPFb@\\dAv@`DnCnA`Ah@^DBDDJDLFTJLDn@LJ@",
                     "maneuver":{
                        "bearing_after":179,
                        "bearing_before":0,
                        "location":[
                           126.977076,
                           37.575001
                        ],
                        "modifier":"right",
                        "type":"depart"
                     },
                     "mode":"driving",
                     "ref":"48; 31",
                     "driving_side":"right",
                     "name":"세종대로",
                     "intersections":[
                        {
                           "out":0,
                           "entry":[
                              true
                           ],
                           "bearings":[
                              179
                           ],
                           "location":[
                              126.977076,
                              37.575001
                           ]
...
```

요청과 응답에 대한 정보는 [공식 api 문서](https://github.com/Project-OSRM/osrm-backend/blob/master/docs/http.md)를 참고해 주세요. 


# 마무리

역시 오픈소스는 위대합니다. 
직접 구현하려면 한참 걸렸을 작업들을 손쉽게 구축 완료했습니다. 
오늘 사용한 오픈소스 라이선스는 모두 BSD이고, OSM 데이터 역시 OSM과 기여진을 밝히는 조건으로 자유롭게 사용이 가능합니다.