# 📌 React 강의 정리

---

## 1. JSX 요소와 부모 요소
- JSX에서 여러 개의 형제 요소를 반환하면 에러 발생
- **이유:** JavaScript에서는 값을 두 개 이상 반환할 수 없음 → 하나의 값만 반환 가능  
- **해결 방법:**  
  - `<div>`로 감싸기  
  - **프래그먼트(Fragment)** 사용하기 → 불필요한 `<div>`를 없앨 수 있음  
    - `<Fragment></Fragment>` 또는 최신 버전에서는 `<>...</>` 가능  

```jsx
// ❌ 잘못된 코드 (에러 발생)
return (
  <h1>안녕하세요</h1>
  <p>React 공부 중</p>
);

// ✅ 올바른 코드 (div 사용)
return (
  <div>
    <h1>안녕하세요</h1>
    <p>React 공부 중</p>
  </div>
);

// ✅ 올바른 코드 (Fragment 사용)
return (
  <>
    <h1>안녕하세요</h1>
    <p>React 공부 중</p>
  </>
);
```

---

## 2. Props 전달과 확장 연산자(...)
- Props는 자동으로 전달되지 않음 → 개발자가 직접 설정해야 함
- **전달 속성(Forwarded Props)**: 부모 → 자식 컴포넌트로 props를 넘길 때 사용  
- **대리 속성(Proxy Props)**: 전달된 props를 가공하여 다른 props에 할당하는 방식  
- `...` (spread operator, 전개 연산자)  
  - **컴포넌트에서 사용:** 여러 개의 props를 한꺼번에 전달  
  - **데이터에서 사용:** 객체나 배열을 펼쳐서 전달  

```jsx
const Button = (props) => {
  return <button {...props}>{props.label}</button>;
};

// 사용 예시
<Button label="클릭" onClick={() => alert("클릭!")} disabled={false} />
```

---

## 3. Wrapper Component (래퍼 컴포넌트)
- 특정 JSX를 감싸서 HTML 구조까지 실행하는 컴포넌트
- 다수의 형제 요소는 하나의 값으로 허용되지 않음 → `<div>` 또는 `<Fragment>`로 감싸야 함

```jsx
const Card = ({ children }) => {
  return <div className="card">{children}</div>;
};

// 사용 예시
<Card>
  <h2>제목</h2>
  <p>내용</p>
</Card>
```

---

## 4. 커스텀 컴포넌트와 동적 속성 설정
- **커스텀 컴포넌트 사용 시 반드시 대문자로 시작해야 함**
- 속성 값을 동적으로 설정하려면 `{}` 사용  

```jsx
// 올바른 예시
const MyButton = () => <button>클릭</button>;

// 잘못된 예시 (HTML 태그로 인식됨)
const myButton = () => <button>클릭</button>;
```

---

## 5. index.html과 정적 마크업
- `index.html`이 **사용자가 직접 보는 화면**
- 리액트에서 작업할 때, 마크업을 **반드시 컴포넌트 내부에 추가할 필요 없음**
  - **정적 마크업**이라면 `index.html` 내부에 둘 수도 있음
- **이미지 파일 관리**
  - `public/` 폴더 → 사용자가 직접 접근 가능 (경로 지정 불필요)
  - `src/assets/` 폴더 → 코드에서만 사용 가능, 빌드 과정에서 `public/`으로 옮겨짐

```jsx
// public 폴더 이미지 사용
<img src="/logo.png" alt="로고" />

// src/assets 폴더 이미지 사용
import logo from '../assets/logo.png';
<img src={logo} alt="로고" />
```

---

## 6. useState와 상태 업데이트
- 상태를 변경할 때, 이전 상태 값을 직접 변경하는 것은 **안 좋은 방법**
- `setState` 함수는 **비동기적으로 동작**
- 상태 업데이트 시, **이전 상태를 기반으로 새 상태를 설정하는 함수형 업데이트 사용**

```jsx
setEditing((prevEditing) => !prevEditing);
```
- `setEditing(!isEditing)` → 현재 상태를 즉각 변경하는 것이 아니라 **변경 스케줄을 조율**
- **함수형 업데이트(`prevState`)를 사용하면 최신 상태를 보장할 수 있음**

```jsx
// ❌ 잘못된 예시
setCount(count + 1);
setCount(count + 1); // 예상과 다르게 1번만 증가

// ✅ 올바른 예시
setCount((prevCount) => prevCount + 1);
setCount((prevCount) => prevCount + 1); // 2번 증가
```

---

## 7. useState 여러 번 사용 가능
- 동일한 컴포넌트에서 `useState` 여러 번 선언 가능
- `onChange` → 입력값이 변경될 때마다 실행됨 (이벤트 객체를 통해 값 제공)
- **양방향 바인딩 (Two-way Binding) 가능**

```jsx
const [name, setName] = useState("");

const handleChange = (e) => {
  setName(e.target.value);
};
```

---

### 📌 마무리
이 문서는 React의 주요 개념을 정리한 자료입니다. 추가할 내용이나 수정할 부분이 있다면 PR이나 Issue를 남겨주세요!
