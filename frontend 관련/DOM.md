# DOM(Document Object Model)

## DOM이란?
- HTML 문서의 내용을 트리 형태로 구조화하여 웹페이지와 프로그래밍 언어를 연결시켜주는 역할
- ![[프로젝트 관련 정보/imgs/Pasted image 20240111133618.png]]![[프로젝트 관련 정보/imgs/Pasted image 20240111133625.png]]
- 이런식으로 바뀌게 됨.
## DOM tree 접근방법
- document 객체를 통해 HTML 문서에 접근 가능.
- document= 브라우저가 불러온 웹페이지=DOM 트리의 진입점.
```
// 해당하는 Id를 가진 요소에 접근하기
document.getElementById();

// 해당하는 모든 요소에 접근하기
document.getElementsByTagName();

// 해당하는 클래스를 가진 모든 요소에 접근하기
document.getElementsByClassName();

// css 선택자로 단일 요소에 접근하기
document.querySelector("selector");

// css 선택자로 여러 요소에 접근하기
document.querySelectorAll("selector");
```

- HTML collectoion과 NodeList
```
<script> 
	const cont = document.getElementById('container'); 
	const item1 = cont.getElementsByTagName('li'); 
	const item3 = cont.querySelectorAll('li'); 
</script>
```
- 하위에 여러 개의 노드가 있는 태그에서 이렇게 썼을때 결과는
- ![[프로젝트 관련 정보/imgs/Pasted image 20240111134321.png]]
- 이렇게 뜬다.

| HTMLCollection | NodeList |
| ---- | ---- |
| HTML 요소만 포함 | 모든 유형의DOM 요소(text, 주석을 다 포함) |
| 객체 구성 값 변경 가능 | 정적이라 해당 객체에 대한 변경 사항은 반영 X |
# 추가적인 사항은 필요할때 더할 예정.