---
title: "Chapter.14 to 17 React Native Home Screen CSS"
excerpt: "ExpoApp - Navigation / Home / Search / Post / Profile && etc"

categories:
  - 인스타그램 클론코딩
tags:
  - Instagram
  - Clone
  - Coding
  - React
  - React-Native
last_modified_at: 2020-02-10T08:18:00-19:30
---

여러 스크린을 구성하며 CSS를 많이 공부하게 되었습니다. 여기서는 간략히 느낀 내용을 적고자 합니다.


   <br>
- CSS / Screen Tip <br>
- BackEnd에 Token 전달하기 <br>
-  <br>


## CSS / Screen Tip

Expo를 이용하여 앱에 들어가는 Screen을 구성할 때 노마드코더가 애용하는 간략한 팁을 정리하려고 합니다. 앱은 기본적으로 모든 디바이스에 대해 반응형으로 버튼이나 이미지를 구성해야 합니다. 일반적인 CSS로는 width와 height를 지정하여 원하는 디자인을 연출하지만 앱에서는 costants.js를 만들어서 사용하는 것이 편리합니다.   

`constants.js`
```javascript
import { Dimensions } from "react-native";

const { width, height } = Dimensions.get("screen");

export default { width, height };

```   

이렇게 Dimensions를 이용하여 screen의 width, height를 구하여 놓습니다. 그리고 나서 스타일을 입힐 때에는 아래와 같이 적용합니다.
```javascript
import styles from "../../styles.js";
import constants from "../../constants.js";
import styled from "styled-components";

const PostCard = styled.View`
  background-color: white;
  box-shadow: 3px 3px 3px ${styles.darkGreyColor};
  width: ${constants.width / 1.5};
  height: ${constants.height / 3};
  margin-bottom: 28px;
  justify-content: center;
`;
```
위와 같이 styles 폴더를 통해 자주 사용하는 색을 정해놓고 styled-components를 이용하여 함께 사용하는 방법과 CSS를 사용할 때 자동으로 계산되는 width와 height를 가지고 스타일을 지정해주면 반응형 CSS 디자인이 되도록하는 아주 **간편한 TIP**입니다.   


## BackEnd에 Token 전달하기

이 부분은 `apolloClinet`의  `request`함수를 이용하여 전달합니다. 항상 [공식문서](https://www.apollographql.com/docs/react/networking/authentication/#header)를 먼저 읽어본 이후 사용하시는 것을 추천드립니다.   

```javascript
import ApolloClient from 'apollo-boost'

const client = new ApolloClient({
  request: (operation) => {
    const token = localStorage.getItem('token')
    operation.setContext({
      headers: {
        authorization: token ? `Bearer ${token}` : ''
      }
    })
  }
})
```
공식문서에 있는 사용법입니다. 이것을 Expo에 적용 시켜보겠습니다.

```javascript
import ApolloClient from 'apollo-boost'

const client = new ApolloClient({
  cache,
  request: async operation => {
    const token = await AsyncStorage.getItem("jwt");
    return operation.setContext({
      headers: { Authorization: `Bearer ${token}` }
    });
  },
  ...apolloClientOptions
});
```
이렇게 구성이 되는 이유는 먼저, request는 opration을 인자로 같습니다. Token을 웹상에서는 LocalStorage에서 꺼내고 App에서는 AsyncStorage에서 꺼내어 사용합니다. AsyncStorage.getItem을 이용하여 jwt Token을 추출한 뒤 opration의 setContext를 이용하여 token을 헤더에 붙혀 전달합니다.   


### 참고자료

- [academy.nomadcoders.InstagramClonCoding](https://academy.nomadcoders.co/courses/enrolled/503371)


