# 스프링 MVC 2편 - 섹션 3. 메시지, 국제화

> 김영한의 [스프링 MVC 2편 - 백엔드 웹 개발 활용 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-2) 강의 **섹션 3**을 따라 실습한 프로젝트입니다.

---

## 학습 목표

- Thymeleaf의 `th:field`, `th:object`를 활용한 폼 처리 방법 이해
- `@ModelAttribute`를 이용한 폼 데이터 바인딩
- `RedirectAttributes`를 활용한 리다이렉트 후 메시지 전달
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

## 프로젝트 구조

```
src
└── main
    ├── java/hello/itemservice
    │   ├── ItemServiceApplication.java     # 애플리케이션 진입점
    │   ├── TestDataInit.java               # 테스트용 초기 데이터 설정
    │   ├── domain/item
    │   │   ├── Item.java                   # 상품 도메인 객체
    │   │   └── ItemRepository.java         # 상품 저장소 (메모리)
    │   └── web/form
    │       └── FormItemController.java     # 폼 처리 컨트롤러
    └── resources
        └── templates/form
            ├── items.html                  # 상품 목록
            ├── item.html                   # 상품 상세
            ├── addForm.html                # 상품 등록 폼
            └── editForm.html               # 상품 수정 폼
```

---

## 핵심 학습 내용

### 1. `th:object` / `th:field` 활용

폼에서 `th:object`로 모델 객체를 바인딩하고, `th:field`로 각 필드를 연결합니다.  
`th:field`는 `id`, `name`, `value` 속성을 자동으로 처리해줍니다.

```html
<form th:action method="post" th:object="${item}">
    <input type="text" th:field="*{itemName}">
    <input type="text" th:field="*{price}">
    <input type="text" th:field="*{quantity}">
</form>
```

### 2. `@ModelAttribute`로 폼 데이터 바인딩

폼 submit 시 요청 파라미터를 자동으로 객체에 바인딩합니다.

```java
@PostMapping("/add")
public String addItem(@ModelAttribute Item item, RedirectAttributes redirectAttributes) {
    Item savedItem = itemRepository.save(item);
    redirectAttributes.addAttribute("itemId", savedItem.getId());
    redirectAttributes.addAttribute("status", true);
    return "redirect:/form/items/{itemId}";
}
```

### 3. PRG (Post-Redirect-Get) 패턴

폼 submit 후 새로고침 시 중복 등록을 방지하기 위해 POST 이후 리다이렉트를 적용합니다.  
`RedirectAttributes`로 리다이렉트 URL에 파라미터를 안전하게 전달합니다.

---

## 실행 방법

```bash
./gradlew bootRun
```

실행 후 브라우저에서 확인:

```
http://localhost:8080/form/items
```
