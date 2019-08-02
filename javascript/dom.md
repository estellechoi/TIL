
## 문서객체모델(DOM)과 브라우저객체모델(BOM)
### 문서객체모델 (DOM, Document Object Model)
* HTML, XML 문서의 프로그래밍 인터페이스
* 웹페이지를 자바스크립트와 같은 스크립트언어로 제어하기 위한 객체 모델
* 윈도우에 로드된 문서를 의미
* HTML 문서의 계층구조를 트리 형태로 표현


### 브라우저객체모델 (BOM, Browser Object Model)
* 웹브라우저 창을 관리할 목적으로 제공되는 객체 모음을 대상으로 하는 모델
* 웹브라우저의 창이나 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단
* 전역객체인 Window의 프로퍼티와 메소드를 통해 제어

### 동적 웹페이지 작성
#### 1. id로 접근

```Javascript
    document.all.bb[2].style.color = "blue";
    // #bb 가 2 개 이상일 경우 배열 처리

    document.getElementById("bb").style.fontSize = "40px";
    // 현재문서에서 #bb 중 첫번째만 호출
```

#### 2. tag로 접근

```Javascript
    var x = document.getElementsByTagName("li");
    // <li> 태그가 1개 이더라도 자동 배열 처리
    x[0].style.color = "red";
```

#### 3. class로 접근

```Javascript
    var x = document.getElementsByClassName("aa");
    // .aa 가 1 개 이더라도 자동 배열 처리
    x[2].style.background="blue";
    document.getElementsByClassName("aa")[3].style.background = "blue";
```

#### 4. name으로 접근

```Javascript
    <input type = "text" name = "leh">
    document.all.leh.style.color = "green";
    <input type = "text" name = "userid"><p>
    <input type = "text" name = "pwd"><p>
    var x = document.getElementsByName("userid");
    x[0].style.border = "1px solid pink";
```

#### 5. querySelector

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
