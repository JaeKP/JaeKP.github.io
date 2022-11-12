---
title: 실전-웹-성능-최적화-(feat. React)-Part2
author: 박재경
date: 2022-11-12
categories: [WEB, Performance]
tag: [perfomance]
---

[강의 링크](https://www.inflearn.com/course/%EC%9B%B9-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94-%EB%A6%AC%EC%95%A1%ED%8A%B8-2)

**웹 성능은 크게 두 가지로 분류할 수 있다.** 

바로, 로딩 성능과 렌더링 성능이다.

- 로딩 성능: 각 리소스를 불러오는 성능을 의미한다. 
- 렌더링 성능: 불러온 리소스를 화면에 보여주는 성능을 의미한다.

<br>


# 1. 일반 홈페이지 최적화

`사용하는 툴`

- 크롬 Network 탭
- 크롬 Performance 탭
- 크롬 Lighthouse 탭 
- Coverage 탭

<br>

## 1) 이미지 레이지(lazy) 로딩

`로딩 성능 최적화`

가끔 히어로 컨텐츠보다 이미지가 먼저 로딩 되어 중요한 컨텐츠의 로딩이 늦어지는 경우가 있다. 히어로 컨텐츠보다 먼저 로딩되는 이미지들을 처리하는 방법은 두가지가 있다.

- 최대한 빠르게 이미지를 다운로드한다. (궁극적인 해결방법이 아니다. ) 
- 이미지는 당장 눈에 보이는 요소가 아니기 때문에 나중에 로딩 시킨다. 

<br>

### (1) 이미지 레이지 로딩

>  이미지를 나중에 필요할때 로딩하겠다라는 의미이다.
{: .prompt-tip }

그렇다면 해당 이미지들은 언제 불려야할까?

바로 이미지가 보여지는 순간 혹은 직전에 이미지가 로드되면 좋을 것이다.

이미지가 스크롤에 닿는 순간에 이미지를 로드한다. 

즉, 이미지가 보여지는 요소까지 스크롤이 된다면 이미지를 로딩하는 것이다.

**스크롤 이벤트를 통해, 스크롤이 이미지가 있는 곳까지 판단을 해서 이미지를 로드한다.**   

<br>

### (2) intersection observer

> 스크롤 이벤트마다 많은 함수가 실행된다는 단점이 있다. 이는 `intersection observer`를 통해 해결할 수 있다. 
{: .prompt-tip }

`intersection observer`를 통해 특정 엘레멘트를 observer를 하면, 해당 요소가 화면에 보여지는 지를 확인할 수 있다.

**즉, 화면에 이미지가 보여지면 로딩을 하는 것이다.**

성능에 이득을 챙길 수 있다.

<br>

```javascript
function createObserver() {
  let observer;
    
  let options = {
    root: null,
    rootMargin: "0px",
    threshold: buildThresholdList()
  };

    // args: 콜백 함수, options 객체
  observer = new IntersectionObserver(handleIntersect, options);
    
    // observe라는 메서드를 통해 DOMElement를 이제 observe 상태가 된다.
  observer.observe(boxElement);
}
```

- **콜백함수는 처음엔 oberve할 때, 페이지에서 보일 때, 페이지에서 안 보일때 호출한다.**

<br>

### (3) 리액트에 적용하기

```javascript
function Card(props) {
  const imgRef = useRef(null);

  // 마운트할 때 한번만 하면 된다.
  useEffect(() => {
    const options = {};
      
    const callback = (entries, observer) => {
      entries.forEach(entry => {
        // 2. 이미지가 보이는 순간에만 콜백함수가 실행하도록 한다.
        if(entry.isIntersecting) {
           // 3. data-src속성에 저장한 이미지 소스를 넣는다.
          entry.target.src = entry.target.dataset.src;
            // 4. 이미지를 넣었으면 이제 unobserve한다.
          observer.unobserve(entry.target);
        }
      });
    };
    const observer = new IntersectionObserver(callback, options);

     // 1. useRef를 통해 접근한다.
    observer.observe(imgRef.current);
  }, [])

	return (
		<div className="Card text-center">
			<img data-src={props.image} ref={imgRef}/>
			<div className="p-5 font-semibold text-gray-700 text-xl md:text-lg lg:text-xl keep-all">
				{props.children}
			</div>
		</div>
	)
}

export default Card
```

<br>

## 2) 이미지 사이즈 최적화

`로딩 성능 최적화`

이미지 자체가 너무 커서 로딩하는 것 조차 네트워크 리소스를 많이 잡아 먹을 때가 있다.

큰 사이즈를 이미지를 우리가 필요한 사이즈의 이미지로 줄이고 압축을 통해 더 낮은 용량의 이미지를 만들 수 있다.

<br>

### (1) 이미지 사이즈 포맷

- PNG: 무손실 압축으로 용량이 크다
- JPG: 압축이 되어있다. 
  - PNG 보다 JPG 사용을 권장한다.
- WEBP: JPG보다 더 좋은 이미지 포맷
  - 지원하지 않는 브라우저가 존재한다
  - 화질, 사이즈, 용량에 있어서 JPG보다 성능이 좋다.

**squoosh.app사이트를 통해 이미지를 변환한다.** 

<br>

### (2) 분기 추가하기

> WEBP를 지원하지 않는 브라우저가 존재한다. `<picture>`태그를 활용하여 브라우저 별로 이미지 확장자를 다르게 제공할 수 있다. 
{: .prompt-tip }

```html
<!-- 사이즈 -->
<picture>
  <source srcset="mdn-logo-wide.png" media="(min-width: 600px)" />
  <img src="mdn-logo-narrow.png" alt="MDN" />
</picture>


<!-- 확장자 -->
<picture>
  <source srcset="photo.avif" type="image/avif" />
  <source srcset="photo.webp" type="image/webp" />
  <img src="photo.jpg" alt="photo" />
</picture>
```

<br>

```html
<picture>
	<source data-srcset={main_items_webp} type="image/webp" />
	<img data-src={main_items} ref={imgEl1} alt="" />
</picture>
```

- webp 파일 확장자를 제공하면 main_items_webp 이미지를 제공한다.
- 그러지 않을 경우 main_items 이미지를 제공한다. 

<br>

### (3) webp에도 레이지 로딩 적용하기 

```javascript
const callback = (entries, observer) => {
    entries.forEach(entry => {
        if(entry.isIntersecting) {
            console.log(entry.target.dataset.src);
			
            //이전 태그 (soure태그를 잡는다)
            const sourceEl = entry.target.previousSibling;
            sourceEl.srcset = sourceEl.dataset.srcset;
            entry.target.src = entry.target.dataset.src;

            observer.unobserve(entry.target);
        }
    });
}
```



<br>

## 3) 동영상 최적화

`로딩 성능 최적화`

<br>

### (1) 동영상 압축

동영상도 가로와 세로의 사이즈가 있고 화질에 따라 용량이 달라진다.

단순하게, 동영상을 작은 용량을 압축해 주는 작업을 통해 최적화를 한다.

그런데, 동영상 화질이 저하되기 때문에 동영상이 메인 컨텐츠면 해당 방법을 추천하지 않는다. 

- `media.io` 에서 압축한다.  

- WEBM 확장자로 압축

<br>

### (2) 분기 처리

> WEBM을 지원하지 않는 브라우저가 존재하기 때문에 분기처리를 해야 한다.  
{: .prompt-tip }

```javascript
<video autoPlay loop muted>
    <source src={video_webm} type="video/webm" />
    <source src={video} type="video/mp4" />
</video>
```

<br>

### (3) 화질 팁

> 화질이 너무 저하되면 오히려 유저의 경험이 나빠질 수 있다.
{: .prompt-tip }

- 영상 길이를 줄이고 영상을 반복한다.
- 영상 위에 작은 도트를 반복해서 올린다. (동영상이 메인 컨텐츠가 아닐 때 )
- 영상 자체에 블러처리를 한다. (동영상이 메인 컨텐츠가 아닐 때 )

<br>

## 4) 폰트 최적화

`로딩 성능 최적화`

폰트를 로딩하느라, 텍스트가 나중에 화면에 보일 수 있다. (텍스트가 깜빡거린다)

폰트는 하나의 리소스로서 네트워크에 다운받는다.

- FOUT (Flash of Unstyled Text): 폰트를 다운로드 하기 전에는 기본 폰트로 텍스트를 보여준다.
  - IE, Edge 브라우저
- FOIT (Flash of Invisible Text): 폰트를 다운로드 하기 전에는 아예 텍스트를 보여주지 않는다. 
  - Chrome , 사파리 브라우저 

궁극적으로 웹폰트를 최적화 하여 위의 현상을 최소화 해야 한다. 

<br>

### (1) 웹 폰트 적용 시점 컨트롤

> font-display 속성을 활용하여 FOUT와 FOIT를 컨트롤 할 수 있다.
{: .prompt-tip }

| 속성       | 설명                                                         |
| ---------- | ------------------------------------------------------------ |
| `auto`     | 브라우저 기본 동작                                           |
| `block`    | FOIT (timeout = 3s)<br />텍스트를 보여주지 않다가 3초 이내에 다운로드가 안되면, 3초 후에 기본 폰트를 적용한다. |
| `swap`     | FOUT                                                         |
| `fallback` | FOIT (timeout = 0.1s)<br />텍스트를 보여주지 않다가 0.1초 이내에 다운로드가 안되면, 0.1초 후에 기본 폰트를 적용한다. 3초 후에도 불러오지 못했을 시, 기본 폰트로 유지한다. 다운로드된 폰트는 캐시해둔다. |
| `optional` | FOIT (timeout = 0.1s)<br />이후 네트워크 상태에 따라 기본 폰트로 유지 할지 웹폰트를 적용할지 결정한다. 폰트를 캐시해둔다.<br />**구글에서는 optional을 권장한다.** |

<br>

### (2) 페이드 효과

> 유저 체감 성능을 위해 fade효과를 적용할 수 있다.
>
> 폰트가 로드가 된 시점을 캐치해서 이벤트를 넣어준다.
{: .prompt-tip }

- 폰트가 다운로드 되기 전에는 opacity 0을 준다
- 폰트가 다운로드 되면 opacity 1을 준다.

폰트가 다운되었는지 파악하기 위해서는 [fontfaceobserver](https://www.npmjs.com/package/fontfaceobserver)를 사용하면 된다.

```bash
$ npm install fontfaceobserver
```

<br>

```react
import FontFaceObserver from 'fontfaceobserver'

function BannerVideo() {
  const [isFontLoaded, setIsFontLoaded] = useState(false);

  const font = new FontFaceObserver('BMYEONSUNG');

  // 폰트가 로드 되었는지 확인한다.
  useEffect(() => {
    font.load().then(function () {
      setIsFontLoaded(true);
    });
  }, []);

  return (
    <div style={{opacity: isFontLoaded ? 1 : 0, transition: 'opacity 0.3s ease'}}>
      <p>KEEP</p>
    </div>
  )
}

export default BannerVideo
```

<br>

### (2) 폰트 사이즈 줄이기

#### 2-1. 웹폰트 포멧 사용

![image-20221111111104979](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221111111104979.png)

- 파일 크기: EOT >  TTF/OTF  > WOFF > WOFF2 
- `transfonter.org`에서 폰트 포맷을 변환한다. 

<br>

지원하지 않는 브라우저를 생각해야 한다. 

```css
@font-face {
  font-family: BMYEONSUNG;
  src: url('./assets/fonts/BMYEONSUNG.woff2') format('woff2'),
      url('./assets/fonts/BMYEONSUNG.woff') format('woff'),
      url('./assets/fonts/BMYEONSUNG.ttf') format('truetype');
  font-display: block;
}
```

<br>

#### 2-2. local 폰트 사용

```css
@font-face {
  font-family: BMYEONSUNG;
  src: local('BMYEONSUNG') 
      url('./assets/fonts/BMYEONSUNG.woff2') format('woff2'),
      url('./assets/fonts/BMYEONSUNG.woff') format('woff'),
      url('./assets/fonts/BMYEONSUNG.ttf') format('truetype');
  font-display: block;
}
```

- local 키워드를 사용하면 이미 사용자가 PC에 존재하는 폰트를 체크해서 네트워크 없이 폰트를 사용할 수 있다.

<br>

#### 2-3. Subset 사용

> 사용하는 글자만 일부 가져온다.
{: .prompt-tip }

 `transfonter.org`사이트에서 특정 글자만 가져올 수 있다. `Characters`칼럼에 작성

- 폰트 파일 사이즈가 작아지기 때문에 폰트가 엄청 빠르게 로딩된다. 

<br>

#### 2-4. Unicode Range 적용

> unicode-range를 사용하여 특정 유니코드 값을 작성하면 지정된 유니코드에만 폰트를 적용한다는 의미이다. 
{: .prompt-tip }

```css
@font-face {
  font-family: BMYEONSUNG;
  src: local('BMYEONSUNG') 
      url('./assets/fonts/BMYEONSUNG.woff2') format('woff2'),
      url('./assets/fonts/BMYEONSUNG.woff') format('woff'),
      url('./assets/fonts/BMYEONSUNG.ttf') format('truetype');
  font-display: block;
    
  /* 'A'만 폰트를 적용한다.*/
  unicode-range: u+0041;
}
```

- 불필요한 폰트를 로딩하는 것을 막는다. 

<br>

#### 2-5. data-uri로 변환

> `transfonter.org`사이트에서 base64 토클을 키고 변환한다.
{: .prompt-tip }

- data-uri는 이미지 등의 외부 바이너리 파일을 웹페이지에 인라인으로 넣기 위해서 사용한다.

- 네트워크 호출이 아니기 때문에 용량이 커진 대신 별도로 폰트 파일을 가져오지는 않는다.

<br>

#### 2-6. 폰트 Preload

CSS보다 먼저 불리기 위해서는 HTML 파일에 알려줘야 한다. 

```html
<link rel="preload" href="./static/mdeia/BMYEONGSUNG.b184ad44.woff2" as="font" type="font/woff2" crossorigin>
```

- html이 로드하자마자 폰트가 어떤 리소스보다 먼저 load한다.  (css, js 보다)

이는 webpack을 수정해서 적용할 수도 있다.

```js
// config-overrides.js 

const PreloadWebpackPlugin = require('preload-webpack-plugin');

module.exports = function override(config, env) {
  config.plugins.push(new PreloadWebpackPlugin({
    rel: 'preload',
    as: 'font',
    include: 'allAssets',
    fileWhitelist: [/(.woff2?)/i]
  }));

  return config;
}
```

<br>

## 5) 캐시 최적화

`로딩 성능 최적화`

![image-20221111125901380](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221111125901380.png)

- 효율적인 캐시 정책이 적용되어 있지 않다.
- 리소스들의 캐시 설정을 적용해야 한다. 
- cache-control이라는 헤더를 통해 캐시 설정을 할 수 있다.

<br>

### (1) 캐시란

> 데이터나 값을 미리 복사해 놓는 임시 장소나 그런 동작이다.
{: .prompt-tip }

웹 브라우저는 두 가지 장법으로 캐싱을 한다.

- 메모리 캐시: RAM에 데이터를 저장해두는 방식
- disk: file로 데이터를 저장하고 file을 읽어서 캐시를 불러들인다.

<br>

![image-20221111130417918](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221111130417918.png) 

- 브라우저의 알고리즘에 따라서 알아서 적용된다.

<br>

### (2) cache-control

브라우저가 특정 리소스를 요청할 때 cache-control이 http 헤더에 추가하여 캐시 관련 설정을 할 수 있다. 즉, 서버에서 설정을 해주어야 한다. 

ex) 몇 초 동안 캐시를 유지해~

<br>

| 속성       | 설명                                                         |
| ---------- | ------------------------------------------------------------ |
| `no-cache` | 캐시를 사용하기 전에 서버에 검사 후 사용을 결정한다. <br />max-age=0과 유사하다. |
| `no-store` | 캐시를 사용 안한다.                                          |
| `public`   | 모든 환경에서 캐시 사용이 가능하다.                          |
| `private`  | 브라우저 환경에서만 캐시를 사용한다. 외부 캐시 서버에서는 사용 불가하다. |
| `max-age`  | 캐시의 유효시간 (sconds기준)<br />캐시가 만료되면 서버에 계속 사용해도 되는 지 물어본다. 서버의 응답에 따라 다시 요청할 지가 결정된다.<br />`304`: not modified (다시 요청하지 않고 계속 쓴다.) |

그렇다면 서버는 어떻게 리소스가 변경되었음을 알 수 있는 걸까?

- 리소스에는 ETag가 붙어있다. 해쉬와 유사하다.
- 브라우저가 리소스를 가지고 있을 때 `ETag`를 가지고 있다. 서버에게 이 이미지 사용해도 괜찮아? 라고 물어볼때 `ETag`를 보내준다. 서버는 `ETag`의 변화 상태를 감지하여 브라우저에게 다시 알려주어 브라우저가 리소스를 다시 요청할지 결정하게 된다.

<br>

![image-20221111142311127](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221111142311127.png)

<br>

### (3) node를 활용한 서버 설정

```js
//server.js

const express = require('express')
const app = express()
const port = 5000
const path = require('path')


const header = {
  setHeaders: (res, path) => {
     res.setHeader('Cache-Control', 'max-age=20')
    }
  },
}

app.use(express.static(path.join(__dirname, '../build'), header))
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, '../build/index.html'))
})

app.listen(port, () => console.log(`Example app listening at http://localhost:${port}`))
```

<br>

### (4) 리소스별 관리

>  캐시는 최신 데이터를 어떻게 즉각 반영할 지가 중요하다. 
>
> 그런데 절대 변경이 발생하지 않는 리소스가 있다면 캐시를 오랫동안 하는 것이 좋다. 
{: .prompt-tip }

- html 리소스: 즉각 변경을 위해 캐시가 걸려있지 않는 것이 좋다.
  - 수정사항을 바로바로 반영할 수 없다.  
  - `no-cahe` 
- js, css 리소스: 항상 최신으로 유지되어 있어야 한다. 
  - 그러나 webpack을 사용하여 번들링 할 때, js가 수정되면 최신의 js 리소스를 생성하기에 수정을 반영할 필요가 없다.   
    - 버전을 위해 해쉬를 설정하기 때문
  - 항상 최신의 html을 유지하기에 항상 최신의 js를 요청하게 된다.
  - `max-age=30d` 

```
html: no-cache
js: public, max-age: 3153600
css: public, max-age: 3153600
img: public, max-age: 3153600
```

<br>

```js
//server.js

const express = require('express')
const app = express()
const port = 5000
const path = require('path')


const header = {
  setHeaders: (res, path) => {
    if(path.endsWith('.html')) {
      res.setHeader('Cache-Control', 'no-cache')
    } else if(path.endsWith('.js') || path.endsWith('.css') || path.endsWith('.webp')) {
      res.setHeader('Cache-Control', 'public, max-age=31536000')
    } else {
      res.setHeader('Cache-Control', 'no-store')
    }
  },
}

app.use(express.static(path.join(__dirname, '../build'), header))
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, '../build/index.html'))
})

