---
layout: post
comments: true
title:  "React Fragment와 div"
category: [frontend, react]
---

React Fragment와 div



Code들을 보게 되면 가끔 다음과 같은 것들을 볼 수 있을 것입니다.

```react
const AppContainer = ({data}) => (
	<React.Fragment>
      <ChildA />
      <ChildB />
    </React.Fragment>
)
```

이 이유는 하나만 리턴이 가능한 특성에서, 여러 값을 불필요한 div로 묶지 않고 해결 하기 위해 나온 방법입니다.

Fragment없이 구현을 하려면 다음과 같이 구현해야 하는데.

```react
class Columns extends React.Component {
  render() {
    return (
      <div>
        <td>Hello</td>
        <td>World</td>
      </div>
    );
  }
}
```

이 이유는 하나만 리턴이 되려면 무언가로 묶어야 하는데, 이 경우 Column을 각각 사용하거나, 혹은 위와 같이 하나의 div로 묶어야만 합니다.

발생할 수 있는 문제는

```react
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}
```

가 render 된다면 tr 아래 td가 아닌 div가 들어가기 때문입니다.

```react
<table>
  <tr>
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table>
```

- 다음과 같이 output이 뽑혀져 나오면 당황스럽겟죠.



PS. 그리고 지금은 다음과 같은 것들도 지원을 한다고 합니다!

```react
class Columns extends React.Component {
  render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
출처 : https://reactjs.org/docs/fragments.html#short-syntax
```



출처 : <https://reactjs.org/docs/fragments.html>



