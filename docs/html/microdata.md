# 마이크로데이터(Microdata)를 사용하여 Schema.org 시작하기

> [Getting started with schema.org using Microdata](https://schema.org/docs/gs.html)를 번역한 글입니다.

<br>

웹 개발자라면 대부분 HTML 태그에 대해 잘 알고 친숙할 것입니다. 일반적으로 HTML 태그는 브라우저에게 태그에 포함된 정보를 어떻게 렌더링해야 하는지 알려주는 역할을 합니다. 예를 들어, `<h1>Avatar</h1>`이라고 작성한 코드는 `Avatar`라는 텍스트가 헤딩(Heading) 1 포맷에 맞게 보여지도록 렌더링됩니다. 하지만 `<h1>`이라는 태그 자체는 `Avatar`라는 텍스트가 무엇을 의미하는지에 대해서는 전혀 알려주지 못합니다. 가령, 성공한 3D 영화의 제목이라거나, 프로필 사진의 타입 중 하나라는 정보를 말이죠. 따라서 태그 자체만으로는 검색엔진이 적절한 콘텐츠를 보여주도록 하는데 도움이 되지 못합니다.

Schema.org는 Google, Microsoft, Yandex, Yahoo! 등 주요 검색엔진들이 인식할 수 있도록 문서를 마크업하는 키워드들을 제공합니다. 웹 페이지에 정보를 추가하기 위해 Schema.org에서 제공하는 키워드를 사용하세요. 마이크로데이터(Microdata), RDFa, JSON-LD의 형태로 사용할 수 있습니다. 이 글은 마이크로데이터(Microdata)를 사용하여 Schema.org를 빠르게 시작할 수 있는 가이드 입니다.

이 글은 마이크로데이터 위주로 설명되어 있지만, schema.org 사이트에서 대부분의 예제가 RDFa, JSON-LD 방식을 포함하고 있으니 참고하세요. 이 글에서 소개하는 타입(Types), 속성(Properties) 등의 기본 개념들은 마이크로데이터 외에도 적용할 수 있습니다. 예제를 통해 각 방법을 비교해보세요.

<br>

## 목차

1. 마이크로데이터를 사용하여 마크업하기

   - 1a. 마이크로데이터를 왜 사용하나요?
   - 1b. `itemscope`과 `itemtype`
   - 1c. `itemprop`
   - 1d. 아이템 속성의 값으로 다른 아이템 지정하기

2. Schema.org 키워드 사용하기

   - 2a. Schema.org 타입과 속성
   - 2b. 예상 타입, 텍스트, URL
   - 2c. 작성한 마크업 테스트하기

3. 심화 토픽: 기계가 이해할 수 있는 정보
   - 3a. 날짜, 시간, 지속시간
   - 3b. Enumerations과 고전적인 레퍼런스
   - 3c. 암시적 정보
   - 3d. Schema.org 더 알아보기

<br>

## 1. 마이크로데이터를 사용하여 마크업하기

### 1a. 마이크로데이터를 왜 사용하나요?

웹 페이지는 사람들이 해당 페이지를 읽었을 때 자연스레 이해할 수 있는 암시적인 정보를 가지고 있습니다. 하지만 검색엔진은 사람들이 웹 페이지를 보고 자연스레 추론할 수 있는 정보들을 제한된 범위 내에서만 이해할 수 있습니다. HTML 문서에 적절한 태그들을 추가함으로써 검색엔진을 도울 수 있습니다. 이는 검색엔진에게 "이 정보는 특정한 영화, 장소, 사람, 영상을 묘사하고 있어"라고 알려주는 것과 같습니다. 마이크로데이터란, 이런 역할을 하는 태그들을 의미합니다. 마이크로데이터는 HTML5에서 소개되었습니다.

<br>

### 1b. `itemscope`과 `itemtype`

예제 코드와 함께 알아봅시다. 영화 아바타에 대한 웹 페이지가 있다고 상상해보세요. 이 페이지에는 영화의 예고편 영상 링크, 감독에 대한 정보 등이 포함되어 있습니다. 이 페이지의 마크업은 아래와 비슷할 것입니다.

```html
<div>
	<h1>Avatar</h1>
	<span>Director: James Cameron (born August 16, 1954)</span>
	<span>Science fiction</span>
	<a href="../movies/avatar-theatrical-trailer.html">Trailer</a>
</div>
```

<br>

영화 아바타에 "대한" 섹션을 지정해볼까요? 이를 위해서는, `itemscope` 이라는 전역 속성을 사용합니다. 아래와 같이, `itemscope` 속성을 원하는 정보들을 모두 포함하는 태그에 추가하세요.

```html
<div itemscope>
	<h1>Avatar</h1>
	<span>Director: James Cameron (born August 16, 1954) </span>
	<span>Science fiction</span>
	<a href="../movies/avatar-theatrical-trailer.html">Trailer</a>
</div>
```

<br>

`itemscope` 속성을 추가하는 것은 이 웹 페이지가 특정 아이템에 대한 정보를 담은 `<div>` 블록을 포함하고 있음을 명시하는 것과 같습니다. 하지만 이 것만으로는 정보를 구체화하기에 충분치 않은데요, `itemtype` 속성을 추가하면 해당 아이템의 타입(Type)을 지정할 수 있습니다.

```html
<div itemscope itemtype="http://schema.org/Movie">
	<h1>Avatar</h1>
	<span>Director: James Cameron (born August 16, 1954)</span>
	<span>Science fiction</span>
	<a href="../movies/avatar-theatrical-trailer.html">Trailer</a>
</div>
```

이로써 위의 `<div>` 블록 내에 포함된 아이템이 "영화"에 대한 정보라는 것을 구체화했습니다. 아이템 타입(`itemtype`)의 값은 URL 형태로 지정하며, 예제에서는 `http://schema.org/Movie`를 사용했습니다.

<br>

### 1c. `itemprop`

영화 아바타에 대한 또 어떤 추가적인 정보를 검색엔진에 줄 수 있을까요? 영화라는 아이템은 배우, 감독, 별점 등 재미있는 속성(Property)들을 갖고있죠. 이러한 아이템의 속성들에 이름을 부여하기 위해 `itemprop` 속성(Attribute)을 사용합니다. 예를 들어, 아이템에 영화 감독에 대한 데이터를 추가하기 위해 감독의 이름을 감싸고 있는 태그에 `itemprop="director"`라고 작성하세요. ([여기](http://schema.org/Movie)에서 영화 아이템의 속성 이름으로 사용 가능한 전체 리스트를 확인할 수 있습니다.)

```html
<div itemscope itemtype="http://schema.org/Movie">
	<h1 itemprop="name">Avatar</h1>
	<span
		>Director: <span itemprop="director">James Cameron</span> (born August 16,
		1954)</span
	>
	<span itemprop="genre">Science fiction</span>
	<a href="../movies/avatar-theatrical-trailer.html" itemprop="trailer"
		>Trailer</a
	>
</div>
```

각 단어에 `itemprop` 속성을 부여하기 위해 `<span>` 태그로 단어들을 감싼 것이 보이시나요? `<span>` 태그는 페이지 레이아웃에 영향을 주지 않기 때문에 특정 텍스트에 `itemprop` 속성을 추가할 때 유용합니다.

검색엔진은 이제 `http://www.avatarmovie.com`이 단순한 URL이 아니라, James Cameron 감독이 만든 공상과학 영화 아바타의 예고편 링크라는 정보를 인식할 수 있게 되었습니다.

<br>

### 1d. 아이템 속성의 값으로 다른 아이템 지정하기

아이템의 속성(Itemprop) 값으로 문자열이나 URL이 아닌, 아이템 자체를 지정할 수 있습니다. 자신의 속성 그룹을 갖고있는 또 하나의 아이템을 말이죠. 가령, 영화감독(`director`) 속성의 값으로 `Person` 타입의 또 다른 아이템을 지정할 수 있습니다. 이 `Person` 타입의 아이템은 `name`, `birthDate`와 같은 속성들을 가질 수 있습니다. 다만 속성의 값이 단순 문자열이나 URL이 아니라, 또 다른 아이템이라는 것을 명확히 하기 위해 이름을 지정하는 `itemprop`과 함께 `itemscope` 속성을 지정하세요.

```html
<div itemscope itemtype="http://schema.org/Movie">
	<h1 itemprop="name">Avatar</h1>
	<div itemprop="director" itemscope itemtype="http://schema.org/Person">
		Director: <span itemprop="name">James Cameron</span> (born
		<span itemprop="birthDate">August 16, 1954</span>)
	</div>
	<span itemprop="genre">Science fiction</span>
	<a href="../movies/avatar-theatrical-trailer.html" itemprop="trailer"
		>Trailer</a
	>
</div>
```

<br>

## 2. Schema.org 타입 키워드 사용하기

### 2a. Schema.org 타입과 속성

모든 웹 페이지가 영화와 사람들에 대한 것은 아닐 것입니다. 위에서 소개한 `Movie`, `Person` 외에도 Schema.org는 다양한 타입들을 제공합니다. 또한 각 타입은 해당 아이템을 설명하는데 필요한 속성들을 가지고 있습니다.

가장 범용적으로 사용되는 타입은 `Thing`입니다. 4 개의 속성(`name`, `description`, `url`, `image`)을 갖고 있는데요, 이 속성들은 덜 범용적인 다른 타입들의 속성으로도 사용할 수 있습니다.

예를 들어, `Place`는 `Thing` 보다 덜 범용적인 타입이죠. `LocalBusiness`는 `Place` 보다도 덜 범용적인 타입입니다. 더 구체적이고 덜 범용적인 아이템들은 자신 보다 범용적인 부모 타입의 속성을 상속받습니다. (`LocalBusiness` 타입은 `Place`/`Organization` 타입보다 구체적인 타입, 즉 하위의 타입입니니다. 따라서 두 타입의 속성들을 모두 상속받죠.)

아래는 인기있는 타입들입니다.

- Creative works: `CreativeWork`, `Book`, `Movie`, `MusicRecording`, `Recipe`, `TVSeries` ...
- Embedded non-text objects: `AudioObject`, `ImageObject`, `VideoObject`
- `Event`
- `Organization`
- `Person`
- `Place`, `LocalBusiness`, `Restaurant` ...
- `Product`, `Offer`, `AggregateOffer`
- `Review`, `AggregateRating`

[모든 타입 리스트](https://schema.org/docs/full.html)를 확인해보세요.

<br>

### 2b. 예상 타입, 텍스트, URL

Schema.org 타입을 마크업에 추가할 때 주의할 점들이 있습니다.

- <strong>Schema.org 타입 키워드는 많이 사용할 수록 좋지만, 숨겨진 텍스트에는 사용을 지양하세요.</strong> 일반적으로 더 많은 콘텐츠를 마크업 할 수록 좋습니다. 하지만 사람들이 웹 페이지에 방문했을 때 시각적으로 확인할 수 있는 콘텐츠에만 키워드를 사용하는 것이 일반적인 규칙입니다. 숨겨진 블록들에는 키워드를 사용하지 마세요.

- <strong>예상 타입(Expected types)과 일반 문자열 중 무엇을 써야 할까요.</strong> Schema.org 타입을 검색하다보면, 많은 속성들이 예상 타입을 명시하고 있다는 것을 알게 될 것입니다. 이는 속성의 값으로 다른 타입의 아이템을 지정할 수 있다는 말입니다. (<em>섹션 1d. 아이템 속성의 값으로 다른 아이템 지정하기</em>를 보세요.) 하지만 이는 필수사항은 아니며, 속성의 값으로 일반적인 문자열이나 URL만 사용해도 됩니다. 한편, 예상 타입이 명시되어 있다면, 그 예상 타입의 자식 타입을 포함하는 것도 허용됩니다. 예를 들어, 예상 타입으로 `Place`가 명시되어 있다면 `Place`의 자식 타입인 `LocalBusiness` 타입의 아이템을 속성의 값으로 사용할 수 있습니다.

- <strong>`url` 속성을 사용할 때는 같은 타입의 아이템이더라도 따로 마크업하세요.</strong> 어떤 웹 페이지는 특정한 하나의 주제에 관한 페이지일 수 있습니다. 예를 들어, 어떤 특정한 사람에 대한 페이지를 마크업한다고 상상해보세요. `Person` 타입을 사용하여 하나의 아이템으로 마크업 할 수 있겠죠. 반면 어떤 페이지는 여러 다른 것들을 담고 있는 페이지일 것입니다. 예를 들어, 회사 홈페이지에 직원들을 소개하는 페이지가 있고, 해당 페이지에 각 직원들의 프로필 페이지로 이동할 수 있는 링크들이 포함되어 있다고 가정해봅시다. 이 경우에는, 각 직원 정보를 담고 있는 요소마다 `Person` 타입의 아이템을 직접 마크업하고, `url` 속성을 지정해야 합니다. 이렇게요.

```html
<div itemscope itemtype="http://schema.org/Person">
	<a href="alice.html" itemprop="url">Alice Jones</a>
</div>
<div itemscope itemtype="http://schema.org/Person">
	<a href="bob.html" itemprop="url">Bob Smith</a>
</div>
```

<br>

### 2c. 작성한 마크업 테스트하기

웹 페이지 레이아웃을 수정하고 테스트할 때는 웹 브라우저가 중요하고, 코드를 테스트할 때는 컴파일러가 중요합니다. 이처럼 Schema.org 마크업을 테스트하는 것 역시 중요합니다. 적용한 마이크로데이터가 실제로 제 역할을 해야 하니까요. Google에서 제공하는 테스트 툴을 사용하면 마크업을 테스트하고 에러를 찾아낼 수 있습니다.

<br>

## 3. 심화 토픽: 기계가 이해할 수 있는 정보

Schema.org에서 제공하는 타입과 속성 키워드들을 사용하면 `itemscope`/`itemtype`/`itemprop`만으로도 많은 페이지들을 설명할 수 있습니다.

하지만 추가적인 설명 없이는 엔진이 아이템 속성(Itemprop)이 제공하는 정보를 완벽하게 이해하기 어려운 경우도 있습니다. 이 심화 토픽 섹션에서는 기계(엔진)이 이해할 수 있는 정보를 마크업하는 방법을 설명합니다.

- 날짜, 시간, 지속시간 : `<time>` 태그와 `datetime` 속성을 사용하세요
- Enumerations과 고전적인 레퍼런스 : `<link>` 태그와 `href` 속성을 사용하세요
- 암시적 정보: `<meta>` 태그와 `<content>` 속성을 사용하세요

<br>

### 3a. 날짜, 시간, 지속시간 : `<time>` 태그와 `datetime` 속성을 사용하세요

날짜와 시간 정보는 기계가 이해하기 어렵습니다. 예를 들어, `04/01/11`로 표기된 날짜 정보는 `2004년 1월 11일`을 의미하나요? 아니면 `2011년 1월 4일` 혹은 `2011년 4월 1일`을 의미하나요? 날짜 정보의 불명확성을 없애기 위해 `<time>` 태그와 `datetime` 속성을 사용하세요. `datetime` 속성의 값은 고정된 포맷(`YYYY-MM-DD`)을 따르기 때문에 명확합니다. 아래와 같이 마크업하면 명확하게 `2011년 4월 1일`을 의미하는 날짜 정보를 제공할 수 있습니다.

```html
<time datetime="2011-04-01">04/01/11</time>
```

<br>

`hh:mm` 또는 `hh:mm:ss` 포맷을 이용하여 시간 정보 역시 명확하게 제공할 수 있습니다. 시간은 대문자 `T` 프리픽스(Prefix)를 사용하여 지정할 수 있으며 날짜 데이터 뒤에 붙여 제공합니다. 아래 예제를 보세요.

```html
<time datetime="2011-05-08T19:30">May 8, 7:30pm</time>
```

<br>

다음 예제는 2011년 5월 8일에 예정된 콘서트 정보를 담고 있는 HTML 마크업입니다. `Event` 타입의 아이템은 `name`, `description`, `startDate` 속성을 가질 수 있습니다.

```html
<div itemscope itemtype="http://schema.org/Event">
	<div itemprop="name">Spinal Tap</div>
	<span itemprop="description"
		>One of the loudest bands ever reunites for an unforgettable two-day
		show.</span
	>
	Event date:
	<time itemprop="startDate" datetime="2011-05-08T19:30">May 8, 7:30pm</time>
</div>
```

<br>

비슷한 방법으로 지속시간 정보도 제공할 수 있습니다. 지속시간은 대문자 `P` 프리픽스와 `00H00M` 포맷을 사용하여 지정합니다. 아래는 1시간 30분이 걸리는 요리 시간 정보를 나타내는 마크업입니다.

```html
<time itemprop="cookTime" datetime="PT1H30M">1 1/2 hrs</time>
```

`H`는 시간, `M`은 분을 나타냅니다. 날짜, 시간, 지속시간 마크업 표준은 ISO 8601에 명시되어 있습니다.

<br>

### 3b. Enumerations과 고전적인 레퍼런스 : `<link>` 태그와 `href` 속성을 사용하세요

#### Enumerations

어떤 속성은 제한된 몇 개의 값들 중 하나를 값으로 지정할 수 있어야 합니다. 이런 속성들을 흔히 `Enumerations`라고 부릅니다. 예를 들어, 세일 상품을 판매하는 온라인 숍에서 `Offer` 타입의 아이템을 마크업한다고 상상해봅시다. 이 경우, `availability` 속성은 전형적으로 다음과 같은 것들을 값으로 사용할 수 있을 것입니다. `In stock`, `Out of stock`, `Pre-order` 등과 같은 값들 말이죠. 이 경우 속성의 값으로 Schema.org에서 제공하는 키워드를 지정할 수 있으며, URL 형식으로 작성합니다. `itemtype`의 값으로 URL을 지정하는 것과 마찬가지로요.

아래는 세일 상품 정보를 `Offer` 타입의 아이템으로 마크업한 예제입니다.

```html
<div itemscope itemtype="http://schema.org/Offer">
	<span itemprop="name">Blend-O-Matic</span>
	<span itemprop="price">$19.95</span>
	<span itemprop="availability">Available today!</span>
</div>
```

<br>

아래는 같은 상품 정보를 `<link>` 태그와 `href` 속성을 사용하여 마크업한 예제입니다. `availability` 속성의 값을 지정하기 위해 `href` 속성을 사용했습니다.

```html
<div itemscope itemtype="http://schema.org/Offer">
	<span itemprop="name">Blend-O-Matic</span>
	<span itemprop="price">$19.95</span>
	<link itemprop="availability" href="http://schema.org/InStock" />Available
	today!
</div>
```

<br>

Schema.org에서는 다양한 속성들의 값을 `Enumerations`로 지정할 수 있도롤 많은 타입을 제공합니다. 가능한 속성의 값을 제한해야 하는 거의 모든 경우에 Schema.org에서 적절한 `Enumerations` 링크를 찾을 수 있을 것입니다. 예로, `availability` 속성의 값으로 가능한 값들은 [`ItemAvailability`](https://schema.org/ItemAvailability)에 명시되어 있습니다.

<br>

### 3c. 고전적인 레퍼런스

전형적으로 링크들은 `<a>` 태그를 사용하여 나타냅니다. 가령, 아래 마크업은 책 "Catcher in the Rye"에 대한 Wikipedia 페이지 링크를 제공합니다.

```html
<div itemscope itemtype="http://schema.org/Book">
	<span itemprop="name">The Catcher in the Rye</span>— by
	<span itemprop="author">J.D. Salinger</span> Here is the book's
	<a itemprop="url" href="http://en.wikipedia.org/wiki/The_Catcher_in_the_Rye"
		>Wikipedia page</a
	>.
</div>
```

<br>

위에서와 같이 페이지 링크를 나타내기 위해 `itemprop="url"` 속성 값을 사용할 수도 있습니다. 이러한 외부 사이트 링크를 사용하면, 해당 웹 페이지가 무엇을 설명하고 있는지 검색엔진이 이해하는데 도움이 됩니다.

눈에 보이는 링크를 추가하지 않고 검색엔진에 정보만 제공해야 할 때는 아래와 같이 `<link>` 태그를 사용하세요.

```html
<div itemscope itemtype="http://schema.org/Book">
	<span itemprop="name">The Catcher in the Rye</span>—
	<link
		itemprop="url"
		href="http://en.wikipedia.org/wiki/The_Catcher_in_the_Rye"
	/>
	by <span itemprop="author">J.D. Salinger</span>
</div>
```

<br>

### 3d. 암시적 정보: `<meta>` 태그와 `<content>` 속성을 사용하세요

종종 어떤 정보들은 중요한 정보이기 때문에 마크업하는 것이 적절하지만, 시각적인 문제로 마크업하기 곤란할 수 있습니다.

이러한 정보들은 이미지를 통해 제공할 수 있습니다. 예로, 5점 중 4점의 별점을 나타내기 위해 별 이미지를 사용할 수 있겠죠. 혹은 영상의 시간을 나타내기 위해 `Flash` 객체를 사용하거나, 결제 통화 정보를 페이지에 노출시키지는 않으면서 포함시킬 수 있을 것입니다. 이와 같은 모든 경우에 `<meta>` 태그와 `content` 속성을 사용하세요. 아래의 예제를 보시죠. 5점 중 4점의 별점을 나타내는 이미지 예제입니다.

```html
<div itemscope itemtype="http://schema.org/Offer">
	<span itemprop="name">Blend-O-Matic</span>
	<span itemprop="price">$19.95</span>
	<img src="four-stars.jpg" />
	Based on 25 user ratings
</div>
```

<br>

이제 검색엔진에 별점 정보를 제공하기 위해 정보를 마크업 해봅시다.

```html
<div itemscope itemtype="http://schema.org/Offer">
	<span itemprop="name">Blend-O-Matic</span>
	<span itemprop="price">$19.95</span>
	<div
		itemprop="reviews"
		itemscope
		itemtype="http://schema.org/AggregateRating"
	>
		<img src="four-stars.jpg" />
		<meta itemprop="ratingValue" content="4" />
		<meta itemprop="bestRating" content="5" />
		Based on <span itemprop="ratingCount">25</span> user ratings
	</div>
</div>
```

이 기법은 최소로 사용해야 합니다. 시각적으로 마크업하기 곤란할 때만 `<meta>` 태그를 사용하세요.

<br>

#### Schema.org 더 알아보기

대부분의 사이트와 기관들은 Schema.org 사용을 확대할 이유가 없을 것입니다. 하지만, Schema.org에서는 추가적인 속성이나 하위 타입들을 명시하는 방법들을 제공합니다. 관심이 있다면 [schema.org extension mechanism](https://schema.org/docs/extension.html)을 읽어보세요.

<br>

---

### References

- [Getting started with schema.org using Microdata](https://schema.org/docs/gs.html)
- [Google Search - Reference](https://developers.google.com/search/docs/data-types/article)
