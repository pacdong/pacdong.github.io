---
title: "Chapter12 React Native Navigation 추가 설명."
excerpt: "ExpoApp - Navigation / createBottomTabNavigator, createStackNavigator"

categories:
  - 인스타그램 클론코딩
tags:
  - Instagram
  - Clone
  - Coding
  - React
  - React-Native
last_modified_at: 2020-01-29T08:19:30-20:30
---

**React Native** Expo의 활용성이 더욱 커지고 있습니다. 이전 Expo에서는 외부의 API를 쓸 수 없어서 효용 가치가 떨어졌지만 여러 라이브러리를 종합하여 쓰는 현 생태계에서 버전에 맞도록 버그없이 설치한다는 것은 어렵고 불편한 일입니다. 그렇지만 `Expo insall (API)`와 같은 명령어를 통해 `yan add (API)`를 쓰는 것과 같이 설치하면 버전을 Expo에서 맞추어 설치하여 주기 때문에 Warning 없이 설치되는 것은 아직 React-Native 초보인 저는 더 편하였습니다.    

다만, 외부 API를 쓸 수 없다는 것은 단점으로 느껴졌으나 Expo를 Reject하지 않고 Expo-Kit를 이용하면 외부 API도 쓸 수 있도록 발전하고 있는 것에 대해서는 점점 더 코딩이 좋아지는 계기가 되고 있고 앞선 글에서 말씀드렸듯이 Expo로, React-Native를 이용하여 코드를 짜면 iOS / Android / Web 모두 쓸 수 있는 것은 정말 편한 일이라 생각합니다. 또한, React-Native의 stack navigator와 React Navigation의 stck navigator의 큰 차이점 중 하나로는 Stack의 경로 사이를 탐색할 때 Android, IOS에서 예상되는 제스처 및 애니메이션을 제공한다는 점에 있습니다.

이제 다시 Login Part로 넘어가서 하나씩 살펴보도록 하겠습니다. 이번시간에 정리할 내용은 Navigation의 추가 설명으로 아래와 같습니다.
   <br>
- createBottomTabNavigator <br>
- Navigation Icon <br>
- Navigation Header  <br>
- Navigation Back title  <br>
- createStackNavigator <br>


--- 

## Navigation Tab Icon