app.listen(port, () => console.log(`Example app listening at http://localhost:${port}`))
```



## 6) 불필요한 CSS 제거

`로딩 성능 최적화`

- Coverage 탭을 활용
- 해당 파일에서 실제로 사용하는 코드의 비율을 보여준다.
- css 파일 사이즈를 줄이는데 도움을 준다.

<br>

### (1) Purge.css

```bash
$npm i purgecss --save-dev
```

CSS에서 사용하지 않는 선택기를 제거하여 CSS 파일을 더 작게 만든다.

<br>

# 2. 이미지 갤러리 서비스 최적화

## 1) Layout Shife 피하기

`렌더링 성능 최적화`

![image-20221111160443569](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221111160443569.png)

<br>

![image-20221111160525519](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221111160525519.png)



**랜더링이 느려서, 요소들이 이동하는 현상**

- 화면상에서 위치를 다시 계산해야 한다. 
- 사용성에 큰 영향을 준다. 

<br>

### (1) 원인

- 사이즈가 정해져 있지 않은 이미지

- 사이즈가 정해져 있지 않은 광고
- 동적으로 삽입된 콘텐츠
- Web font (FOIT, FOUT)

<br>

### (2) 해결방안

사이즈가 정해져 있지 않아 문제가 발생하는 경우, 

- 사이즈를 px이나 rem와 같이 고정시키면 된다. 
- 반응형을 한다면, 가로 세로 비율을 고정함으로 사이즈를 정할 수 있다.

<br>

```react
import React from 'react';
import styled from 'styled-components';

