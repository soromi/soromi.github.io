---
layout: post
title: "10가지 컨셉으로 React 이해하기"
tags: [TIL, react, translation]
comments: true
permalink: /blog/:title/
---

Chris Achard의 [Learn React in 10 tweets (with hooks)](https://twitter.com/chrisachard/status/1175022111758442497?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1175022111758442497&ref_url=https%3A%2F%2Fpublish.twitter.com%2F%3Fquery%3Dhttps%253A%252F%252Ftwitter.com%252Fchrisachard%252Fstatus%252F1175022111758442497%26widget%3DTweet)을 번역한 글입니다.

오역, 의역이 있을 수 있습니다.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">🔥 Learn React in 10 tweets (with hooks) 👇</p>&mdash; Chris Achard (@chrisachard) <a href="https://twitter.com/chrisachard/status/1175022111758442497?ref_src=twsrc%5Etfw">September 20, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## 1. React 작동 방식

-   웹 페이지에 데이터를 보여줍니다.
-   사용자가 상호 작용합니다.
-   그로인해 데이터가 변경됩니다.
-   웹 페이지가 다르게 보여집니다. (우리가 원하는 것! 🙌🏻)

이러한 작동 방식을 통해 UI를 업데이트 할 수 있습니다.

<!-- **#The React UI Cycle** -->

![The React UI Cycle](../../images/blog/2019-09-22-learn-react-in-10/react_ui_cycle.jpg)

## 2. 컴포넌트(component)

UI를 커스텀 컴포넌트(component)로 나눕니다.

각 컴포넌트는 사용 가능한 외부 및 내부 데이터를 기반으로 자체 **표현을 담당**합니다.

예를 들면, 다음과 같은 컴포넌트 트리를 작성 할 수 있습니다.

(ex. Contariner, SearchBar, TodoList, TodoItem)

![ex. ui component](../../images/blog/2019-09-22-learn-react-in-10/ex_ui_component.jpg)

## 3. JSX

**컴포넌트는 JSX를 반환하는 함수**입니다.

JSX는 HTML처럼 보이지만 실제로는 JavaScript입니다.

JSX 내부에서는 중괄호를 사용하여 JS를 포함합니다.

JS가 혼합 된 HTML을 볼 때 많은 사람들이 거부감을 느끼지만, 사실 React를 멋지게 만드는 요소 중 하나입니다.

![ex. JSX](../../images/blog/2019-09-22-learn-react-in-10/ex_jsx.jpg)

## 4. 컴포넌트를 정의하고 사용하기

함수 컴포넌트를 정의한 후에는 다른 컴포넌트 내에서 사용할 수 있습니다.

이 방법을 사용하여 전체 UI를 정의하는 컴포넌트 트리를 작성합니다.

![ex. use component](../../images/blog/2019-09-22-learn-react-in-10/ex_use_component.jpg)

## 5. props

**컴포넌트 외부에서 들어오는 데이터**를 "props(properties)"라고 합니다.

props는 JSX 프로퍼티를 통해 부모에서 자식으로 전달될 수 있습니다.

prop는 함수의 첫 번째 인수(argument)로써 함수 컴포넌트로 들어옵니다.

![ex. props](../../images/blog/2019-09-22-learn-react-in-10/ex_props.jpg)

## 6. state

**컴포넌트 내부의 변경가능한 데이터**를 "state"라고합니다.

state는 데이터를 반환하는 `useState` 함수, 해당 데이터를 변경하는 함수로 정의됩니다.

state를 직접 설정하지 말고, **함수를 사용해서 설정**해야합니다. (이미지 참고)

`setState`를 통해 변경하는 방법도 있지만, 가독성 등의 문제로 hooks가 도입되었습니다. (`useState`는 hooks.)

https://dev-momo.tistory.com/entry/React-Hooks

https://moonformeli.tistory.com/26

![ex. useState](../../images/blog/2019-09-22-learn-react-in-10/ex_useState.jpg)

## 7. 컴포넌트 업데이트

state 또는 props가 변경되면 컴포넌트가 자동으로 업데이트됩니다

이것이 바로 React의 마술입니다!✨

![ex. update component](../../images/blog/2019-09-22-learn-react-in-10/ex_update_component.jpg)

## 8. map

`map`으로 데이터 배열을 반복하여 목록을 생성하고, 각 루프 반복에서 요소를 반환합니다.

생성된 목록의 각 요소에 고유한 `key`를 제공하여 최상의 성능을 보장합니다.

![ex. map](../../images/blog/2019-09-22-learn-react-in-10/ex_map.jpg)

## 9. 컴포넌트 스타일링 방법

기본 방법에는 2가지가 있다.

1. `className`으로 클래스를 설정하고 일반 CSS 파일 사용

2. 이중 중괄호를 사용한 인라인 스타일 설정  
   (css 속성은 카멜 표기법으로 작성)

![ex. style](../../images/blog/2019-09-22-learn-react-in-10/ex_style.jpg)

## 10. useEffect, fetch

`useEffect` 내부에서 `fetch`를 사용해 비동기 함수 및 부작용을 수행합니다. (콜백 발생)

`useEffect`의 두 번째 인수(argument)에 넣은 데이터가 변했을 때에만 실행됩니다.

첫 렌더링 이후 업데이트가 필요없다면, 비어있는 배열을 넣어주면 됩니다.

![ex. useEffect](../../images/blog/2019-09-22-learn-react-in-10/ex_useEffect.jpg)
