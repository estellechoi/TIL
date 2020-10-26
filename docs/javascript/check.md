## JSP 문서 로드시 \<input type\=\"radio\"\> \& \<select\> 체크 설정하기
### 1. \<input type\=\"radio\" checked\>
 - radio 배열의 인덱스(index)와 값(value)을 동일하게 설정
 - value = index = rs.getInt(\"name\")
 - rs.getInt(\"name\") → aa[\<\%\=rs.getInt(\"aa\")\%\>]
 - checked 속성의 값은 true

#### 예시

```Javascript
    function Check() {
    document.form.aa[<%=rs.getInt("aa")%>].checked = true;
    }
```

### 2. \<select\> \<option value\=\"2019\" checked\>
 - \<select\>의 value 사용

#### 예시

```JavaScript
    function Check() {
    document.form.age.value = "<%=rs.getString("age")%>";
    // value = ""; 문법 주의
    // 따옴표를 잊으면 에러가 난다 !
    }
```

### 3. 함수 호출
 - 문서 로드 시 \: \<body onload\=\"Check()\"\>

#### 예시

```JavaScript
  <body onload="Check()">
```

#### 참고 코드
[Test_board3/WebContent/content.jsp](https://github.com/estellechoi/jsp-tutorials/blob/master/Test_board3/WebContent/content.jsp)
