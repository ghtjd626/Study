## 외부 데이터 가져오기
Next.js 에선 이 부분을 클래스형 컴포넌트에 ```getInitialProps``` 라는 메소드를 설정하여 해결한다. 이 메소드가 서버사이드에서도 실행될수있고, 클라이언트 사이드에서도 실행될 수 있다. 심지어 필요에 따라 ```prefetch``` 도 될 수 있다.
### 사용법 살펴보기
```pages``` 에 ```ssr-test.js```라는 페이지를 만든다.
```
import Layout from '../components/Layout';

class SSRTest extends React.Component {
    static async getInitialProps ({req}) {
        return req 
            ? { from: 'server' } // 서버에서 실행 할 시
            : { from: 'client '} // 클라이언트에서 실행 할 시
    }

    render() {
        return (
            <Layout>
                {this.props.from} 에서 실행이 되었어요.
            </Layout>
        );
    }
}

export default SSRTest;
```
```getInitialProps``` 에서 실행한 메소드에서 리턴하는 값이 해당 컴포넌트의 props 로 전달된다.<br><br>
파라미터로는 서버측의 req 가 들어간다.그렇기에 client 에서의 req 는 undefined 이다.<br><br>
그 다음엔 헤더 컴포넌트에 SSR 테스트 링크를 추가한다.
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
            <Link href="/ssr-test"><a style={linkStyle}>SSR 테스트</a></Link>
        </div>
    );
};

export default Header;
```
주소로 직접 들어가면 server 에서 실행했다고 하지만 링크 컴포넌트를 눌러서 들어가면 client 에서 실행을 했다고 한다.<br><br>
자, 이제 기본적인 사용법을 이해했으니 진짜로 데이터를 fetching 해보자.
### axios 를 통한 ajax 요청
여기선 ```axios```를 사용하여 ajax 요청을 하도록 하겠다.<br>
우선 axios를 설치한다
```yarn add axios```
그 다음에, ```ssr-test.js``` 파일을 수정한다.
```
import Layout from '../components/Layout';
import axios from 'axios';

class SSRTest extends React.Component {
    static async getInitialProps ({req}) {
        const response = await axios.get('https://jsonplaceholder.typicode.com/users');
        return {
            users: response.data
        }
    }

    render() {
        const { users } = this.props;

        const userList = users.map(
            user => <li key={user.id}>{user.username}</li>
        )
        
        return (
            <Layout>
                <ul>
                    {userList}
                </ul>
            </Layout>
        );
    }
}

export default SSRTest;
```
### prefetch 기능 사용하기
링크 컴포넌트를 렌더링할때 ```<Link prefetch href="...">``` 형식으로 ```prefetch``` 값을 전달해주면 데이터를 먼저 불러온다음에 라우팅을 시작한다.<br><br>
```Header.js``` 컴포넌트를 다음과 같이 수정한다.
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
            <Link prefetch href="/ssr-test"><a style={linkStyle}>SSR 테스트</a></Link>
        </div>
    );
};

export default Header;
```
이렇게하면 링크를 누를때 prefetching 이 된다.
