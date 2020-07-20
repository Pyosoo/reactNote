# ※ React의 꽃 state
- - -

React에서 컴포넌트는 상탯값과, 속성값을 갖는다.   
상탯값은 우리가 알고있듯 **state** 이다.   
속성값은 props로 사용되는 값이다.   

리액트는 UI데이터를 상탯값과 속성값으로 관리하므로, 그 것들이 변경되면 UI를 갱신하는 render가 실행된다. 

### ■setState 사용하기   
setState를 사용하여 버튼 클릭시 배경색을 바꾸는 간단한 코드를 보자.   

```
class MYC extends React.Component {
  state = { color : 'red' };
  onClick = () => {
    this.setState({ color : 'blue' };
  };
  
  render(){
    return (
      <button style={{ backgroundColor : this.state.color }} onClick={this.onClick}>
        변경!
      </button>
      );
    }
 }
 ```  
setState 메서드가 호출되면 상탯값을 변경하고, 해당 컴포넌트를 다시 렌더링한다.   
따라서 **render 안에서 setState를 사용하는 것은 무한 루프에 빠질 수 있는 위험이 있다.**   
setState 메서드는 비동기로 동작하며, 리액트는 여러개의 setState 메서드를 배치(batch)로 처리한다.   
하지만, 순서는 보장된다. ```setState({count : this.state.count+1})``` 과 ```setState({count : this.state.count+2})``` 가 나란히 있어도   
비동기로 작동하지만 순서대로 작동한다.    


### ■props(속성값) 사용하기   
props는 컴포넌트간 데이터 교환을 가능하게 해준다.   
부모->자식 으로 이동할 수 있고 자식->부모로도 이동을 할 수 있다.  비교적 부모->자식 이동이 간단하다.   
또한 이러한 컴포넌트 간 데이터이동은 프로그램이 복잡해질수록 가독성은 물론 관리가 어려워질 수도있다.
그래서 큰 프로젝트를 할땐 상태값을 관리할때는 Redux 등 을 사용한다.   
다음 코드는 부모컴포넌트에서 속성값을 받아 자식컴포넌트를 변경하는 코드이다.   

```
//부모 컴포넌트
class Parrent extends React.Component {
  state = {
    count : 0
  }
  onClick = () => {
    const {count} = this.state;
    this.setState({ count : count + 1 });
  };
  const {count} = this.state;
  
  render(){
    return(
      <div>
        <Child value={`current count : ${count}`} />
        <button onClick={this.onClick}> 증가 </button/>
      </div>
    );
  }
}

//자식 컴포넌트
function Child(props){
  return <p>{props.value}</p>;
}
```

Child 컴포넌트는 부모 컴포넌트가 렌더링될 때마다 같이 렌더링 된다.   
만약 value 속성값이 변경될 때만 렌더링 되길 원한다면 React.memo || React.PureComponent 를 이용할 수 있다.   
함수형 컴포넌트에서는 React.memo를 사용하고 클래스형에서는 React.PureComponent를 사용한다.   


또한, 클래스형 컴포넌트는 리액트 내부에서 인스턴스로 존재한다.   
각 인스턴스는 **자신만의 메모리 공간을 갖고 있기 때문에** 같은 컴포넌트라고 해도 자신만의 상탯값이 존재한다.   
그래서 같은 컴포넌트를 다른 props를 주며 여러개를 같이 사용할 수 있다.   

