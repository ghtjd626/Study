# SPA에서의 SSR과 CSR
## 초기 View 로딩 속도
전통적인 SSR의 경우에는 View를 서버에서 렌더링해 가져오기 때문에, **첫 로딩이 매우 짧다.** 물론 SSR을 사용하면 클라이언트에서 JS 파일을 모두 다운로드하고 적용하기 전까지는 각각의 기능이 동작하지 않겠지만, 사용자의 입장에서는 매우 빠른 속도로 로딩이 됐다고 생각할 것이다.<br><br>
반면에 CSR은 서버에서 View를 렌더링하지 않기 전에 HTML와 JS 파일, 서비스에 필요한 리소스를 다운로드한 후 브라우저에서 렌더링 해 보여준다. 그렇기 때문에 SSR보다는 초기 View를 보기까지의 시간이 길다. 하지만, **보여진 기능 모두 사용자에게 보여지는 것과 동시에 동작한다.**
## SEO(Search Engine Optimization) 문제
SEO는 Search Engine Optimization의 약자로 검색 엔진 최적화를 의미한다. 즉, SEO 문제는 검색 엔진 최적화 문제를 말하고 이는 검색 엔진에 우리의 서비스를 노출시키는 전략을 이야기하기도 한다.<br><br>
일반적으로 검색 엔진의 크롤러들은 데이터를 긁어올 때 웹 페이지의 JS를 해석해 노출시키기 때문에 크롤링을 할 수 없는 시점에서는 검색 엔진에 데이터를 노출시키지 않는 것이고, 이는 상대적으로 우리의 서비스가 검색 엔진 서비스에 노출되는 것이 줄어듦을 의미한다.<br><br>
따라서 **CSR** 을 사용하면 View를 생성하기 위해서 JS가 필요하고(그 전까지는 빈 페이지이기 때문에 View가 완성되지 않아서), View를 생성하기 전까지는 검색 엔진 크롤러의 데이터 수집이 제한적이기 때문에 상대적으로 검색 엔진이 이해하는 정보가 부족해 **SEO에 유리하지 않게 된다.** 보편적으로 SEO가 낮은 서비스는 노출이 적어져 사용자 유입이 적어질테니 비즈니스적으로 좋지 않은 선택이 될 것이다.<br><br>
반대로 전통적인 **SSR** 은 View를 서버에서 렌더링해 제공하기 때문에(View를 먼저 그리기 때문에) 상대적으로 **SEO에 유리해져 사용자 유입이 많을 것이다.** 사실 SEO는 비즈니스적인 문제이기 때문에 여기에 대해서 깊게 이해하기 위해서는 비즈니스의 SEO와 관련된 문서를 읽어보면 도움이 될 것이라고 생각한다.<br><br>
정리하면 전통적인 **SSR** 은 초기 로딩 속도가 빠르고 SEO에 유리하지만, View 변경(화면 전환) 시 계속적으로(새로고침하며) 서버에 요청해야 하므로 서버에 부담이 크다. 그리고 **CSR** 은 초기 로딩 속도가 느리고 SEO에 대한 문제가 있지만, 초기 로딩 후에는 View를 서버에 요청하는 것이 아닌 Client에서 직접 렌더링하기 때문에 화면 전환이 매우 빠르다는 장점이 있다.
> 그래서 이를 해결하고자 최근에는 이 두 가지 렌더링 방법을 적절하게 융합한 방법들도 나오고 있다. 즉, 첫 번째 페이지 로딩에서는 서버 사이드 렌더링을 사용하고, 그 후에 모든 페이지 로드에는 클라이언트 사이드 렌더링을 활용하는 방안이 그것이다.
## 다시 본론으로...
NextJS의 탄생 배경을 이해하기 위해 앞선 것처럼 SSR과 CSR을 알아봤다. NextJS는 **리액트에서의 SSR을 위한 '도구(프레임워크)'라는 것** 을 어렴풋 유추해볼 수도 있다.<br><br>
**SPA에서의 SSR** 은 전통적인 SSR의 장점을 가져오며 CSR의 단점을 극복한 방법이라고 할 수 있다. 부분적으로 CSR, SSR을 사용한다거나 코드 스플리팅 등을 사용할 수 있다.<br><br>
NextJS에서 제공하는 [문서](https://nextjs.org/learn/basics/create-nextjs-app)에 따르면, 최근 각광받는 single page(단일) JavaScript applications에 대한 이야기와 이를 구현하는 것에 대한 어려움을 이야기한다.<br><br>
아직도 적절한 SSR을 적용한 SPA를 만들기 위해서는 높은 러닝 커브(client-side routing, page layout 등)를 감수해야 하기 때문에 이와 같은 이슈는 빠른 페이지 로드를 위해 사용하는 SSR을 적용하기 위한 어려움으로 이어질 수 있다고 한다.<br><br>
그래서 NextJS 팀은 사용자 설정이 가능하지만 간단한 무언가를 원했고, 이것을 PHP의 그것과 같이 라우팅에 대해서 걱정할 필요없으며 기본적으로 응용 프로그램이 서버에 렌더링되게 만들고자 생각했다고 한다. 그래서 탄생한 것이 NextJS이다.<br><br>
그렇다면, NextJS를 '잘' 사용하기 위해 문서에서 제공하는 NextJS로 할 수 있는 것에 대한 목록을 살펴보자<br><br>
* Server-rendered by default
* Automatic code splitting for faster page loads
* Simple client-side routing (page based)
* Webpack-based dev environment which supports Hot Module Replacement(HMR)
* Able to implement with Express or any other Node.js HTTP server
* Customizable with your own Babel and Webpack configurations<br><br>
위의 내용을 한글로 살펴보면 다음과 같이 이해할 수 있을 것이다.<br><br>
* 기본적으로 서버 렌더링
* 보다 빠른 페이지 로드를 위한 자동 코드 스플리팅
* 간단한 클라이언트 사이드 라우팅(page 기반)
* Hot Module Replacement(HMR)을 지원하는 웹팩 기반 개발 환경
* 익스프레스 혹은 어떤 Node.js HTTP server로 구현이 가능
* 바벨과 웹펙 설정으로 사용자 정의 가능<br><br>
여기까지 읽고 나서야 우리는 NextJS를 왜 사용하고, 언제 사용하면 좋을지를 알게 되었다.
