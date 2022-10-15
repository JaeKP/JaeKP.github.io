---
title: 실전 웹 성능 최적화(feat. React) Part1
author: 박재경
date: 2022-10-15
categories: [WEB, Performance]
tag: [perfomance]
---

[강의 링크](https://www.inflearn.com/course/%EC%9B%B9-%EC%84%B1%EB%8A%A5-%EC%B5%9C%EC%A0%81%ED%99%94-%EB%A6%AC%EC%95%A1%ED%8A%B8-1/dashboard)

웹 성능은 크게 두 가지로 분류할 수 있다. 

바로, 로딩 성능과 렌더링 성능이다.

- 로딩 성능: 각 리소스를 불러오는 성능을 의미한다. 
- 렌더링 성능: 불러온 리소스를 화면에 보여주는 성능을 의미한다.

<br>

# 1. 블로그 사이트 최적화

총 4 가지의 분석 툴을 사용하여 사이트의 최적화를 진행한다. 

- 크롬 Network 탭
- 크롬 Performance 탭
- 크롬 Light house 탭
- webpack-bundle-analyzer





![image-20221015144447270](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015144447270.png)

- 추천: 로딩 성능 최적화와 관련된 내용
- 진단: 렌더링 성능 최적화와 관련된 내용

<br>

## 1) 이미즈 사이즈 최적화 

`로딩 성능 최적화`

<br>

![image-20221015144801614](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015144801614.png)



최저화가 필요한 이미지의 리스트를 볼 수 있다. 

- 실제로 사용되는 이미지 사이즈보다 과도하게 큰 사이즈의 이미지를 불러오면 안 된다.
- ex) 120 x 120 이미지일 경우, 2배 정도 더 큰 이미지를 사용하는 것이 좋다. 

<br>

```react
/* 파라미터 참고: https://unsplash.com/documentation#supported-parameters */
function getParametersForUnsplash({ width, height, quality, format }) {
  return `?w=${width}&h=${height}&q=${quality}&fm=${format}&fit=crop`;
}

// width 와 height를 240으로 제한한 것 만으로 성능이 개선된다. 
<img
    src={props.image + getParametersForUnsplash({ width: 240, height: 240, quality: 80, format: "jpg" })}
    alt="thumbnail"
/>
```

<br>

만약, 이미지를 api를 통해 가져오는 경우 어떻게 해야 하는가.

Image CDN를 사용하여 해결한다. 이미지에 전처리를 하여 사용자에게 제공한다. 

일반적으로 CDN은 물리적 거리의 한계를 극복하기 위해 유저와 가까운 곳에 콘텐츠 서버를 두는 것을 의미하지만, Image CDN은 이미지를 전처리하여 사용자에게 제공하는 것을 의미한다. 실제로 브런치에서도 이미지 CDN을 활용하고 있다. 

ex) `http://cdn.image.com?src=[img.cdn]&width=200&height=100`

<br>

## 2) Code Split

`로딩 성능 최적화`

자바스크립트 파일이 다운로드가 된 이후부터 JS를 평가하고 코드가 실행되기 때문에, 다운로드가 걸리는 시간만큼 화면이 뜨는 시간도 오래 걸리게 된다. 

번들 analyzer를 통해 번들 파일을 분석하여 볼 수 있다. 

- https://www.npmjs.com/package/webpack-bundle-analyzer

- CRA를 사용하는 경우: https://www.npmjs.com/package/cra-bundle-analyzer

<br>

![image-20221015161646962](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015161646962.png)

- Chunk.js > node_modules 이기 때문에, 우리가 설치한 모듈들이 chunk.js를 차지하는 것을 알 수 있다. 
- 그 중에서도  refractor가 많은 비중을 차지하고 있다.
  - package-lock.json에서는 설치한 모듈들의 하위 디펜던시를 확인할 수 있는데, refractor라는 라이브러리를 확인할 수 있다.
  - 그러나, 해당 모듈은 특정 페이지에서만 사용하기 때문에 필요할 때만 로드하면 성능을 개선할 수 있다. 

