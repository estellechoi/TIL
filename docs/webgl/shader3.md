# Shader 입히기 3: 벡터 타입의 유연성, 색 섞을 때 유용한 mix 함수, 원형 패턴을 위한 distance, length, sqrt, dot

<br>

1. 벡터 타입의 유연성
2. 색 섞을 때 유용한 mix 함수
3. 원형 패턴을 위한 distance, length, sqrt, dot

<br>

## 1. 벡터 타입의 유연성

GLSL에서는 벡터 타입의 값이 상당히 많이 사용됩니다. 특히 Fragment Shader의 `main` 함수에서 `gl_FragColor` 변수에는 꼭 `vec4` 타입의 색상값을 넣어줘야 하지요. 벡터 타입은 픽셀의 좌표값을 나타낼 때도 유용하게 사용할 수 있습니다. 이러한 이유로 벡터 객체는 색의 요소인 `r`, `g`, `b`, `a` 속성과 좌표를 나타내는 `x`, `y`, `z`, `w` 속성들을 모두 지원합니다. 단 속성의 이름만 다를 뿐, `r` 값은 `x`와 같고, `g`는 `y`, `b`는 `z`, `a`는 `w`와 같은 값을 반환합니다. 이 외에도 `s`, `t`, `p`, `q` 속성들과 index 탐색도 지원합니다.

```glsl
vec3 color = vec3(1.0, 0.0, 0.0);
vec3 coordinates = vec3(1.0, 0.0, 0.0);

bool isSameValue1 = color[0] == color.r; // true
bool isSameValue2 = color.g == coordinates.y; // true
```

<br>

위와 같은 특성때문에 GLSL에서는 각 픽셀의 좌표 벡터를 활용하여 해당 픽셀에 렌더링할 색의 값을 추출하는 것이 일반적입니다. 따라서 각 픽셀의 좌표값을 담고있는 벡터 변수 `gl_FragCoord`를 활용하는 GLSL 코드를 많이 볼 수 있지요.

```glsl
uniform vec2 u_resolution;

void main() {
    vec2 st = gl_FragCoord.xy / u_resolution.xy;
    gl_FragColor = vec4(st.x, st.y, 0.0, 1.0);
}
```

<br>

## 2. 색 섞을 때 유용한 mix 함수

[`mix`](https://thebookofshaders.com/glossary/?search=mix) 함수는 두 개의 벡터값을 섞은 새로운 벡터 값을 반환하는 함수입니다. 다음 예제와 같이 사용할 수 있는데요, 두 개의 색을 나타내는 벡터 값과 두 번째 색을 섞을 비율 값을 `0.0` - `1.0` 사이의 값으로 넣어주면 됩니다. 만약 `percentage` 값이 `0.0` 이면 `colorA`를 그대로 반환하고, `1.0`이면 `colorB` 값을 반환할겁니다. 

```glsl
vec3 colorA = vec3(0.149,0.141,0.912);
vec3 colorB = vec3(1.000,0.833,0.224);

void main() {
    float percentage = 0.5;
    vec3 mixedColor = mix(colorA, colorB, percentage);

    gl_FragColor = vec4(mixedColor, 1.0);
}
```

<br>

`uniform` 변수를 사용해서 시간에 따라 `percentage` 값을 바꿔준다면 색이 변하는 애니메이션도 구현할 수도 있겠습니다. `u_time` 변수에는 연속되는 시간값이 할당될 것이므로, `sin(u_time)`은 `-1.0` 에서 `1.0` 사이의 연속되는 값을 반환합니다.

```glsl
uniform float u_time; // live time

vec3 colorA = vec3(0.149,0.141,0.912);
vec3 colorB = vec3(1.000,0.833,0.224);

void main() {
    float percentage = abs(sin(u_time)); // returns 0.0 - 1.0
    vec3 mixedColor = mix(colorA, colorB, percentage);

    gl_FragColor = vec4(mixedColor, 1.0);
}
```

<br>

## 3. 원형 패턴을 위한 distance, length, sqrt, dot

### 3-1. distance 

[`distance`](https://thebookofshaders.com/glossary/?search=distance)와 같은 함수는 Canvas의 특정 지점을 중앙으로하는 원형 패턴을 그릴 때 유용합니다. 가령 GLSL에서 Canvas 중앙의 좌표는 `vec2(0.5)`로 표기할 수 있으므로, 다음과 같이 GLSL을 작성하면 중앙에서 바깥으로 퍼지는 원형의 그라데이션을 구현할 수 있습니다.

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main(){
	vec2 st = gl_FragCoord.xy / u_resolution;

    float brightness = distance(st, vec2(0.5));

	gl_FragColor = vec4(brightness, 1.0);
}
```

<br>

<img src="/docs/img/radial.jpg" />

<br>

### 3-2. length 

다음과 같이 [`length`](https://thebookofshaders.com/glossary/?search=length) 함수를 사용해도 동일한 결과를 얻습니다.

```glsl
vec2 toCenter = vec2(0.5) - st;
float brightness = length(toCenter);
```

<br>

### 3-3. sqrt

[삼각함수와 원의 관계](https://en.wikipedia.org/wiki/Hypotenuse)를 활용하면 [`sqrt`](https://thebookofshaders.com/glossary/?search=sqrt) 함수를 사용해서 동일한 결과물을 만들 수 있습니다.


```glsl
vec2 toCenter = vec2(0.5) - st;
float brightness = sqrt(toCenter.x * toCenter.x + toCenter.y * toCenter.y);
```

<br>

### 3-4. dot

`sqrt` 함수의 연산은 컴퓨팅 파워를 많이 쓰기 때문에, [`dot`](https://thebookofshaders.com/glossary/?search=dot) 함수로 대체할 수도 있습니다. 두 벡터의 [스칼라곱](https://ko.wikipedia.org/wiki/%EC%8A%A4%EC%B9%BC%EB%9D%BC%EA%B3%B1)을 반환하는 함수입니다.

```glsl
float circle(in vec2 st, in float size){
    // 중앙까지의 좌표 차
    vec2 toCenter = st - vec2(0.5);

    float thresholdStart = size - ( size * 0.01);
    float thresholdEnd = size + (size * 0.01);

	return smoothstep(thresholdStart, thresholdEnd, dot(toCenter, toCenter) * 4.0);
}

void main() {
	vec2 st = gl_FragCoord.xy / u_resolution.xy;

	vec3 color = vec3(circle(st, 0.9));

	gl_FragColor = vec4(color, 1.0);
}
```

<br>
<br>

---

### References

- [Colors | The Book of Shaders](https://thebookofshaders.com/06/)
- [Shapes | The Book of Shaders](https://thebookofshaders.com/07/)