`createBottomTabNavigator`인 [BattomTabNavigator](https://reactnavigation.org/docs/en/bottom-tab-navigator.html)에서 여러 옵션들을 통해서 아이콘과 텍스트를 보여주게 할 수도 변경할 수도 있습니다. 또한, `tabBarIcon`이라는 옵션을 통해 포커스 되었을 때 사용자가 해당 탭에 있다는 효과를 주기 위하여 색을 바꿀 수도 있습니다. 

> **tabBarIcon**
> Function that given { focused: boolean, color: string, size: number } returns a React.Node, to display in the tab bar.

여기서 focused 값을 이용하여 현재 선택된 tab의 value로써 boolean 값을 받았을 때 선택된 Tab의 색상을 지정할 수 있습니다. 그렇다면 사용법을 먼저 알아봅시다. BottomTabNavigator에 Icon을 추가하기 위하여 NavIcon이라는 Component를 추가합니다.   
`prismagram-app/components/NavIcon.js`
```javascript
import React from "react";
import { Ionicons } from "@expo/vector-icons";
import PropTypes from "prop-types";
import styles from "../styles";

const NavIcon = ({
  focused = true,
  name,
  color = styles.blackColor,
  size = 30
}) => (
  <Ionicons
    name={name}
    color={focused ? color : styles.darkGreyColor}
    size={size}
  />
);

NavIcon.propTypes = {
  name: PropTypes.string.isRequired,
  color: PropTypes.string,
  size: PropTypes.number,
  focused: PropTypes.bool
};

export default NavIcon;
```   
이를 통해 [expo vector icons list](https://expo.github.io/vector-icons/)에서 Ionicons 카테고리의 아이콘을 BottomTabNavigator에 추가할 수 있는 컴포넌트를 만들었습니다. 이 컴포넌트의 속성으로 `createBottomTabNavigator`의 옵션에 있는 값인 focused와 color, size를 변경하고자 합니다. 다시 createBottomTabNavigator로 돌아가면   
`navigations/TabNavigation.js`
```javascript
// 생략
export default createBottomTabNavigator(
  {
    Home: {
      screen: stackFactory(Home, {
        title: "Home",
        headerRight: () => <MessagesLink />,
        headerTitle: () => (
          <Image style={{ height: 32 }} resizeMode="contain" source={LOGO} />
        )
      }),
      navigationOptions: {
        tabBarIcon: ({ focused }) => (
          <NavIcon
            focused={focused}
            name={Platform.OS === "ios" ? "ios-home" : "md-home"}
          />
        )
      }
    },
```
createBottomTabNavigator에 Home Screen을 추가하고 navigationOptions으로 tabBarIcon이 focused 되었을 때 값을 NavIcon에 전달하기 위해서 prop으로 받아옵니다. 그런 후 TabBar 아이콘을 아까 만들어둔 NavIcon을 통해 설정합니다. React-Native의 Platform 컴포넌트로 ios일 때는 ios-home 아이콘을, ios가 아닐 때에는 md-home 아이콘으로 설정할 수 있습니다.    

```javascript
 {
    initialRouteName: "Notifications",
    tabBarOptions: {
      showLabel: false,
      style: {
        backgroundColor: "#FAFAFA"
      }
    }
  }
);
```
그리고 바로 아래에 tabBarOptions를 통하여 tabBar의 배경 색상을 지정할 수 있습니다. 자, 여기까지가 TabNavigator의 색상을 지정하는 것이었습니다. 참고로 showLabel은 bottomTabBar에서 텍스트를 함께 보여줄 것인지 여보를 나타내는 것입니다. 만약 `showLaber: true,`로 설정하게 되면 home 아이콘과 함께 "home"이라는 텍스트도 함께 나타나게 됩니다. 
   
      

## stackNavigator

Nomard Coder는 `TabNavigor.js` 안에 같이 두었습니다. 코드는 아래와 같습니다.
```javascript
const stackFactory = (initialRoute, customConfig) =>
  createStackNavigator(
    {
      InitialRoute: {
        screen: initialRoute,
        navigationOptions: {
          ...customConfig
        }
      },
      Detail: {
        screen: Detail,
        navigationOptions: {
          headerBackTitle: noContext,
          title: "Photo"
        }
      },
      UserDetail: {
        screen: UserDetail,
        navigationOptions: ({ navigation }) => ({
          title: navigation.getParam("username")
        })
      }
    },
    {
      defaultNavigationOptions: {
        headerBackTitle: noContext,
        headerTintColor: styles.blackColor,
        headerStyle: { ...stackStyles }
      }
    }
  );
```
이렇게 createStackNavigator를 stckFactory라는 변수로 지정하여 BottomTabNavigator를 지정할 때 함께 추가하였습니다. 즉, BottomTabNavigator로 해당 화면을 클릭하여 들어갈 때 stackNavigator도 함께 동작하게 됩니다. 이 부분은 바로 위 코드의    
`navigations/TabNavigation.js`
```javascript
// 생략
export default createBottomTabNavigator(
  {
    Home: {
      screen: stackFactory(Home, {
        title: "Home",
        headerRight: () => <MessagesLink />,
        headerTitle: () => (
          <Image style={{ height: 32 }} resizeMode="contain" source={LOGO} />
        )
      }),
      navigationOptions: {
        tabBarIcon: ({ focused }) => (
          <NavIcon
            focused={focused}
            name={Platform.OS === "ios" ? "ios-home" : "md-home"}
          />
        )
      }
    },
```
이 부분에서 `screen: stackFactory(Home, {})`과 같습니다. 이렇게 하여 react-Navigator의 stack Navigator를 통해 gesture를 제공하게 되며 title을 넣을 수 있도록 합니다. 다만 stack Navigator에서 bottomTabNavigator에 등록되지 않은 navigation으로 들어갈 때는 backTitle(뒤로가기) 버튼이 텍스트와 함께 생기게 됩니다. 이 부분을 지우고 싶을 때는 
```javascript
const stackFactory = (initialRoute, customConfig) =>
  createStackNavigator(
    {
      // ... 생략
    },
    {
      defaultNavigationOptions: {
        headerBackTitle: noContext,
        headerTintColor: styles.blackColor,
        headerStyle: { ...stackStyles }
      }
    }
  );
```
defaultNavigationOptions으로 위와같이 설정하면 됩니다. Nomard Coder는 headerBackTitle을 null로 지정하였으나 어찌된 영문인지 저에게는 정상적으로 동작하지 않아서 또다른 value인 noContext를 지정해주었더니 잘 동작하였습니다. 참고하시어 진행하면 좋을 듯 합니다.

### 참고자료

- [academy.nomadcoders.InstagramClonCoding](https://academy.nomadcoders.co/courses/enrolled/503371)