<br>

페이지 별로 코드를 분할하거나 모듈 별로 코드를 분할할 수 있다. 

여기서 중요한 것은, 불필요한 코드 또는 중복되는 코드가 없이 적절한 사이즈의 코드가 적절한 타이밍에 로드될 수 있도록 하는 것이다.

[리액트 공식 문서](https://ko.reactjs.org/docs/code-splitting.html) / [webpack 공식 문서](https://webpack.js.org/guides/code-splitting/#root)

리액트 공식 문서의 Route-based Code Splitting을 참고할 수 있다. 

<br>

```javascript
//* lazyloading과 Suspense를 활용하여 성능을 개선한다. 

import React, { Suspense, lazy } from "react";
import { Switch, Route } from "react-router-dom";

const ListPage = lazy(() => import("./pages/ListPage/index"));
const ViewPage = lazy(() => import("./pages/ViewPage/index"));

function App() {
  return (
    <div className="App">
      <Suspense fallback={<div>로딩중</div>}>
        <Switch>
          <Route path="/" component={ListPage} exact />
          <Route path="/view/:id" component={ViewPage} exact />
        </Switch>
      </Suspense>
    </div>
  );
}

export default App;

```

<br>

![image-20221015163502115](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015163502115.png)

위의 이미지를 보면, react-dom과 refactor가 분리된 것을 확인할 수 있다. 

<br>

## 3) 텍스트 압축

`로딩 성능 최적화`

![image-20221015163924160](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015163924160.png)

웹 페이지를 로드할 때는 html, js, css와 같은 텍스트들로 이루어진 리소스를 다운 받는다. 해당 문서의 사이즈를 줄이면 당연히, 로딩 성능이 개선된다. 

텍스트 압축이란 말 그대로 서버에서 보내는 리소스를 압축해서 보낸다. 네트워크 탭을 통해 압축을 하고 있는지 확인 할 수 있다. 

<br>

![image-20221015164135386](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015164135386.png)

텍스트 압축은 크게 두 가지 방법이 있다.

- GZIP: Deflate보다 좋은 압축률을 갖고 있다. 
- Deflate

그래서 압축이 되어 있지 않는 파일들을 압축하여 성능을 개선할 수 있다. 그런데 압축을 푸는 데도 시간이 걸리기 때문에, 무조건 압축을 하는 것은 좋지 않다.

**파일 크기가 2KB이상이 되면 인코딩을 하는 것이 좋다.**

<br>

## 4) Bottleneck 코드 최적화

`렌더링 성능 최적화`

<br>

![image-20221015154241800](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015154241800.png)

Perfomance 탭을 보면 Article이 굉장히 오랫동안 렌더링 되는 것을 알 수 있다. 

이는 아래에 있는 `removeSpecialCharacter`함수 때문으로 보이기 때문에 해당 로직을 수정하면 해결된다. 

<br>

```js
/*
 * 파라미터로 넘어온 문자열에서 일부 특수문자를 제거하는 함수
 * (Markdown으로 된 문자열의 특수문자를 제거하기 위함)
 * */

function removeSpecialCharacter(str) {
  const removeCharacters = ["#", "_", "*", "~", "&", ";", "!", "[", "]", "`", ">", "\n", "=", "-"];
  let _str = str;
  let i = 0,
    j = 0;

  for (i = 0; i < removeCharacters.length; i++) {
    j = 0;
    while (j < _str.length) {
      if (_str[j] === removeCharacters[i]) {
        _str = _str.substring(0, j).concat(_str.substring(j + 1));
        continue;
      }
      j++;
    }
  }

  return _str;
}

