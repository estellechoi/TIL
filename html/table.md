# 테이블 태그(Table content tags)

테이블 데이터를 표시하기 위한 태그들입니다.

- `<table>`/`<caption>`/`<tr>`

- `<th>`/`<td>`

- `<colgroup>`/`<col>`

- `<thead>`/`<tbody>`/`<tfoot>`

<br>

## `<table>`/`<caption>`/`<tr>`

테이블(표)을 나타냅니다.

<br>

## `<th>`/`<td>`

> (Table Header / Table Data)

<br>

### 속성

#### `<th>`

- `scope` : 누구의 Header인지 명시합니다.

  - `col` : 자신이 속한 컬럼
  - `colgroup` : 모든 컬럼
  - `row` : 자신이 속한 행
  - `rowgroup` : 모든 행
  - `auto`

<br>

#### `<th>`/`<td>` 공통

- `headers` : 자신이 어떤 `<th>` 요소에 속하는지 명시합니다. 상위 `<th>` 요소의 `id` 값을 사용하여 연결합니다.

- `abbr` : 컬럼에 대한 간단한 설명을 작성합니다.
  > `<td>` 에서는 Depricated

<br>

### 예시

```html
<table>
	<caption>
		Sample Table
	</caption>
	<tr>
		<th rowspan="2" id="th-root">Data</th>
		<th headers="th-root" id="th-type">Type</th>
		<td headers="th-type">Number</td>
		<td headers="th-type">String</td>
	</tr>
	<tr>
		<th headers="th-root" id="th-value">Value</th>
		<td headers="th-value">1</td>
		<td headers="th-value">Hello</td>
	</tr>
</table>
```

<br>

## `<colgroup>`/`<col>`

각 컬럼에 속하는 Cell들을 묶어서 조작하기 위해 사용합니다.

```html
<table>
	<caption>
		Superheros and sidekicks
	</caption>
	<colgroup>
		<col />
		<col span="2" class="batman" />
		<col span="2" class="flash" />
	</colgroup>
	<tr>
		<td></td>
		<th scope="col">Batman</th>
		<th scope="col">Robin</th>
		<th scope="col">The Flash</th>
		<th scope="col">Kid Flash</th>
	</tr>
	<tr>
		<th scope="row">Skill</th>
		<td>Smarts</td>
		<td>Dex, acrobat</td>
		<td>Super speed</td>
		<td>Super speed</td>
	</tr>
</table>
```

<br>

## `<thead>`/`<tbody>`/`<tfoot>`

테이블 행들을 역할에 따라 묶는 태그입니다. 레이아웃에는 영향을 주지 않고 의미를 위해 사용합니다.

```html
<table>
	<caption>
		Superheros and sidekicks
	</caption>
	<colgroup>
		<col span="2" class="batman" />
		<col span="2" class="flash" />
	</colgroup>
	<thead>
		<tr>
			<th scope="col">Batman</th>
			<th scope="col">Robin</th>
			<th scope="col">The Flash</th>
			<th scope="col">Kid Flash</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Smarts</td>
			<td>Dex, acrobat</td>
			<td>Super speed</td>
			<td>Super speed</td>
		</tr>
	</tbody>
</table>
```

<br>

---

### References

- [\<table\>: The Table element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/table)
- [\<th\>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/th)
- [\<td\>: The Table Data Cell element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/td)
- [\<colgroup\>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/colgroup)
- [\<thead\>: The Table Head element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/thead)
