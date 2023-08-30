# Shader 입히기 4: 회전변환행렬(Rotation Matrix), 축척행렬(Scale Matrix)

<br>

1. 회전변환행렬(Rotation Matrix)
2. 축척행렬(Scale Matrix)

<br>

## 1. 회전변환행렬(Rotation Matrix)

### 1-1. 회전

[선형 대수학](https://ko.wikipedia.org/wiki/%EC%84%A0%ED%98%95%EB%8C%80%EC%88%98%ED%95%99) 분야에서 잘 알려진 행렬(Matrix) 중에 [회전변환행렬(Rotation Matrix)](https://ko.wikipedia.org/wiki/%ED%9A%8C%EC%A0%84%EB%B3%80%ED%99%98%ED%96%89%EB%A0%AC)이 있습니다.

<img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/d4a02e6b5990244f4427309f6732239b9633d62d" width="157" />

<br>

이 행렬은 GLSL에서 물체를 회전시킬 때 아주 유용하게 사용되는데요, 위의 행렬과 도형을 구성하는 각 좌표값 `(𝙭, 𝙮)`의 곱으로 도형이 제자리에서 회전하는 애니메이션을 구현할 수 있습니다. 시간의 흐름에 따라 `𝜭`에 연속된 값을 넣어주면 물체가 반시계 방향으로 제자리에서 뱅글 뱅글 회전하는 결과를 얻을 수 있지요.

<img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/e02da33f45679713d15de997449a76df48efb282" width="361" />

<br>

이 회전변환행렬을 이해하려면 가장 먼저, `𝜭`에 연속된 값을 차례로 넣어주었을 때 결과값 역시 연속된 값들임을 상기할 필요가 있습니다. 어떤 원의 중앙 좌표가 `(0, 0)`이고, 반지름이 `𝙧`이라고 해봅시다. 이 원의 둘레를 이루는 점들의 좌표는 `(𝙘𝙤𝙨𝜭, 𝙨𝙞𝙣𝜭)` 이고, 그 값들은 `-𝙧` 과 `𝙧` 사이의 연속된 수로 이루어져 있지요. [Shader 입히기 2](./shader2.md)에서 다루었던 내용입니다.

<img src="https://thebookofshaders.com/05/sincos.gif" width="500" />

<br>

자 그럼 회전변환행렬의 각 요소인 `𝙘𝙤𝙨𝜭`, `𝙨𝙞𝙣𝜭`, `-𝙨𝙞𝙣𝜭`의 값들도 모두 `-𝙧` 과 `𝙧` 사이의 연속된 수로 이루어진다는 것을 추론할 수 있습니다. `(𝙘𝙤𝙨𝜭, 𝙨𝙞𝙣𝜭)`는 원의 둘레를 따라 회전하는 벡터 값이지만, 약간의 수학이 더해진 회전변환행렬은 어떤 모양의 도형도 회전시킬 수 있습니다.

<br>

### 1-2. 회전변환행렬 함수 구현하기

GLSL에서는 다음과 같이 회전변환행렬을 함수로 구현할 수 있습니다.

```glsl
mat2 rotate2D(float angle) {
    return mat2(cos(angle), -sin(angle), sin(angle), cos(angle));
}
```

<br>

그리고 이 함수는 아래와 같이 사용할 수 있습니다. `rotate2D` 함수가 반환하는 행렬에 각 쓰레드의 좌표값을 곱하기 전에 `-vec2(0.5)` 만큼 이동시키는 코드를 확인할 수 있는데요, 이는 행렬 곱을 할 때 회전시키려는 도형의 중앙점이 `(0, 0)`이어야 제자리에서 회전하는 애니메이션을 구현할 수 있기 때문입니다.

```glsl
uniform vec2 u_resolution;
uniform float u_time;

void main() {
    vec2 st = gl_FragCoord.xy / u_resolution.xy;

    // 좌표값을 -0.5씩 이동시켜서 전체 벡터의 중앙값을 (0, 0)으로 변환
    st -= vec2(0.5);

    st = rotate2D(u_time) * st;

    // -0.5씩 이동시킨 모든 좌표값을 원상복귀 시킴
    st += vec2(0.5);

    // box는 특정한 모양을 띄도록 각 쓰레드의 컬러값을 반환해주는 함수라고 가정
    vec3 shapedColor = vec3(box(st));

    gl_FragColor = vec4(shapedColor, 1.0);
}
```

<br>

## 2. 축척행렬(Scale Matrix)

### 2-1. 축척

행렬과 좌표의 곱을 이해하면, 특정 도형을 축소/확대할 수 있는 축척도 구현할 수 있습니다.

<img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/f8b981072c2d00c1ce373cd483c00fd6d927f668" width="294" />

<br>

### 2-2. 축척 함수 구현하기

다음과 같이 함수를 작성해서 구현할 수 있고요.

```glsl
mat2 scale2D(vec2 multiplier){
    return mat2(multiplier.x, 0.0, 0.0, multiplier.y);
}
```

<br>

도형의 좌우상하 좌표 움직임을 동일하게 하기 위해서 역시나 행렬 곱을 하기 전 모든 벡터를 `-vec2(0.5)` 만큼 이동시키고, 행렬 곱이 완료된 후에 다시 `vec2(0.5)`씩 이동시키는 교정을 합니다. `multiplier`의 값은 무한히 증가하는 수 대신 `sin(u_time)`과 같이 삼각함수를 사용하여 특정 구간을 반복하는 수를 넣어주어 확대 축소를 반복하는 애니메이션을 구현했습니다.

```glsl
uniform vec2 u_resolution;
uniform float u_time;

void main() {
    vec2 st = gl_FragCoord.xy/u_resolution.xy;

    st -= vec2(0.5);

    vec2 multiplier = vec2(sin(u_time));
    st = scale(multiplier) * st;

    st += vec2(0.5);
}
```

<br>

---

### References

- [Rotation matrix | Wikipedia](https://en.wikipedia.org/wiki/Rotation_matrix)
- [Scaling (geometry) | Wikipedia](https://en.wikipedia.org/wiki/Scaling_(geometry))
- [Rotation Matrix | The Book of Shaders](https://thebookofshaders.com/08/)
