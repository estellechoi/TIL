
## 객체모델 (Object Model) - 문서객체모델(DOM)과 브라우저객체모델(BOM)
### 1. 객체모델 (Object Model)
 - 웹브라우저의 구성요소들은 하나하나가 객체화되어 있다.
 - 자바스크립트로 이 객체들을 제어해서 웹브라우저를 제어할 수 있다.
 - 객체들은 서로 계층적인 관계로 구조화되어 있다.
 - DOM과 BOM은 window 객체의 하나의 속성이며, 브라우저 환경에서만 사용할 수 있다.

 [계층구조]

 | Window |
 | --- |

 |

 | Document Object Model | Browser Object Model | JavaScript Core |
 | --- | --- | --- |


####  * Window 객체란 ?
 - 브라우저 전체를 담당하는 전역객체
 - [ex] `window.close();` 현재 브라우저의 창을 닫습니다.

<br/>

### 2. 문서객체모델 (DOM, Document Object Model)
 - 문서에 대한 모든 내용을 담고있는 객체
 - 문서 즉 열려진 페이지에 대한 정보를 따로 객체화 시켜서 관리
 - elements(요소) 에 대한 접근을 통해 HTML의 내용을 변경, CSS (style) 의 내용 또한 변경 가능
 - document 객체, event 속성

<br/>

### 3. 브라우저객체모델 (BOM, Browser Object Model)
 - 브라우저에 대한 모든 내용을 담고있는 객체
 - 뒤로가기, 북마크, 즐겨찾기, 히스토리, URL정보 등
 - 브라우저가 가지고 있는 정보를 따로 객체화 시켜서 관리
 - history, location 객체

<br/>

### 4. DOM - 동적 웹페이지 작성 / 요소(Element) 접근
#### 1) id로 접근

```Javascript
    document.all.bb[2].style.color = "blue";
    // #bb 가 2 개 이상일 경우 배열 처리

    document.getElementById("bb").style.fontSize = "40px";
    // 현재문서에서 #bb 중 첫번째만 호출
```

#### 2) tag로 접근

```Javascript
    var x = document.getElementsByTagName("li");
    // <li> 태그가 1개 이더라도 자동 배열 처리
    x[0].style.color = "red";
```

#### 3) class로 접근

```Javascript
    var x = document.getElementsByClassName("aa");
    // .aa 가 1 개 이더라도 자동 배열 처리
    x[2].style.background="blue";
    document.getElementsByClassName("aa")[3].style.background = "blue";
```

#### 4) name으로 접근

```Javascript
    <input type = "text" name = "leh">
    document.all.leh.style.color = "green";
    <input type = "text" name = "userid"><p>
    <input type = "text" name = "pwd"><p>
    var x = document.getElementsByName("userid");
    x[0].style.border = "1px solid pink";
```

#### 5) querySelector

```Javascript
    var x = document.querySelector("ul");
    x.style.display = "flex";

    var x = document.querySelectorAll("li");
    for (i = 0; i <= 3; i++) {
      x[i].style.border = "1px solid black";
      x[i].style.marginLeft = "10px";
      x[i].style.listStyle = "none";
    }
```

### ?

<br/>

####  * 참고문서
 - [JavaScript Tutorial](https://www.w3schools.com/js/)
