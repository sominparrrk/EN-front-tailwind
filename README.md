# Tailwind CSS

[Open in notion](https://www.notion.so/somin-park/Tailwind-CSS-16e712df4fd04319aa8c8873e8f7b4f7)

# Tailwind CSS 란?

> 바로 마크업 내에 디자인할 수 있는 `flex`, `pt-4`, `text-center`, `rotate-90` 등의 클래스를 가진 utility-first CSS 프레임워크

> A utility-first CSS framework packed with classes like `flex`, `pt-4`, `text-center` and `rotate-90` that can be composed to build any design, directly in your markup.

- `flex` = `display: flex`
- `pt-4` = `padding-top: 1rem`
- `text-center` = `text-align: center`
- `rotate-90` = `transform: rotate(90deg)`

간단하게 부트스트랩과 비슷하다고 생각하면 된다.  
하지만 부트스트랩처럼 컴포넌트가 만들어진 것이 아닌 css 스타일링이 가능한 class 를 제공한다.  
반응형 뿐만 아니라 다크모드, 커스터마이징이 가능하고  
쉬운 예시를 들자면 왼쪽 사진처럼 만들고 싶으면 오른쪽처럼 마크업 하면 된다.

![result](https://user-images.githubusercontent.com/48925632/166291194-22518bfb-ca4d-4ed0-a104-a7ea8e8258e5.png)

# 사용

순수 마크업 뿐만 아니라 React, Vue, Angular 에서도 다 사용 가능하다.  
우리는 CRA 를 사용하기 때문에 이 기준으로 설명하려고 한다.  
TypeScript 사용을 위해서는 craco 도 추가로 설치해주어야 한다.  
이번에는 공식문서에 나온 기본 CRA 셋팅을 하려고 한다.

## 설치

### 라이브러리

PostCSS 플러그인으로 설치하는 것이  
webpack, Rollup, Vite, Parcel 과 같은 빌드 툴과 함께 사용할 때 좋다고 한다.

```markdown
npm install -D tailwindcss postcss autoprefixer

npx tailwindcss init
```

`postcss.config.js` 를 직접 생성한다.

```jsx
// postcss.config.js

module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
```

`tailwind.config.js` 에 템플릿 파일 경로를 설정해준다.  
앞으로 커스텀하고 싶은 특정 스타일을 theme 안에 설정해주면 된다.

```jsx
// tailwind.config.js

module.exports = {
  content: ['./src/**/*.{js,jsx,ts,tsx}'],
  theme: {
    // 이 곳에 선언하게 되면 tailwind 에서 기본으로 제공하는 값은 사용할 수 ❌
    extend: {
      // extend 내부에 선언하면 tailwind 기본 값과 커스텀값 동시에 사용 가능 ⭕️
      colors: {
        'blue-base': '#156aff',
        'red-base': '#e52f2f',
        'green-base': '#08b468',
        'purple-base': '#6400ff',
      },
      screens: {
        '3xl': '1600px',
      },
    },
  },
  plugins: [],
};
```

### 익스텐션

Tailwind 팀에서 제공하는 Tailwind CSS IntelliSense 익스텐션을 설치하면  
이렇게 className 자동완성 기능을 사용할 수 있다.

![extension](https://user-images.githubusercontent.com/48925632/166291187-e81a16cd-25b7-49a8-84bf-f90d880bb58b.png)
![extension_example](https://user-images.githubusercontent.com/48925632/166291179-9bed1853-ad56-4650-a02a-e35ac8da4626.png)

# 장점

## utility-first

스타일링을 위해 새로 컴포넌트를 만든다던지 새로 css / scss 파일을 만들지 않아도 되고  
바로 jsx 안에 인라인 스타일링을 하기 때문에 해당 컴포넌트 / 클래스의 스타일 변경을 위해  
긴 라인의 코드를 작성하거나 화면을 이동하지 않아도 된다.

## 커스텀

`tailwind.config.js` 에서 자유롭게 커스텀이 가능하기 때문에  
기존 tailwind 가 제공하는 스타일에서 우리가 원하는 스타일을 추가하여 일관성을 유지할 수 있다.

# 단점

## 퍼블리싱 코드가 더러워짐

```html
<figure class="md:flex bg-slate-100 rounded-xl p-8 md:p-0 dark:bg-slate-800">
  <div
    class="flex flex-col justify-center pt-6 md:p-8 text-center md:text-left space-y-4"
  ></div>
</figure>
```

세부적인 디자인들을 적용하다 보면 스크롤이 생길 수 밖에 없다💦  
특히 반응형 디자인도 클래스 안에 같이 작성하기 때문에  
breakpoint 마다 디자인이 다르다면 엄청나게 클래스가 길어질 것이다,,,  
디자인이 복잡한 페이지라면 css 파일을 따로 만들게 되는 수 밖에 없다,,,

## 헷갈리는 클래스명

### flex

직접 tailwind 를 사용하여 개발했을 때 많이 겪었던 일 인데  
대부분의 클래스명들은 직관적이지만 가끔 헷갈리는 클래스명들이 있다.

- `justify-content: center` = `justify-center`
- `align-items: center` = `items-center`
- `justify-items: center` = `justify-items-center`
- `align-content: center` = `content-center`

음 지금 이렇게 나열해보니 일관성이 있긴 있지만 막상 개발을 하다보면

`align-items: center` 를 계속 `align-center` 로 작성하게 됐었다.

### width, margin, padding 등

![width_class](https://user-images.githubusercontent.com/48925632/166291202-b7c36760-3a2f-45d7-826d-741f7abbe5f8.png)

기본 4px 기준이고 px 이 커질 수록 4의 배수가 아니라면 [***px] 이런식으로 작성해야 하는 불편함이 있다.  
이미 있는 px 은 가져다 쓰면 되지만 초반에 tailwind 를 사용할 땐 이 시스템이 익숙하지 않아  
계속 공식홈페이지를 왔다갔다 해야한다는 불편함이 있다.

## Ant Design 과 혼합 사용?!

나의 판단이지만 혼합 사용 시 코드가 정말 더러워지지 않을까 하는 의구심이 든다.  
아무래도 antd 자체 컴포넌트에서 조작할 수 있는 디자인 요소들이 있기에  
혼합하여 쓴다면 코드 일관성이 떨어질 거 같다는 생각이 든다.  
만약 쓴다면 테이블이나 antd 기능을 많이 사용하지 않는 정적 페이지에 한해서  
적용해 보는 것도 좋은 방법일 듯 하다!  
다들 어떻게 생각하시나요?? 🧐

# Tailwind UI

Tailwind 팀에서 만든 UI 컴포넌트로 antd 나 material-ui 와 비슷하다.  
하지만 유료로 예시 코드조차 볼 수 없었다,,🫤

![ui_price](https://user-images.githubusercontent.com/48925632/166292325-43888435-94e3-49a9-bfd4-1a65ec68a10f.png)
