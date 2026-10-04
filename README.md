# Week N - JavaScript DOM & Simple CRUD

## Deployment

- Vercel URL: (배포 후 추가)
- index.html : 이번 주 페이지 링크
- js_dynamic.html : DOM 연습 페이지
- crud.html : 영화 관리 CRUD 페이지

---

## Key Learning

1. **DOM 조작** - createElement로 요소를 만들어도 appendChild()로 붙이기 전까지는 화면에 안 나온다는 걸 알게 됐다.
2. **Array를 DB처럼 쓰기** - 데이터는 배열 내에서만 바꾸고, 화면은 그 Array를 보고 다시 그리는 방식으로 CRUD를 만들었다.
3. **render() 함수** - 추가/수정/삭제할 때마다 화면을 직접 고치는 게 아니라 render를 다시 호출하면 돼서 코드가 훨씬 단순해졌다.

---

## CRUD Service

### 주제
영화 관리 시스템을 제작했다. 본 영화들을 제목, 감독, 장르, 개봉연도, 평점으로 관리할 수 있게 제작했다
최근에 본 오디세우스 영화가 인상 깊어서 제작하게 되었다

### 데이터 Field

| Field | 설명 | 입력 |
|---|---|---|
| id | 번호 (자동으로 증가) | - |
| title | 영화 제목 | text |
| director | 감독 | text |
| genre | 장르 | select |
| year | 개봉연도 | number |
| rating | 평점 (0~10) | number |

초기 데이터로 인터스텔라, 기생충, 라라랜드 3개를 넣었다.

### 구현 방법

- **Create**
  form에 입력한 값을 객체로 만들어서 `push()`로 movies 배열에 추가했다. id는 `nextId` 변수를 따로 둬서 추가할 때마다 1씩 올렸다. (`length + 1`로 하면 중간 데이터를 지웠을 때 id가 겹칠 수 있어서)

- **Read**
  `render()`에서 `forEach()`로 배열을 돌면서 `tr`, `td`를 만들어 table에 붙였다. 시작할 때 `tbody.innerHTML = ""`로 비우고 다시 그려야 데이터가 중복으로 안 쌓인다.

- **Update**
  수정 버튼을 누르면 `find()`로 해당 영화를 찾아서 form에 값을 채운다. `editId` 변수로 지금이 추가 모드인지 수정 모드인지 구분했고, 저장을 누르면 찾은 객체의 값을 바꾼 뒤 `render()`를 호출한다.

- **Delete**
    삭제 버튼을 누르면 먼저 `confirm()`으로 삭제 여부를 물어보고, 취소를 누르면 `return`으로 바로 끝낸다. 확인을 누르면 `findIndex()`로 해당 영화가 배열의 몇 번째에 있는지 찾고, `splice(index, 1)`로 배열에서 그 데이터를 지운 뒤 `render()`를 호출한다. 화면의 `tr`만 지우는 게 아니라 배열에서 지워야 다음 render 때 다시 안 나타난다. 그리고 수정 중인 영화를 삭제하는 경우에는 form에 지워진 데이터가 남아 있게 되므로, `editId`를 확인해서 수정 모드도 같이 해제했다.

---

## JavaScript

| 기능 | 설명 / 사용한 곳 |
|---|---|
| `window.onload` | 페이지가 다 로드된 다음에 실행. 처음 render()와 버튼 이벤트 연결을 여기서 함 |
| `getElementById()` | id로 요소를 찾음. form 입력값 읽을 때 주로 사용 |
| `addEventListener()` | 클릭 이벤트 연결. Add/저장, 취소, 수정, 삭제 버튼에 사용 |
| `createElement()` | tr, td, button 같은 요소를 새로 만듦 |
| `appendChild()` | 만든 요소를 부모 안에 넣어서 화면에 보이게 함 |
| `remove()` | js_dynamic에서 li를 삭제할 때 사용 |
| `push()` | 배열 맨 끝에 데이터 추가 (Create) |
| `forEach()` | 배열을 하나씩 돌면서 처리 (render) |
| `find()` | 조건에 맞는 첫 번째 데이터를 반환 (Update) |
| `findIndex()` / `splice()` | (Delete 구현 후 작성) |
| `confirm()` | (Delete 구현 후 작성) |
| `render()` | 배열 내용을 table로 다시 그리는 함수. 직접 만든 함수 |
| `findIndex()` / `splice()` | findIndex는 조건에 맞는 데이터의 위치(인덱스)를 반환하고, splice(index, 1)는 그 위치에서 1개를 배열에서 삭제함 (Delete) |
| `confirm()` | 확인/취소 창을 띄워서 확인이면 true, 취소면 false를 반환. 삭제 전에 한 번 물어볼 때 사용 |
---

## AI / Search Usage

- **Tool**: Claude, gpt
- **Purpose**: 전체적으로 어떤 흐름으로 코드를 짜야하는지 파악하는데 도움을 주고 어떤 java script문법이 있는지 그리고 CSS에서 어떻게 이쁘게 스타일을 줄 수 있는지 도움을 주었다. 또한 git 관련해서 이번에도 파일이랑 commit이 날아가는 경험을 맛 보게 되었는데 이를 해결해주었다
- **Used**: java script 문법들을 배웠고 commit log를 보는 명령어를 알 수 있게 되었다
- **What I Learned**: text, select, number를 섞어두면 validation 종류를 다양하게 걸 수 있다는 걸 알게 되었고, getElementById를 꽤 유용하게 사용하는 방법을 알 수 있었다

## Problem & Solution

### 1. Add 버튼이 안 눌림
- **문제**: 취소 버튼을 넣으면서 버튼 영역을 새로 만들었는데 기존 걸 안 지워서 `id="saveBtn"`이 두 개가 됐다. 그리고 `addMovie()` 함수만 만들고 버튼에 연결을 안 해서 눌러도 아무 일이 없었다.
- **해결**: 중복된 버튼 영역을 지우고 `window.onload` 안에서 `addEventListener()`로 `addMovie()`를 연결했다.
- **배운 점**: id는 한 페이지에 하나만 있어야 하고, 같은 id가 여러 개면 `getElementById()`는 첫 번째 것만 찾는다.


## Reflection

- 처음엔 삭제할 때 화면에서 `tr`만 지우면 될 줄 알았는데, 그러면 배열에는 데이터가 남아 있어서 다음 render() 때 다시 나타난다. 데이터랑 화면을 따로 생각해야 한다는 게 이번 과제에서 제일 크게 와닿았다.
- 지금은 새로고침하면 추가한 데이터가 다 사라진다. 실제 서비스에서는 이걸 서버나 DB에 저장할 텐데, 다음에 그 부분을 어떻게 연결하는지 궁금하다.