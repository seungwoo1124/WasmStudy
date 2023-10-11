> *컴퓨터 과학에서 추상 구문 트리, 또는 간단히 구문 트리는 프로그래밍 언어로 작성된 소스코드의 추상 구문 구조의 트리이다. 이 트리의 각 노드는 소스코드에서 발생되는 구조를 나타낸다. 구문이 추상적이라는 의미는 실제 구문에서 나타나는 모든 세세한 정보를 나타내지는 않는다는 것을 의미한다. 예를 들어, 그룹핑을 위한 괄호는 암시적으로 트리 구조를 가지며, 분리된 노드로 표현되지는 않는다. 마찬가지로, if-condition-then 표현식과 같은 구문 구조는 3개의 가지에 1개의 노드가 달린 구조로 표기된다.*
> <https://ko.wikipedia.org/wiki/%EC%B6%94%EC%83%81_%EA%B5%AC%EB%AC%B8_%ED%8A%B8%EB%A6%AC>

## 예제
- 코드
- 
```javascript
function square(n) {
	return n * n 
}
```

를 AST로 변환하면
- 
```
{
  "type": "Program",
  "start": 0,
  "end": 37,
  "body": [
    {
      "type": "FunctionDeclaration",
      "start": 0,
      "end": 37,
      "id": {
        "type": "Identifier",
        "start": 9,
        "end": 15,
        "name": "square"
      },
      "expression": false,
      "generator": false,
      "async": false,
      "params": [
        {
          "type": "Identifier",
          "start": 16,
          "end": 17,
          "name": "n"
        }
      ],
      "body": {
        "type": "BlockStatement",
        "start": 19,
        "end": 37,
        "body": [
          {
            "type": "ReturnStatement",
            "start": 23,
            "end": 35,
            "argument": {
              "type": "BinaryExpression",
              "start": 30,
              "end": 35,
              "left": {
                "type": "Identifier",
                "start": 30,
                "end": 31,
                "name": "n"
              },
              "operator": "*",
              "right": {
                "type": "Identifier",
                "start": 34,
                "end": 35,
                "name": "n"
              }
            }
          }
        ]
      }
    }
  ],
  "sourceType": "module"
}
```
가 된다.


코드에서 AST를 가져오는것은 컴파일러가 합니다.
1. 렉시컬 분석 (Lexical analyzer aka scanner)
	- 코드의 문자들을 읽어서 정해진 룰에 따라서 이들을 토큰으로 만들어 합칩니다. 여기에서 공백, 주석 등을 지우고 마지막으로는 이 전체 코드를 토큰들로 나눕니다. 이 렉시컬 분석기가 소스 코드를 읽을 때, 코드를 글자 단위로 읽습니다. 그 과정에서 공백, operator symbol, special symbol을 만나면 해당 단어가 끝난 것으로 간주합니다.

2. 신택스 분석 (Syntax analyzer aka parser)
	- 위에서 결과로 나온 토큰 목록을 트리 구조로 만들며, 구조적 혹은 언어적으로 문제가 있을 경우 에러를 내뱉는다. 트리를 만드는 과정에서, 일부 파서들은 불필요한 토큰 (중복된 괄호라던지)를 생략합니다. 그리고 그 결과로 'Abstract Syntax Tree'를 만듭니다. 이는 코드와 100% 일치하지는 않지만 코드를 다루기에는 충분합니다.