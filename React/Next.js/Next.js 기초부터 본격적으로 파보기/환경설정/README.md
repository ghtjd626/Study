## 환경설정
> 이 포스트에선 npm 대신 yarn을 사용했다.
```
mkdir hello-next
cd hello-next
yarn init -y
yarn add react react-dom next
mkdir pages
```
package.json 파일을 열어 다음과 같이 스크립트를 추가해준다.
```
{
  "name": "hello-next",
  "version": "1.0.0",
  "license": "MIT",
  "scripts": {
    "dev": "next"
  },
  "dependencies": {
    "next": "^2.1.0",
    "react": "^15.4.2",
    "react-dom": "^15.4.2"
  }
}
```
index.js 파일을 만들어서 다음과 같이 코드를 작성한다.
```
const Index = () => (
    <div>
        <h1>
            안녕, Next.js
        </h1>
    </div>
);

export default Index;
```
코드에서 **import React from 'react';** 를 할 필요가 없다. 함수형 컴포넌트가 아닌 클래스 형 컴포넌트도 동일하지만 **class Index extends Component** 와 같은 형식으로 하는 것을 선호한다면, **import { Component } from 'react';** 는 선언 해주어야 한다. 혹은 **class Index extends React.Component** 로 작성해도 된다.<br><br>
그 다음, 이 명령어를 통해 개발 서버를 실행해 본다.
```
yarn run dev
```
