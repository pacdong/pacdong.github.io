---
title: "리액트 라우터"
excerpt: "Instagram Clone Coding - Nomard Coder"

categories:
  - 인스타그램 클론코딩
tags:
  - Instagram
  - Clone
  - Coding
  - React
  - React-Native
last_modified_at: 2020-01-09T08:10:30-11:00
---
노마드 코더의 인스타 클론 코딩 두번째 프론트엔드 셋업 세번째 시간입니다. <br>
리액트 라우터의 개념을 저와 같이 초보자분들이 이해하기 쉽게 설명하였습니다.

## React-Router

SPA(Single Page Application)에서 리액트 라이브러리를 사용하여 뷰 렌더링을 합니다. 어플리케이션을 브라우저에서 요청한 뒤 필요한 데이터를 전달받아 페이지에 출력할 때 Path(경로)에 따라 다른 페이지를 보여주는 것을 라우팅이라고 합니다. <br>

여기서 react-router는 Client-Side에서 이루어지는 라우팅을 간단하게 만들게 해주는 라이브러리로써 가장 많이 쓰이는 라우터 라이브러리 중 하나입니다.

## 폴더 구성하기

어떤 프로젝트를 진행할 때 항상 폴더부터 구조화 시키는 것이 가장 좋다고 앞선 포스트에서 설명을 드렸는데요. 폴더 구조를 명확하게 나누면 코드 리뷰할 때 리뷰어 입장에서 더 확실히 와 닿는 것이 있기에 노마드 코더와 같은 경험 많은 개발자들의 폴더 구성을 참조하시는 것을 강추드립니다.

'src/Component/Router'로 구성하시어 작업하셔도 되고 'src/Router'로 폴더를 만드신 후 작업하셔도 됩니다. 또한 'src/Componet/screen.js' 등으로 작업후 진행하셔도 무방하지만 노마드 코더는 구분되어 있는 것이 더 가시성이 좋다고 합니다. 아래는 폴더 구조 이미지입니다.

<!-- ![라우터폴더구성](/assets/images/instagramClone/routerFolder.png){: width="50%"} -->
<center><img src="/assets/images/instagramClone/routerFolder.png" width="50%" alt="라우터폴더구성"></center>

## 라우터 사용하기

개념만 알면 사용법은 생각보다 직관적이어서 쉽게 구성할 수 있습니다.  
`src/Componet/Router/Router.js`

```javascript
import React from "react";
import PropTypes from "prop-types";
import { HashRouter as Router, Route, Switch } from "react-router-dom";
import Auth from "../Routes/Auth";
import Feed from "../Routes/Feed";

const LoggedIn = () => (
  <><Route exact path="/" component={Feed} /></>
);

const LoggedOut = () => (
  <><Route exact path="/" component={Auth} /></>
);

const AppRouter = ({ isLoggedIn }) => (
  <Router>
    <Switch> {isLoggedIn ? <LoggedIn /> : <LoggedOut />} </Switch>
  </Router>
);

AppRouter.propTypes = {
  isLoggedIn: PropTypes.bool.isRequired
};

export default AppRouter;
```

그 다음엔, App.js에서 Router를 import하여 사용하기만 하면 됩니다. `Route excat ...`에서 exact는 주어진 경로와 정확하게 맞아 떨어졌을 때 설정한 컴포넌트를 보여주도록 하기위한 옵션입니다.

`src/Componet/App.js`

```javascript
import React from "react";
import { ThemeProvider } from "styled-components";
import GlobalStyles from "../Styles/GlobalStyles";
import Theme from "../Styles/Theme";
import Router from "./Router";

export default () => (
  <ThemeProvider theme={Theme}>
    <>
      <GlobalStyles />
      <Router isLoggedIn={false} />
    </>
  </ThemeProvider>
);
```

이렇게 Router를 설정하시면 정상적으로 출력이 되는 것을 보실 수 있습니다.  
P.S 노마드 코더 수업에서 나오는 코드와는 일부분이 다를 수 있습니다.

### 참고자료

- [academy.nomadcoders.InstagramClonCoding](https://academy.nomadcoders.co/courses/enrolled/503371)
