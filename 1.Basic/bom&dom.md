## ⭐️기초 ⭐️

## 1. `BOM (Browser Object Model)`

- 브라우저와 관련된 객체들의 집합이다.
- Brower와 관련된 기능들을 구성하게 한다.
- `DOM( Document Object Model)` 은 `BOM`중 하나이다.
- `BOM` 의 최상위 객체는 `window` 다

## 2.`DOM (Document Object Model)`

- Document 문서 Object 객체 문서 객체 ~?
- **문서 객체란?**
  - `<html> <body>` 등 html문서의 태그들을 JavaScript가 이용할 수 있는 객체(Object)로 만들면 그것을 문서 객체라 한다.
- Model 은 문서 객체를 **'인식하는 방식' 이라고 볼 수 있다.**
- `DOM` 은 Node구조다. 그러므로 node tree가 `DOM` 의 구조다.
  - Node 란??
  - 어떤 연결망에서 특정 지점과 지점을 연결하는데 표시한 것이다.
- 웹브라우저는 DOM 덕분에 자바스크립트와 CSS를 사용해서 상호작용이 가능하다.
- 노드를 검색, 수정, 삭제, 생성이 가능하다.
- 하나의 Root Node 에서 시작된다.

## DOM 기본 인터페이스

- Node   - 모든 객체의 부모 인터페이스로서 공통적으로 기능하는 함수를 가짐
- NodeList  -  노드들을 리스트로 받아서 처리하기 쉽도록 한것 (주로 getElementsbyTagName("태그네임") 메서드의 리턴 타입으로서 사용됨)
- Document - DOM 트리 구조의 최상위 노드 / XML 문서 자체에 해당
- Element - XML의 엘리먼트에 해당하는 객체 유형
- Attr - XML의 Attribute에 해당하는 객체 유형
- CharacterData - XML의 데이터에 해당하는 객체 유형
- Text - 문자 데이터(내용)에 해당하는 객체 유형
