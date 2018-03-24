---
layout: post
title: React 02) Hello React!
date: 2018-03-24 14:00:00 +0900
description: # Add post description (optional)
img: ./react01/react-background.png # Add image post (optional)
tags: [frontend, React] # add tag
---


> React의 개발환경을 설정하여 화면에 간단한 component를 그려보겠습니다.

## 1. 개발환경 설정


- ### 1. node.js + npm
React를 개발하기 위해서는 먼저 `node.js`와 `npm`을 설치해야 합니다.

[node.js 설치 방법](https://nodejs.org/en/download/)

각자 환경에 맞게 설치를 하시면 됩니다! 설치가 실행되며 남는 시간에 이들의 특징을 ___간단하게___ 살펴보고자 합니다.

1. `node.js`란?

`node.js`는 Chrome V8 Javascript 엔진으로 빌드된 javascript runtime입니다. 즉, javascript를 web browser 이외의 환경에서도 실행을 시킬수 있게 도와준다고 보시면 됩니다.

2. 특징

    - [비동기 I/O처리, Event driven](https://nodejs.org/en/docs/guides/blocking-vs-non-blocking/)
    - [단일 Thread, Event loop](http://meetup.toast.com/posts/89)

3. `node.js`가 최상의 선택이 될 수 있는 어플리케이션

    - 입출력이 잦음
    - 데이터 스트리밍, 혹은 실시간으로 handling
    - JSON API 기반

4. `npm`이란?

    - Node.js 패키지/모듈 저장소
    - Node.js 패키지 설치 및 버전 / 호환성 관리를 할 수 있는 커맨드라인 유틸리티

자, 이 설명을 다 읽으셨으면 설치가 다 되셨을 겁니다. 그렇다면 terminal을 열고 다음 명령어를 실행하여 설치가 무사히 끝났는지 확인합니다.

```terminal
node -v
npm -v
```

화면에 다음과 같이 나온다면 설치가 잘 된 것입니다.

![image](../assets/img/react02/check-install-node.gif)

- ### 2. 나에게 맞는 Text editor 설치

IDE는 취향이지만 (저는 취존러입니다) 보통의 경우 IDE가 아닌, 아래 두 가지 `Text editor`중 하나를 설치하는 것을 추천드립니다.

  - [VS Code](https://code.visualstudio.com/)
  - [Atom](https://atom.io/)

- ### 3. create-react-app 설치

사실, React를 개발하기 위해서는 알아야 할 것들이 많습니다. 그 중 하나가 정말 필수적으로 알아야 할 [webpack](https://webpack.js.org/)이라는 module bundler 입니다.

javascript application의 규모가 커지면서 유지보수의 필요성에 의해 코드를 모듈로 나누어 관리하는 시스템이 필요해 졌습니다. 하지만 javascript 자체에는 언어가 지원하는 모듈 시스템이 없어, 이런 한계를 극복하는 것을 도와주는 도구 입니다.

정말 마법같은 도구인 webpack의 단점은 `복잡하다`는 것입니다. 보통 frontend를 개발하기 위해서 webpack을 먼저 공부하다가 "이게 뭐야 ~~WTF~~!"를 외치고는 그대로 `GG`를 치는 분들을 종종 보아왔습니다. 

일단 그렇기 때문에 이런 복잡한 설정을 미리 다 해주고, 필요에 따라 사용자가 설정을 조금씩 바꿔 사용할 수 있게(정확히는 빠르게 React app을 개발할 수 있게)만들어 주는 툴을 사용하고자 합니다.

그것이 바로 `create-react-app`입니다.

망설일 이유가 없습니다. 다음 명령어로 CRA를 설치하도록 합니다.

```
npm install -g create-react-app
```

설치가 끝났다면, 확인해 봅니다.

![image](../assets/img/react02/check-install-cra.gif)

자! 이제 본격적으로 React app을 개발하러 출발하도록 할까요? 갈길이 아직 많이 남았습니다!

## 2. Hello React!

먼저 `create-react-app` command를 통해 프로젝트를 생성해 봅니다. 프로젝트의 이름은 my-blog입니다.

```
create-react-app my-blog
```

성공적으로 생성이 되었다면

```
npm run start
```

명령어를 통해 화면이 정상적으로 보이는지 확인합니다.

![image](../assets/img/react02/project-init-success.png
 =350x184)

확인하셨다면, 아까 설명드렸던 text editor로 해당 프로젝트를 open해 보도록 합니다.

프로젝트 root에서 open을 선택하셨다면, 다음과 같은 directory scaffolding을 확인할 수 있습니다.

![image](../assets/img/react02/react-scaffolding.png){ : width="200px" height="372px" }

directory scaffolding에 대한 설명은 다음 시간으로 미루고 일단은 `App.js`를 선택하여 App class가 선언된 부분을 다음과 같이 고쳐봅니다.

```javascript
class App extends Component {
  render() {
    return (
      <div className="App">
        Hello React!
      </div>
    );
  }
}
```

그리고 저장을 해 보면 hot-reloader에 의해 자동으로 화면이 바뀌게 됩니다. 화면에 **Hello React!** 가 보이시나요? 축하합니다! 드디어 React의 멋진 세계에 발을 내딛게 되셨습니다. 이제 React의 Component와 중요한 개념을 설명하고 이번 포스트를 마치고자 합니다.

## 3. Component?