function PhotoItem({ photo: { urls, alt } }) {
  return (
    <ImageWrap>
      <Image src={urls.small + '&t=' + new Date().getTime()} alt={alt}/>
    </ImageWrap>
  );
}

// padding을 활용하여 너비의 56.25만큼 높이를 준다.
const ImageWrap = styled.div`
  width: 100%;
  padding-bottom: 56.25%;
  position: relative;
`;

const Image = styled.img`
  cursor: pointer;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
`;

export default PhotoItem;
```
<br>

## 2) 이미지 지연(lazy) 로딩

`로딩 성능 최적화`

react-lazyload를 활용하여 좀 더 간편하게 이미지 레이지로딩을 할 수 있다.

```bash
$ npm install --save react-lazyload
```

<br>

```react
import styled from 'styled-components';
import LazyLoad from 'react-lazyload';


function PhotoItem({ photo: { urls, alt } }) {
  // LazyLoad는 스크롤 이벤트를 사용한다. 
  // offset은 얼마나 미리 로딩하겠는가이다. 1000px이전에 이미지를 미리 불러온다. 
  return (
    <ImageWrap>
      <LazyLoad offset={1000}>
        <Image src={urls.small + '&t=' + new Date().getTime()} alt={alt} onClick={openModal} />
      </LazyLoad>
    </ImageWrap>
  );
}

