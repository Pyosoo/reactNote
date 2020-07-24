# ※ 리액트 훅(React Hook)
- - - 
이전 까지의 클래스형 컴포넌트들은 다양한 문제?와 복잡성이 있었다.   
함수형 컴포넌트에서 클래스형 컴포넌트의 기능을 할 수 있는 리액트 훅은, 앞으로의 리액트 프로그래밍 패턴에 큰 변화를 줄 것이라고 한다.    
기존에 많이 작성된 프로그램들이 리액트 훅으로 인하여 사용못하게 되는 것이아니라 항상 하위 호환성을 중요시하므로 이상은 없을 것이다.   
하지만 앞으로 새로운 프로젝트를 하게 된다면 훅을 사용하여 작성하는 것이 바람직하다.   


## ■ 리액트 훅이란?
훅은 함수형 컴포넌트에서도 클래스형 컴포넌트의 기능을 사용할 수 있게 하는 기능이다.   
여기서 기능이란 상탯값을 관리할 수 있고, 컴포넌트의 생명주기 함수를 이용할 수 있는 기능들이다.   
클래스형 컴포넌트에서 고차 컴포넌트와 랜더 속성값 패턴의 복잡도가 해결 될 수 있고,    
코드 압축이 잘 안되는 경우, 핫 리로드에서 난해한 버그 발생하는 경우, 컴파일 단계에서 코드를 최적화하기 어려운 경우 를 해결 한다.   


### ■ 훅의 장점
1.  재사용 가능한 로직은 쉽게 만들 수 있다.   
  => 훅이 단순한 함수이므로 함수안에서 다른 함수를 호출하는 것으로 새로운 훅을 만들 수 있기 때문이다.    
2.  같은 로직을 한곳으로 모을 수 있어 가독성이 좋다.    
3.  훅은 단순한 함수이므로 정적 타입 언어로 타입을 정의하기 쉽다.   
  => 고차 컴포넌트의 타입 정의와 비교되는 부분이다.    
  

# ■ 훅의 기능들
- - -
**① 상탯값 추가하기 useState**    
```
// 한개의 상탯값 추가
import React, {useState} from 'react';
function Profile(){
  const [name, setName] = useState('initail value');    //useState를 이용해 name이라는 상탯값을 만들고 초깃값을 넣어준다.
  return(
    <div>
      <p>{`name is ${name}`}</p>
      <input type="text" value={name} onChange={e => setName(e.target.value)} />  //setName이라는 정의된 함수를 통해 상탯값 name을 관리한다.
    </div>
  );
}

// 훅 하나로 여러개 상탯값 관리하기
const [state, setState] = useState({ name:'', age:0 });
//...
setState({ ...state, age:e.target.value });     // 클래스형 컴포넌트에서는 setState를 통해 기존 상탯값과 병합하지만,
                                                // 훅은 이전 상탯값을 지운다. 따라서 ...state와 같은 코드가 필요하다.
                                                // 이렇게 상태값을 하나의 객체로 관리하는 경우 useReducer 훅이 제공된다.(나중에설명)
``` 

**② 생명주기 함수 이용하기 useEffect**   
클래스형의 문제 : 서로 연관성없는 여러 기능이 하나의 생명 주기 메서드에 섞이게 된다.   
usetEffect 훅에 입력된 함수는 렌더링 결과가 실제 돔에 반영된 후 호출된다.   
```
userEffect( () => {
  //...
});
```

★함수형 컴포넌트에서 useEffect 훅에서 API호출 하는 코드   
```
import React, {useEffect, useState} from 'react';

function Profile({ userId }) {
  const [user, setUser] = useState(null);                   //API 결괏값을 저장할 상탯값이다.
  useEffect( () => {
    getUserApi(userId).then(data => setUser(data));
    },
    [usedId],                                               // useEffect 훅에 입력된 함수는 렌더링할 때마다 호출되기 때문에 API통신을 불필요하게 많이 하게된다.
                                                            // 이를 방지하기 위해 두 번째 매개변수로 배열을 입력하면, 배열의 값이 변경되는 경우에만 함수가 호출된다. 
  );
  return(
    <div>
      {!user && <p>사용자 정보를 가져오는 중... </p>}
      {user && (
        <>
          <p>{`name is ${user.name}`}</p>
          <p>{`age is ${user.age}`}</p>
        </>
      )}
    </div>
  );
}
```

★ 이벤트 처리 함수를 등록하고 해제하는 기능    
클래스형 컴포넌트에서는 componentDidMount 에서 무언가를 등록하고, componentWillUnmount 에서 해제하는 코드가 자주 사용된다.   
서로다른 여러 로직이 하나의 생명주기에 추가되며 복잡해지고, 해제하는걸 깜빡하게 한다.   
useEffect를 이용해 한곳에서 등록과 해제절차를 한번에 할 수 있다.   
```
import React, { useEffect, useState } from 'react';
function widthPrinter(){
  const [width, setWidth] = useState(window.innerWidth);
  useEffect(()=>{
     const onResize = () => setWidth(window.innerWidth);
     window.addEventListener('resize', onResize);         //창 크기가 변경될 때마다 onResize함수가 호출 되도록 등록
     return()=>{
      window.removeEventListener('resize', onResize);
    };
  }, []);                                                 //두번째 매개변수로 빈배열을 넣으면 마운트될때 첫번째 매개변수로 입력된 함수가 실행되고 
  return <div>{`width is ${width}`}</div>                 //언마운트될때 반환된 함수가 실행된다.
}
```
