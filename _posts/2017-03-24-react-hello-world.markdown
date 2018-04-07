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

```bash
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

```bash
npm install -g create-react-app
```

설치가 끝났다면, 확인해 봅니다.

![image](../assets/img/react02/check-install-cra.gif)

자! 이제 본격적으로 React app을 개발하러 출발하도록 할까요? 갈길이 아직 많이 남았습니다!

## 2. Hello React!

먼저 `create-react-app` command를 통해 프로젝트를 생성해 봅니다. 프로젝트의 이름은 my-blog입니다.

```bash
create-react-app my-blog
```

성공적으로 생성이 되었다면

```bash
npm run start
```

명령어를 통해 화면이 정상적으로 보이는지 확인합니다.

![image](../assets/img/react02/project-init-success.png)

확인하셨다면, 아까 설명드렸던 text editor로 해당 프로젝트를 open해 보도록 합니다.

프로젝트 root에서 open을 선택하셨다면, 다음과 같은 directory scaffolding을 확인할 수 있습니다.

![image](../assets/img/react02/react-scaffolding.png)

directory scaffolding에 대한 설명은 다음 시간으로 미루고 일단은 `App.js`를 선택하여 App class가 선언된 부분을 다음과 같이 고쳐봅니다.

```jsx
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

그리고 저장을 해 보면 react-hot-loader에 의해 자동으로 화면이 바뀌게 됩니다. 화면에 **Hello React!** 가 보이시나요? 축하합니다! 드디어 React의 멋진 세계에 발을 내딛게 되셨습니다. 이제 React의 Component와 간단한 예제를 보여드리고 이번 포스트를 마치고자 합니다.

## 3. Component?

React의 뿌리는 바로 `Component`라는 개념 입니다. Component의 사전적인 의미는 **구성 요소**입니다.

글 만으로는 와닿지 않으니 다음 이미지를 통해 예시를 들어보겠습니다.

- ### 1. Component 단위로 나누어 보기

![image](../assets/img/react02/components.png)

우리에게 익숙한 화면입니다. 그렇다면 이 화면을 구성하고 있는 Component는 얼마나 될까요?

![components-header](../assets/img/react02/components.png)

먼저 저는 맨 위의 이 navigation bar를 하나의 Component로 생각하고 싶습니다. 그리고 다시 곰곰히 생각해보면 이 navigation bar역시 여러개의 Component로 이루어져 있다고 생각할 수 있을 것 같습니다. 바로 아래와 같이 말이죠.

* 메뉴과 로고
![components-header-menu](../assets/img/react02/components.png)
* 검색창
![components-header-input](../assets/img/react02/components.png)
* 여러 기능들 (역시 기능/버튼 마다 Component라고 생각할 수 있겠죠?)
![components-header-functions](../assets/img/react02/components.png)
* 유저 메뉴
![components-header-user](../assets/img/react02/components.png)

navigation bar의 아래쪽도 마찬가지로 Component 단위로 나눠보겠습니다. 한 번 그림과 설명을 같이 보면서 머리속으로 상상해 보세요.
![components-body](../assets/img/react02/components.png)

가장 먼저 눈에 띄는 Component는 역시 동영상이 재생되는 부분입니다. 아마 저 Compoenet에는 content를 streaming하는 기능이 포함되어 있겠죠? 그 다음 오른쪽을 보시면 아래로 길게 다음 동영상 목록이 보입니다. 이 역시 Component입니다. 그럼 슬슬 감이 오시는 분들이 이렇게 말씀하시겠죠? 

```
"다음 동영상 목록을 구성하는 각각의 동영상들도 Component 맞죠? 왼쪽의 좋아요/싫어요/동영상 제목 등등... 모두요!"
```

맞습니다! 정말 잘 하셨어요. 개인적으로 저는 button과 input역시 모두 Component라고 생각합니다. 그래서 html에 native tag가 있음에도, 항상 Common Component로 만들어두곤 하지요. native tag가 있는데 왜 꼭 Component로 만드는지 의문이 생기실 수도 있는데, 이것의 이점에 대해서는 차차 알아가실 수 있습니다.

Component에 대해 예시를들어 설명을 해 드렸습니다. 이해가 가시나요? 그래도 복잡하셨다면, `React는 Component의 집합`이라는 사실만 잘 알아두시면 될 것 같습니다.