const ImageWrap = styled.div`
  width: 100%;
  padding-bottom: 56.25%;
  position: relative;
`;

const Image = styled.img`
  cursor: pointer;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
`;

export default PhotoItem;
```

<br>

## 3) useSelector 렌더링 문제 해결

`렌더링 성능 최적화`

리액트 렌더링 라이프 사이클은 성능에 많은 영향을 미친다.

불필요한 시점에 발생하는 렌더링이나 빈번하게 발생하는 렌더링은 성능에 많은 영향을 미친다.

- 서비스에 어떤 타이밍에 컴포넌트가 렌더링이 되는 지 확인하기위해 ReactDevTool을 사용하면 된다.
- 의도한 렌더링이 맞는지 확인해야 한다. 

<br>

### (1) useSelector 원리

![image-20221112135258119](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221112135258119.png)

- Redux Store에는 다양한 State가 존재한다. 컴포넌트는 State를 구독하고 있다. 
- 만약, State가 변경되어 Store도 변경된다면, 구독된 컴포넌트들에게 State가 변경되었음을 알린다.
- component는 변경되었다는 State와 이전 State와 비교하여 State가 변경되었으면 리렌더링을 한다. 
- 그런데 useSelector과 값을 비교하는 방식은 return값을 통해 비교한다. 
  - a,b의 값을 갖는 객체이다.
  - state가 변하면 객체를 새로 반환한다. 

그래서 State가 변하지 않아도 리렌더링 되는 상황이 발생하는 것이다. 

<br>

`해결 방안`

1. Object를 새로 만들지 않도록 State 쪼개기
2. 새로운 Equality Function 사용

<br>

### (2) Object를 새로 만들지 않도록 State 쪼개기 

```react
import React from 'react';
import { useSelector } from 'react-redux';
import ImageModal from '../components/ImageModal';

