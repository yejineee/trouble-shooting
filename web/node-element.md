# DOM Node와 Element의 차이점

## 🧩 DOM Node

DOM은 Node의 계층 구조로 이루어져 있다. 각 노드는 부모와 children을 가질 수 있다.

```htmlembedded
<!DOCTYPE html>
<html>
  <head>
    <title>My Page</title>
  </head>
  <body>
    <!-- Page Body -->
    <h2>My Page</h2>
    <p id="content">Thank you for visiting my web page!</p>
  </body>
</html>
```

위의 HTMl은 아래에 있는 노드들의 계층으로 구성되어있따.

![](https://i.imgur.com/RL1IrMs.png)

HTML에 있는 tag (\<html>, \<p>와 같은...)들은 node를 표현하게 된다. 여기서 그저 text이더라도 node가 된다는 점이다. 노드트리를 보면, \<p> 자식 노드로 text node가 있는 것을 확인할 수 있다.

- **Node Type**
  ```
  Node.ELEMENT_NODE
  Node.ATTRIBUTE_NODE
  Node.TEXT_NODE
  Node.CDATA_SECTION_NODE
  Node.PROCESSING_INSTRUCTION_NODE
  Node.COMMENT_NODE
  Node.DOCUMENT_NODE
  Node.DOCUMENT_TYPE_NODE
  Node.DOCUMENT_FRAGMENT_NODE
  Node.NOTATION_NODE
  ```
  Node.ELEMENT_NODE 외에도, 주석의 타입인 COMMENT_NODE와 전체 document tree를 표현하는
  Node.DOCUMENT_NODE도 있다.

## 🧩 DOM Element

Element는 node의 특정 타입 즉, Node.ELEMENT_NODE인 것이다.

element는 HTML에서 태그로 적은 노드들을 지칭한다. 예를 들어, \<html>, \<div>, \<title> 과 같은 태그로 나타낸 것들은 전부 element인 것이다. 주석이나 text node와 같은 것들은 HTML 태그로 표현된 것이 아니므로 element가 아니다.

JS DOM에서`Node`는 node의 constructor이고, `HTMLElement`는 element의 constructor이다.

paragraph는 node이자 동시에 element이다.

```javascript
const paragraph = document.querySelector("p");

paragraph instanceof Node; // => true
paragraph instanceof HTMLElement; // => true
```

## 🧩 DOM 프로퍼티

node에만 있는 DOM 프로퍼티와 element에만 있는 DOM 프로퍼티를 구분할 줄 알아야 한다.

다음 Node의 프로퍼티들은 node나 NodeList라고 한다.

```
node.parentNode; // Node or null

node.firstChild; // Node or null
node.lastChild;  // Node or null

node.childNodes; // NodeList
```

다음 Node의 프로퍼티들은 element나 element의 집합(HTMLCollection)이다.

```
node.parentElement; // HTMLElement or null

node.children;      // HTMLCollection

```

여기서 주목할 것은 Node의 children을 가져오는 프로퍼티라는 점에서는 같지만, NodeList 형태로 가져오는 `node.childNodes`와 HTMLCollection 형태로 가져오는 `node.children`이 있다는 것이다.

왜 이 두 개를 만들게 되었으며, 그 차이는 무엇일까?

```htmlembedded=
<p>
  <b>Thank you</b> for visiting my web page!
</p>
```

```javascript
const paragraph = document.querySelector("p");

paragraph.childNodes; // NodeList:       [HTMLElement, Text]
paragraph.children; // HTMLCollection: [HTMLElement]
```

위에 있는 p tag를 childNodes와 children으로 접근하였을 때, 그 결과가 달라지게 된다.

childNodes로 접근하면 HTMLElement와 Text가 있다. 이는 p tag 안에 자식노드로 태그가 있는 element인 `<b>Thank you</b>`와 text node인 `for visiting my web page!`가 있음을 나타낸 것이다.

children으로 접근하면, 오로지 HTMLCollection 즉 element만 가져오므로 text node인 `for visiting my web page!`는 빠지고, element node인 `<b>Thank you</b>`만 가져오게 된다.

---

## 정리

Element와 Node의 차이점을 잘 이해하고, 적절한 때에 적절한 것에 접근하면 되는 것 같다. 예를 들자면, 현재 element의 parent로 넘어가서 sibling element에 접근해야 했다. 이 때, parentNode로 parent를 가져오려고 했을 때, 내가 의도한 바를 이루지 못했었다. element로 넘어가려면 parentElement를 했어야 하는데, 그러지 못한 것이었다. 이제 Node와 Element의 차이점을 알았으니, 파싱할때나 바닐라로 플젝할 시에 좀 더 삽질을 덜 할 수 있을 것이라고 믿는다...

```javascript
productExampleElement.parentElement.nextElementSibling;
```

## 참고

- [What's the Difference between DOM Node and Element?
  ](https://dmitripavlutin.com/dom-node-element/)
