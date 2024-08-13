### this
자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수

**`this`**
자바스크립트 엔진에 의해 암묵적으로 생성.<br>
**`this`**을 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 **`프로퍼티나 메서드 참조 가능`.**

#### this의 필요성
자기 자신이 속한 객체를 **재귀적으로 참조하는 방식은 일반적X, 좋지X.**
<details>
  <summary>🤖 TMI GPT - <b>재귀적으로 참조하는 방식이 좋지 않은 이유</b></summary>

### 유연성 부족
객체 이름을 직접 참조하면 그 메서드는 해당 객체에서만 동작하게 됩니다. 예를 들어, 객체 이름을 참조하면 다른 객체에 같은 메서드를 할당해도 원래 객체의 속성에 접근하게 됩니다.

### 유지보수성 문제
코드의 유지보수성이 떨어집니다. 만약 객체 이름을 변경해야 하는 상황이 발생한다면, 메서드 내부의 모든 객체 참조도 함께 수정해야 합니다. 이는 코드가 커질수록 관리가 어렵고 실수를 유발할 가능성이 높아집니다.

### 스코프 및 네임스페이스 문제
객체 이름을 직접 참조하면, 해당 이름이 전역 스코프에 있거나 다른 네임스페이스에 있는 변수와 충돌할 가능성이 생깁니다. 이는 의도하지 않은 버그를 일으킬 수 있습니다.

### this의 의도
자바스크립트의 `this`는 현재 메서드를 호출한 객체를 가리키도록 설계되었습니다. 이는 메서드가 객체 내에서 어떻게 호출되었는지에 따라 동적으로 결정됩니다. 이 메커니즘을 사용하면 메서드를 여러 객체에서 재사용할 수 있습니다. 반면에, 객체 이름을 직접 참조하면 이러한 동적 바인딩의 이점을 잃게 됩니다.

### 요약
자바스크립트에서 메서드 내부에서 자신이 속한 객체를 재귀적으로 참조하는 대신 `this`를 사용하는 것이 권장됩니다. 이는 코드의 유연성을 높이고, 유지보수를 쉽게 하며, 잠재적인 버그를 예방할 수 있기 때문입니다.
</details>


**⚠️ 재귀적으로 참조하는 방식의 코드 - 좋지 않음**
```js
const circle = {
  radius: 5,
  getDiameter(){
    return 2 * circle.radius;
  }
};
console.log(circle.getDiameter());
```

**this을 사용한 코드**
```js
function Circle(radius){
  this.radius = radius;
}
Circle.prototype.getDiameter = function(){
  return 2 * this.radius;
}

const circle = new Circle(5);
```


**객체 리터럴의 메서드 내부에서 this는 메서드를 호출한 객체.**
```js
// 객체 리터럴
const circle = {
  radius: 5,
  
  getDiameter(){
    // this는 메서드를 호출한 객체
    return 2 * this.radius;
  },
}
console.log(circle.getDiameter()); // 10
```

**생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스.**
```js
// 생성자 함수
function Circle(radius){
  // 여기 this : 생성자 함수 Circle이 생성할 인스턴스.
  this.radius = radius;
}

Circle.prototype.getDiameter = function(){
  // 여기 this : 생성자 함수 Circle이 생성할 인스턴스.
  return 2 * this.radius;
}

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter());
```

> **💡this  바인딩**
바인딩 : 식별자와 값을 연결하는 과정.
this 바인딩 :  this와 this가 가리킬 객체를 바인딩.

**`자바스크립트의 this`**는 **`함수가 호출되는 방식`**에 따라 this에 바인딩될 값, 즉 **`this 바인딩이 동적으로 결정`**된다.

..Java, C++ 같은 클래스 기반 언어에서 this는 언제나 클래스가 생성하는 인스턴스를 가리킨다.

strict mode는 this 바인딩에 영향을 준다.<br>
this의 본질은 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수의 개념.<br>
→ 객체가 아닌 일반 함수 네부의 this는 필요가 없으므로, strict mode에서 this을 참조할 경우 undefined을 반환.

