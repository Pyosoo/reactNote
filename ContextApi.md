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

