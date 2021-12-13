
# Lighthouse 퍼포먼스 점수 올리기 4: 압축, SSR

<br>

1. Webpack으로 CSS Minify 하기
2. Brotli, Gzip으로 텍스트 압축하기
3. SSR

<br>

## 1. Webpack으로 CSS Minify 하기

[Webpack](https://webpack.js.org/)과 같은 번들러를 사용한다면 [CSS Minify](https://web.dev/minify-css/)도 간단하게 할 수 있습니다.

<br>

## 2. Brotli, Gzip으로 텍스트 압축하기

네트워크 통신을 통해 전달되는 리소스의 적정 사이즈는 통상 `1,700` ~ `1,900` KiB 수준입니다. `1,600` KiB를 넘지 않도록 하는 것이 좋다는 것이 중론이고요. 그래야 3G 네트워크 환경에서도 사용자가 10초 이내에 콘텐츠를 볼 수 있기 때문입니다.

<br>



- [Gzip 아시나요? 그러면 Brotli는요?](https://snyung.com/content/2021-02-11--Brotli) 블로그 글이 도움이 되었습니다.

<br>

## 3. SSR

SSR, 서버 사이드 렌더링은 그 이름대로 서버에서 HTML 문서를 렌더링하여 클라이언트에 응답하는 방식입니다. SSR은 기본적으로 FCP를 빠르게 하는데요, 브라우저에 렌더링이 완료된 페이지를 응답하기 때문에 브라우저 입장에서는 사용하지도 않는 모든 CSS, JavaScript를 로드하고 탐색할 필요성이 완전히 제거되기 때문이죠.

<br>

[Rendering on the Web](https://developers.google.com/web/updates/2019/02/rendering-on-the-web) 문서에서 더 자세한 내용을 확인할 수 있고요, 다음은 일부 설명을 발췌한 것입니다.

> Server rendering generally produces a fast First Paint (FP) and First Contentful Paint (FCP). Running page logic and rendering on the server makes it possible to avoid sending lots of JavaScript to the client, which helps achieve a fast Time to Interactive (TTI). This makes sense, since with server rendering you’re really just sending text and links to the user’s browser.

<br>

---

### References
