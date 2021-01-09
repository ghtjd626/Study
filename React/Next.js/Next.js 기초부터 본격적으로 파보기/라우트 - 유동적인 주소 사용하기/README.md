## 라우트 - 유동적인 주소 사용하기
### 쿼리 파라미터
쿼리 파라미터는 ```/search?keyword=something```의 형태이다.<br><br>
pages 디렉토리에 search.js 파일을 만들어서 다음과 같이 적는다
```
import React from 'react';
import Layout from '../components/Layout';

const Search = ({url}) => {
    return (
        <Layout>
            당신이 검색한 키워드는 "{url.query.keyword}" 입니다.
        </Layout>
    );
};

export default Search;
```
직접 주소를 쳐서 keyword 값을 설정해본다.
```lacalhost:3000/search?keyword=깃허브```
> url props 를 보면 push 메소드도 딸려서 온다. 이 메소드를 사용하여 원하는곳으로 라우팅을 할 수 있다.
### Pathname 파라미터
pathname 파라미터는 **/post/:id** 에서 id 부분을 칭하는데요. Next.js 에서는 이걸 하려면 커스텀 서버 API 를 사용해야한다.
