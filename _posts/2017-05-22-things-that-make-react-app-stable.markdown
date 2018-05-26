---
layout: post
title: React app을 단단하게 만들어 주는 것들
date: 2018-05-22 19:00:00 +0900
description: # Add post description (optional)
img: zzal.jpg # Add image post (optional)
tags: [Productivity, React, Project, React app Stable] # add tag
---

> 오늘은 저의 개인취향을 공개합니다. 이것들을 사용했을때 React로 만든 application이 단단해진다, 내 실수를 줄일 수 있다, 그런거 다 필요없고 무조건 써야한다 싶은 것들을 순위와 상관없이 적어보고자 합니다. 저의 취향이 이 글을 보시는 분들에게 1%라도 도움이 되었으면 좋겠네요.

**먼저, 이런 COOL한 패턴들을 알려주시고 같이 고민할 수 있는 기회를 주신 저희 Streami FE team에게 무한한 감사를 올립니다!**

`warning: 아래 쓰여질 것들은 지극히 개인적인 취향이 반영되어 보는 이에 따라 혐오감, 구토 등 다양한 증상이 있을 수 있음을 미리 밝힙니다.` ~~다행히 php는 없습니다~~

## 1. Lodash fp

[Lodash](https://lodash.com/docs/4.17.10)는 정말 환상적인 라이브러리라고 생각합니다. 각 함수의 기능과 성능이 보장되어있기 때문에 고민하지 않고 ~~내 코드와 다르게~~자신있게 코드에 사용할 수 있습니다. 예를들어 간단한 `map`함수를 javascript의 모든 타입에 적절히 대응할 수 있게 개발하는 것은 귀찮은 일이 될 수도 있지만, 이 라이브러리를 사용한다면 이러한 귀찮은 일이 사라지게 되죠. 덕분에 해당 기능을 개발하는 이유도 없어져 상당 부분 부담이 줄어들게 됩니다.

하지만 lodash를 사용하다 보면 굉장히 고민되는 순간이 옵니다. 간단한 예시를 들어보겠습니다. '?test=all&type=a'라는 query string을 받아 Object로 만드는 기능을 하는 함수를 lodash를 이용해 작성해 보겠습니다. 일단 정말 간단하게 시작해 보죠. [query-string](https://github.com/sindresorhus/query-string)이라는 훌륭한 라이브러리를 통해 문제를 해결 할 수도 있겠지만, 일단은 예시를 위한 코드이니 읽어주시기 바랍니다.

```javascript
import { replace, split, map, fromPairs } from 'lodash'

const queryStringToObject = queryString => {
  /** Query string will convert to object.
   * parameter: string ('?test=all&type=a')
   * return: Object ({ test: all, type: a })
   */
  // input queryString = '?test=all&type=a'
  const questionMarkReomvedQuery = replace(queryString, '?', '')
  // 'test=all&type=a'
  const splitedQueryByOption = split(questionMarkReomvedQuery, '&')
  // ['test=all', 'type=a']
  const splitedQueryByKeyOrValue = map(splitedQueryByOption,
    query => split(query, '='))
  // [['test', 'all'], ['type', 'a']]
  const returnObject = fromPairs(splitedQueryByKeyOrValue)
  // { test: all, type: a }
  return returnObject
}
```

정말 간단한 코드입니다. 그러나 코드는 간단하지만 읽는데에는 꽤나 복잡해 보이는군요. 일단 쓸데없이 선언되는 변수들이 많습니다. 각 변수들에 담기는 값의 의미를 표현하기도 힘들어 보입니다. 제일 큰 것은 무언가 functional하게 작성하려고 노력하였으나, 전혀 그렇게 보이지 않는다는 것 입니다. 결국 lodash를 뭔가 `hip`하지 ~~간지나지~~않게 사용한 것 같아 마음이 무겁습니다. 그러면, 이것을 이렇게 고쳐보도록 합니다.

```javascript
import { replace, split, map, fromPairs } from 'lodash'

const queryStringToObject = queryString => {
  /** Query string will convert to object.
   * parameter: string ('?test=all&type=a')
   * return: Object ({ test: all, type: a })
   */
  return fromPairs(
    map(split(replace(queryString, '?', ''), '&'), query => split(query, '=')))
}
```

뭔지 모르겠지만, 일단 한 가지 문제를 해결한 것 같네요. 쓸데없는 변수들이 사라졌습니다. ~~간지가 안나는 문제도 해결~~ ~~사실은 두 가지 문제를 해결함~~ 하지만 위의 코드에서 없었던 문제가 발생합니다. 바로 `가독성이 심각하게 낮아진다`는 문제입니다. 사실 저는 이 문제가 더 크다고 생각합니다. 왜냐하면 개발자들은 새로운 코드를 쓰는 시간보다 코드를 **읽는 시간이 더 길다**고 생각하는데, 그런 측면에서는 오히려 *더 나빠졌기 때문*입니다.

하지만 늘 그렇듯 해결책은 있습니다. 바로 lodash의 `chain` method를 이용하는 것 입니다.

```javascript
import { chain, split } from 'lodash'

const queryStringToObject = queryString => {
  /** Query string will convert to object.
   * parameter: string ('?test=all&type=a')
   * return: Object ({ test: all, type: a })
   */
  return chain(queryString)
    .replace('?', '')
    .split('&')
    .map(query => split(query, '='))
    .fromPairs()
    .value()
}
```

어떤가요? 코드의 가독성 문제를 확실하게 해결하였습니다. 훨씬 보기 좋네요. 하지만 여기서 보이지 않는 한 가지 중요한 문제가 발생합니다. 바로 `chain` method의 문제입니다. `chain`은 string, array같은 객체를 받아 `wrapped chain object`를 반환합니다. 실제로 `value`함수를 실행하지 않고 queryStringToObject함수를 실행한다면 다음과 같은 return 값을 확인할 수 있습니다.

![image](../assets/img/stablereactapp/chainobject.png)

__actions__내부의 정체는, 쉽게 말하자면 현재 chaining되어 있는 lodash의 method 입니다. 문제는, 이런 method를 사용하기 위해서 라이브러리 전체가 load된다는 것입니다. 간단하게 설명하자면 다음과 같습니다.

```javascript
lodash.chain = (parameter) => wrap(parameter, lodash)
```

이렇게 된다면, build 시점에 필요하지 않은 lodash function이 load되기 때문에 결과적으로 bundle의 사이즈가 증가하게 됩니다. 그렇다면 이 문제는 어떻게 해결할 수 있을까요? 정답은 Functional Programming의 composition 개념을 이용하는 것입니다.

Composition이란 간단한 기능을 결합하여 더 복잡한 기능을 만드는 행위 또는 메커니즘입니다. 수학 함수의 일반적인 구성과 마찬가지로 각 함수의 결과는 다음 함수의 인수로 전달되고 마지막 함수의 결과는 전체 함수의 결과입니다([참고](https://en.wikipedia.org/wiki/Function_composition_(computer_science))).

개인적으로, Side effect를 발생시키지 않고 함수와 함수간에 오로지 parameter와 return 값으로만 소통하는 Functional Programming의 상징과도 같은 개념이라고 생각합니다. 다행히, lodash에는 해당 라이브러리가 이미 구현되어 있습니다.

```javascript
import {
  flow,
  replace,
  split as splitFp,
  map,
  fromPairs
} from 'lodash/fp' // import another path!
import { split } from 'lodash'

const queryStringToObject = flow(
  replace('?', ''),
  splitFp('&'),
  map(x => split(x, '=')),
  fromPairs
)
```

`flow`함수는 parameter에 있는 함수들을 차례차례 실행하는 composing function을 return합니다. 즉 queryStringToObject는 일반적인 모습으로 선언하지는 않았지만, 엄연히 `함수`인 것이죠. 이렇게 하면, 원하는 함수만 골라 load할 수 있으면서도, chain 함수를 썼을 때 비해서 가독성이 떨어지지 않게 됩니다. 이렇게 되면 chain 함수의 단점까지 보완할 수 있습니다.

개인 프로젝트에서는 lodash 내부의 함수 뿐만 아니라 제가 만든 함수들을 composing하기 위해 다음과 같은 함수를 만들어 사용하기도 합니다.

```javascript
const pipe = (...funcs: Array<() => mixed)>) => {
  return (val: any) => {
    let res = val
    funcs.reduce((acc, f) => (res = f(acc), res), res)

    return res
  }
}
```

제 첫 번째 취향이 어떤가요? 위에서 적었던 여러가지 문제를 정말 잘 해결했습니다 ~~제일 큰 것은 간지를 획득했다는 것~~.

이에 대해서는 [**`이 글`**](https://medium.com/making-internets/why-using-chain-is-a-mistake-9bc1f80d51ba)을 꼭 읽어보시길 **강력히 추천**합니다.

## 2. flow or TypeScript

Flow vs TS는 요새 FE 커뮤니티의 가장 큰 논쟁거리~~떡밥~~라고 생각이 듭니다. 특히 `flow vs TS`라는 질문은 항상 올라올 때 마다 호날두 vs 메시의 댓글전쟁을 방불케 하는 논쟁이 펼쳐지곤 합니다.

하지만 이 글에서 말씀드리고자 하는 것은 flow와 TS중 무엇이 낫다! 같은 결론이 아니라, javascript의 type check에 유용성에 대해 설명드리기 위함입니다. 특히나 저는 아직 TS로 어느 정도 규모있는 프로젝트를 진행해 보지 않았기 때문에, TS에 대해 아직 논할 수는 없다고 생각합니다.

일단 저희 팀은 [`flow`](https://flow.org/)를 적극적으로 사용하고 있습니다. 사용방법도 매우 간단한데요, flow를 프로젝트에 세팅한 후 flow check를 원하는 파일 맨 위에 "`// @flow`"라는 주석만 달아주면 끝납니다.

```javascript
// @flow

//...something code
```

위와 같이 말이죠. 정말 간단하지 않나요?

flow와 TS를 처음 보았을때 커뮤니티에서 저 두 가지를 어떻게 생각하는지 궁금했던적이 있었습니다. 그러던 중, javascript에서의 type check는 동적 타입 언어의 매력을 스스로 포기하는 것이라는 글을 보았던 적이 있습니다. 저는 정말 **바보같은 생각**이라고 자신있게 말 할 수 있습니다. 실행되는 순간 타입이 결정 되는것과 type check를 통해 strict하게 개발하는 것은 완전 다른 문제 입니다. 일단 아래를 보시죠.

```javascript
const someFunction = (val, param) => {
  const a = val[param.length - 1]
  //...something
  return some
}
```

어떤 FE개발팀에서 일하는 a는 string과 배열을 parameter로 받아서 본인이 원하는 모습으로 가공한 number를 return하는 함수를 만들었습니다. 예상대로 잘 동작하자 a는 만족합니다. 얼마간의 시간이 흐른 후, 같은 팀원 b가 이 함수를 보게 됩니다. 그리고는 이 함수의 역할에 대해서 깊은 고민에 빠집니다.

일단, 첫 줄을 보면 parameter인 val을 index로 접근하고 있으니 val은 array, object, string 셋 중 하나일 것이라는 상상을 하게 됩니다. param도 마찬가지 입니다. param역시 array이거나 string일 것 같군요. 그리고는 아래의 로직을 주-욱 따라가게 됩니다. 결국 무언가가 return은 되는데, 도대체 이 return값이 정확하게 어떤지 알 방법이 없습니다. 그래서 b는 자신이 예상한 타입의 변수를 계속 넣어보며 ~~노가다~~테스트를 해봅니다.

어느정도 시간이 흐른 후, b는 하나의 결론을 낼 수 있었습니다. val은 string 혹은 array를 받아도 동작한다는 것입니다. 그래서 b는 해당 함수를 자신의 파트에 가져다 쓰기 시작했습니다. 얼마 후 프로그램에서 버그가 발견되었습니다. 원인을 면밀히 분석한 b는 그제서야 첫 번째 parameter인 val에 array를 넣었으면 의도하지 않은 동작을 한다는 사실을 알아냅니다. 그 때 당시에 정말 운이 좋은~~건지는 모르겠지만~~ 테스트 케이스에 함수의 동작을 확신한 것이 화근이었다는 것을 깨닫게 됩니다. 그리고는 이미 자신이 맡은 부분에 여기저기 사용된 ~~지뢰~~someFunction을 보고는 ~~이때는 대략 정신이~~ 아득해 지는 것을 느끼게 됩니다.

아마 억지스럽고 비약적인 예시에 공감하지 못하시는 분들이 있으실 수도 있겠지만 슬프게도 ~~그런데 그것이 실제로 일어났습니다~~ 이 이야기는 **실화를 각색한 것**입니다. 사실 이 사고는 몇 글자면 막을 수 있었습니다. 다음과 같이 말이죠.

```javascript
// @flow

const someFunction = (val: string, param: Array<string>): number => {
  const a = val[param.length - 1]
  //...something
  return some
}
```

너무나 명확하게 함수가 변했다는 생각이 듭니다. string과 string of Array를 parameter로 받아서 number를 return한다는 사실을, a가 모든 사람들에게 일일이 설명해 주지 않아도 아무나 이 함수를 보는 순간 의도를 알 수 있습니다. 물론 "a에게 물어보지 않은 b가 잘못이다!!"라고 생각하시는 분도 있겠지만 (그렇게 ~~치사하게~~나오신다면 저는 a가 퇴사했다는 상황을 가정해버리겠습니다) 많은 인원이 모여서 코드를 관리하는 경우 이 코드를 쓴 사람을 찾아가서 의도를 일일이 물어보는 것이 더 낭비라는 생각이 듭니다.

flow는 react component에 적용할 수 있어서 더욱 매력적입니다. 특히 props나 state의 타입을 정확히 명시해 두면 `3대가 편할 수 있습니다`.

```jsx
// @flow

import React, { Component } from 'react'

type Props = {
  users: Array<User>,
  setAge: (SyntheticInputEvent<EventTarget>) => void,
  renderInside: () => React$Node,
  className?: string,
  address?: string | number,
}

type State = {
  isLoading: boolean,
}

class Test extends Component<Props, State> {
  userComments: Array<string>
  // ...something code
}
```

개인적으로 느끼기에, flow를 적용하고 나서 실수와 버그가 체감상 50% 정도는 줄어든 것 같습니다. 그 만큼 type check를 통한 strict한 개발이 효과를 본 것이겠죠. 저는 'TS가 좋다, 혹은 flow가 좋다'라는 결론은 내리지 않겠습니다. 여기서 내릴 아주 적절한 결론은 `무엇을 사용하든 type check는 정말 좋다` 입니다. type check는 `eslint`와 더불어 코드를 정말 단단하게 만들어 줄 것입니다.

> 아래의 경우라면 TS를 사용하는 것이 좋고 그렇지 않다면 flow가 좋다는 저희팀 내부의 결론이 있어 참고하시라고 남겨둡니다.
> 1. `100명 이상`의 개발자가 한 프로젝트를 함께하거나
> 2. 프로젝트를 `처음` 시작할 때 `팀원들의 합의`가 있는경우

## 3. Observable in rxjs

GOPAX 이전에 진행했었던 프로젝트에서 [rxjs](https://github.com/Reactive-Extensions/RxJS)를 사용한 적이 있습니다. 정확히는 [redux-observable](https://redux-observable.js.org/)을 말이죠. 그 당시에는 진짜, 정말 아무것도 모르고 매뉴얼에 나와있는대로 사용하기만 했었습니다. 덕분에 일반적인 패턴은 정말 기계적으로 잘 사용했~~는지 모르겠~~지만 약간 다른 패턴이 나와버리면 손을 놓아버리곤 했습니다.

```javascript
const somethingEpic = action$ =>
  action$.ofType(SOMETHING_ACTION)
    .mergeMap(action =>
      ajax.getJSON(`${API_URL}/${action.payload}`)
        .map(response => {
          if (response.result) {
            success(response)
            return
          }
          failed()
        })
        .catch(err => console.error(err))
    );
```
~~똑같은 패턴의 향연!~~ ~~나는야 패턴 복붙 기계!~~

GOPAX에도 일단 redux-observable을 사용하기 위해 해당 패키지를 깔아놓기는 했지만, web socket에 의해 완전 비동기로 움직이는 request / response를 어떻게 제어해야 할 지 감도 잡지 못했습니다. 그래서 그렇게 rxjs는 package.json에서 지우느냐 마느냐의 기로에 서버렸습니다. 하지만, 바람처럼 합류하신 [**그 분**](https://seokjun.kim)께서 rxjs를 이용해 API와 연결하는 부분을 조금씩 바꿔보자고 제안하셨고 저는 rxjs를 한 번 제대로 보기로 마음먹었습니다.

제일 처음 'rxjs란 이런것이다!'라는 감을 잡는데 도움이 되었던 글이 있습니다. [이 글](http://tech.kakao.com/2017/01/09/daummovie-rxjs/) 인데요, 이 글을 읽어보고 개인 프로젝트에 조금씩 적용해 보기로 마음먹었습니다. 제일 처음 rxjs를 이용해 만들어보기로 한 기능은 이런 스펙을 가지고 있었습니다.

1. 입력을 받고 0.5초 후에 동작
2. loading indicator를 켜고, db에 키워드를 보내 검색
3. 검색이 되는 동안 현재 url에 키워드를 이용한 url query를 붙여주고
4. db에서 response가 왔을시,
    - 오류가 있으면 오류를 표시
    - 오류가 없다면 loading indicator를 끄고, redux에 response data를 담아주기

여러번의 삽질 끝에, rxjs를 이용한 코드는 아래와 같았습니다.

```javascript
Observable
  .fromEvent(this.inputForm, 'keyup')
  .debounceTime(500)
  .map(e => e.target.value)
  .mergeMap(keyword => {
    this.setState({
      isLoading: true,
    })

    const queryObject = queryStringToObject(location.search)
    queryObject.search = keyword

    const requestUrl = queryObjectToString(queryObject)
    this.props.history.push(`/?${requestUrl}`)

    return Observable.ajax.getJSON(
      `${DB}/post/posts?${requestUrl}`
    )
  })
  .catch((error, caught) => {
    this.setState({
      isLoading: false,
    })

    return caught
  })
  .subscribe(result => {
    this.setState({
      isLoading: false,
    })
    
    if (result) {
      this.props.getPostListSuccess(result)  // redux action
      return
    }

    this.props.getPostListFailed()
  })
```

뭔가 길지만, 복잡한 액션을 의식의 흐름처럼 작성할 수 있습니다. rxjs에 대해 잘 모르고 보셔도 어느정도 이해하실 수 있을거라 생각합니다. rxjs를 사용하지 않고, 다른 방법을 이용해 이 기능을 작성한다고 생각하면, 딱히 좋은 방법이 떠오르지 않습니다(제가 rxjs를 많이 쓰다보니 그럴수도 있을거라 생각합니다. 늘 말씀드렸지만 이보다 더 좋은 해결책은 언제나 환영입니다). 개인적으로 특히 좋았던 부분은 redux action과 ui를 쉽게 연계할 수 있었다는 점입니다.

React를 시작한지 얼마 되지 않았을 때의 큰 고민은, request -> processing -> response의 세 단계별로 각각 ui를 바꾸어 주어야 하는 경우였습니다. 대표적인 것이 db에서 data가 response 될 때까지 loading indicator를 보여주는 것 입니다. 

부끄러운 이야기지만, 그 전에는 아무런 아이디어가 없어서 redux state에 각각의 loading state를 관리해줬습니다(그리고는 container에서 저 아래의 자식 component로 props를 넘기고 넘기고 넘기고...). 그러다보니 `is...Loading`이라는 이름의 redux state가 한없이 늘어나는 지옥을 경험했었습니다.

다시 GOPAX 프로젝트의 이야기로 넘어오겠습니다. 사실 GOPAX에 처음부터 rxjs를 적용했던 것은 아닙니다. 그렇다고 redux에 `is...Loading` state를 마구 넣은것도 아니었구요. 대신 꽤나 tricky한 방법을 사용했었습니다. 해결책이라고 하지 않은 이유는 방법과 모습이 weird했기 떄문입니다(바로 Promise를 사용한 방법인데요, 이 부분에 대해서는 따로 posting을 하겠습니다). 

Promise를 사용하다 보니 코드가 많이 복잡해질 수 밖에 없었는데, rxjs를 도입하고 나서 굉장히 직관적이고 깔끔하게 바뀌었습니다. **그 분**께서 제시하신 패턴은 대략 아래와 같습니다.

```javascript
handleClick = () => {
  const { someApi, someApiSuccess, someApiFailed } = this.props

  this.setState({
    isLoading: true,
  })

  Observable
    .of(someApi())
    .mergeMapTo(socketStream$)
    .filter(frame => frame.apiName === 'someApi')
    .take(1)
    .subscribe(frame => {
      this.setState({
        isLoading: false,
      })

      if (frame.result) {
        someApiSuccess(frame.payload)
        return
      }

      someApiFailed()
    })
}
```

정말 깔끔합니다. 

> API를 호출하고 -> socket stream에서 해당 api가 response 되기를 기다렸다가 -> response가 오면 1개의 response를 받아서 -> response 결과에 따라 redux action 호출

rxjs는 정말 강력하고 간결하지만, 다른 분들에게 '쓰세요, 두 번 쓰세요'라고 말하기 힘든 부분이 있습니다. 바로 `learning curve`의 문제 때문인데요, 제가 느낀바로는 기본 concept부터 이해하는데 조금 어려움이 있고, 생각보다 공식 문서가 친절하지 않습니다 ~~보시면 압니다~~ ~~동그라미가 그래서 뭐 어쩌라고 확 마 씨~~.

하지만 익숙해지기만 한다면 정말 좋은 선택이 될 것이라 생각합니다(그래서 저는 오늘도 rx를 공부하러...). rxjs의 기본 컨셉을 알고자 하신다면 [이 글](http://huns.me/development/2051)을 읽어보시면 좋습니다(저도 다시 읽어 봐야겠네요).

> 마찬가지로, rxjs에 대한 저희 팀원들의 의견을 참고하시라고 남겨둡니다.
> 1. 도입하려면 정말 한 사람도 빼놓지 않고 모두의 동의가 있어야 함(코드 리뷰를 위해서).
> 2. 사용하면 정말 좋은데 처음보면 정말 읽기 힘든 코드. 하지만 알고보면 깔끔하다.
> 3. 처음에는 공부하기 어렵고 와닿지 않음.
> 4. 이게 뭐임? 으로 시작해서 몇번 써보면 '아 조금 알겠다'. 그 다음부터는 rx 덕후.
> 5. thunk를 대체할 좋은 라이브러리. 하지만 어렵고 거부감이 든다면 redux-saga가 좋은 선택일수도.


## 4. Unit test

Unit test의 장점에 대해서 설명하자면 정말 끝도없이 설명할 수 있습니다(정말 3시간은 쉬지않고 말할 수 있습니다). 개인적으로 `'unit test를 작성하는데에도 시간이 걸리니 사실상 2배의 시간이 걸리는거 아니냐. 시간낭비다.'`라고 말씀하시는 분들과는 친분관계를 쌓고있지 않을 정도로 저는 unit test를 정말 중요하게 생각하고 있습니다(오해가 있을까봐 말씀 드리지만 unit test의 가치를 아예 폄하하시는 분들을 말씀드린겁니다). 이 것도 역시 **그 분**에게 받은 영향인데요, 저도 어느 순간 부터인가 util function을 작성하고 반드시 test case를 작성하는 습관이 생겼습니다.

GOPAX 프로젝트에서는 [mocha](https://mochajs.org/) + [chai](http://www.chaijs.com/)를 사용하고, 개인적으로는 [jest](https://facebook.github.io/jest/)를 선호합니다. 왜냐하면 다음의 세가지 이유 때문입니다(결국 개인 차이지만요).

1. coverage check가 내장되어있어 쉽게 옵션으로 설정 가능함
2. chai의 도움이 크게 필요 없음
3. test에 대한 설정을 따로 관리하기 편함

Test case를 작성함으로 얻는 이득은 정말 많습니다. 물론 작성할 때는 시간이 걸리지만, 나중에 들이는 시간을 생각하면 오히려 *시간을 절약*하게 되는 마법의 코드입니다. 게다가 작성하는 것도 어렵지 않습니다. 작성하는 방법은 공식문서에 더 자세히 나와있지만, 대략 아래와 같습니다.

```javascript
// queryUtils.js
import { split, join } from 'lodash'
import {
  flow,
  replace,
  split as splitFp,
  fromPairs,
  toPairs,
  join as joinFp,
  map
} from 'lodash/fp'

export const queryObjectToString: ({ [string]: string | number }) => string =
  flow(
    toPairs,
    map(x => join(x, '=')),
    joinFp('&')
  )

export const queryStringToObject: (query: string) => { [string]: string | number } =
  flow(
    replace('?', ''),
    splitFp('&'),
    map(x => split(x, '=')),
    fromPairs
  )

// queryUtils.spec.js
import { 
  queryObjectToString,
  queryStringToObject
} from './queryUtils'

describe('query util test', () => {
  test('queryObjectToString should be return query string from option object', () => {
    expect(queryObjectToString({
      page: 1,
      q: 'best'
    })).toEqual('page=1&q=best')
  })

  test('queryStringToObject should be return object from option qurey string', () => {
    expect(queryStringToObject('?page=1&q=best')).toEqual({
      page: '1',
      q: 'best'
    })
  })
})
```
물론 test case에 대해서 일일이 생각하고 작성하는것이 귀찮기는 하지만, 이런 노력을 들임으로 인해 얻을 수 있는 이득은 아주 큽니다.

1. 특정 function을 refactoring한 이후, 원래의 기능이 동작하는지의 여부를 간단히 테스트 가능
2. 테스트 케이스에 따라 function을 작성하면 되기 때문에 첫 개발단계에서 부터 안정성을 확보할 수 있음
3. 배포 혹은 push 전 unit test를 전체적으로 실행하여, production의 안정성을 확보 ~~마음의 안정성은 덤~~

처음에는 쉽지 않았지만, test code를 작성하는 습관을 들이고 나니 시간이 절약된다는 것을 조금씩 체감하고 있습니다. test case를 작성하는 과정에서 미처 고려하지 못했던 치명적인 에러에 대해서도 처음부터 잡아낸 경우도 꽤 많이 있습니다. 그 만큼 들이는 노력에 비해서 확실한 효율을 보장한다고 생각합니다.

**여러분, 테스트 코드를 쓰세요. 두 번 쓰세요.**

## 5. Conclusion
모든 `개발자의 숙명`은 1. refactoring과 2. 단단하면서도 좋은 패턴을 갖고있는 3. scalable한 코드에 대한 고민이라고 생각합니다. 위에 소개해 드린 것들은 `개발자의 숙명`에 대한 고민을 조금 덜 수 있는 취향이라고 120% 확신하고 있습니다. 실제로 저것들을 하지 않았을때와 하고난 후의 차이가 체감이 될 정도로 간극이 크니까요.

아무튼, 저의 대표적인 개인취향 네 가지를 소개해 드렸습니다. 물론 읽으시는 분들에 따라 불편~~구토~~하셨을 수도 있으리라 생각합니다. 여러분들의 의견은 어떠신가요?