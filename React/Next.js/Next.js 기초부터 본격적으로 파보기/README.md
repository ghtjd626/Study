# Next.js 기초부터 본격적으로 파보기
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
## 페이지 라우팅
Next.js는 라우터를 내장하고 있다.
page 디렉토리에 about.js 파일을 생성해서 다음과 같이 컴포넌트를 만든다.
```
const About = () => (
    <div>
        <h2>안녕하세요 저는 Velopert 입니다.</h2>
    </div>
)

export default About;
```
그 다음에 Index 페이지에서 링크를 달아준다.
```
import Link from 'next/link'

const Index = () => (
    <div>
        <h1>
            안녕, Next.js
        </h1>
        <h2>
            <Link href="/about">
                <a>소개</a>
            </Link>
        </h2>
    </div>
);

export default Index;
```
주의할 점은 <Link> 컴포넌트를 사용 할 때, react-router에서는 **to** 를 썼겠지만 여기선 **href** 라는 점과, 이 컴포넌트 내부에 **문자열이 아닌 컴포넌트 혹은 엘리먼트가 있어야 한다.**<br><br>
추가적으로 스타일링을 할 땐, 그 내부의 엘리먼트에 **style** 혹은 **className** 을 설정하면 된다. *꼭 a 태그가 아니여도 된다.* 만약에 컴포넌트를 넣은다면 해당 컴포넌트가 **onClick** props를 전달받아서 실행하도록 해야한다.
## 공용 컴포넌트 만들기
  각 페이지에서 보여지는 헤더 컴포넌트를 만들어보자
  components 디렉토리를 만들어서 내부에 Header.js 라는 파일을 만든다.
```
import Link from 'next/link';

const linkStyle = {
    marginRight: '1rem'
}
const Header = () => {
    return (
        <div>
            <Link href="/"><a style={linkStyle}>홈</a></Link>
            <Link href="/about"><a style={linkStyle}>소개</a></Link>
        </div>
    );
};

export default Header;
```
그 다음엔, about 페이지, index 페이지 각각 페이지에서 Header.js 를 불러온다음에 상단에 넣는다.
```
import Header from '../components/Header';

const About = () => (
    <div>
        <Header/>
        <h2>안녕하세요 저는 Velopert 입니다.</h2>
    </div>
)

export default About;
```
```
import Link from 'next/link'
import Header from '../components/Header';

const Index = () => (
    <div>
        <Header/>
        <h1>
            안녕, Next.js
        </h1>
        <h2>
            <Link href="/about">
                <a style={{background: 'black', color: 'white'}}>소개</a>
            </Link>
        </h2>
    </div>
);
```
페이지 관련 컴포넌트들은 꼭 **pages** 디렉토리에 넣어야 하는 반면, 다른것들은 꼭 **pages** 디렉토리에 넣지 않아도, 상대 경로를 사용하여 불러올 수 있다.<br><br>
근데 헤더 컴포넌트가 필요한 모든 페이지마다 다 불러오는건 조금 번거롭다.
그러므로, components 디렉토리에 **Layout** 이란 컴포넌트를 만든다.
```
import Header from './Header';

const Layout = ({children}) => (
    <div>
        <Header/>
        {children}
    </div>
);

export default Layout;
```
기존에 index.js 와 about.js 에서 Header 를 지워주고 Layout 을 불러와서 감싸준다.
```
import Layout from '../components/Layout';

const About = () => (
    <Layout>
        <h2>안녕하세요 저는 Velopert 입니다.</h2>
    </Layout>
)

export default About;
```
```
import Link from 'next/link'
import Layout from '../components/Layout';

const Index = () => (
    <Layout>
        <h1>
            안녕, Next.js
        </h1>
        <h2>
            <Link href="/about">
                <a style={{background: 'black', color: 'white'}}>소개</a>
            </Link>
        </h2>
    </Layout>
);

export default Index;
```
## 라우트 - 유동적인 주소 사용하기
