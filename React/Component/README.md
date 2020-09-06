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
true의 경우 프로퍼티의 이름만 선언해도 전달할 수 있다.
프로퍼티에 true를 전달한 예제
```
<ChildComponent boolValue />
```
프로퍼티에 false를 전달한 예제
```
<ChildComponent />
