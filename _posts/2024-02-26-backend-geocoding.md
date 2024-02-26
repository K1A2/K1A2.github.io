---
title: '[Python] 좌표를 행정동 주소로 바꾸기'
categories: [web]
tags: [web, backend, data, python]
---

# 개요

서비스를 개발하다 보면 **역지오코딩** 또는 **리버스지오코딩**이라고 불리는 좌표를 주소, 지역 등 텍스트로 변환하는 작업이 필요할때가 있습니다.    
  
네이버, 구글, 카카오 등 많은 기업에서 관련 api를 제공하고 있지만, 무료인 대신 사용량 제한이 있거나 건당 유료인 경우도 있습니다.
필요한 목적에 따라서 제한에 걸리지 않는 경우도 있지만, 수십만개의 데이터를 전처리 해야한다거나 하는 상황에서는 기업에서 제공하는 api로는 한계가 있습니다. 
그래서 역지오코딩 코드를 직접 작성하게 되었습니다.


# 구현하기

## 데이터 준비

역지오코딩을 구현하기 위해서 행정동 shapefile과 행정동 코드를 텍스트로 바꿔줄 행정동 코드 정보 파일 두 가지 데이터가 필요합니다.  
이러한 파일들은 v-world에서 제공하고 있습니다. 
[다운로드 받기](https://www.vworld.kr/dtmk/dtmk_ntads_s002.do?dsId=30017)  

해당 사이트에서 **BND_ADM_DONG_PG.zip**과 **센서스 공간정보 지역 코드.xlsx** 파일을 다운받아 주세요.
그 후 zip파일은 압축 해제해 주세요. 모두 완료하면 데이터 준비는 끝났습니다.


## 데이터 보기

다음 코드를 다운받은 shp 데이터 파일을 확인해봅시다.  
우선 **geopandas**와 **matplotlib** 패키지를 먼저 설치해주세요. 
그리고 zip 파일을 압축 해제하고 dbf, prj, shp, shx 확장자로 끝나는 4개의 파일을 같은 실행하려는 파이썬 파일과 폴더에 모두 넣어주세요.  

```python
import geopandas as gpd
import matplotlib.pyplot as plt

shp_file_path = './BND_ADM_DONG_PG.shp'
shp_data = gpd.read_file(shp_file_path)
print(shp_data.head())
shp_data.info()

shp_data.convex_hull.plot(color='gray', edgecolor="w")
plt.show()
```

콘솔에는 다음과 같이 출력될겁니다.
>   BASE_DATE  ...                                           geometry  
> 0  20220630  ...  POLYGON ((197702.069 553187.311, 197703.431 55...  
> 1  20220630  ...  POLYGON ((198170.457 553770.678, 198172.189 55...  
> 2  20220630  ...  POLYGON ((196621.023 556395.880, 196628.323 55...  
> 3  20220630  ...  POLYGON ((197800.719 559064.245, 197782.581 55...  
> 4  20220630  ...  POLYGON ((196444.745 553384.564, 196471.618 55...  
> [5 rows x 4 columns]  
> <class 'geopandas.geodataframe.GeoDataFrame'>  
> RangeIndex: 3518 entries, 0 to 3517  
> Data columns (total 4 columns):  
>  #   Column     Non-Null Count  Dtype   
> ---  ------     --------------  -----    
>  0   BASE_DATE  3518 non-null   object   
>  1   ADM_CD     3518 non-null   object  
>  2   ADM_NM     3518 non-null   object  
>  3   geometry   3518 non-null   geometry  
> dtypes: geometry(1), object(3)  
> memory usage: 110.1+ KB

총 3518개의 지역 정보가 있고, 행정동 코드, 행정동 이름, 폴리곤 정보로 구성되있음을 알 수 있습니다. 
폴리곤 정보는 지역의 경계를 다각형으로 표현한겁니다. 출력된 그래프를 보시면 한번에 이해하실 수 있을겁니다.

![winner](/assets/images/2024-02-26-backend-geocoding/myplot.png){: width="700px"}
_각 지역의 경계가 표현됨_

이번엔 센서스 공간정보 지역 코드.xlsx 파일을 확인해봅시다.  
이번에는 **pandas**와 **openpyxl** 패키지를 설치해주세요.  

```python
import pandas as pd
adm_file_path = './센서스 공간정보 지역 코드.xlsx'
adm_data = pd.read_excel(adm_file_path, engine='openpyxl', header=1, sheet_name='2022년6월')
print(adm_data.head())
adm_data.info()
```

콘솔에는 다음과 같이 출력될겁니다.
>    시도코드   시도명칭  시군구코드 시군구명칭  읍면동코드 읍면동명칭  
> 0    11  서울특별시     10   종로구    530   사직동  
> 1    11  서울특별시     10   종로구    540   삼청동  
> 2    11  서울특별시     10   종로구    550   부암동  
> 3    11  서울특별시     10   종로구    560   평창동  
> 4    11  서울특별시     10   종로구    570   무악동  
> <class 'pandas.core.frame.DataFrame'>  
> RangeIndex: 3518 entries, 0 to 3517  
> Data columns (total 6 columns):  
>  #   Column  Non-Null Count  Dtype   
> ---  ------  --------------  -----   
>  0   시도코드    3518 non-null   int64   
>  1   시도명칭    3518 non-null   object  
>  2   시군구코드   3518 non-null   int64   
>  3   시군구명칭   3518 non-null   object  
>  4   읍면동코드   3518 non-null   int64   
>  5   읍면동명칭   3518 non-null   object  
> dtypes: int64(3), object(3)  
> memory usage: 165.0+ KB

총 3518개의 지역 정보가 있고, 시도, 시군구, 읍면동으로 분리되어 각각 코드와 명칭이 매칭되어있음을 알 수 있습니다. 
행정동 코드의 구성도 알 수 있는데, 처음 두 자리는 시도 코드, 세 번째에서 네 번째 자리는 시군구 코드, 다섯 번쨰에서 마지막까지는 읍면동코드 입니다.


## 데이터 통합하기

이제 두 파일을 통합하여 좌표를 입력하면 시도, 시군구, 읍면동 이름을 가져올 수 있게 만들어봅시다.

```python
import geopandas as gpd
import pandas as pd
from tqdm import tqdm

shp_file_path = './BND_ADM_DONG_PG.shp'
shp_data = gpd.read_file(shp_file_path, encoding='euc-kr')
print(shp_data.head())
shp_data.info()

print()

adm_file_path = './센서스 공간정보 지역 코드.xlsx'
adm_data = pd.read_excel(adm_file_path, engine='openpyxl', header=1, sheet_name='2022년6월')
print(adm_data.head())
adm_data.info()

adm_name = {'ADM_NM_FIRST': [], 'ADM_NM_MID': [], 'ADM_NM_LAST': []}
for i in tqdm(range(shp_data.shape[0])):
    adm_code = shp_data.loc[i, 'ADM_CD']
    code_first = int(adm_code[:2])
    code_mid = int(adm_code[2:5])
    code_last = int(adm_code[5:])
    check = 0
    for j in range(adm_data.shape[0]):
        adm_first = adm_data.loc[j, '시도코드']
        adm_mid = adm_data.loc[j, '시군구코드']
        adm_last = adm_data.loc[j, '읍면동코드']
        if code_first == adm_first and code_mid == adm_mid and code_last == adm_last:
            adm_name['ADM_NM_FIRST'].append(adm_data.loc[j, '시도명칭'])
            adm_name['ADM_NM_MID'].append(adm_data.loc[j, '시군구명칭'])
            adm_name['ADM_NM_LAST'].append(adm_data.loc[j, '읍면동명칭'])
            check = 1
            break
shp_data_edit = shp_data.drop(['ADM_NM'], axis=1)
shp_data_edit['ADM_NM_F'] = adm_name['ADM_NM_FIRST']
shp_data_edit['ADM_NM_M'] = adm_name['ADM_NM_MID']
shp_data_edit['ADM_NM_L'] = adm_name['ADM_NM_LAST']
shp_data_edit = shp_data_edit[['ADM_CD', 'ADM_NM_F', 'ADM_NM_M', 'ADM_NM_L', 'geometry']]
shp_data_edit = shp_data_edit.to_crs(epsg=4326)
print(shp_data_edit.head())

shp_data_edit.to_file("./data/adm_polygon.shp", encoding='utf-8')
```

shp 파일에서 행정동 코드를 가져와 시도/시군구/읍면동 코드로 분리하고, 각각의 코드에 맞는 행정동 이름을 엑셀 파일에서 가져와 추가하는 방식입니다. 
이렇게 만들어진 파일은 data 폴더 안에 amd_polygon.shp라는 이름으로 저장됩니다.


## 역지오코딩 테스트

이제 만들어진 shp 파일로 역지오코딩 테스트를 해봅시다. 
좌표가 주어졌을때 해당 좌표의 행정동 위치는 어디인지 알려면 주어진 좌표가 어느 행정동 폴리곤 안에 위치하는지 찾으면 됩니다.

![winner](/assets/images/2024-02-26-backend-geocoding/polygon.png){: width="200px"}
_예시_

예시에서 빨간 점이 주어진 좌표라면, 폴리곤 안에 좌표가 포함되어 있다면 그 폴리곤의 행정동 이름을 리턴하면 되는겁니다. 
이러한 코드를 직접 작성해도 좋지만, **shapely**라는 패키지를 이용하면 훨씬 간단하게 구현 가능합니다. shapely를 이용해 역지오코딩 테스트를 해봅시다.

```python
import geopandas as gpd
import time

from shapely.geometry import Point

shp_file_path = './data/adm_polygon.shp'
shp_data = gpd.read_file(shp_file_path)

point = Point(126.922668, 37.553979)

start = time.time()

region = shp_data[shp_data.geometry.contains(point)]

if not region.empty:
    region_name = f"{region.iloc[0]['ADM_NM_F']} {region.iloc[0]['ADM_NM_M']} {region.iloc[0]['ADM_NM_L']}"
else:
    region_name = "Unknown"
print(region_name, f'{time.time() - start} sec')
```

위 코드를 실행해보면 동경 126.922668, 북위 37.553979은 서울특별시 마포구 서교동임을 알 수 있습니다. 
만약 어디에도 포함되지 않는 좌표가 주어진다면 Unknown을 리턴합니다.


# 활용하기

저는 FastAPI로 만들어서 서버에 올려두고 팀원들이랑 사용했습니다. 
혹시 필요한 분들은 깃허브에 fastapi로 작성된 코드 올려두었으니 데이터만 넣어서 사용해보세요. 
[레포지토리 이동](https://github.com/K1A2/reverse-geocoding-fastapi)  
   
shp 파일을 찾다보면 건물별 도로명 주소, 법정동 등 다양한 shp 파일들이 존재합니다. 
이것들도 잘 이용한다면 행정동 코드 뿐만 아니라 다양한 주소변환 코드 작성이 가능합니다.