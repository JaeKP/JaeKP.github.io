---
title: 견고한-UI-설계를-위한-마크업-가이드-Part1-HTML
author: 박재경
date: 2022-10-23
categories: [WEB, Performance]
tag: [perfomance]
---

[강의 링크](https://fastcampus.co.kr/dev_red_jcm)

> HTML은 문서의 골격, 분위기, 느낌을 제공한다. 
>
> 웹에서 의사소통을 할 때, 문서의 골격을 제공하고 의미를 제공하여 사용자가 어떻게 읽어야 하는지 제공한다. 
{: .prompt-tip }

<br>

# 1. HTML Content 분류

![html-content-venn](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/html-content-venn.png)

<br>

- **Flow content (플로우 컨텐츠)**
  - body에 포함할 수 있는 모든 요소이다.
  - base, style, title 요소를 제외한 나머지 모든 요소.

```
a, abbr, address, area, map, article, aside, audio, b, bdi, bdo,
blockquote, br, button, canvas, cite, code, data, datalist, del,
details, dfn, dialog, div, dl, em, embed, fieldset, figure,
footer, form, h1, h2, h3, h4, h5, h6, header, hgroup, hr, i,
iframe, img, input, ins, kbd, label, link, main, map, mark, math,
menu, meta, meter, nav, noscript, object, ol, output, p, picture,
pre, progress, q, ruby, s, samp, script, section, select, slot,
small, span, strong, sub, sup, svg, table, template, textarea,
time, u, ul, var, video, wbr, autonomous custom elements, text
```

<br>

- **Metdata content (메타데이터 콘텐츠)**
  - 콘텐츠와 문서를 구조화 하는 요소를 의미, 다른 콘텐츠의 표시, 동작, 관계 등을 설정한다.
  - 실제로 화면에 출력하는 요소들은 아니기 때문에, CSS에서는 ✨ `display:none`처리한다.

```
base, link, meta, noscript, script, style, template, title
```

<br>

- **Heading content (헤딩 콘텐츠)**
  - 섹셔닝 콘텐츠의 헤더.
  - 섹셔닝 콘텐츠가 없어도 헤딩 콘텐츠가 있으면 암시적으로 섹션(==문서의 개요)이 형성된다.
  - ✨`display: block;`

```
h1, h2, h3, h4, h5, h6, hgroup
```

<br>

- **Sectioning content (섹셔닝 콘텐츠)**
  - 문서의 개요를 형성. 헤딩, 헤더, 풋터의 범위.
  - 각 섹셔닝 콘텐츠는 암시적인 개요를 형성한다. 또한, 섹셔닝 콘텐츠와 헤딩 콘텐츠를 함께 사용하면 명시적인 개요를 형성.
  - ✨ `display: block;`

```
article, aside, nav, section
```

<br>

- **Phrasing content (프레이징 콘텐츠)**
  - 구문 콘텐츠. 단락을 형성하는 콘텐츠.
  - ✨ `display: inline | inline-block | none;`

```
a, abbr, area, audio, b, bdi, bdo, br, button, canvas, cite, code,
data, datalist, del, dfn, em, embed, i, iframe, img, input, ins,
kbd, label, link, map, mark, math, meta, meter, noscript, object,
output, picture, progress, q, ruby, s, samp, script, select, slot,
small, span, strong, sub, sup, svg, template, textarea, time, u,
var, video, wbr, autonomous custom elements, text
```

<br>

- **Embedded content (임베디드 콘텐츠)**
  - 외부 자원을 참조하는 콘텐츠.
  - 모든 임베디드 콘텐츠는 구문 콘텐츠이다. 또한, 외부 자원을 지원하지 않는 경우 대체 자원을 명시할 수 있다.
  - ✨ `display: inline | inline-block;`

```
audio, canvas, embed, iframe, img, math, object, picture, svg, video
```

<br>

- **Interactive content (인터렉티브 콘텐츠)**
  - 사용자와 상호 작용할 수 있는 콘텐츠.
  - 입력 장치(키보드, 마우스)로 조작할 수 있다.
  - ✨ `display: inline | inline-block;`

```
a, audio, button, details, embed, iframe, img, input, label, select, textarea, video
```

<br>

# 2. HTML 명세의 구성 

[HTML Living Standard](https://html.spec.whatwg.org/multipage/)

<br>

## 1) 명세서 분석 예시

`a element`

![image-20221023174346465](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023174346465.png)

- **Category: 해당 태그가 어떤 카테고리에 속해있는 지에 대해 할 수 있다.** 
- **Contexts: 부모 요소의 콘텐츠 모델을 유추할 수 있다. (사용되는 맥락)**
  - Contexts in which this elemenet can be used: 보통은 이런 맥락에서 쓰인다. (비 규범적)
- **ContentModel: 자식 요소의 카테고리 (허용하는 자식 요소)**
  - Transparent content models (투명 콘텐츠 모델): 투명 콘텐츠 모델. 부모의 콘텐츠 모델을 따르며, 투명한 요소를 제거해도 부모와 자식 관계가 문법적으로 유효해야 한다.
- **Tag omission in text/html: 해당 태그를 생략해도 되는 가**
- **Contect Attributes**

<br>

# 2. 검색 엔진 밥상 차려주기

HTML은 사용자에게 올바른 정보를 전달하기 위함이다. 그런데, 사용자에게 도달하지 않으면 아무런 의미가 없다.

그런데 HTML을 사용하면 검색엔진에 잘 노출되도록 도와줄 수 있다.

<br>

## 1) SEO에 영향을 주는 요인들

- 검색 결과 페이지(SERP) 노출 대비 클릭률
- 백링크(backlink): 다른 웹 페이지로부터 인용(링크)되는 횟수.
- 도메인 권력(Domain authority): 검색 결과 페이지 순위 예측 점수.
- 페이지 타이틀
- 메타 디스크립션
- 로딩 속도
- SSL(https) 사용 여부
- 콘텐츠의 양, 질, 개연성
- 사용자 경험: LCP(최대 콘텐츠 블럭 그리기), CLS(누적 배치 변경)
- ...

<br>

## 2) 페이지 타이틀

### (1) 페이지 타이틀 명세

- 분류: 메타데이터 콘텐츠.
- 문맥: head 요소의 자식.
- 콘텐츠 모델: 텍스트. 공백만으로 구성할 수 없음.
- 태그 생략: 시작 태그와 종료 태그 모두 생략 불가능.

<br>

### (2) 모범 사례

![image-20221023180234457](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023180234457.png)

- 본문을 가장 잘 설명하는 키워드를 중심으로 작성해야 한다.
- 페이지마다 구체적이고 고유한(흔하지 않은) 키워드를 사용할 수 있다.
- 페이지마다 반복하는 키워드를 최소화해야 한다.
- 구체적인 키워드를 앞에 작성해야 한다. 

- 브라우저 탭에도 표시되기 때문에 어떻게 적느냐에 따라 사용자의 경험이 달라진다. 

- 검색 로봇은 JS에 의해 동적으로 생성한 페이지 타이틀도 크롤링을한다. 

<br>

### (3) 접근성

화면 낭독기 사용자는 웹 페이지 접속 시 페이지 타이틀을 음성으로 전달 받는다.

그래서 페이지 타이틀을 통해 요청한 페이지에 접속했는지 빠르게 판단할 수 있다.

<br>

## 3) 메타 디스크립션

![image-20221023181227426](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023181227426.png)

<br>

# 3. HTML 개요 알고리즘 이해

> 웹 문서의 개요는 헤딩과 섹션으로 형성된다.
{: .prompt-tip }

`Chrome HeadingsMap Extension`을 활용하여 웹사이트의 Heading을 확인할 수 있다.

<br>

## 1) Title과 Heading

> 헤딩이 없으면 개요가 없다. (문서의 목차가 없다.)
{: .prompt-tip }

- **`<title>`요소는 문서의 제목이다.**

  - 문서에서 한 번만 사용할 수 있다.

- **`<h*>` 요소는 섹션의 제목이다.**

  - 문서에서 여러 번 사용할 수 있다.

  - 문서의 골격을 설명하는 가장 중요한 태그이기 때문에, 생략하면 안 된다. 
  - 헤딩은 1부터 순차적으로 표기하는 것이 좋다.

<br>

**Heading은 문서 개요를 형성하는 기본(필수) 아이템으로 웹 브라우저와 화면 낭독기에 문서 개요를 드러내는 방법이다.** 

<br>

## 2) Sectioning Root

> HTML 5에서 새롭게 추가된 명세로서 독립적인 개요를 생성하는 개요 컨테이너이다.
{: .prompt-tip }

- 브라우저 제조사가 오랜시간 HTML5 아웃라인 알고리즘을 구현하지 않았기 때문에 이 개념을 실무에서 사용하는 것을 권장하지 않는다.
- 섹셔닝 루트 외부에서 내부 개요에 접근이 불가능하다.
- 문맥이 아닌 콘텐츠를 전체 개요에서 격리하는 역할이다.
- ex) 인용문

<br>

## 3) Sectioning Content

> 개요의 범위를 명시적으로 지정하는 역할을 한다. 
>
> 그 결과, 개요 형성에 많은 도움이 된다. 
{: .prompt-tip }

| 태그        | 설명                                             |
| ----------- | ------------------------------------------------ |
| `<article>` | 기사, 본문, 맥락으로 독립적으로 배포가 가능하다. |
| `<aside>`   | 페이지의 주요 내용이 아닐 때 사용된다.           |
| `<nav>`     | 사이트의 주된 탐색 메뉴이다.                     |
| `<section>` | 주제 별로 나누거나 묶는다.                       |

- **HTML5에서 새롭게 추가된 명세로서 명시적 개요를 생성하는 개요 컨테이너이다.**
- 적절한 수준의 헤딩을 자식 요소로 사용해야 한다. 

<br>

### 4) 실제 적용

> 섹셔닝 요소를 적극 사용하되 아웃라인 알고리즘(섹셔닝 루트, 헤딩 수준 자동 보정) 명세에는 의존하지 말아야 한다.
{: .prompt-tip }

- 헤딩을 사용하고 헤딩과 섹션(article, aside, nav, section)을 1:1로 맵핑해야 한다.
- 헤딩은 순차적으로 적용되어야 한다. 
- HTML5 개요 알고리즘(섹셔닝 루트, 헤딩 수준 자동 보정)에 의존하지 않는다.

```html
<h1>A
  <article>
	<h2>B
	  <section>
		<h3>C
```

<br>

# 4. HTML 의미론

- The div element has no special meaning at all.
- The span element doesn't mean anything on its own.

**즉, `<div>`와 `<span>`을 많이 사용하는 것은 HTML 태그를 의미 적절하게 사용하지 않는 것이다.**

보통은 스크립트나 css로 돔을 선택하거나 래핑할 때 사용해야 한다. (의미를 찾지 못했을 때 사용하는 태그) 

<br>

## 1) 요소를 대체할 만한 요소들

### (1) `<div>`

```
h1, h2, h3, h4, h5, h6, p, ul, ol, li, dl, dt, dd, blockquote, pre, address
...
article, aside, nav, section, hgroup, header, footer,
main, figure, figcaption, details, summary, dialog,
datalist
UA { display: block }
```

<br>

### (2) `<span>`

```
a, em/strong, label, q, sub/sup, ins/del, code, dfn, abbr, cite, kbd, ruby,
samp, var, small, b, u, i, s
...
data, time, mark, output, meter, progress
```

<br>

## 2) HTML Semantics

### (1) `<header>`, `<footer>`

![image-20221023200608404](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023200608404.png)

<br>

![image-20221023200735294](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023200735294.png)

- `<header>`:  도입부, 헤딩, 헤딩 그룹, 목차, 검색, 로고
- `<footer>` : 저자, 저작권, 연락처, 관련 문서
- **섹셔닝 루트(body) 또는 섹셔닝 콘텐츠(article, aside, nav, section)는 아니지만 이런 의미일 때 div 대신 사용하길 권장한다.**

<br>

```html
<body>
    <header>
        페이지 헤더
    </header>
    ...
	<footer>
        페이지 푸터
    </footer>
</body>
```

<br>

### (2) `<section>`, `<article>`

![image-20221023201021574](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023201021574.png)

<br>

![image-20221023201119894](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023201119894.png)



- `<section>`: 제목이 있는 주제별 콘텐츠 그룹 또는 응용 프로그램 영역.
- `<article>`: 섹션 요소 중 독립적으로 배포 가능한 완결형 콘텐츠. 뉴스 기사, 블로그, 본문, 사용자의 댓글 등.
- **h1~h6 요소를 포함하는 것을 강력하게 권장한다. header, footer 요소를 사용하는 것은 선택 사항**

<br>

```html
<section>
	<h2>
        섹션 헤딩
    </h2>
</section>

<article>
	<h2>
        완결형 콘텐츠 헤딩
    </h2>
</article>
```

<br>

### (3) `<nav>`

![image-20221023201313053](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023201313053.png)

- 현재 사이트 또는 현재 페이지 일부를 링크 하고 있는 주요 탐색 섹션이다.
- 사이트 또는 페이지의 주요 탐색 경로에 해당하지 않는 빵부스러기 링크, 풋터의 약관, 저작권 고지 같은 링크는 nav 요소로 적절하지 않다.
- **h1~h6 요소를 포함하는 것을 강력하게 권장한다.**

<br>

```html
<nav>
	<h2>
       섹션 헤딩
    </h2>
</nav>
```

<br>

### (4) `<aside>`

![image-20221023201417349](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023201417349.png)

- 페이지의 주된 내용과 관련이 약해서 구분할 필요가 있는 섹션이다.
- 부수적인 콘텐츠, 관련 기사, 광고 등의 내용을 포함할 수 있다.
- h1~h6 요소를 포함하는 것을 강력하게 권장한다.

<br>

```html
<aside>
	<h2>
       섹션 헤딩
    </h2>
</aside>
```

<br>

### (5) `<main>`

![image-20221023201546390](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023201546390.png)

- 문서의 핵심 주제 또는 애플리케이션의 핵심 기능과 직접 관련 있는 콘텐츠 영역을 의미한다.
- 페이지마다 반복되지 않는 내용을 포함해야 한다.
- 섹셔닝 콘텐츠가 아니므로 hx, header, footer 요소의 범위와 관련 없다.
- 하나의 페이지 안에서 하나의 main 요소만 표시할 수 있고 섹셔닝 관련 요소(article, aside, nav, section, header, footer)의 자식이 될 수 없다.
- body, div 요소를 제외한 다른 요소의 자손이 될 수 없다.

<br>

### (6) `<dialog>`

![image-20221023201629418](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221023201629418.png)

- dialog 요소는 사용자와 상호 작용하는 응용프로그램 (대화상자)을 의미한다.
- open 속성을 추가하면 대화 상자가 활성화되고 사용자가 대화 상자를 통해 상호 작용할 수 있다.
- 보통 입력 양식과 콘트롤을 포함하고 있으며 닫기 기능을 제공한다.
- 키보드 초점이 dialog 요소 내부에서 순환하도록 처리해야 한다.
  - 키보드 초점이 dialog에서 나가면 안된다.
  - tab만으로도 dialog 내부 간 이동이 가능해야 한다. 

<br>

```html
<dialog >
	<h2>
       헤딩을 포함하는 것이 자연스럽다.
    </h2>
</dialog >
```

<br>

### (7) `<figure>`, `<figcaption>`

- 문서의 주된 흐름을 위해 참조하는(부록으로 옮겨도 괜찮은) 독립적인 완결형 요소로서 이미지, 도표, 코드 등의 내용물과 설명(figcaption)을 포함한다.
- 선택적으로 처음 또는 마지막에 figcaption 요소를 자식 요소로 포함할 수 있고 또는 생략할 수 있다.
- figure 안에서 figcaption 요소가 선언 된다면 한 번만 선언할 수 있다.
  - figcaption은  부모 figure 요소의 내용에 대한 설명 또는 범례를 의미한다.
  - 반드시 figure 요소의 처음 또는 마지막에 포함할 수 있고 또는 생략할 수 있다.

```html
<figure>
	<img>
    <figcaption>이미지 설명</figcaption>
</figure>
```

<br>

### (8) etc

| 태그                       | 설명                                                         |
| -------------------------- | ------------------------------------------------------------ |
| `<mark>`                   | 독자의 주의를 끌기 위한 하이라이트로 현재 독자의 행위나 관심에 따라 강조한 것이다. <br />검색 결과 목록에서 사용자가 입력한 키워드이다. |
| `<address>`                | 가까운 조상 article 또는 body 요소를 범위로 하는 관련 연락처 정보이다.<br/>우편 정보를 의미하는 것이 아님에 유의해야 하며. 흔히 footer 요소에 나타남.<br/>p 요소의 자손이 될 수 없다. |
| `<ins>`, `<del>`           | `<ins>`는 추가한 내용을 의미한다.<br/>`<del>`은 삭제한 내용을 의미한다.<br/>콘텐츠 모델이 투명해서 어떤 요소라도 포함할 수 있지만, 여러 단락을 한꺼번에 포함하는 것은 부적절하다. |
| `<progress>`               | 계산 또는 사용자 과업의 진척도를 의미한다.<br/>원격 호스트의 응답을 기다려야 하는 경우도 있을 수 있기 때문에 진척도는 정확하지 않을 수 있다.<br/>낡은 브라우저를 위해 value 값과 별도로 콘텐츠를 제공하는 것이 좋다. |
| `<b>`, `<i>`, `<s>`, `<u>` | `<b>`: 강조할 의도가 없는 볼드를 의미하며 `<strong>` 요소를 고려한 뒤 사용하는 것이 좋다. <br />`<i>`: 현재 맥락과 다른 분위기를 의미하며 `<em>`요소를 고려한 뒤 사용하는 것이 좋다. <br />`<s>`: 정확하지 않거나 관련 없는 것을 의미하며 `<del>`요소를 고려한 뒤 사용하는 것이 좋다. <br />`<u>`: 오타, 중국 고유 명사 등을 표기할 때 사용하며 `<ins>` 요소를 고려한 뒤 사용하는 것이 좋다. |

<br>

# 5. 상호작용 콘텐츠의 올바른 용법

> 사용자와 상호 작용하는 부분에 대한 태그가 존재한다. 
>
> 사용자는 웹페이지를 조작하거나 내용을 입력하기도 한다. 이 과정 중에서 만나는 문제들을 HTML로 처리할 수 있다. 
{: .prompt-tip }

- 사용자와 상호 작용할 수 있는 콘텐츠.
- 입력 장치(키보드, 마우스)로 조작할 수 있다.

```
a, audio, button, details, embed, iframe, img, input, label, select, textarea, video
```

<br>

## 1) `<a>` vs `<button>`

> 같은 외형을 지닌 경우라도 a, button 요소를 구별해서 사용해야 한다. 
{: .prompt-tip }

- 실행 결과를 가리킬 수 있는 URL이 있으면 a 요소를 사용한다.
  - **혹은 대화 상자를 참조하는 경우. a 요소의 href 값은 dialog 요소의 id (해시 URL)를 참조할 수 있다.**
  - 링크 위에 마우스를 올리면 웹 브라우저는 상태 표시줄에 목적지 URL을 표시해 준다.
  - 문맥 메뉴를 이용하면 새 탭에서 링크 열기 기능을 사용할 수 있다.
- 참조할 URL이 없으면 button 요소를 사용한다.
  - 타겟 URL을 설정할 수 없거나, 타겟 URL을 설정하지 않는 게 나은 상황.
- 커서 모양이 다름에 유의해야 한다. 
  - 버튼에 손 모양 커서를 사용하면 안 된다.

<br>

### (1) a target

```html
<a
   href="링크"
   target="_blank"
   rel="noopener noreferrer"
>
```

- 안전하지 않은 외부 페이지 새 창 링크이다. 
- `rel`속성을 사용하지 않으면, 새 창으로 열린 외부 페이지 B는 자바스크립트 window.opener 객체를 통해 부모 페이지 A의 제어 권한을 획득.
  - 사용자는 탭 가로채기(tabnabbing) 공격에 노출된다.
- `rel`속성의 noopener 값은 window.opener 객체를 제거한다. 그리고 noreferrer 값은 window.opener 제어 불능 시킨다.
  - noopener를 지원하지 않는 낡은 브라우저를 위해 noreferrer를 함께 표기한다.
- 최신 브라우저는 target="_blank" 링크에 rel="noopener" 속성을 암시적으로 적용하고 있다.

<br>

### 2) `<details>`

>  열림 상태일 때 정보를 표시하는 위젯이다.
{: .prompt-tip }

- details 요소에 open 속성을 넣으면 열린 상태로 표시한다.
- summary 요소는 details 요소의 나머지 부분에 대한 요약, 캡션, 범례를 의미한다.

```html
<details open>
	<summary>details 요소란?</summary>
	<p>열림 상태가 아닐 때, 보이지 않는다.</p>
</details>
```

<br>

## 3) `<input>`

> 다양한 쓰임의 type을 아는 것이 input 활용의 모든 것이다.
{: .prompt-tip }

[input-type](https://html.spec.whatwg.org/multipage/input.html#attr-input-type)

<br>

### (1) attrs

| 속성          | 설명                                                         |
| ------------- | ------------------------------------------------------------ |
| `required`    | 필수 값임을 명시한다.                                        |
| `placeholder` | 콘트롤에 초깃값이 없을 때 사용자에게 데이터 입력을 지원하기 위해 제공하는 짧은 힌트나 샘플이다.<br />`<label>`의 대안으로 사용하면 안 된다. |
| `datalist`    | 다른 콘트롤을 위해 미리 정의된 옵션 세트를 의미한다.         |

```html
<label for="local">지역 번호</label>
<input type="text" id="local" list="local-list">
<datalist id="local-list">
	<option value="02" label="서울"></option>
    <option value="031" label="경기"></option>
</datalist>
```

<br>

# 6. 이미지 마크업 최적화

| 이미지 포맷   | 설명                                                         |
| ------------- | ------------------------------------------------------------ |
| `.jpg`, `png` | 모든 브라우저에서 지원하는 폴백 이미지이다.                  |
| `.webp`       | jpg/png 대비 30 ~ 70% 수준의 용량이다. (알파 채널, IE 미지원) |
| `.avif`       | 저용량 + 고품질 (알파 채널, 크롬 / 삼성 인터넷 지원)         |

<br>

## 1) `<picture>`

> `<picture>` 태그의 속성에 따라 분기할 수 있다. 
{: .prompt-tip }

### (1) 확장자 분기

avif 포맷을 제공하고 webp, jpg를 대체 수단으로 제공할 수 있다. 

```javascript
if ('avif'를 지원하면) {
'avif' 출력
} else if ('webp'를 지원하면) {
'webp' 출력
} else {
'jpg' 출력
}
```

<br>

```html
<picture>
	<source srcset="x.avif" type="image/avif">
    <source srcset="x.webp" type="image/webp">
    <source srcset="x.jpg" alt >
</picture>
```

<br>

### (2) media 쿼리에 따른 사이즈 분기

```html
<picture>
    <source srcset="samll.webp" media="(max-width:960px)">
    <source srcset="large.webp" alt >
</picture>
```

<br>

### (3) 해상도 분기

`2x`는 사용자가 레티나 디스플레이를 사용할 때를 의미한다. 

```html
<picture>
    <source srcset="retina.webp 2x, default.webp" >
    <img srcset="retina.jpg 2x" src="default.jpg" alt>
</picture>
```

<br>

### (4) 성능 개선

빠른 로딩 속도를 통해 이미지 전송 비용(User/Service)을 절약할 수 있다.

```html
<img
	loading="lazy" // 지연 로딩
    decoding="async" // 디코딩 지연
>
```

<br>