// 이전 코드
function ImageModalContainer() {
  const { modalVisible, bgColor, src, alt } = useSelector(
    state => ({
      modalVisible: state.imageModal.modalVisible,
      bgColor: state.imageModal.bgColor,
      src: state.imageModal.src,
      alt: state.imageModal.alt,
    }),
  );

  return (
    <ImageModal
      modalVisible={modalVisible}
      bgColor={bgColor}
      src={src}
      alt={alt}
    />
  );
}

export default ImageModalContainer;
```

위와 같이 object로 반환하는 것이 아니라 단일 값으로 만든다. 

<br>

```react
import React from 'react';
import { useSelector } from 'react-redux';
import ImageModal from '../components/ImageModal';

// state를 분리한다. 
const modlaVisible = useSelector(state => state.imageModal.modalVisible);
const bgColor = useSelector(state => state.imageModal.bgColor);
const src = useSelector(state => state.imageModal.src);
const alt = useSelector(state => state.imageModal.alt);
  return (
    <ImageModal
      modalVisible={modalVisible}
      bgColor={bgColor}
      src={src}
      alt={alt}
    />
  );
}

export default ImageModalContainer;
```

<br>

#### (3) 새로운 Equality Function 사용

```react
import React from 'react';
import { useSelector, shallowEqual } from 'react-redux';
import ImageModal from '../components/ImageModal';

