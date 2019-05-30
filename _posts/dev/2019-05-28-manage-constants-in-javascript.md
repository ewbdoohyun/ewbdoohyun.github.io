---
title: Javascript에서 constants 관리하기
layout: post
comments: true
category: [dev, react, javascript]
---

`/components/users/constants.js`

```javascript
const userStatesActive = 0;
const userStatesInActive = 1;
const userStatesLabels = {
  [userStatesActive]: "active user",
  [userStatesActive]: "active user"
};
export default {
  userStatesActive,
  userStatesInActive,
  userStatesLabels
};
```

`/components/users/userStates.jsx`

```javascript
import {userStatesLabels} from './constants'

...

  render{
    const {state} = this.props;
    return (
      <span>{userStatesLabels[state]}</span>
    )
  };
```
