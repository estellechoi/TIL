# 웹 애플리케이션에서 파일 사용하기

<br>

## `<input type="file">`

웹 애플리케이션에서 사용자가 파일을 업로드할 수 있는 기능을 제공할 때 `file` 타입의 `<input>` 요소가 사용됩니다. 해당 요소를 통해 1 개 이상의 파일들을 선택할 수 있습니다. 선택된 파일들은 폼 제출(`form submission`)을 통해 서버로 전송할 수 있고요, `File` API를 사용하면 자바스크립트 코드에서 접근할 수도 있습니다.

<br>

다음은 MDN 문서에서 가져온 예시 마크업입니다.

```html
<label for="avatar">Choose a profile picture:</label>

<input type="file" id="avatar" name="avatar" accept="image/png, image/jpeg" />
```

<br>

### `value` 속성

`<input type="file">` 요소의 `value` 속성은 선택된 파일의 경로를 나타내는 `DOMString`을 값으로 합니다. 2 개 이상의 파일을 선택한 경우에도 첫 번째 파일의 경로에만 접근할 수 있다는 점에 주의하세요. 자세한 내용은 아래의 내용들을 확인하세요.

- 다른 파일들의 경로에도 접근하려면 JS 코드에서 `HTMLInputElement.files` 속성을 사용하세요.

- 아무 파일도 선택하지 않았다면 값은 빈 문자열(`""`) 입니다.