// 새로운 값을 비교하는 equality 함수를 useSelector 인자에 추가한다. 
// 기본적으로는 단순 비교를 한다. 
function ImageModalContainer() {
  const { modalVisible, bgColor, src, alt } = useSelector(
    state => ({
      modalVisible: state.imageModal.modalVisible,
      bgColor: state.imageModal.bgColor,
      src: state.imageModal.src,
      alt: state.imageModal.alt,
    }),shallowEqual);

  return (
    <ImageModal
      modalVisible={modalVisible}
      bgColor={bgColor}
      src={src}
      alt={alt}
    />
  );
}

export default ImageModalContainer;
```

- shallowEqual은 단순 비교가 아닌 첫번째 depth에 있는 값들을 각각 비교한다. 

<br>

## 4) Redux Reselect를 통한 렌더링 최적화

`렌더링 성능 최적화`

`reselect`는 리더스와 같이 사용하는 라이브러리로 state에 있는 값을 가공해야 하는 경우 가공된 값을 state에서 관리할 수 있도록 한다. 

```react
import React, { useEffect } from 'react';
import { shallowEqual, useDispatch, useSelector } from 'react-redux';
import PhotoList from '../components/PhotoList';
import { fetchPhotos } from '../redux/photos';

function PhotoListContainer() {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(fetchPhotos());
  }, [dispatch]);

  const { category, allPhotos, loading } = useSelector(
    state => ({
      category: state.category.category,
      allPhotos: state.photos.data,
      loading: state.photos.loading,
    }),
    shallowEqual
  );

  // photos를 store에서 관리할 수 있도록 한다!
  const photos =
    category === 'all'
      ? allPhotos
      : allPhotos.filter(photo => photo.category === category);

  if (loading === 'error') {
    return <span>Error!</span>;
  }

  if (loading !== 'done') {
    return <span>loading...</span>;
  }

  return <PhotoList photos={photos} />;
}

