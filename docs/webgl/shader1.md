# Shader 1: GLSL과 OpenGL이란, GLSL 문법, Uniform, 내장 함수, gl_FragCoord, Three.js에서 사용하기

<br>

1. GLSL과 OpenGL이란
2. GLSL 문법, Uniform, 내장 함수, gl_FragCoord
3. Three.js에서 사용하기

<br>

## 1. GLSL과 OpenGL이란

[OpenGL](https://openglbook.com/chapter-0-preface-what-is-opengl.html)은 컴퓨터의 그래픽 하드웨어(GPU)에 명령을 보내는 소프트웨어에 필요한 기능들에 대한 API 표준인데요, GPU에서 처리했을 때 성능이 극대화되는 2D와 3D 그래픽을 표현하기 위해 사용합니다. 가령 [CAD(Computer Aided Design)](https://en.wikipedia.org/wiki/Computer-aided_design)와 같은 프로그램이 OpenGL 표준을 사용하여 개발된 것으로 알려져있고요, 웹 브라우저에서 2D와 3D 그래픽을 구현할 때 사용하는 JavaScript API인 [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API) 역시 OpenGL 표준을 충족하도록 개발되었습니다. 

[GLSL](https://en.wikipedia.org/wiki/OpenGL_Shading_Language)은 OpenGL Shading Language의 약자로, 2D/3D 그래픽에서 Shader를 구현하기 위한 프로그래밍 언어입니다. 웹 개발에서는 기본적으로 HTML 마크업과 CSS를 사용해서 페이지를 웹 브라우저에서 어떻게 렌더링시킬지 작성하죠. 비디오나 사진을 추가할 수도 있고, WebGL을 사용해서 2D/3D 그래픽을 구현할 수도 있습니다. Shader는 여기에서 나아가, GPU로 직접 명령을 보내서 각 픽셀이 최종적으로 어떻게 렌더링될지 통제할 수 있는 기능입니다. 완성된 화면에 추가적으로 필터를 씌우는 것과 비슷한 개념입니다.

<br>

## 2. GLSL 입문: 문법, Uniform, 내장 함수

### 2-1. GLSL 문법

GLSL은 C언어와 유사한 문법을 사용합니다. 다음은 아주 간단한 GLSL 코드 예제를 통해 몇가지 규칙을 알아볼겁니다.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform float u_time;

void main() {
	gl_FragColor = vec4(1.0,0.0,1.0,1.0);
}
```

1. 하나의 `main` 함수를 갖는다.
2. 내장된 글로벌 변수와 함수가 있다. 예) `gl_FragColor`, `vec4()`
3. 전처리 매크로가 있다. (`#ifdef`) 위에서는 `GL_ES`가 정의되어 있으면 `precision mediump float;`을 수행하라는 매크로가 작성되어 있습니다.
4. 값의 precision을 고려하는 것이 필요한데, precision이 높으면 퀄리티가 좋아지지만 렌더링 속도는 느려지고 precision이 낮으면 그 반대입니다.
    - `precision mediump float;`
    - `precision lowp float;`
    - `precision highp float;`
5. GPU의 모든 쓰레드에 동일하게 전달할 데이터를 Uniform이라하고, `uniform float u_time;`과 같이 정의한다.

<br>

### 2-2. GLSL Uniform

Uniform 데이터는 CPU에서 GPU의 각 쓰레드에 동일하게(uniform) 전달하는 데이터입니다. Uniform 데이터로 사용 가능한 타입들은 다음과 같습니다.

- `float`
- `vec2`/`vec3`/`vec4`
- `mat2`/`mat3`/`mat4`
- `sampler2D`
- `samplerCube`

<br>

이렇게 Uniform을 작성하면 GPU의 모든 쓰레드가 해당 정보를 갖게 되므로, `main` 함수에서 사용할 수 있습니다.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;  // Canvas size (width,height)
uniform vec2 u_mouse;       // mouse position in screen pixels
uniform float u_time;       // Time in seconds since load
```

<br>

다음과 같이 `u_time` 데이터를 사용할 수 있습니다.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform float u_time;

void main() {
	gl_FragColor = vec4(abs(sin(u_time)),0.0,0.0,1.0);
}
```

<br>

### 2-3. GLSL 내장 함수

위 예제를 통해 내장된 몇가지 함수를 알아보겠습니다. GLSL의 내장 함수들은 주로 그래픽에서 많이 사용되는 삼각함수, 지수함수들인데요, 이 함수들은 CPU가 아닌 GPU에서 직접 연산할 수 있는 함수들입니다.

- [`sin`](https://thebookofshaders.com/glossary/?search=sin)/`cos`/`tan`/`asin`/`acos`/`atan`
- `pow`/`exp`/`log`/`sqrt`
- [`abs`](https://thebookofshaders.com/glossary/?search=abs)/`sign`
- `floor`/`ceil`
- `fract`/`mod`
- `min`/`max`
- `clamp`

<br>

### 2-4. gl_FragCoord

이번에는 `vec4` 타입의 글로벌 변수인 `gl_FragCoord`를 알아볼겁니다. 위에서 봤던 `gl_FragColor`와 같은 GLSL의 내장 변수입니다. `gl_FragCoord`는 GPU의 각 쓰레드가 렌더링 작업을 하고 있는 픽셀의 좌표 정보를 담고있는 변수입니다. 그래서 아래와 같이 코드를 작성하면 GPU의 각 쓰레드에서 실행되는 `main` 함수의 작업 내용은 서로 다르겠지요. `main` 함수 내에서 `st` 변수에 할당되는 값이 다르기 때문에, `gl_FragColor` 에 할당되는 값도 다르고, 따라서 각 쓰레드가 담당하는 픽셀에 렌더링되는 색상이 달라지기 때문입니다! 이런식으로 한 화면의 각 픽셀의 렌더링을 컨트롤 할 수 있습니다.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main() {
	vec2 st = gl_FragCoord.xy / u_resolution; // 전체 Canvas 사이즈로 나누어서 0.0 - 1.0 사의 값으로 변환! (색을 나타내는 vec4 값은 0.0 - 1.0 사의의 값이기 때문)
	gl_FragColor = vec4(st.x,st.y,0.0,1.0);
}
```

<br>

GLSL 코드를 작성하고 콘솔을 사용해서 바로 렌더링해볼 수 있는 프로그램으로 [editor.thebookofshaders.com](http://editor.thebookofshaders.com/)이 있고요, 설치해서 사용할 수 있는 프로그램으로는 [glslViewer](https://github.com/patriciogonzalezvivo/glslViewer)가 있습니다. JavaScript 코드에서 간단히 사용해볼 목적으로는 [glslCanvas](https://github.com/patriciogonzalezvivo/glslCanvas)라는 라이브러리가 있습니다.

<br>

## 3. Three.js에서 사용하기

### 3-1. Shader 작성하기

[Three.js](https://threejs.org)에서는 `THREE.ShaderMaterial` API를 사용해서 `ShaderMaterial` 인스턴스를 만들면 되는데요, 파라미터의 `vertexShader`, `fragmentShader` 필드에 GLSL 문법으로 작성된 Shader 문자열을 넣어줍니다. 이 둘의 차이는 후에 별도로 다루겠습니다. `uniforms` 필드에는 문자열로 넣어준 Shader 코드에서 사용될 `uniform` 변수들의 초기값을 넣어주면 됩니다.

```typescript
export const INITIAL_UNIFORMS: ShaderMaterialProps['uniforms'] = {
  u_time: { value: 1.0 },
  u_resolution: { value: new Vector2() },
  u_mouse: { value: new Vector2() },
};

export const VERTEX_SHADER = `
    void main() {
        gl_Position = vec4(position, 1.0);
    }
`;

export const FRAGMENT_SHADER = `
    uniform vec2 u_resolution;
    uniform float u_time;

    void main() {
        vec2 st = gl_FragCoord.xy/u_resolution.xy;
        gl_FragColor=vec4(st.x, st.y, 0.0, 1.0);
    }
`;

export const shaderMaterial = new THREE.ShaderMaterial({
    uniforms: UNIFORMS,
    vertextShader: VERTEX_SHADER,
    fragmentShader: FRAGMENT_SHADER,
})
```

<br>

### 3-2. Shader 사용하기

위에서 생성한 `shaderMaterial`을 `Mesh` 객체 생성시 파라미터로 넣어줍니다. 

```typescript
export const geometry = new THREE.PlaneBufferGeometry(2, 2);
export const mesh = new THREE.Mesh(geometry, shaderMaterial);


let camera = new THREE.Camera();
camera.position.z = 1;

const scene = new THREE.Scene();
scene.add(mesh);

const renderer = new THREE.WebGLRenderer();
renderer.setPixelRatio(window.devicePixelRatio);

container.appendChild(renderer.domElement); // container 엘리먼트가 있다고 가정
```

<br>

### 3-3. Shader Uniform 변경하기

다음과 같이 사용자 이벤트에 따라 Uniform이 업데이트 되도록 작성하면, GPU에 바로 바로 전달해주기 때문에 인터렉티브한 Shader 애니메이션을 구현할 수 있습니다.

```typescript
const uniforms: ShaderMaterialProps['uniforms'] = {
    u_time: { value: 1.0 },
    u_resolution: { value: new THREE.Vector2() },
    u_mouse: { value: new THREE.Vector2() }
};

const shaderMaterial = new THREE.ShaderMaterial({
    uniforms,
    vertextShader: VERTECT_SHADER,
    fragmentShader: FRAGMENT_SHADER,
})

const onMouseMove: MouseEventHandler = (event) => {
    uniforms.u_mouse.value.x = event.clientX;
    uniforms.u_mouse.value.y = event.clientY;
}

document.onmousemove = onMouseMove;
```

<br>

### 3-4. 다른 방법으로 GLSL 작성하기

참고로 GLSL 파일은 원래 `.vert`, `.frag` 등의 확장자를 사용하는데요, HTML 문서에서는 다음과 같이 script 태그 안에 작성할 수도 있습니다.

```html
<script id="vertexShader" type="x-shader/x-vertex">
    void main() {
        gl_Position = vec4( position, 1.0 );
    }
</script>

<script id="fragmentShader" type="x-shader/x-fragment">
    uniform vec2 u_resolution;
    uniform float u_time;

    void main() {
        vec2 st = gl_FragCoord.xy/u_resolution.xy;
        gl_FragColor=vec4(st.x,st.y,0.0,1.0);
    }
</script>
```

<br>

---

### References

- [OpenGLBook.com](https://openglbook.com/chapter-0-preface-what-is-opengl.html)
- [What is a fragment shader? | The Book of Shaders](https://thebookofshaders.com/01/)
- [Uniforms | The Book of Shaders](https://thebookofshaders.com/03/)
- [Running your shader | The Book of Shaders](https://thebookofshaders.com/04/)
