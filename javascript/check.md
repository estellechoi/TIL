## JSP 문서 로드시 <input type="radio"> & <select> 체크 설정하기
### 1. \<input type\=\"radio\" checked\>
 - JavaScript 배열 인덱스 값 사용
 - checked 속성 값에 true 설정

#### 예시

```Javascript
    function Check() {
    document.form.aa[<%=rs.getInt("aa")%>].checked = true;
    }
```

### 2. \<select\> \<option value\=\"2019\" checked\>
 - <select>의 value 사용

#### 예시

```JavaScript
    function Check() {
    document.form.age.value = "<%=rs.getString("age")%>";
    // value = ""; 문법 주의
    // 따옴표를 잊으면 에러가 난다 !
    }
```
