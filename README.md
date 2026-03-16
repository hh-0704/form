# 스프링 MVC 2편 - 섹션 3. 타임리프 - 스프링 통합과 폼

> 김영한의 [스프링 MVC 2편 - 백엔드 웹 개발 활용 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2) 강의 **섹션 3 (강의 23~32)**을 따라 실습한 프로젝트입니다.

---

## 강의 목차

| 강의 | 제목 |
|------|------|
| 23 | 프로젝트 설정 |
| 24 | 타임리프 스프링 통합 |
| 25 | 입력 폼 처리 |
| 26 | 요구사항 추가 |
| 27 | 체크 박스 - 단일1 |
| 28 | 체크 박스 - 단일2 |
| 29 | 체크 박스 - 멀티 |
| 30 | 라디오 버튼 |
| 31 | 셀렉트 박스 |
| 32 | 정리 |

---

## 학습 목표

- 타임리프와 스프링의 통합 기능 이해 (`th:object`, `th:field`)
- 입력 폼 처리 및 `@ModelAttribute` 바인딩
- 체크박스 단일/멀티 처리 방법 (`th:field` + `hidden` 필드 활용)
- 라디오 버튼 및 셀렉트 박스 처리
- `@ModelAttribute`를 이용한 폼 공통 데이터(Enum, 리스트) 자동 모델 추가
- PRG(Post-Redirect-Get) 패턴 적용

---

## 기술 스택

| 항목 | 내용 |
|------|------|
| Language | Java 25 |
| Framework | Spring Boot 4.0.3 |
| Template Engine | Thymeleaf |
| Build Tool | Gradle |
| 기타 | Lombok |

---


## 핵심 학습 내용

### 1. 타임리프 스프링 통합 - `th:object` / `th:field`

`th:object`로 모델 객체를 지정하고, `th:field`로 각 필드를 연결합니다.  
`th:field`는 `id`, `name`, `value` 속성을 자동으로 생성해줍니다.

```html
<form th:action method="post" th:object="${item}">
    <input type="text" th:field="*{itemName}">
    <input type="text" th:field="*{price}">
    <input type="text" th:field="*{quantity}">
</form>
```

### 2. 체크박스 - 단일

HTML 체크박스는 체크하지 않으면 값이 서버로 전송되지 않는 문제가 있습니다.  
타임리프의 `th:field`를 사용하면 히든 필드(`_open`)를 자동으로 생성해 이 문제를 해결합니다.

```html
<input type="checkbox" th:field="*{open}">
```

### 3. 체크박스 - 멀티

`@ModelAttribute`를 활용해 체크박스 목록을 모든 컨트롤러에 자동으로 전달하고,  
`th:each`로 반복 렌더링합니다.

```java
@ModelAttribute("regions")
public Map<String, String> regions() {
    Map<String, String> regions = new LinkedHashMap<>();
    regions.put("SEOUL", "서울");
    regions.put("BUSAN", "부산");
    regions.put("JEJU", "제주");
    return regions;
}
```

```html
<div th:each="region : ${regions}">
    <input type="checkbox" th:field="*{regions}" th:value="${region.key}">
    <label th:text="${region.value}"></label>
</div>
```

### 4. 라디오 버튼

Enum을 활용해 라디오 버튼 목록을 구성합니다.

```html
<div th:each="type : ${itemTypes}">
    <input type="radio" th:field="*{itemType}" th:value="${type.name()}">
    <label th:text="${type.description}"></label>
</div>
```

### 5. 셀렉트 박스

`th:each`로 옵션을 렌더링하고, `th:field`로 선택값을 자동 처리합니다.

```html
<select th:field="*{deliveryCode}">
    <option value="">==배송 방식 선택==</option>
    <option th:each="deliveryCode : ${deliveryCodes}"
            th:value="${deliveryCode.code}"
            th:text="${deliveryCode.displayName}"></option>
</select>
```
