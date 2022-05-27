# about-unity

# 스크립트 최적화 기법

## - Object Pooling
짧게 쓰고 자주 쓰는 오브젝트 (총알, 코인)를 매번 생성/해제 하면 메모리 부하가 생긴다. 이러한 오브젝트를 Pool에 보관했다가 재사용하는 기법이다. 
메모리 할당을 간단하게 할 수 있고 동적 메모리 할당 오버헤드, GC(가비지 컬렉션)을 줄일 수 있다.

## - String 대신 StringBuilder
+로 string을 연결하면 객체의 할당과 해제가 여러번 일어난다. 단편화 문제로 GC에 부담을 줄 수 있다.
따라서 System.Text의 StringBuilder 객체를 사용하는 것이 좋다. Append 메소드로 문자열을 연결할 수 있다.

## - Class vs Struct
class의 instance는 heap영역에 할당되고, struct의 instance는 stack에 할당된다.
짧게 쓰고 자주 쓰는 오브젝트는 구조체 형식으로 선언하는 것이 좋다.

## - Mathf 지양
Mathf의 메소드, 나눗셈, 제곱근 등은 곱셈 연산과 비교했을 때 100배가량 느리다.
오베헤드를 줄이기 위해 더 가벼운 수식을 사용하는 것이 좋다.

## - FindObjectOfType 대신 FindWithTag
FindWithTag가 그나마 빠르다.

## - GetComponent() 대신 TryGetComponent()
GetComponent는 언제나 GC Allocation이 발생한다. TryGetComponent()를 이용하는 것이 좋다.

## - .name, .tag 지양
.name과 .tag를 호출하면 가비지가 생성된다. name, tag를 참조하기 위해 문자열을 새롭게 heap에 할당하기 때문이다.
gameObject.tag == "Tag"대신에 gameObject.CompareTag("Tag")를 사용하여 가비지 생성을 방지하자.

## - 빈 Unity Event Method 삭제
Update()등의 유니티 이벤트 메소드는 사용하지 않는다면 삭제하는 것이 좋다.

## - StartCoroutine() 지양
StartCoroutine은 GC에 부담을 준다. 짧게 쓰고 자주 쓰는 코루틴은 UniTask, UniRx 등으로 대체하는 것이 좋다. 

## - yield return new
매번 new로 생성할 경우, GC에 부담을 준다. 미리 변수에 담아두고 캐싱하여 사용하는 것이 좋다.

## - 메소드 호출 줄이기
메소드를 호출하는 것만으로도 성능을 소모한다. 캐싱을 적극 이용하자.
예로 Update문 안에서 Time.deltaTime을 여러번 사용한다면 필드에 Time.deltaTime을 담아두고 사용하는 방식.
또한 프로퍼티도 메소드의 일종이기 때문에 자주 사용하는 녀석은 필드로 담아 사용하는 것이 좋다.


_참고 https://rito15.github.io/posts/unity-opt-script-optimization/_
