---
layout: post
title: React 03) State, Props, Children
date: 2018-04-07 14:00:00 +0900
description: # Add post description (optional)
img: ./react01/react-background.png # Add image post (optional)
tags: [frontend, React] # add tag
---


> React Component를 설계, 구성하는데 반드시 고려해야할 요소들인 State, Props, Children을 알아봅니다.

지난 시간에 Component에 대해서 알아보았었습니다 ([다시보기](https://stardustrain.github.io/react-hello-world/)). 지난 시간의 글을 읽어보신다면, 분명히 큰 불편함을 느끼셨을 겁니다. 바로 Dom element안에 사용자들에게 보여줄 data가 hard coding되어있었기 때문입니다 (개인적인 생각으로 **개발자**라면 `hard coding에 대해서 본능적인 적개심`이 있어야 한다고 생각합니다). 그렇다면, 본능을 일깨우기 위해서 ~~치가 떨렸을법한~~ 저번 시간의 코드를 다시 가져와 보죠.

```jsx
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
```

이 ~~극혐~~코드를 보면서 다음과 같은 합리적인 의심이 발생한다면 정말 좋은 현상입니다.
> - 할 일중'글쓰기'가 끝났을때, 스타일 변경 혹은 목록에서 삭제를 해야 한다면 어떻게 할 것인가(~~새로운 배포가 답~~)?
> - 반대로, 새로운 할 일을 추가해야 한다면 어떻게 할 것인가?
> - 궁극적으로, 이런 서비스를 만든다고 했을 때, 각 이용자 마다 다른 할 일을 어떻게 나타낼 것인가?

React는 이런 문제들을 `State`와 `Props`라는 방법으로 관리하고 있습니다. 저번 포스트에서 사용했었던 Component들을 간단히 Refactoring해 가면서 State와 Props의 개념을 알아보겠습니다.

## 1. State
React의 모든 Component는 자신의 상태, 즉 '`State`'를 가질 수 있습니다. Component는 State를 이용하여 화면을 그려주거나, 자식 Component 상태를 변경할 수 도 있습니다(그 밖의 여러가지 일이 가능합니다).  
아까의 코드를 좀 더 합리적이고, 보기 좋은 형태로 바꾸어 보겠습니다. 아래의 코드를 Refactoring하는 과정에서 처음보는 기능들이 등장할 수도 있지만 너무 신경쓰지 마시고, State자체의 역할에 대해 보아 주시길 바랍니다.

```jsx
// ./TodoItemList.js
import React, { Component } from 'react'

class TodoItemList extends Component {
  state = {
    todoItems: [
      '글쓰기',
      '운동하기',
      'React 공부',
      '다른 사람 글 읽기',
      '산책하기',
      '다이어트',
      '식단관리',
      '자료구조 114페이지',
    ]
  }

  render() {
    return (
      <ul>
        {this.state.todoItems.map(item => (
          <li key={item}>{item}</li>
        ))}
      </ul>
    )
  }
}
```

TodoItemList를 이렇게 바꾸어도 화면은 변함이 없습니다. 하지만 code상으로는 정말 중요한 변화가 발생하였다는 것을 너무 쉽게 확인하실 수 있으실 수 있습니다. 바로, 화면에서 보여지는 부분이 State에 따라 유동적으로 바뀔 수 있다는 것입니다. 즉 State에 선언되어있는 todoItems를 바꿔주면, 그에 따라 화면이 유동적으로 변한다는 말 입니다. 이번에는 버튼 클릭을 통해 todoItems를 변경해줄 수 있도록 코드를 약간 수정하여 봅니다.

```jsx
// ./TodoItemList.js
import React, { Component } from 'react'

class TodoItemList extends Component {
  state = {
    todoItems: [
      '글쓰기',
      '운동하기',
      'React 공부',
      '다른 사람 글 읽기',
      '산책하기',
      '다이어트',
      '식단관리',
      '자료구조 114페이지',
    ]
  }

  addRandomTodo = () => {
    this.setState({
      todoItems: [
        ...this.state.todoItems,
        '새로운 할 일',
      ]
    })
  }

  render() {
    return (
      <ul>
        {this.state.todoItems.map((item, index) => [
          <li key={item}>{item}</li>,
          <button
            key={`TodoItemListButton-${index}-${item}`}
            onClick={this.addRandomTodo}
          >
            할 일 추가하기
          </button>
        ])}
      </ul>
    )
  }
}
```

> - 이제 각 할 일의 밑에 새로운 버튼들이 그려지게 되었습니다.
> - 이 버튼은 클릭할 때 마다 addRandomTodo를 호출하게 되고, addRandomTodo는 호출될 때 마다 state의 todoItems에 새로운 요소를 push하게 됩니다.
> - React의 State는 반드시 **`setState()`** 함수를 통해 변경해야 합니다.

버튼을 클릭하게 되면 이제 다음과 같은 화면의 변화가 있을 겁니다.

![image](../assets/img/react03/click-add-random-todo.gif)

자, State와 간단한 기능을 넣었을 뿐인데 Component의 기능이 좀더 유연하고, 유동적인 모습으로 바뀌었다는 것을 금방 느낄 수 있습니다. 혹시 첫 번째 포스트에서 설명드렸던 [**`One Way Data Flow`**](https://stardustrain.github.io/what-is-a-react/)를 기억하고 계신다면 이해가 더욱 빠를것이라 생각합니다. Compoenent의 State(Data)가 화면에 보여지고(View), 이 화면에서 발생한 클릭(Action)이 State(Data)를 변경시켜, 다시 화면(View)에 반영되었죠.

## 2. Props
음, 그런데 잘 생각해보니 TodoItemList가 가지고 있는 todoItems를 여러 Component들이 알아야 할 수도 있다는 생각이 듭니다. 기억하실지 모르겠지만, 우리는 TodoInput이라는 할일 입력만을 담당하는 Component를 만들었기 때문입니다(기억이 나...시나요?).  

이런 고민을 React에서는 쉽게 해결 할 수 있습니다. 바로 `부모` Component의 State를 `자식`에게 알려주는 방법인데요, 그것이 바로 `Props`라는 것 입니다.  

그렇기 때문에 todoItems는 TodoItemList가 아닌 TodoList의 State인 것이 더 맞는것 같습니다. 왜냐하면 TodoList는 TodoItemList와 TodoInput라는 두 Component를 가지고 있는 `부모` Component기 때문이죠.  

너무 많은 개념들이 한번에 나와서 조금 힘들게 느껴지수도 있지만, 한 번 이녀석을 이해해 보겠다는 생각으로 다시 코드를 써보겠습니다.

```jsx
// ./TodoList.js
import React, { Component } from 'react'

import TodoInput from './TodoInput'
import TodoItemList from './TodoItemList'

class TodoList extends Component {
  state = {
    todoItems: [
      '글쓰기',
      '운동하기',
      'React 공부',
      '다른 사람 글 읽기',
      '산책하기',
      '다이어트',
      '식단관리',
      '자료구조 114페이지',
    ]
  }

  render() {
    return (
      <div className="App">
        <h2>Todo List</h2>
        <TodoInput />
        <TodoItemList
          todoItems={this.state.todoItems}
        />
      </div>
    )
  }
}

export default TodoList

// ./TodoItemList.js
import React, { Component } from 'react'

class TodoItemList extends Component {
  addRandomTodo = () => {
    this.setState({
      todoItems: [
        ...this.state.todoItems,
        '새로운 할 일',
      ]
    })
  }

  render() {
    return (
      <ul>
        {this.props.todoItems.map((item, index) => [
          <li key={item}>{item}</li>,
          <button
            key={`TodoItemListButton-${index}-${item}`}
            onClick={this.addRandomTodo}
          >
            할 일 추가하기
          </button>
        ])}
      </ul>
    )
  }
}

export default TodoItemList
```

> - todoItmes는 이제 TodoList의 State가 되었습니다.
> - todoItems를 ```todoItems={this.state.todoItems}``` 이런 방법으로 TodoItemList에 전달 합니다.
> - TodoItemList의 입장에서는 todoItems는 이제 State가 아닌, 부모로 부터 전달받은 Props 입니다.

코드를 바꾸어 보셨나요? 화면은 여전히 변함이 없습니다. 하지만 버튼을 클릭하게된다면, 에러가 발생합니다. 바로, TodoItemList의 State에 더 이상 todoItems가 없다는 에러죠. 아까도 말씀드렸지만, 우리는 이러한 문제를 해결하기 위해 TodoInput를 만들어 놓았습니다. 더 이상 TodoItemList에서 todoItems의 변경에 관여하지 않게 코드를 바꿔보겠습니다.

```jsx
// ./TodoList.js
import React, { Component } from 'react'

import TodoInput from './TodoInput'
import TodoItemList from './TodoItemList'

class TodoList extends Component {
  state = {
    todoItems: [
      '글쓰기',
      '운동하기',
      'React 공부',
      '다른 사람 글 읽기',
      '산책하기',
      '다이어트',
      '식단관리',
      '자료구조 114페이지',
    ]
  }

  addTodoItems = item => () => {
    this.setState({
      todoItems: [
        ...this.state.todoItems,
        item,
      ]
    })
  }

  render() {
    return (
      <div className="App">
        <h2>Todo List</h2>
        <TodoInput
          addTodoItems={this.addTodoItems}
        />
        <TodoItemList
          todoItems={this.state.todoItems}
        />
      </div>
    )
  }
}

export default TodoList

// ./TodoInput.js
import React, { Component } from 'react'

class TodoInput extends Component {
  state = {
    todoItem: '',
  }

  handleChange = (e) => {
    this.setState({
      todoItem: e.target.value,
    })
  }

  render() {
    return [
      <input
        key="TodoInput-input"
        type="text"
        placeholder="할일 입력해보기"
        onChange={this.handleChange}
      />,
      <button
        key="TodoInput-button"
        onClick={this.props.addTodoItems(this.state.todoItem)}
      >
        입력하기
      </button>,
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
        {this.props.todoItems.map(item =>
          <li key={item}>{item}</li>,
        )}
      </ul>
    )
  }
}

export default TodoItemList
```

> - todoItem을 추가하는(todoItems라는 state를 변경하는) 함수가 TodoList에 선언되어있고 TodoInput은 이 함수를 Props로 받았습니다.
> - TodoInput에 있는 버튼을 클릭하면, 부모 Component인 TodoList에서 받은 addTodoItems에 todoItem이라는 State를 parameter로 넣어 실행하게 됩니다.
> - TodoItemList는 부모로부터 받은 todoItems라는 Props가 바뀔때 마다 화면을 다시 그리게 됩니다. 그리고 단지 todo item만을 보여주는 가볍고 명확한 Component로 변했습니다.

무언가 많이 정신 없으시죠? 하지만 이왕 코드를 많이 바꾸었으니 우리가 의도한 대로 기능이 동작하는지 확인해 봅니다.

![image](../assets/img/react03/input-todo.gif)

So Coooooooooool 합니다! 우리가 원하는 대로 Component들의 기능이 동작하고 있습니다. 더불어, 위에서 설명했다시피 각 Component들의 역할이 좀 더 명확해지고 가벼워졌습니다.

Props를 사용하실때의 주의점은, Props는 **`Read-Only`** 값이라는 것 입니다. 부모로부터 받은 Props는 **`절대 변경해서는 안됩니다.`** 만약 Props의 변경이 필요하다면, `부모`에서 변경하여 `자식`으로 값을 내려보내는 것이 맞습니다(혹은 자식 Component의 State에 담아 쓰는 방법도 있습니다).

## 3. Children
많이 길었지만, State와 Props에 대해 명확히 이해하셨다면 children은 정말 쉽게 이해할 수 있습니다.  

쉽게말해 Children은 어떤 컴포넌트에 nested되어있는 컴포넌트에 접근하고자 할 때 사용합니다. 아래에 간략한 코드를 적어 놓았습니다.

```jsx
// ./Button.js
<button>
  {this.props.children}
</button>

// ./Confirmbutton.js
<Button>
  확인
</Button>

// ./Cancelbutton.js
<Button>
  취소
</Button>
```

> - Confirmbutton은 '확인'이라는 글자가 보이는 button이 그려집니다. 이 컴포넌트가 그려질때, this.props.children은 '확인'이라는 string 입니다.
> - Cancelbutton은 '취소'라는 글자가 보이는 button이 그려집니다. 이 컴포넌트가 그려질때, this.props.children은 '취소'라는 string 입니다.

this.props.children에는 string, undefined, React node, array등이 올 수 있습니다.  
Children은 기본적으로 Props에 선언되는 특수 property 중 하나입니다. 그러므로, 평소 Props를 사용하실 때 key, children 등의 naming은 피하셔야 합니다.

## 4. Immutable?
`Immutable`은 State와 Props를 다룰때 정말 중요한 개념 중 하나입니다. 이를 알아보기에 앞서 javascript의 재미있는 특성을 알아보고자 합니다. 모두 Chrome을 켜주시고, 개발자 도구의 console을 열어주세요.

그리고 다음과 같이 입력해 봅니다.
```javascript
const a = [1, 2]
const b = a

a === b
// true
b.push(3)
console.log(b)
// [1, 2, 3]
a === b
// true
```

아시고 계시는 분들에게는 그리 놀라운 일이 아닙니다만, b의 값이 바뀌었음에도 a와 b는 같다고 합니다. 그럼 이번에는 object를 테스트 해 볼까요?

```javascript
const a = {b: 1, c: 2}
const b = a

a === b
// true
b.c = 3
console.log(b)
// {b: 1, c: 3}
a === b
// true
```

음, 마찬가지로 b에 속해있는 property인 c의 값이 바뀌었음에도 계속 a와 b는 같다고 합니다. 정말 놀라운(~~동시에 근본없는~~) javascript의 특성입니다.  javascript는 기본적으로 object, array등을 '==='를 통해 비교할 때 그 내부의 값이 아닌 `참조값`으로 비교하기 때문입니다. 위의 코드에서 a와 b는 같은 인스턴스를 참조하고 있기 때문에 내부의 값은 다르지만 같은 객체로 javascript는 판단하고 있는 것 입니다.

React에서도 기본적으로 State와 Props의 변경을 참조값으로만 판단합니다(물론 shouldComponentUpdate()를 통해 개발자가 deep diff check를 할 수도있지만 이것은 나중에 다루겠습니다). TodoList의 코드를 잠시 바꾸어 볼까요?

```jsx
// ./TodoList.js
import React, { Component } from 'react'

import TodoInput from './TodoInput'
import TodoItemList from './TodoItemList'

class TodoList extends Component {
  state = {
    todoItems: [
      '글쓰기',
      '운동하기',
      'React 공부',
      '다른 사람 글 읽기',
      '산책하기',
      '다이어트',
      '식단관리',
      '자료구조 114페이지',
    ]
  }

  addTodoItems = item => () => {
    this.state.todoItems.push(item)
    console.log('add item!', this.state.todoItems)
    // this.setState({
    //   todoItems: [
    //     ...this.state.todoItems,
    //     item,
    //   ]
    // })
  }

  render() {
    console.log('render!', this.state.todoItems)
    return (
      <div className="App">
        <h2>Todo List</h2>
        <TodoInput
          addTodoItems={this.addTodoItems}
        />
        <TodoItemList
          todoItems={this.state.todoItems}
        />
      </div>
    )
  }
}

export default TodoList
```
정말 간단한 변화입니다. 우리는 state에 있는 todoItems에 push()함수를 통해서 입력된 item을 직접 넣어보았지만, 아무리 입력 버튼을 클릭해도 화면이 변하지 않습니다. 심지어 console.log()도 최초 Component가 그려졌을 시에만 호출이 되고, 그후로는 호출되지 않습니다.  
![image](../assets/img/react03/not-immutable.gif)
바로 Component가 todoItems의 참조값이 변하지 않았다고 판단했기 때문에 TodoList의 render()함수가 재 호출되지 않는 것 입니다.  

그렇기때문에, 우리는 State와 Props의 값을 `Immutable하게 관리`해야 합니다. Immutable이란 `불변의`라는 뜻으로, 객체 내부의 값이 변하지 않는 것을 의미합니다. 이는 다른말로 해석하자면, 객체 내부의 값이 바뀌었다면 **`반드시 새 객체로 생성`** 해야 한다는 것을 의미합니다. 그렇기 때문에 State를 안전하게 바꿔주기 위해서 React에서는 반드시 `setState()`를 사용할 것을 권고하고 있습니다.  

Props의 경우 한 가지 고민에 봉착하게 됩니다. 바로 _Props로 자식에게 전달할 모든 것들을 State에 관리해야하나?_ 라는 고민입니다. 결론부터 말씀드리자면, **아니오**입니다. 다행히도 javascript에서는 새 객체를 생성하는 행동을 아주 쉽게 할 수 있는 문법을 지원합니다. 바로 [`spread oerator`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_operator)입니다.

아까와 마찬가지로 Chrome의 개발자 도구를 열고 console에 다음과 같이 입력해 봅니다.

```javascript
const a = [1, 2]
const b = [...a]
console.log(b)
// [1, 2]
a === b
// false
```
배열 내부의 값은 똑같지만 **다르다**라고 판단하고 있습니다. 이번에는 내부의 값을 바꿔 볼까요?

```javascript
const a = [1, 2]
const b = [...a, 3]

console.log(b)
// [1, 2, 3]
a === b
// false

const c = {b: 1, c: 2}
const d = {...a, c: 3}

console.log(d)
// {b: 1, c: 3}
c === d
// false
```
결과를 놓고 보았을 때, spread operator는 새 객체를 생성해 준 다는 것을 알 수 있습니다. 이렇게 되면 React의 Component는 '아, Props가 바뀌었네!'라고 판단할 수 있을 것입니다.

> 그렇기 때문에, 객체의 내부 값을 비교할 때에는 [`lodash`의 `isEqual`함수](https://lodash.com/docs/4.17.5#isEqual)를 사용하시는 것을 개인적으로 추천드립니다.

## 5. Conclusion
이번 포스트에서는 꽤 많은 내용을 다루어 보았습니다. 많이 혼란스럽고 어려우실 수도 있으시겠지만, 이런 개념을 아는 것과 모르는 것은 큰 차이가 있다고 생각합니다(~~삽질의 시간도~~). 여기까지 오셨다면 이미 React와 50%정도 친해지셨다고 생각하셔도 좋습니다! 나머지는 익숙해 지는 정도의 문제니까요. 그렇기 때문에 무엇보다 오늘의 포스트는 반드시 이해하시고 넘어가셨으면 좋겠습니다.

> 이해가 안가신다면 메일(lucas.han.public@gmail.com)이나 댓글로 꼭 질문해 주세요.

---
#### 참고 링크
> https://reactjs.org/docs/state-and-lifecycle.html  
> https://blog.coderifleman.com/2015/06/24/learning-react-2/  
> http://adripofjavascript.com/blog/drips/object-equality-in-javascript.html  
> https://blog.coderifleman.com/2015/08/16/react-and-immutable/  
> https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_operator
---