# 콘텍스트 API
- - -
보통 상위 컴포넌트에서 하위 컴포넌트로 데이터를 전달할 때 속성값이 사용된다.   
하지만 가까운 컴포넌트들이 아니라 프로그램이 복잡해지면서 멀리떨어져있는 컴포넌트에 데이터를 전달하려고 하면 상당히 복잡해 질 수가 있다.   
이럴때 사용하면 좋은게 ContextAPI이다.    


우선 ContextAPI는 세가지? 과정을 거치는 것 같다.    
1.  선언 ```ex)const UserContext = React.createContext('defalutValue');```    
2.  Provider  ```<UserContext.Provider value="Value"> //... </UserContext.Provider>    
3.  Consumer  ```<UserContext.Consumer> //데이터 사용.. </UserContext.Consumer>   

먼저 맨위에 선언문을 보면 구조는 다음과 같다.   
React.createContext(defaultValue) => {Provider, Consumer} 

Consumer컴포넌트는 데이터를 찾기 위해 상위로 올라가면서 가장 가까운 Provider 컴포넌트를 찾는다.   
만약 최상위에 도달할 때까지 Provider 컴포넌트가 찾지 못한다면 기본값('defalutValue'로 정해놨던것)을 사용하게 된다.   
**Provider 컴포넌트의 속성값이 변경되면 하위의 Consumer 컴포넌트는 다시 렌더링 된다.**   


## ■여러 콘텍스트를 중첩해서 사용하기   
```
const UserContext = React.createContext('unknown');
const ThemeContext = React.createContext('dark');

class App extends React.Component {
  render(){
    return(
      <div>
        <ThemeContext.Provider value="light">
          <UserContext.Provider value="mike">
            <div>메뉴</div>
            <Profile /<
            <div>푸터</div>
          </UserContext.Provider>
        </ThemeContext.Provider>
      </div>
    );
  }
}

function Profile(){
  return(
    <div>
      <Greeting />
      {/* ... */}
    </div>
  );
}

function Greeting(){
  return(
    <ThemeContext.Consumer>
      {theme => (
        <UserContext.Consumer>
          {username => (
            <p style={{ color: theme === 'dark' ? 'gray' : 'green' }}>{`$username}님 안녕하세요`}</p>
        )}
        </UserContext.Consumer>
      )}
    </ThemeContext.Consumer>
  );
}
```  


## ■생명주기 메서드에서 contextType을 이용하여 사용하기   
```
const ThemeContext = React.createContext('dark');

class App extends React.Component {
  comonentDidMount(){
    const theme = this.context;
     // ...
   }
   //...
 }
 App.contextType = ThemeContext;
```  
이렇게 class에서 contextType을 지정해주고 사용할 수 있다.   
하지만! **이런식의 사용은 하나의 콘텍스트만 연결할 수 있다는 단점이 있다.**     


이 외에 고차 컨텍스트API 중첩, 하위컴포넌트에서 콘텍스트 데이터 수정이 있다.. 이는 실제 사용 사례가 생기면 추가하겠다.   


### ★ContextAPI 사용지 주의할 점

1.  불필요한 렌더링이 발생하는 경우   (잘 이해가 되지 않는다....)
```
const UserContext = React.createContext({ name : 'unknown' });

class App extends React.Component{
  // ...
  onChangeName = e => {
    const name = e.target.value;
    this.setState({ name });
  };
  render(){
    const {name} = this.state;
    return(
      <div>
        <UserContext.Provider value={( name })>     …①
          {/*...*/}
        </UserContext.Provider>
      </div>
    );
  }
}
```
① 콘텍스트 데이터로 객체를 전달하고 있다. 이처럼 작성하면 render 메서드가 호출될 때마다 새로운 객체가 생성된다.    
따라서 name이 변경되지 않아도 render메서드가 호출될때마다 하위의 Consumer 컴포넌트도 다시 렌더링 된다. 
    
콘텍스트 데이터 전체를 컴포넌트의 상탯값으로 관리하면서, 해결한 코드↓  
```
class App extends React.Component{
  state = {
    userContextValue: {
      name: 'unknown',
    },
  }
  onChangeName = e => {
    const name = e.target.value;
    this.setState({ userContextValue: {name} });
  };
  render(){
    const {userContextValue} = this.state;
    return(
      <div>
        <UserContext.Provider value={( userContextValue })>     
          {/*...*/}
        </UserContext.Provider>
      </div>
    );
  }
}
```

2.  Provider를 못찾는 경우   
COnsumer 컴포넌트와 Provider 컴포넌트를 적절한 위치에서 사용하지 않으면 데이터가 전달되지 않는다.
```
<UserContext.Provider value="A">
  {/* ... */}
</UserContext.Provider>
<Profile />
```
이 상황에서 Profile에서 사용된 Consumer는 Provider를 못찾고 기본 값을 사용하게 된다.(unknown)   
