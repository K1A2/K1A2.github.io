---
title: '[React] React의 기본 구조'
categories: [개발, react]
tags: [web, front, react]
---

# React의 구조

![winner](/assets/img/2023-03-05-react-structure/structure.png){: width="400px"}

* **node_modules**  
    npm 패키지들이 설치되는 공간
* **public**  
    static 파일들을 저장하는 공간
* **src**  
    리엑트를 구성하는 파일을 저장하는 공간
* **package.json**  
    프로젝트에 설치된 패키지들을 명시하는 파일
    ```json
    "dependencies": {
        "@testing-library/jest-dom": "^5.16.5",
        "@testing-library/react": "^13.4.0",
        "@testing-library/user-event": "^13.5.0",
        "react": "^18.2.0",
        "react-dom": "^18.2.0",
        "react-scripts": "5.0.1",
        "web-vitals": "^2.1.4"
    }
    ```

## public 폴더

* **favicon.ico**  
    웹페이지의 아이콘. 브라우저 상단 탭에 나오는 아이콘이다.  
    ![winner](/assets/img/2023-03-05-react-structure/fav_ex.png){: width="400px"}
* **index.html**  
    가상 DOM을 위한 HTML 파일.  
    index.js가 렌더링한 결과가 index.html에 root div에 표시됨.
    ```html
    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="utf-8" />
            <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
            <meta name="viewport" content="width=device-width, initial-scale=1" />
            <meta name="theme-color" content="#000000" />
            <meta
            name="description"
            content="Web site created using create-react-app"
            />
            <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
            <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
            <title>React App</title>
        </head>
        <body>
            <noscript>You need to enable JavaScript to run this app.</noscript>
            <div id="root">
                <!--여기에 렌더링-->
            </div>
        </body>
    </html>
    ```
* **manifest.json**  
    PWA(Progressive Web App)을 지원하기 위한 설정 파일
    ```json
    {
        "short_name": "React App",
        "name": "Create React App Sample",
        "icons": [
            {
            "src": "favicon.ico",
            "sizes": "64x64 32x32 24x24 16x16",
            "type": "image/x-icon"
            },
            {
            "src": "logo192.png",
            "type": "image/png",
            "sizes": "192x192"
            },
            {
            "src": "logo512.png",
            "type": "image/png",
            "sizes": "512x512"
            }
        ],
        "start_url": ".",
        "display": "standalone",
        "theme_color": "#000000",
        "background_color": "#ffffff"
    }
    ```
* **robots.txt**  
    웹 크롤러같은 로봇들의 접근 제한을 위한 파일

## src 폴더

* **index.js**  
    HTML탬플릿과 JS 컴포넌트를 조합하여 화면을 렌더링 하는 파일  
    ```js
    import React from 'react';
    import ReactDOM from 'react-dom/client';
    import './index.css';
    import App from './App';
    import reportWebVitals from './reportWebVitals';

    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(
    <React.StrictMode>
        <App />
    </React.StrictMode>
    );

    reportWebVitals();
    ```
* **index.css**  
    index.html의 스타일을 정의하는 파일
    ```css
    body {
        margin: 0;
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
            'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
            sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }

    code {
        font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
            monospace;
    }
    ```
* **App.js**  
    컴포넌트를 정의하는 파일. 실제 화면에 렌더링 되는 내용을 정의한다.
    ```js
    import logo from './logo.svg';
    import './App.css';

    function App() {
    return (
        <div className="App">
        <header className="App-header">
            <img src={logo} className="App-logo" alt="logo" />
            <p>
            Edit <code>src/App.js</code> and save to reload.
            </p>
            <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
            >
            Learn React
            </a>
        </header>
        </div>
    );
    }

    export default App;
    ```
* **App.css**  
    App.js에서 작성한 컴포넌트에 사용할 css를 정의하는 파일
    ```css
    .App {
        text-align: center;
    }

    .App-logo {
        height: 40vmin;
        pointer-events: none;
    }

    @media (prefers-reduced-motion: no-preference) {
        .App-logo {
            animation: App-logo-spin infinite 20s linear;
        }
    }
    ...
    ```
* **reportWebVitals.js**  
    React의 성능을 측정하기 위한 파일  
    ```js
    const reportWebVitals = onPerfEntry => {
    if (onPerfEntry && onPerfEntry instanceof Function) {
            import('web-vitals').then(({ getCLS, getFID, getFCP, getLCP, getTTFB }) => {
                getCLS(onPerfEntry);
                getFID(onPerfEntry);
                getFCP(onPerfEntry);
                getLCP(onPerfEntry);
                getTTFB(onPerfEntry);
            });
        }
    };

    export default reportWebVitals;
    ```
* **setupTests.js**  
    React 테스트를 위한 setup을 정의하는 파일  
* **App.test.js**  
    React App을 테스트하기 위한 파일  
    ```js
    import { render, screen } from '@testing-library/react';
    import App from './App';

    test('renders learn react link', () => {
        render(<App />);
        const linkElement = screen.getByText(/learn react/i);
        expect(linkElement).toBeInTheDocument();
    });
    ```