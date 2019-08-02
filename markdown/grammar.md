## 마크다운(Markdown) 파일 작성법
### 1. 마크다운(Markdown) &#127776;
 마크다운(Markdown)은 텍스트 기반의 마크업 언어로 2004년 존 그루버에 의해 만들어졌다. 쉽게 쓰고 읽을 수 있으며 HTML로 변환이 가능하다. 특수기호와 문자를 이용한 매우 간단한 구조의 문법을 사용하여 웹에서도 빠르게 컨텐츠를 작성하고 직관적으로 인식할 수 있다. 마크다운이 최근 각광받기 시작한 이유는 깃헙(https://github.com) 덕분이다. 깃헙을 사용하는 사람이라면 누구나 리드미(README.md)를 통해 마크다운 문서를 접했을 것이다. 리드미는 깃헙 저장소(Repository)에 관한 설명을 작성하는 파일이다. 마크다운을 통해 소스코드에 대한 설명, 이슈 등을 간단하게 기록하고 가독성을 높일 수 있다는 강점이 부각되면서 점점 여러 곳으로 퍼져가게 된다.

<br/>

### 2. 마크다운 작성법
###  1) 헤더 (Headers)
#### 문법

    # h1
    ## h2
    ### h3
    #### h4
    ##### h5
    ###### h6

#### 사용예시

# h1
## h2
### h3

<br/>

###  2) 인용구
#### 문법

    > This is a blockquote.
    >> 2단
    >>> 3단

#### 사용예시

> This is a blockquote.
>> 2단
>>> 3단


블럭인용문자 내에서는 다른 마크다운 요소들을 포함할 수 있다. 아래와 같이 !

> #### h4
> * list

>     code block

<br/>

### 3) 목록 (List)
#### 문법

    1. apple
    2. banana
    3. carrot

    * apple
      * banana
        * carrot

    + apple
      + banana
        + carrot

    - apple
      - banana
        - carrot

#### 사용예시

1. apple
2. banana
3. carrot

* apple
  * banana
    * carrot

+ apple
  + banana
    + carrot

- apple
  - banana
    - carrot

<br/>

### 4) 코드블럭 (Fenced Code Blocks)
#### 문법
 - 줄 앞에 4개의 공백(Space) 또는 1개의 탭(Tab)이 있는 텍스트를 코드블럭으로 변환
 - 들여쓰기 없이 \`\`\` 으로 감싸는 방법도 있다.
 - \`\`\`Ruby 와 같이 특정 언어의 문법을 강조할 수 있다. (Syntax highlighting)
 - 코드블럭 바로 앞에는 빈 줄이 있어야 한다.
 - \` 으로 감싸면 인라인 블럭을 만든다.

      ```
      This is a code block.
      ```

          This is a code block.

      ```Javascript
          var x = document.getElementsByTagName("li");
              // <li> 태그가 1개 이더라도 자동 배열 처리
          x[0].style.color = "red";
      ```

#### 사용예시

```
This is a code block.
```

    This is a code block.

```Javascript
    var x = document.getElementsByTagName("li");
        // <li> 태그가 1개 이더라도 자동 배열 처리
    x[0].style.color = "red";
```

#### 문법

    `---`

#### 사용예시

`---`

<br/>

### 4) 수평선
#### 문법

    * * *

    ***

    *****

    - - -

    ---------------------------------------

    <hr/>

#### 사용예시
* * *

***

*****

- - -

---------------------------------------

<hr/>

<br/>

### 5) 링크 (Links)
#### 문법
  * 참조링크

        [Google][googlelink]
        googlelink]: https://google.com "Go google"

  * 인라인 링크

        Link: [Google](https://google.com)

  * 자동연결

        <http://example.com/>
        <estele.choi@gmail.com>

#### 사용예시
  * 참조링크

     [Google][googlelink]
     [googlelink]: https://google.com "Go google"

  * 인라인 링크

    Link: [Google](https://google.com)

  * 자동연결

    <http://example.com/>
    <estele.choi@gmail.com>

 #### 6) 강조
 #### 문법

     *single asterisks*
    _single underscores_
    **double asterisks**
    __double underscores__
    ++underline++
    ~~cancelline~~

#### 사용예시

*single asterisks* <br/>
_single underscores_ <br/>
**double asterisks** <br/>
__double underscores__ <br/>
++underline++ <br/>
~~cancelline~~

<br/>

### 6) 이미지 (Image)
#### 문법

    ![Alt text](/path/to/img.jpg)
    ![Alt text](/path/to/img.jpg "Optional title")

<br/>

### 7) 이모티콘 자동완성 (Emoji autocomplete)
#### 문법

    :를 타이핑하면 이모티콘 추천 목록을 불러온다. :+1:

#### 사용예시

:+1:

#### 문법

    &#128525; &#128513; &#128519; &#10047;

#### 사용예시

&#128525; &#128513; &#128519; &#10047;

 * [더 많은 이모지를 확인하려면 다음 링크를 클릭](https://steemit.com/steemkr-guide/@snow-airline/steemkr-quick-start-guide)

<br/>

### 8) 체크박스 (Checkbox / Task Lists)
#### 문법

    - [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
    - [x] list syntax required (any unordered or ordered list supported)
    - [x] this is a complete item
    - [ ] this is an incomplete item

#### 사용예시

- [x] @mentions, #refs, [links](), **formatting**, and <del>tags</del> supported
- [x] list syntax required (any unordered or ordered list supported)
- [x] this is a complete item
- [ ] this is an incomplete item

<br/>

### 9) 띄어쓰기와 줄바꿈 (Space and Enter)
#### 문법
 - 단락 바꿈 : 엔터 2 번
 - 강제로 여러 줄을 맘대로 띄고 싶다면 \<br\/\> 사용

```
    띄어쓰기(space)       여러 번 = 한 번
    엔터 한 번은 글이 붙는다.

    엔터 두 번은 단락을 바꾼다.


    엔터 여러 번은 여러 줄을  띄우지 않는다.
```

#### 사용예시

띄어쓰기(space)       여러 번 = 한 번
엔터 한 번은 글이 붙는다.

엔터 두 번은 단락을 바꾼다.


엔터 여러 번은 여러 줄을  띄우지 않는다.

<br/>

### 10) 표 (Table)
#### 문법
 - 열 구분 \|
 - 행 구분 \| \-\-\- \|

```
      1 | 2 | 3
      --- | --- | ---
      일반 텍스트 | &#128525; | `---`
```

#### 사용예시

1 | 2 | 3
--- | --- | ---
일반 텍스트 | &#128525; | `---`

<br/>

### 11) 마크다운 포맷 무시하기
#### 문법
 - 역슬래시(Backslash) \\ 사용

```
    \*\*
```

#### 사용예시

\*\*

<br/>

***
###     * 참고문서

  * [마크다운 markdown 작성법](https://gist.github.com/ihoneymon/652be052a0727ad59601)
  * [존 그루버 마크다운 페이지 번역](https://nolboo.kim/blog/2013/09/07/john-gruber-markdown/)
  * [깃허브 취향의 마크다운 번역](https://nolboo.kim/blog/2014/03/25/github-flavored-markdown/)
  * [마크다운 총정리 All in One](https://steemit.com/kr/@nand/markdown)
  * [Quick Start Guide - 👍 😎 👀 💃 스팀잇 이모지 마스터 리스트 🚀 😝 🌍 🍒](https://steemit.com/steemkr-guide/@snow-airline/steemkr-quick-start-guide)
