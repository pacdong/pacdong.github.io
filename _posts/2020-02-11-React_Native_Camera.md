---
title: "Chapter.18 React Native Camera in Expo"
excerpt: "ExpoApp - Camera 사용하기"

categories:
  - 인스타그램 클론코딩
tags:
  - Instagram
  - Clone
  - Coding
  - React
  - React-Native
  - Camera
last_modified_at: 2020-02-11T08:17:30-18:00
---

이번시간에는 별도의 포스터를 작성하려고 합니다. Nomard Coder의 Instagram Clone Coding에서 Expo를 이용하여 Native Camera를 사용할 수 있습니다. 이러한 테크닉은 추후 다른 어플리케이션에서도 항상 사용할 수 있을 것 같아서 Camera에 대한 내용을 정리하고자 합니다. 

   <br>
- Permission <br>
- Expo Camera <br>
- Photo Select <br>
- Photo Upload <br>
 <br>

 ## Permission

 먼저 사용자의 핸드폰에서 카메라을 찍고, 휴대폰에 저장하고, 저장되어있는 사진을 사용하려면 사용자의 동의를 구해야합니다. 이러한 과정은 `Expo-Permission`을 사용하여 권한을 설정할 수 있습니다. Permission에 관련된 [Expo 공식문서](https://docs.expo.io/versions/latest/sdk/permissions/)입니다.    

 먼저 설치는 `expo install expo-permissions`의 명령어로 설치합니다. 그럼 사용법을 바로 보겠습니다. 먼저 공식 문서상의 사용법입니다.
```javascript
async function alertIfRemoteNotificationsDisabledAsync() {
  const { status } = await Permissions.getAsync(Permissions.NOTIFICATIONS);
  if (status !== 'granted') {
    alert('Hey! You might want to enable notifications for my app, they are good.');
  }
}

async function checkMultiPermissions() {
  const { status, expires, permissions } = await Permissions.getAsync(
    Permissions.CALENDAR,
    Permissions.CONTACTS
  );
  if (status !== 'granted') {
    alert('Hey! You heve not enabled selected permissions');
  }
}
```    

`Permissions.getAsync(...permissionTypes)`로써 원하는 권한을 요청하도록 합니다. 아래와 같이 요청을 날린 후 콘솔에 response로 log를 찍어보면 여러 타입이 들어있습니다.
 ```javascript
import * as Permissions from 'expo-permissions';
//.. 기타코드 생략

const requestPermisison = async () => {
  const response = await Permissions.askAsync(Permissions.CAMERA);
  console.log(response);
};

useEffect(() => {
  requestPermisison();
}, []);
```   
    
이렇게 많은 데이터가 콘솔 창에 찍히는 것을 볼 수 있습니다.

```javascript
{
  status, // combined status of all component permissions being asked for, if any of has status !== 'granted' then that status is propagated here
  expires, // combined expires of all permissions being asked for, same as status
  canAskAgain,
  granted,
  permissions: { // an object with an entry for each permission requested
    [Permissions.TYPE]: {
      status,
      expires,
      canAskAgain,
      granted,
      ... // any additional permission-specific fields
    },
    ...
  },
}
```
위와 같은 여러 타입 중에 status가 granted 된 것이 중요합니다. 만약 사용자가 승인 버튼을 누르지 않았다면 status는 초기 값인 denied 되어 있게 됩니다. 따라서 `Permission`이 이루어 지고 난 이후에 무언가 코드를 실행하고자 한다면 공식문서 처럼 if 구문으로 진행하면 됩니다. 만약 실수도 동의하지 않음을 눌렀을 경우에는 직접 앱의 저장소에 가서 관한을 허용해주는 작업이 필요합니다.


## Expo-mediaLibrary

권한 요청이 끝났다면 이제는 찍힌 사진을 가져오는 라이브러리를 써야할 때입니다. 앨범에서 사진을 가져올 수 있는 손쉬운 방법은 바로 `Expo`의 [MediaLibrary](https://docs.expo.io/versions/latest/sdk/media-library/)를 사용 하는 것입니다.    

설치 방법은 항상 그렇듯 `expo install expo-media-library`를 사용하여 설치해 줍니다. 공식 문서에서도 볼 수 있듯이, media-library는 Permissions.CAMERA_ROLL이 먼저 요청되어져야합니다. 아무리 손쉽게 가져올 수 있다고 하여도 사진을 선택하는 것부터 원하는 화면에 띄우기까지 모든 방법을 일일이 다 코딩해야하기에 **useState**를 많이 만들어야 합니다.

```javascript
  const [hasPermission, setHasPermission] = useState(false); // 권한 상태 여부
  const [selected, setSelected] = useState(); // 사진 선택 여부
  const [allPhotos, setAllPhotos] = useState(); // 전체 사진 가져오기
  const changeSelected = photo => {
    setSelected(photo);                       // 선택된 사진으로 뷰로 바꾸기
  };
```

다음은 사진을 가져오는 코드입니다.
```javascript
  const getPhotos = async () => {
    try {
      const { assets } = await MediaLibrary.getAssetsAsync();
      const [firstPhoto] = assets;
      setSelected(firstPhoto);
      setAllPhotos(assets);
    } catch (e) {
      console.log(e);
    } finally {
      setLoading(false);
    }
  };
```
MediaLibrary.getAssetsAsync(); 호출을 통해 assets에 있는 사진을 추출합니다. assets에서 첫번째 사진을 자동으로 선택하게끔 만들어주었습니다.

```javascript
return(
  <View>
    {hasPermission ? (
      <Image
        style={{ width: constants.width, height: constants.height / 2 }}
        source={{ uri: selected.uri }}
      />)
      : null}
  </View>
);
```
위와 같이 권한을 가지고 있으면 사진을 띄우는 코드를 작성해보았습니다.



## Expo Camera 사용하기