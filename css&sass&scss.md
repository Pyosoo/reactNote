# ※CSS와 SCSS 그리고 SASS   
- - -
### CSS 란 ?   
(Cascading Style Sheets)   

문서의 스타일을 구현하는 스타일 시트 언어이다.   
문서와 표현을 관리하는 코드를 분리하여 수정 관리를 쉽게 만들 수 있게 해준다.   
만약 클래스명이 겹치는 요소들이 여러 CSS파일에 사용되고 있다면 클래스명이 충돌되어 원하는 결과가 나오지 않을 수 도 있다..   

### Sass (Syntactically Awesome Stylesheets)   
CSS 파일이 길어지고 복잡해지게 되면 눈으로 알아보기도 힘들어진 뿐만아니라 유리보수가 어렵다.   
이를 해결하고자 CSS의 기능 + variable, nesting, mixin, inheritance 등의 개념이 추가 되며   
파일을 보기 좋게 작성 할 수 있게 된다.    
**Sass는 Preprocessing 과정을 통해 css로 해석 및 컴파일 된다.**   


### SCSS   
CSS 작성 스타일과 가장 유사하게 만들 수 있는 표현방법이다.   
따라서 많은 디자이너와 개발자들에게 이용되고 있다.   
SCSS와 Sass중 편한 것을 주로 사용하면 된다! 


#### 코드 차이
```
/**CSS Syntax**/

nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}


/**Sass Syntax**/

nav
  ul
    margin: 0
    padding: 0
    list-style: none

  li
    display: inline-block

  a
    display: block
    padding: 6px 12px
    text-decoration: none
    
    
    
    
  /**SCSS Syntax**/

nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}

```

출처 : https://yunzema.tistory.com/269
