---
title: React + ESLint + VSCode
layout: post
comments: true
comments: true
category: [dev, react, eslint, vscode]
---

## VSCode eslint 설치

- command + shift + p
- Install Extensions
- Search 'ESLint'
- Click Install

## 저장시 eslint 자동 고침 설정

- command + ,
- Search 'ESLint'
- Turn on auto fix on save

## eslint-config-airbnb 적용

```
// Package 확인
npm info "eslint-config-airbnb@latest" peerDependencies
// with CRA v2.0
npm install eslint-config-airbnb --save
// without CRA
npx install-peerdeps --dev eslint-config-airbnb
```

## eslint-config-prettier 적용

```
npm install eslint-config-prettier --save
```

`.eslintrc`

```
  extends: [
    prettier
  ]
```

## eslint config

`.eslintrc` 파일 생성

```
module.exports = {
  env: {
    browser: true,
    es6: true,
    jest: true
  },
  extends: [
    "eslint:recommended",
    "plugin:react/recommended",
    "airbnb",
    "prettier",
    "prettier/babel",
    "prettier/react",
    "prettier/standard",
    "prettier/unicorn",
    "prettier/vue"
  ],
  globals: {
    Atomics: "readonly",
    SharedArrayBuffer: "readonly"
  },
  parser: "babel-eslint",
  parserOptions: {
    ecmaFeatures: {
      jsx: true
    },
    ecmaVersion: 2018,
    sourceType: "module"
  },
  plugins: ["react"],
  rules: {
    "react/prefer-stateless-function": 0,
    "import/no-named-as-default": 0,
    "react/jsx-filename-extension": 0,
    "import/no-extraneous-dependencies": [2, { devDependencies: true }]
  }
};

```

## 특정 lint rule 제거하기

`.eslintrc`

```
 rules: {
    "react/prefer-stateless-function": 0,
 }
```

## dev save module 이 project 외부에 있다고 나올 때

```
  rules: {
    "import/no-extraneous-dependencies": [2, { devDependencies: true }]
  }
```

## Jest 사용시, 'it' 이 undefined 로 나올 때

```
  env: {
    jest: true
  },
```

## document undefined

```
  env: {
    browser: true
  }
```

---

credit : [Velopert-리액트 프로젝트에 ESLint 와 Prettier 끼얹기](https://velog.io/@velopert/eslint-and-prettier-in-react)