- ### 2. 간단하게 실습해보기

자, 방금전 수정했던 App.js파일을 다시 보실까요? 그리고 다음과 같이 수정해 주세요(딱 봐도 굉장히 _code smell_ 이 물씬 풍기는 코드지만, 간단한 예시일 뿐이니 걱정 안하셔도 됩니다). 아무 기능도 없지만, Component를 나누어 보기 위함입니다.

```jsx
class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>Hello React!</h2>
        <h2>TodoList</h2>
        <input type="text" placeholder="할일 입력해보기" />
        <button>입력하기</button>
        <ul>
          <li>글쓰기</li>
          <li>운동하기</li>
          <li>React 공부</li>
          <li>다른 사람 글 읽기</li>
          <li>산책하기</li>
          <li>다이어트</li>
          <li>식단관리</li>
          <li>자료구조 114페이지</li>
        </ul>
      </div>
    );
  }
}
```

저장하시면, 다음과 같은 화면을 보실 수 있습니다.

![image](../assets/img/react02/todo-sample.png)

화면을 보시자마자 어떻게 Component를 나눠야 좋을지 각자 생각이 드셨을거라 생각합니다. 저라면 TodoList라는 큰 제목은 놔두고, 입력 하는 부분과 할 일의 목록들을 나누어 줄 것 같습니다(여러분의 생각은 어떤가요?). 이제 각 3개의 파일을 App.js와 **같은 경로**에 생성하셔서 다음과 같이 작성해 보세요.

```jsx
// ./TodoInput.js
import React, { Component } from 'react'

class TodoInput extends Component {
  render() {
    return [
      <input
        key="TodoInput-input"
        type="text"
        placeholder="할일 입력해보기"
      />,
      <button key="TodoInput-button">입력하기</button>,
    ]
  }
}

export default TodoInput

// ./TodoItemList.js
import React, { Component } from 'react'

class TodoItemList extends Component {
  render() {
    return (
      <ul>
        <li>글쓰기</li>
        <li>운동하기</li>
        <li>React 공부</li>
        <li>다른 사람 글 읽기</li>
        <li>산책하기</li>
        <li>다이어트</li>
        <li>식단관리</li>
        <li>자료구조 114페이지</li>
      </ul>
    )
  }
}

export default TodoItemList

// ./TodoList.js
import React, { Component } from 'react'

import TodoInput from './TodoInput'
import TodoItemList from './TodoItemList'

class TodoList extends Component {
  render() {
    return (
      <div className="App">
        <h2>Todo List</h2>
        <TodoInput />
        <TodoItemList />
      </div>
    )
  }
}

export default TodoList

```

다 작성하셨으면, App.js를 다음과 같이 수정해 주세요.

```jsx
import React, { Component } from 'react';

import TodoList from './TodoList'

import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>Hello React!</h2>
        <TodoList />
      </div>
    )
  }
}

export default App
```

그리고 저장을 하면 화면이 다시 보입니다. 어떤가요? 화면상으로는 바뀐것이 없습니다. 하지만 이 화면을 구성하는 코드를 살펴보게 되면, 정말 합리적으로 바꿨다는 것을 알 수 있습니다.

React app이 점점 거대해 지는데, 이 코드들이 모듈화 되어있지 않고 모두 App.js에 들어가 있다는 ~~끔찍한~~ 상상을 해보세요. App이 커지면 커질수록, 유지보수에 쏟아야하는 노력이 어마어마해 질 것입니다(한 번시도해 보세요! 수천, 수만줄의 코드 중에서 수정 대상을 찾아 헤메는 ~~쓸데 없는~~코드 방랑자가 되어볼 수 있습니다).

하지만 우리는 이런 상황을 방지하기 위해 Component별로 코드를 분리해 놓았습니다. 만약 나중에 할일 목록 부분의 스타일을 변경한다거나, 기능에 이상이 있을경우 TodoItemList.js 파일만 확인하고 수정하면 됩니다.

## 4. Conclusion

자, 처음으로 React app을 만들고 Component라는 중요한 개념에 대해 알아보았습니다. 다음 포스트는 정말 중요한 개념인 children, state, props에 대해 알아보도록 하죠.

---
#### 참고 링크
> https://velopert.com/133  
---
