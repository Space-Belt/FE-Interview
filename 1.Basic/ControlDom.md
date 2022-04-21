## DOM 제어 속성 및 메소드

---

## 요소를 직접 선택하는 방법

<img width="630" alt="image" src="https://user-images.githubusercontent.com/82592845/164497193-40e26556-8b1e-4320-8c9e-472f3b704c6a.png">

- getElementById() 메서드를 제외한 나머지 메서드들은 복수개의 요소들이 변수에 저장된다 (object HTML Collection // NodeList)
- 복수 선택된 요소들을 다시 개별적으로 선택하려면
  - element.item(index) index 번째 요소를 선택
  - element[n] n번째 요소를 선택
  - element.length 요소의 개수
  - 개별적으로 선택해야만 개별적으로 변경할 수 있다.

### 요소 추가/삭제

- createElement 원하는 요소 생성 가능
- appendChild 선택된 노드에 자식으로 요소 추가 가능
- removeChild 특정 요소 삭제 가능
- getAttribute 해당 요소의 속성값을 얻을 수 있다
- setAttribute 원하는 노드의 속성 값 바꾸기 가능