export default PhotoListContainer;
```

<br>

```bash
$ npm install reselect
```

<br>

### (1) createSelector

>  store에서 데이터를 꺼내오기위한 함수를 selector라고 한다.
>
> 우리는 reselctor 라이브러리로 selector를 생성할 수 있다.
{: .prompt-tip }

<br>

```js
// 예시
createSelector([select할 값], 값에 적용할 함수)

const selectFilteredPhotos = createSelector ([state => state.photos.data], photos => photos)
const photos = useSelector(selectFilteredPhotos);
```

<br>

`적용해보기`

```react
// src/redux/photos
// createSelector로 로직을 연결하여 값을 가져오기
import { createSelector } from 'reselect';

export default createSelector(
  [state => state.photos.data, state => state.category.category],
  (photos, category) =>
    category === 'all'
      ? photos
      : photos.filter(photo => photo.category === category)
);
```

- memoization 기법에 의해 reselect는 함수에 똑같은 인자가 들어오면 반환 값을 이미 캐시된 값으로 전달한다. 
- 매번 값을 계산하지 않는다.

<br>

```react
import React, { useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import PhotoList from '../components/PhotoList';
import { fetchPhotos } from '../redux/photos';
import selectFilteredPhotos from '../redux/selector/selectFilteredPhotos';

function PhotoListContainer() {
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(fetchPhotos());
  }, [dispatch]);

  const photos = useSelector(selectFilteredPhotos);
  const loading = useSelector(state => state.photos.loading);

  if (loading === 'error') {
    return <span>Error!</span>;
  }

  if (loading !== 'done') {
    return <span>loading...</span>;
  }

  return <PhotoList photos={photos} />;
}

export default PhotoListContainer;
```

<br>

## 5) 병목 함수에 memoization 적용

`렌더링 성능 최적화`

함수에는 인풋과 아웃풋이 있다. memoization은 특정 input에 따른 output의 값을 미리 저장해 놓는 것을 의미한다.

즉, 매번 함수를 실행시키지 않는다. 

들어오는 input이 매번 같거나 함수가 헤비한 경우 매우 효율적으로 함수를 구현할 수 있다. 

<br>

### (1) 예제

- input값을 알아야한다.
- 만약 동일한 input값이 왔다면, 캐시한 데이터를 반환한다. 
- 순수함수여야 memoization을 적용할 수 있다. (동일한 input에 대해서 동일한 output을 보장하는 함수이다.)

```js
// 캐시할 데이터 
const cache = {};