![InsideOutGIF (2)](https://github.com/user-attachments/assets/506dd922-64e2-497d-8c0b-86ab9541c2c9)

<details>
  <summary>🤖 TMI GPT <b>JavaScript의 Strict Mode와 TypeScript의 관계</b></summary>

### JavaScript의 Strict Mode
- **Strict Mode**는 ECMAScript 5에서 도입된 기능으로, 코드에서 잠재적으로 문제가 될 수 있는 오류나 비정상적인 동작을 방지하기 위해 더 엄격한 규칙을 적용합니다.
- Strict Mode를 활성화하려면 코드의 최상단에 `"use strict";`라는 지시어를 추가합니다.
- **Strict Mode의 주요 특징:**
    - 암시적 전역 변수 생성 방지
    - `this`의 자동 전역 객체 바인딩 방지
    - `eval()` 사용 시 변수 및 함수 정의 제한
    - 중복된 매개변수 이름 사용 금지
    - 읽기 전용 속성 수정 시 오류 발생
    - `with` 문 사용 금지

### TypeScript
- **TypeScript**는 Microsoft에서 개발한 자바스크립트의 상위 언어로, 정적 타입 시스템을 추가하여 자바스크립트 코드의 품질과 유지보수성을 높입니다.
- TypeScript는 자바스크립트의 상위 집합이므로, 모든 자바스크립트 코드는 유효한 TypeScript 코드입니다.
- **TypeScript의 주요 특징:**
    - 정적 타입 검사
    - 인터페이스와 클래스
    - 고급 타입 기능 (제네릭, 유니언 타입, 인터섹션 타입 등)
    - 컴파일 타임 오류 체크
    - ECMAScript 표준에 맞추어 컴파일된 자바스크립트 코드 생성

### Strict Mode와 TypeScript의 관계
- **TypeScript는 기본적으로 Strict Mode를 사용합니다.**
    - TypeScript로 작성된 코드는 컴파일될 때, 자동으로 `"use strict";`가 포함된 자바스크립트 코드로 변환됩니다. 이는 TypeScript가 보다 안전한 코드 작성을 장려하기 때문입니다.
    - TypeScript는 자바스크립트보다 엄격한 타입 검사와 오류 검사를 제공하므로, strict mode와 유사한 목적을 가지고 있습니다. 그러나 TypeScript는 더 강력한 정적 타입 검사와 같은 기능을 통해 코드 안정성을 더욱 강화합니다.

- **TypeScript의 `strict` 컴파일 옵션:**
    - TypeScript의 `strict` 컴파일러 옵션은 TypeScript에서 더욱 엄격한 검사 규칙을 적용하도록 합니다.
    - 이 옵션을 활성화하면, `strictNullChecks`, `noImplicitAny`, `strictFunctionTypes` 등의 세부적인 검사 규칙이 자동으로 활성화됩니다.
    - 이러한 기능은 코드의 안전성을 강화하는 데 도움을 주며, 자바스크립트의 strict mode와 유사한 역할을 수행합니다.

### 요약
- JavaScript의 strict mode는 런타임에서 코드의 비정상적인 동작을 방지하기 위한 간단한 엄격 모드입니다.
- TypeScript는 정적 타입 시스템과 여러 코드 품질 검사를 제공하는 자바스크립트의 상위 언어로, TypeScript로 작성된 코드는 기본적으로 strict mode가 적용됩니다.
- 두 기능 모두 코드의 안전성과 예측 가능성을 높이는 데 기여하지만, TypeScript는 더욱 강력하고 세밀한 검사 기능을 제공합니다.
</details>


---

### 함수 호출 방식과 this 바인딩
> 함수를 호출하는 방식

- `일반 함수 호출`
- `메서드 호출`
- `생성자 함수 호출`
- `Function.prototype.apply/call/bind 메서드에 의한 간접 호출`


> 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.

```js
const foo = function ()  {
  console.dir(this);
};
```