# 다형성

앞에서 배운 filter, map, each는 javascript에 있는 함수다. 굳이 이미 있는 함수들을 왜 만들었을까? map, filter들은 메서드다. 함수가 아닌 메서드다. 즉, 순수함수가 아니다. 또한, 메서드는 객체의 상태에 따라 결과가 달라진다. 

```javascript
console.log(
    [1,2,3,4].map(function(val) {
        return val * 2;
    })
)

console.log(
    [1,2,3,4].filter(function(val) {
        return val % 2;
    })
)
```

메서드는 객체지향 프로그래밍이다. 위의 map과 filter는 함수가 아닌 객체의 메서드다. 메서드는 해당 클래스에 정의된다. 그래서 해당 클래스의 인스턴스에서만 사용 가능하다. map이라는 함수는 array가 아니면 사용할 수 없다. 자바스크립트에는 array가 아닌데 array처럼 여겨지는 객체들이 있다. (array-like 객체). 대표적으로 jQuery 객체가 있다. 예를 들어,

```javascript
$('div'); // 얘는 Array가 아닌 array-like 객체다.
```

위 코드는 `document.querySelectorAll('div')`와 같다. 이것은 배열이 아닌 array-like 객체다. 그래서 `map()`을 이용할 수 없다.

```javascript
console.log(
    // document.querySelectorAll('div').map(function(node){
    //     return node.nodeName;
    // }); // error

    _map(document.querySelectorAll('*'), function(node) {
        return node.nodeName;
    }); 

    // document.querySelectorAll('*')
    // NodeList(3) [html, head, body]
    // 0: html
    // 1: head
    // 2: body
    // length: 3
    // __proto__: NodeList
)
```

__함수형 프밍에서는 함수를 먼저 만들고 그 함수에 맞는 데이터를 구성한다.__ array가 아닌 array-like 객체지만 _map에서 동작한다. 데이터가 있기 전부터 함수가 있다. 즉, 객체지향은 해당 객체가 생성되어야 기능을 수행할 수 있다. 하지만, __FP는 함수 자체는 혼자 먼저 존재하기 때문에 평가 시점에 영향을 받지 않는다. 이로 인해 더 높은 조합성을 갖는다.  __

---

## 내부 다형성

응용형 함수의 장점을 알아보자. 우리는 `predicate()`, `iter()`, `mapper()`함수를 만들었다. 아래 코드의 두 번째 인자는 무조건 콜백함수라고 부르는 경향이 있다. FP는 두 번째 인자가 어떤 역할을 하는지에 따라 다양한 이름을 갖는 것이 중요하다. 콜백함수는 어떤 일들을 모두 수행 한 후 다시 돌려 주는 것을 뜻한다. 반면에, 어떤 조건을 리턴하는 predicate 함수, 돌면서 반복적으로 실행하는 iter 함수, 어떤 것을 매핑하는 mapper 함수 와 같이 보조함수 이름을 붙여주는 것이 좋다. 

```javascript
_map([1,2,3,4], function(v) {
    return v + 10;
})
```

외부 다형성에서 array, array_like, arguments를(iteration 가능한) 모두 다 수행할 수 있도록 하는 것은 고차함수의 구조 즉, `_map`이 어떻게 구현되었느냐에 따라 달라진다. **배열 안에 어떤 값이든** 들어 있어도 다 수행할 수 있도록 하는 것은 보조 함수가 하는 것이다. (predi, iter, mapper가 책임진다). 즉 내부 값에 대한 다형성(내부 다형성)은 보조함수가 책임진다.

FP의 고차함수, 응용형 함수에서 넘기는 값(배열 등)은 개발자가 내부 함수에 대한 이해를 바탕으로 두 번째 보조 함수 안에서 개발자가 직접 결정한다. 예를 들어 두 번째 인자 값에서 v는 숫자인것을 개발자가 알고 있다. 그래서 더하도록 결정할 수 있다. 또한, node의 nodeName이 필요한 것을 개발자가 알고 있으니까 node.nodeName을 리턴하도록 결정한다.

```javascript
_map([1,2,3,4], function(v) {
    return v + 10;
})
_map(document.querySelectorAll('*'), function(node) {
    return node.nodeName;
}); 
```

또한, 내부에서는 해당 데이터에 대해 mapper, iter, predicate와 같이 위임하기 때문에 데이터형에 굉장히 자유롭다. 그래서 다형성을 높이는데 자유롭다.

> 고차함수 
> 함수를 인자로 받거나 함수를 리턴하거나 인자로 받은 함수를 실행하는 것을 말한다.

> 다형성
OOP 특징 중 하나인 다형성은 상속을 통해 기능을 확장 및 변경하는 것을 가능하게 해준다. 예를 들어 같은 모양의 코드가 다른 행위를 하는 것이다. 자바에서 다형성을 구현하는 방법은 대표적으로 2가지가 있다.
>
> - 오버라이딩
> 슈퍼클래스를 상속받은 서브 클래스에서 슈퍼 클래스의 메서드와 같은 이름 및 같은 인자로 메서드 내의 로직들을 새롭게 정의하는 것이다. 이를 이용하면 하나의 슈퍼클래스를 상속받은 여러 서브 클래스들이 같은 이름에 다른 기능을 하는 메서드를 정의할 수 있다. (얘는 런타임 시점에 어떤 함수를 호출할지 결정한다)
>
> - 오버로딩
> 하나의 클래스에서 같은 이름의 메소드를 여러 개 가질 수 있다. 단, 메서드 인자들은 달라야 한다. 컴파일러가 컴파일시 오버로딩이 되는 함수를 인자들의 타입 및 개수에 맞는 메서드를 찾아서 결정한다.
> 번외로 얘네가 왜 필요하면, 키패드에서 ESC라는 키는 취소, ENTER는 실행을 한다. 동일하게 조작하지만 동작결과 및 방법은 다른 것을 의미한다. 

## 요약

메서드는

    - 객체지향 프밍
    - `[1,2,..].map`, `.filter`는 함수가 아닌 객체의 메서드다.
    - 해당 클래스의 인스턴스에서만 사용 가능(Array가 아니면 사용할 수 없다)
    - array-like 객체는 사용 불가.(`document.querySelectorAll('*')`)

함수형 프밍
    - 함수 먼저 만들고 함수에 맞는 데이터를 구성
    - FP의 함수 자체는 혼자 먼저 존재하기 때문에 평가 시점에 영향을 받지 않은
      - 객체 지향에서 객체가 생성되어야 기능을 수행할 수 있는 반면에, 함수 자체가 혼자 존재한다.
      - __그래서 평가 시점에 영향을 받지 않는다. 이것은 더 높은 조합성을 갖도록 해준다.__


```javascript
function _map(list, mapper) {
    var new_list = [];
    for (var i = 0; i < list.length; i++) {
        new_list.push(mapper(list[i]));
    }
    return new_list;
}
```