export function getAverageColorOfImage(imgElement) {
    
  // input값 체크하여 만약 동일한 input이면 저장된 값을 반환한다.
  if (cache.hasOwnProperty(imgElement.src)) {
    return cache[imgElement.src];
  }

  const canvas = document.createElement('canvas');
  const context = canvas.getContext && canvas.getContext('2d');
  const averageColor = {
    r: 0,
    g: 0,
    b: 0,
  };

  if (!context) {
    return averageColor;
  }

  const width = (canvas.width =
    imgElement.naturalWidth || imgElement.offsetWidth || imgElement.width);
  const height = (canvas.height =
    imgElement.naturalHeight || imgElement.offsetHeight || imgElement.height);

  context.drawImage(imgElement, 0, 0);

  const imageData = context.getImageData(0, 0, width, height).data;
  const length = imageData.length;

  for (let i = 0; i < length; i += 4) {
    averageColor.r += imageData[i];
    averageColor.g += imageData[i + 1];
    averageColor.b += imageData[i + 2];
  }

  const count = length / 4;
  averageColor.r = ~~(averageColor.r / count); // ~~ => convert to int
  averageColor.g = ~~(averageColor.g / count);
  averageColor.b = ~~(averageColor.b / count);

  // 저장!
  cache[imgElement.src] = averageColor;

  return averageColor;
}
```

<br>

### (2) memoization 함수 만들기

```js
// memoize(func)(arg) === func(arg)
function memoize(fn) {
    const cache={};
    
    return function(...args) {
        if (args.length !== 1) {
            return fn(...args);
        };
        
        if (cache.hasOwnProperty(args)){
            return cache[args];
        };
        
        const result = fn(...args);
        cache[args] = result;
        
        return result;
    }; 
};

```

- 이제 메모이제이션 함수로만 래핑을 하면 캐시를 사용할 수 있다. 
- 팩토리 패턴 활용 
- 메모이제이션은 메모리가 많이든다. input값이 매번 다른 경우에는 사용하지 않는 것을 추천한다.

<br>

## 6) 병목 함수 로직 개선하기

`렌더링 성능 최적화`

함수의 로직 자체를 최적화 해도 성능이 개선된다.

<br>

![image-20221112150926998](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221112150926998.png)

- 퍼포먼스 탭을 통한 함수 성능을 분석한 뒤, 어디서 정체가 되는지 파악하고 개선한다.

<br>

### (1) drawImage, getImageData  개선

직접 함수의 로직을 개선할 수 없는데 어떻게 성능 최적화를 할 수 있을까.

- 캔버스에 이미지 데이터를 그리는 작업이기 때문에 이미지 사이즈와 캔버스의 크기가 클 수록 그리는 속도는 느려질 것이다. 
- 이미지를 사이즈를 작게 해서 가져오거나, 캔버스 사이즈를 줄이면 성능이 개선 될 수 있을 것이다.

<br>

```js
export function getAverageColorOfImage(imgElement) {
  // .. 생략
   
  // 캔버스 사이즈 줄이기
  canvas.width = width / 3;
  canvas.height = height / 3;

  context.drawImage(imgElement, 0, 0, canvas.width, canvas.height);

  const imageData = context.getImageData(
    0,
    0,
    canvas.width,
    canvas.height
  ).data;
    
  //.. 생략 
}
```

<br>

### (2) 반복문 개선

- 모든 픽셀을 조사할 필요가없다. 띄엄띄엄 조사하자. 

```js
export function getAverageColorOfImage(imgElement) {
  // .. 생략
   
  // 이전에는 rgba에서 a를 제외하기 위해 i += 4 였음.
  // 10개 픽셀중 한개를 조사하기 위해 40으로 수정.
  for (let i = 0; i < length; i += 40) {
    averageColor.r += imageData[i];
    averageColor.g += imageData[i + 1];
    averageColor.b += imageData[i + 2];
  }

  const count = length / 40;
  averageColor.r = ~~(averageColor.r / count);
  averageColor.g = ~~(averageColor.g / count);
  averageColor.b = ~~(averageColor.b / count);
    
  //.. 생략 
}
```

<br>

