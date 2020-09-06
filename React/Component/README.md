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
