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
