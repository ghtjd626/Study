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
