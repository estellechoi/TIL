# Shader 입히기 2: 그라데이션, pow, 선, smoothstep

<br>

1. 그라데이션, pow
2. 선, smoothstep
3. Three.js에서 사용하기

<br>

## 1. 그라데이션

[The Book of Shaders](https://thebookofshaders.com/05/)에 소개된 예제를 통해 GLSL에 더 익숙해져 보겠습니다.

<img src="/docs/img/shader_book.png" />

<br>

### 1-1. 그라데이션 원리

위 예제에서 그라데이션을 그리는 부분만 남겼고요, 각 코드 라인에 주석으로 해설을 남겼습니다. GLSL은 각 GPU 쓰레드에 동일하게 전달되고 동시에 처리된다는 것을 상기하면서 보시면 좋을 것 같습니다. 물론 `gl_FragCoord` 변수는 각 쓰레드의 정보를 담고 있으므로 서로 다른 데이터를 갖고 있겠지요.

```c
#ifdef GL_ES
// 중간 정도의 precision을 사용하겠다고 명령
precision mediump float;
#endif

// Canvas 사이즈
uniform vec2 u_resolution; 

void main() {
    // st = 0.0 - 1.0 사의의 값으로 변환된 각 픽셀의 x, y 좌표값
    // 각 픽셀의 좌표값을 사용해서 픽셀마다 다른 색상을 입혀줄 것이기 때문에  0.0 - 1.0 사이의 값으로 변환
    // 색상 vec3/vec4 에서 R, G, B의 밝기를 나타내는 0.0 - 1.0 사이의 값을 사용하며 하나의 색을 만들기 때문
	vec2 st = gl_FragCoord.xy / u_resolution;

    // 각 픽셀의 x 좌표값에만 비례하는 색상값 추출
    float colorValue = st.x;

    // vec3(y) => vec3(y, y, y)
    // RGB 값의 밝기 값이 동일하면 무채색이 나옵니다
    vec3 color = vec3(colorValue);

    // vec4(color, 1.0) => vec4(color, color, color, 1.0)
	gl_FragColor = vec4(color, 1.0);
}
```

<br>

위 코드의 결과물은 다음과 같이 좌우로 Linear한 그라데이션이 됩니다.

<img src="/docs/img/shader_gradient.jpg">

<br>

### 1-2. pow

픽셀의 x 좌표값을 다른 방식으로 가공해서 조금 다른 그라이데션도 만들어볼 수 있겠습니다.

```c
// x 좌표값의 1 제곱값인 색상 추출
float colorValue = pow(st.x, 1.0);

// x 좌표값의 2 제곱값인 색상 추출
float colorValue = pow(st.x, 2.0);

// x 좌표값의 3 제곱값인 색상 추출
float colorValue = pow(st.x, 3.0);
```

<br>

<img src="/docs/img/shader_gradient_varient.jpg">

<br>

## 2. 선, smoothstep

위의 예제를 통해 GLSL의 `vec2`, `vec3`, `vec4` 타입과 `gl_FragCoord` 변수를 조금 더 이해할 수 있게 되셨기를 바랍니다. 이제 위 결과물에 선을 그리는 코드를 살펴보면서 새로운 내장 함수인 `smoothstep`을 알아보겠습니다.

```c
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution; 

// 0.0 - 1.0 사이의 값을 사용하여 Y축에 선을 그리는 함수
// 이 함수는 별도로 아래에서 다룰 것이므로 여기에서는 무시합니다!
float plot(vec2 st) {    
    return smoothstep(0.02, 0.0, abs(st.y - st.x));
}

void main() {
	vec2 st = gl_FragCoord.xy / u_resolution;

    float colorValue = st.x;

    vec3 color = vec3(colorValue);

    // 선 그리기
    float pct = plot(st);
    color = (1.0 - pct) * color + pct * vec3(0.0, 1.0, 0.0);

	gl_FragColor = vec4(color, 1.0);
}
```

<br>

<br>
<br>
<br>


---

### References

- [Algorithmic drawing | The Book of Shaders](https://thebookofshaders.com/05/)