// 위의 로직은 replace와 정규식을 활용하여 더 효율적으로 개선할 수 있다. 
// sbustring을 활용하여 로직에 적용하는 문자열을 2000자로 제한한다.
function removeSpecialCharacter(str) {
  let _str = str.substring(0, 2000)
   _str = _str.replace(/[\#\_\*\~\&\;\!\[\]\`\>\/n\=\-]/g, "");
    
  return _str;
}
```

- 로직을 효율적으로 작성하고 작업하는 양을 줄여 성능을 개선했다. 

<br>

# 2. 통계사이트 최적화

## 1) 애니메이션 최적화 (Reflow, Repaint)

`렌더링 성능 최적화`

애니메이션은 여러 장의 이미지가 반복적으로 바뀌면서 움직이는 것처럼 보이는 트릭이다. 그런데, 중간의 프레임이 누실되면 뭔가 중간에 뚝 끊기는 느낌이 든다. 

- Display: 일반적으로 초당 60Frame이다. 일초에 60개의 화면을 보여준다.
- 브라우저도 초당 60Frame으로 화면을 랜더링 하려고 한다.
  - 그러나, 브라우저가 초당 60Frame의 화면을 그리지 못하기 때문에 애니메이션이 버벅이게 보이는 것이다.
  - `쟁크 현상`: 애니메이션이 버벅이는 현상

그렇다면, 왜 브라우저가 60Frame으로 그리지 못하는 가.

<br>

### (1) 브라우저 렌더링 과정

`Critical Rendering Path`, `Pixel Pipeline`

![image-20221015172553186](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015172553186.png)



브라우저는 기본적으로 위의 과정을 거쳐서 화면을 그리게 된다.

- HTML, CSS, JS와 같은 기본적인 파일을 리소스로 받는다. 
- HTML은 DOMTree, CSS 는 CSSOMTree의 데이터 타입으로 변환한다. 
  - DOM: 요소들간의 관계를 트리구조로 만든다.
  - CSSOM: 각 요소의 스타일을 트리 구조로 만든다.
- DOM과 CSSOM을 조합하여 RenderTree를 만든다. 
  - 요소에 대한 컨텐츠와 스타일을 합친다.
- Layout: 위치, 크기를 계산한다.
  - 어느 위치에 해당 요소가 어느 사이즈로 위치해야 할 지 계산한다.
- Paint: 색을 채워 준다.
- Composite: 각 레이어를 합성한다. 
  - 브라우저가 화면을 그릴 때, Layouy과 Paint는 레이어로 쪼개져서 진행이 된다.
  - 이렇게 여러 개로 쪼개진 레이어를 하나로 합쳐서 최종적인 화면을 그리게 된다.

<br>

만약, 이렇게 완성된 화면에서 일부 스타일이 변경된다면 변화된 내용을 가지고 다시 DOM, CSSOM을 만들고 일련의 과정을 진행하게 된다.

![image-20221015173511683](../AppData/Roaming/Typora/typora-user-images/image-20221015173511683.png)

그래서 애니메이션 관점에서는 초당 60Frame (0.016초)안에 화면을 재빠르게 보여주어야 하는데, 이런 짧은 시간에 위의 과정을 진행하다보니 일부 Frame이 유실 되는 것이다. 

<br>

### (2) Reflow와 Repaint

그래서 우리는 브라우저가 맡은 일을 줄여 성능을 개선할 수 있다. 

- 스타일의 width, height가 변경되면 `Pixel Pipeline`을 다시 실행(Reflow)

- 스타일의 색상이 변경되면 Layout을 생략한 `pixel Pipeline`을 다시 실행(Repaint)

<br>

그런데, GPU의 도움을 받아 Reflow와 Repaint를 피할 수 있다. 

- transform, opacity를 변경 하면, Layout과 Paint가 생략된 `Pixel Pipeline`을 다시 실행한다. 

- GPU가 직접 관여하여 바로 Compositie단계로 넘겨준다.

![image-20221015174050019](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015174050019.png)



<br>

### (3) transform을 사용하여 성능 개선

```javascript
const BarGraph = styled.div`
    position: absolute;
    left: 0;
    top: 0;
    width: ${({width}) => width}%;
    transition: width 1.5s ease;
    height: 100%;
    background: ${({isSelected}) => isSelected ? 'rgba(126, 198, 81, 0.7)' : 'rgb(198, 198, 198)'};
    z-index: 1;
`

// transform을 사용하여 성능을 개선
const BarGraph = styled.div`
  /*.. 생략 */
  transform: scaleX(${({ width }) => width / 100});
  transform-origin: left;
  transition: transform 1.5s ease;
`;

```

<br>

![image-20221015175015693](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015175015693.png)

- width로 애니메이션에서는 Frame드롭이 발생하는 것을 확인할 수 있다. 
- 또한, 보라색 영역 (메인 스레드)에서 주로 처리한다. 

<br>

## 2. 컴포넌트 Lazy Loading(Code Splitting)

`로딩 성능 최적화`

![image-20221015175331493](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015175331493.png)

- Gallery 모듈은 모달이 보일때만 사용하는 모듈이기 때문에 처음에 로드할 필요가 없다. 

<br>

ImageModal을 레이지 로드하면, 다음과 같이 분리된다. 

![image-20221015175910293](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20221015175910293.png)

- 로딩속도나 자바스크립트 평가 속도가 빨라져서 더 빠르게 화면을 render할 수 있다.

<br>

## 3. 컴포넌트 Preloading

`로딩 성능 최적화`

위에서 모달을 레이지 로딩 하였기 때문에, 모달을 열기 위해 클릭하면 그때 Modal과 관련된 모듈을 받게 되고 이를 평가 한 뒤 모달을 렌더하게 된다.

즉, 최초 페이지에서는 성능이 좋아졌지만 모달에서는 성능이 나빠졌다. 

그래서 Preloading을 통해 버튼을 클릭하여 모달을 열기 전에 미리 모달과 관련된 코드를 load를 한다.

문제는 사용자가 버튼을 언제 클릭할 지 모르기 때문에 언제 미리 로딩할 지 모른다. 그래서 컴포넌트를 Preloading하는 타이밍은 총 두 가지로 나눌 수 있다.

- 버튼 위에 마우스를 올려 놨을 때
- 최초 페이지가 로드되고, 모든 컴포넌트의 마운트가 끝났을 때

<br>

### (1) 버튼 위에 마우스를 올려 놨을 때

```javascript
const LazyImageModal = lazy(() => import("./components/ImageModal"));
 
const handleMouseEnter = () => {
    const component = import("./components/ImageModal");
  };
```

<br>

### (2) 모든 컴포넌트가 로드가 완료 된 후, 모듈을 미리 로드한다.

```javascript
const LazyImageModal = lazy(() => import("./components/ImageModal"));

useEffect(() => import("./components/ImageModal"), []);


// 만약 여러개의 컴포넌트를 import한다면, 
function lazyWithPreload(importFunction) {
  const Component = React.lazy(importFunction);
  Component.preload = importFunction;
  return Component;
}

const LazyImageModal = lazyWithPreload(() => import("./components/ImageModal"));

  useEffect(() => {
    LazyImageModal.preload();
  }, []);

```

<br>

## 4. 이미지 Preloading

`로딩 성능 최적화`

네트워크에서 `캐시 사용 중지`를 통해 캐시를 리셋하면, 모달 안에 있는 이미지 로딩이 느린 것을 확인할 수 있다.

이미지는 이미지를 화면에 노출하는 시점이 아니면, 로드 하지 않는다.

대신 JS의 `image 객체`를 사용하면 미리 로딩할 수 있다. 

<br>

```javascript
// 이미지를 미리 불러와서 캐시해 두고 사용한다. 
// 그래서 이미지가 캐시가 잘 되고 있는지 확인해야 한다. 그렇지 않으면 다시 또 요청한다. 

useEffect(() => {
    LazyImageModal.preload();
    const img = New Image();
    img.src = 이미지 링크 
}, []);
```

<br>