---
layout: post
title: [React 01] React를 가볍게 알아보기
date: 2018-02-19 14:00:00 +0900
description: # Add post description (optional)
img: react01/react-background.png # Add image post (optional)
tags: [frontend, React] # add tag
---

> React로 무언가를 만들어 보기에 앞서, React란 무엇인가 간단히 알아보고 *제가 생각하는* 중요한 개념을 알아보겠습니다.

## 1. What is a React?

React란 Facebook에서 만든 Javascript `라이브러리`로 사용자 인터페이스(User Interface)의 생산성을 높이기 위해 개발되었습니다. `Virtual DOM`이라는 개념을 사용하여 browser rendering 속도를 높이고, `One Way Data Flow`를 통해 복잡도가 낮은 코드를 생산하는 것을 도와줍니다. 이러한 특징들을 통해 React는 지속적으로 데이터가 변하는 대규모 어플리케이션의 구축을 보다 쉽게 할 수 있습니다. 
정말입니다! React의 [공식 홈페이지](https://reactjs.org/)에 가 보시면 이렇게 적혀있습니다.

```
React makes it painless to create interactive UIs.
```

React를 처음보시거나, 시작한지 얼마 안되시는 분들이 보기에는 알듯 말듯한 말들만 늘어놓는다고 생각하실지 모르겠습니다. 하지만 걱정하지마세요! 이제부터 하나하나 알아보겠습니다.

- ### 1. 라이브러리

React는 Framework가 아닌 `Library`입니다. 필요에 따라 다른 라이브러리를 설치하지않고 React 그 자체만 필요한 곳에 이용할 수도있고, react-router, redux같은 라이브러리의 도움을 받아 대규모의 Frontend Application을 구성할 수도 있습니다. 다른 라이브러리의 도움을 받을경우 약간의 설정이 필요하지만 매우 쉽고 직관적으로 이용할 수 있습니다.

- ### 2. Virtual DOM

네, React를 사용하고 계시다면 지겹게, 사용하고 계시지 않으시다면 생소하거나 한 번쯤은 들어보셨을 만한 개념입니다. 사실 이 개념은 알고계시면 `정말 매우 진짜` 좋은 개념이지만 관심이 없으시다거나 본인이 `Frontend Engineer`가 아니시면 그냥 넘어가셔도 좋습니다 ~~(가지마세요)~~.

`Virtual DOM`에 대해 알아보기 전에, 우리는 browser가 화면을 어떻게 그리는지에 대해 알 필요가 있습니다.

- #### 2-1. 브라우저는 어떻게 화면을 그리는가

모두들 익히 알다시피, web browser(chrome, safari, firefox, ~~Internet Exp...~~)란 간단히 말해 일종의 `해석기` 입니다. 브라우저는 사용자가 요청한 자원을 서버에 요구하고, 요구에 대한 응답(HTML, CSS 등등...)을 표시하게 됩니다. 이 과정에서 브라우저는 W3C가 정한 명세에 따라 HTML과 CSS를 해석하게 됩니다. 이런 역할은 `렌더링 엔진`이 맡습니다. 우리가 흔히 알고있는 `webkit`, `gecko`등이 바로 그것입니다.

예를들면, webkit 렌더링 엔진은 HTML파일을 다음과 같은 과정을 통해 화면에 내놓습니다.

![image](../assets/img/react01/webkit.png)

~~어디선가 욕하는 소리가~~

네, 쉽게 말씀드리자면 다음과 같이 정리할 수 있습니다.

1. 새로운 태그를 만날 때 마다 start token을 생성.
2. 해당 태그가 끝나면 end token을 생성.
3. 생성된 token을 기반으로 DOM Tree 생성.
4. css가 DOM Tree에 `attach`되며 CSSOM Tree 생성.
5. 브라우저가 DOM및 CSSOM을 결합하여 Rendering Tree 생성. 이때, 화면에 정말로 `보여줄` 요소만 Tree에 남는다. 즉, meta, head 태그 등은 제외됨.
6. 각 node들을 화면에 배치함(`Layout`, Reflow).
7. 배치된 node들의 실제 스타일을 적용(`Paint`).

이제 HTML과 CSS가 어떻게 화면을 구성하는지 아셨으리라 생각합니다. 그렇다면 대체 `Virtual DOM`을 이용한다는 것이 어떤 의미인가를 정리할 때가 온 것 같습니다.

- #### 2-2. Virtual DOM

만약, 현재 브라우저가 해석하여 화면에 그려놓은 DOM node에 변화가 생기면 어떤일이 일어날까요? 간단합니다. 브라우저는 node가 바뀌었다고 판단하는 순간, Render Tree를 재생성하고, Layout을 만들고 다시 Paint하는 과정을 거치게 됩니다. 이런 과정이 수 없이 반복된다면 당연히 브라우저의 연산에 영향을 주게되고, 이는 브라우저의 성능을 저하시켜 페이지의 구동을 느리게 만들게 됩니다. 이 때 빛을 발하는 것이 `Virtual DOM`입니다.

Virtual DOM은, 쉽게 말하자면 실제로 그려진 DOM을 메모리상에 올려놓은 복사본입니다. 이 복사본으로 무엇을 하느냐, React는 DOM에 변화가 생기면 일단 Virtual DOM에 적용합니다. 이때, Virtual DOM Tree를 다시 그리지 않고, **`변화가 필요한 부분만 골라서 변경`** 해 줍니다. 그리고 연산이 끝나면 최종적인 결과를 실제 DOM에 반영해 줍니다.

다시 말해, 우리가 100개의 node를 하나 하나 수정하게되면 100번의 Reflow와 Repaint가 발생하지만, Virtual DOM을 이용하게 되면 이 모든변화를 묶어서 한 번의 Reflow와 Repaint로 실제 화면에 적용할 수 있게 된다는 것입니다.

물론 1초내에 10번의 DOM 변경이 있다면 React도 10번의 rendering을 하게됩니다. 하지만, 한 번 rendering되는 과정에 여러부분의 변화가 있다면 React는 이를 `한 번에 적용`시켜 줍니다.

이에 대해서는 제가 만나뵌 적은 없지만 개인적으로 사모하는 [velopert님의 블로그](https://velopert.com/3236)를 읽어보시길 권장드립니다.

- ### 3. One Way Data Flow

React는 `단방향으로 데이터가 흐르는` 특징을 가지고 있습니다. 데이터는 부모 Component에서 자식 Component로, 또는 Store에서 View로 흐르게 됩니다. 데이터를 받는 쪽은 그 데이터의 변화를 감지하여 화면의 변경사항을 갱신합니다. 이러한 패턴을 `Flux`패턴 이라고 합니다.

자식은 부모가 누구인지, 어떤 내부 state를 가지고 있는지 알 필요가 없습니다(알 수는 있지만 자식이 부모에게 access하는 것은 대부분의 경우에 anti pattern에 해당합니다). 단지 자식은 부모가 전해주는 데이터를 통해 자신이 맡은 일만 하면 됩니다. View의 경우도 마찬가지로, Store에서 전해주는 데이터를 통해 View로서의 역할(action을 전달해주고, 데이터를 표시해주는)만 하면 됩니다.

(논란의 여지는 있지만) 이러한 단방향 데이터 흐름은 개발자가 기존의 MVC패턴으로 Application을 개발 할 때 발생하는 혼란을 상당 부분 막아주는 역할을 합니다. 대규모 Application일 수록 이런 현상이 두드러지는데, 점점 증식하는 데이터의 의존성과 연쇄적인 데이터의 갱신은 데이터의 흐름을 꼬아놓습니다. 이는 결과적으로 개발자의 유지보수를 어렵게 만들거나, 예측할 수 없는 데이터를 결과로 내놓게 됩니다.

![image](../assets/img/react01/flux.png)

Flux 패턴에서 모든 데이터는 중앙의 Dispatcher를 통해 흐르게 됩니다. 그리고 데이터를 적절히 관리, 갱신하는 Store는 View에 화면에 표시할 데이터를 제공합니다. 만약 View에서 특정한 action이 발생한다면 이 action은 Dispatcher를 거쳐 Store의 데이터에 변화를 주고, Store가 보내주는 데이터의 변화를 감지한 View는 화면을 다시 갱신하게 됩니다.

(개인에 따라 다르겠지만) React는 Flux패턴을 적극 수용하였기 때문에 기존의 MVC패턴에 비해 몇 배는 간단한 제어를 통해 효율적으로 데이터를 관리하고, 화면을 쉽게 갱신할 수 있습니다.

- ## 2. Conclusion

제가 개인적으로 중요하게 생각하는 React의 세 가지 특징을 정리해 보았습니다. 어떠셨나요? 아직 React를 직접 다뤄보지 않아 감이 잘 오지 않으실테지만 이 문서를 읽어보셨다면 앞으로 React와 친해질 준비는 어느정도 되셨다고 보셔도 됩니다.

다음 시간에는 React와 친해지기 위한 `개발환경`을 간단히 구성해 보고, 브라우저에 Hello React를 그려보는 시간을 갖겠습니다.

#### 틀린 내용이나 좋은 의견이 있으시면 적극 수용하겠습니다. 댓글을 꼭 달아주세요!

---
#### 참고 링크
> https://reactjs.org/

> https://velopert.com/775

> http://webframeworks.kr/getstarted/reactjs/

> http://d2.naver.com/helloworld/59361

> https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction?hl=ko

> https://velopert.com/3236

> https://haruair.github.io/flux/docs/overview.html
---
