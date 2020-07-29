# ※ 리액트 훅(React Hooks)의 useInput
- - -

input을 사용하여 사용자에게 입력을 받고 이를 처리하기 위해 리액트훅의 useInput 기능이 있다.   
먼저 클래스에서 input을 사용하여 어떻게 입력받는지 보자.   

```
// 클래스형 컴포넌트
class App extends React.Component {
  state={
    value : '',
  }
  onChange = e =>{
    this.setState({
      value : e.target.value
    })
  }
  render(){
    return(
      <div>
        <input type="text" value={this.state.value} onChange={this.onChange} />
      </div>
    )
  }
}
    
// 함수형 컴포넌트
const useInput = initialValue => {
    const [value, setValue] = useInput(initialValue);
    const onChange = e =>{setValue(e.target.value)}
    return {value, onChange};
  }
function App(){
  const name = useInput("Mr.");
  return(
    <div>
      <input {...t} />
    </div>
  );
}
```

우선 코드의 길이의 변화가 있다. 코드가 길어지고 input에 기능이 들어갈수록 다른 함수를 추가해주어야 하고 작성해야 하는데   
코드가 길어질수록 그 차이는 명확해질것이다!   
또한 함수형컴포넌트에선 ```{...t}```로 useInput에서 return한 기능을 모두 사용할 수 있게 해준다. 이것이 가장 큰 장점이다.   

참고로 useInput에서 value를 썻다고 input의 value를 담당하는 것이 아니라, return 값에서 기본적으로 value로 해주는 것 같다...   
  
