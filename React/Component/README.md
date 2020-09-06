# Component
## 프로퍼티 사용법
프로퍼티는 상위 컴포넌트가 하위 컴포넌트에 값을 전달할 때 사용한다. **'단방향으로 데이터가 흐른다'** 라고 표현한다. 이때 프로퍼티 값은 수정할 수 없다.
### 문자열 프로퍼티
prop-types 사용
```
<div>
  {this.props.name}
</div>
```
자료형 선언 예제
```
PropsComponent.proptypes ={
  name : Proptypes.string,
};
```

### 그 외의 프로퍼티
프로퍼티에 문자열을 전달할 때는 큰따옴표를 사용한다. 하지만 숫자형이나 불리언 등의 문자열 외의 값을 전달할 때는 따옴표 대신 중괄호({})를 사용해야 한다.
```
class App extends React.Component {
  render() {
    const array = [1, 2, 3];
    const obj = {name: '제목', age : 30};
    const node = <h1>노드<h1>;
    const func = () => { console.log('메시지'); };
    return (
      <ChildComponent
        boolValue={true}
        numValue={1}
        arrayValue={array}
        objValue={obj}
        nodeValue={node}
        funcValue={func}
      />
    );
  }
}
```
### 불리언 프로퍼티
true의 경우 프로퍼티의 이름만 선언해도 전달할 수 있다.<br>
프로퍼티에 true를 전달한 예제
```
<ChildComponent boolValue />
```
프로퍼티에 false를 전달한 예제
```
<ChildComponent />
```
### 객체형 프로퍼티
```
class ChildComponent2 extends React.Component {
  render() {
    const {
      objValue,
    } = this.props;
    
    return (
      <div>
        <div>객체값: {String(Object.entries(objValue))}</div>    //객체를 문자열로 변환하여 출력
      </div>
    );
  }
}

ChildComponent2.propTypes = {
  objValue : PropTypes.shape({          //객체 프로퍼티의 자료형은 PropTypes의 shape를 사용하여 정의
    name: PropTypes.string,
    age: PropTypes.number,
  }),
}
```
### 필수 프로퍼티
특정 컴포넌트에 꼭 전달되어야 하는 프로퍼티가 있다면 해당 프로퍼티를 필수 프로퍼티로 지정하면 된다.
```
ChildComponent2.propTypes = {
  requiredStringValue: PropTypes.string.isRequired,
}
```
### 프로퍼티에 기본값 지정하기
```
DefaultPropsComponent.defalutProps ={
  boolValue : false,
};
```
### 자식 프로퍼티
JSX에서 컴포넌트 하위에 배치한 노드(또는 컴포넌트)를 하위 컴포넌트에서 프로퍼티로 접근할 수 있게 해준다.<br>
*App.jsx
```
...
class App extends React.Component {
  render() {
    return (
      <div>
        <ChildProperty>
          <div><span>자식노드</span></div>
        </ChildProperty>
      </div>
    );
  }
}
...
```
*ChildProperty.jsx
```
...
class ChildProperty extends Component {
  render() {
    return <div>{this.props.children}</div>
  }
}
...
```
## 컴포넌트 상태 관리하기
값을 바꿔야 하는 경우 **state**를 사용한다
### state로 상태 관리하기
state는 '값을 저장하거나 변경할 수 있는 객체'로 보통 버튼을 클릭하거나 값을 입력하는 등의 이벤트와 함께 사용된다.<br>
#### setTimeout() 함수를 통해 4초 후 state에 저장되어 있는 값을 변경하는 예제
```
import React from 'react';

class StateExample extends React.Component{
  constructor(props){
    super(props);
      this.state = {
        loading: true,
        formData : 'no data',
      };
      
      this.handleData = this.handleData.bind(this);
      
      setTimeout(this.handleData, 4000);  //인자를 함수형태를 전달받는 형태를 콜백함수라 한다.
  }

  handleData() {
    const data = 'new data';
    const { formData } = this.state;

    this.setState({
      loading: false,
      formData: data + formData,
    }); 
    console.log('loading값', this.state.loading);
  }
  render() {
    return(
      <div>
        <span>로딩중 : {String(this.state.loading)}</span>
        <span>결과 : {this.state.formData}</span>
      </div>
    );
  }
}
export default StateExample;
```
#### state를 사용할 때 주의할 점
1. 생성자에서 반드시 초기화 해야한다.(빈 객체라도 넣어야한다.)
2. state값을 변경할 때는 setState() 함수를 반드시 사용해야 한다.
3. setState() 함수는 비동기로 처리되며, setState() 코드 이후로 연결된 함수들의 실행이 완료된 시점에 화면 동기화 과정을 거친다.
#### state값은 setState() 함수로 변경한다
render() 함수로 화면을 그려주는 시점은 리액트 엔진이 정하기 때문이다.
#### setState() 함수의 인자로 함수를 전달하면 이전 state값을 쉽게 읽을 수 있다
```
//일반 함수를 사용한 예
handleData(data) {
  this.setState(function(prevState){
    const newState = {
      loading: false,
      formData: data + prevState.formData,
    };
    return newState;
  });
}

//화살표 함수를 사용한 예
handleData(data) {
  this.setState(prevState => ({
    loading: false,
    formData: data + preState.formData
  });
}
```