- `value` 속성 값의 앞부분에는 [`C:\fakepath\`라는 가상 경로가 포함됩니다](https://html.spec.whatwg.org/multipage/input.html#fakepath-srsly). 이는 보안을 위해 사용자의 파일 구조를 숨기기 위함입니다.

<br>

### `file` 타입의 `<input>`에만 있는 속성들

- `accept` : 1 개 이상의 선택 가능한 파일 타입을 지정합니다.

- `capture` : 이미지/영상 데이터를 캡쳐하기 위해 어떤 카메라를 사용할지 지정합니다.

- `files` : 선택된 파일들에 접근할 수 있는 [`FileList`](https://developer.mozilla.org/en-US/docs/Web/API/FileList)에 접근할 수 있습니다.

- `multiple` : 사용자가 1 개 이상의 파일을 선택하도록 허용할지 여부를 지정합니다. (Boolean)

<br>

위 속성들을 하나씩 자세히 살펴보죠.

<br>

## `accept`

선택 가능한 파일 타입을 지정하는 속성입니다. 여러 개의 타입을 지정할 때는 콤마(`,`)로 구분합니다. 특정 타입의 파일을 허용하도록 지정할 때는 관련 확장자를 모두 지정해주는 것을 권장합니다. 예를 들어 MS Word 파일을 허용하려면, `accept` 속성 값을 아래와 같이 작성하는 것이 좋습니다.

```html
<input
	type="file"
	id="docpicker"
	accept=".doc,.docx,application/msword,application/vnd.openxmlformats-officedocument.wordprocessingml.document"
/>
```

<br>

다음은 파일 타입을 지정할 때 따라야 할 양식입니다.

- 점(`.`)으로 시작하며 대소문자를 구분하지 않는 확장자 이름

  > 예) `.jpg`, `.pdf`, `.doc` 등

- [MIME 타입](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

- `audio/*` : 모든 오디오 파일

- `video/*` : 모든 영상 파일

- `image/*` : 모든 이미지 파일, 이 타입을 지정하면 대부분의 모바일 기기에서 카메라로 사진을 직접 찍을 수 있습니다.

<br>

> 주의 : `accept` 속성은 사용자가 선택한 파일의 타입을 검증하지는 않습니다. 브라우저에 선택 가능한 파일들을 알려주어 파일탐색기에서 사용자들이 해당 타입의 파일들을 선택할 수 있도록 유도할 뿐이죠. 가령, 파일탐색기의 Options 메뉴를 통해 선택 가능한 파일의 타입을 사용자가 임의로 변경하는 것이 가능합니다. 따라서 파일의 타입을 제한할 때는 `accept` 속성에 완전히 의존해서는 안되며, `Array.includes()` 메소드를 사용하여 검증하거나 서버에서 체크하는 과정이 반드시 필요합니다.

<br>

## `capture`

`accept` 속성에 이미지 혹은 영상 파일을 허용하도록 지정했다면, 해당 이미지/영상 데이터를 캡쳐할 때 사용할 카메라를 지정하는 `capture` 속성을 사용할 수 있습니다. `capture` 속성의 값으로 `user`를 지정하면 디바이스의 전면 카메라와 마이크가 사용됩니다. `environment`를 값으로 지정하면 후면 카메라와 마이크가 사용되고요. 이 속성 값을 지정하지 않으면, 브라우저는 `user`/`environment` 중 하나의 값을 임의로 선택하여 지정합니다. 한편, 지정한 방법이 사용 불가능한 상태일 수도 있겠죠? 이러한 경우에도 브라우저에 의해 임의로 재선택됩니다.

<br>

## `files`

이 속성을 통해 [`FileList`](https://developer.mozilla.org/en-US/docs/Web/API/FileList) 객체에 접근할 수 있습니다. 이 객체는 선택된 모든 파일들의 리스트입니다.

<br>

### `FileList` 객체를 사용하여 파일에 대한 정보 얻기

JS 코드에서 `HTMLInputElement.files` 속성은 `FileList` 객체를 반환합니다. 이 객체는 선택된 파일들로 구성된 배열과 같고요, 따라서 `length` 속성으로 선택된 파일들의 개수를 얻을 수 있습니다. 이 외에 다음과 같은 정보들을 얻을 수 있습니다.

- `name` : 파일명

- `lastModified` : 마지막 수정일시 (`milliseconds`)

- `size` : 파일의 크기 (bytes)

- `type` : 파일의 [`MIME 타입`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)

<br>

## `multiple`

`multiple` 속성을 명시하면 1 개 이상의 파일들을 선택할 수 있도록 허용합니다. 한 번에 여러 개의 파일을 동시에 선택할 수 있습니다.

<br>

## `<input type="file">` 사용과 관련된 몇가지 유의사항

- 파일선택 요소를 디자인하기 위해 `<input type="file">`요소를 숨기는 경우가 많습니다. 이때 `visibility: hidden`이나 `display: none`을 사용하지 마세요. 보조기술이 파일선택 요소를 사용할 수 없다고 판단하기 때문입니다. 다른 대안들 중 하나를 사용하세요. `opacity: 0`도 괜찮죠.

- 파일이 선택되는 시점에 어떤 작업을 수행하려면 `change` 이벤트리스너를 등록하세요.

- 썸네일 이미지 미리보기가 필요하다면 `URL.createObjectURL()` 메소드를 호출하세요. 이때 인자는 `files` 속성이 반환한 `FileList` 객체의 배열 요소를 넣으세요. 예를 들면, `URL.createObjectURL(input.files[0])` 이런 식으로요. 이 메소드의 반환값을 `<img>` 태그의 `src` 요소에 지정하면 썸네일 이미지를 렌더링할 수 있습니다.

- `size` 속성을 통해 `byte` 단위의 파일 크기를 얻을 수 있는데요, 그 수가 크다면 아래와 같은 함수를 만들어 `KB`/`MB` 단위로 정제하는 것도 좋겠죠.

```javascript
function returnFileSize(number) {
	if (number < 1024) {
		return number + "bytes";
	} else if (number >= 1024 && number < 1048576) {
		return (number / 1024).toFixed(1) + "KB";
	} else if (number >= 1048576) {
		return (number / 1048576).toFixed(1) + "MB";
	}
}
```

<br>

---

### References

- [Using files from web applications | MDN](https://developer.mozilla.org/en-US/docs/Web/API/File/Using_files_from_web_applications)
- [Using the DOM File API in chrome code](https://developer.mozilla.org/ko/docs/Extensions/Using_the_DOM_File_API_in_chrome_code)
- [\<input type="file"\>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file)
