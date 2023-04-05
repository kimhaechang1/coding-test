## Iterator 인터페이스

자바의 컬렉션 프레임워크는 컬렉션에 저장된 요소를 읽어오는 방법을

Iterator 인터페이스로 표준화 하고 있다.

Collection 인터페이스에서는 Iterator 인터페이스를 구현한 클래스의 인스턴스를 반환하는

iterator() 메소드를 정의하여 각 요소에 접근하도록 하고 있다.

```java
LinkedList<Integer> list = new LinkedList<>();

list.add(1);
list.add(2);
list.add(3);

Iterator<Integer> iterator = list.iterator();

while(iterator.hasNext()){
    System.out.print(iterator.next()+" ");
}
```

```
1 2 3
```

### Iterator 인터페이스의 주요 메서드

```boolean hasNext()```

다음 요소를 가지고 있으면 true를 반환, 없으면 false

```E next()```

다음 요소를 반환한다.

```default void remove```

해당 반복자로 반환되는 마지막 요소를 현재 컬렉션에서 제거

### ListIterator 인터페이스

ListIterator 인터페이스는 Iterator 인터페이스를 상속받아 여러 기능을 추가한 인터페이스이다.

Iterator와의 가장 큰 차이점은

Iterator는 한쪽 방향으로만 요소의 이동이 가능하지만

ListIterator는 양방향 이동이 가능하다.

단, ListIterator 인터페이스는 List 인터페이스를 구현한 List 컬렉션 클래스 에서만 listIterator() 메소드를 통해 사용가능하다.


```java
LinkedList<Integer> list = new LinkedList<>();

list.add(1);
list.add(2);
list.add(3);

ListIterator<Integer> listIterator = list.listIterator();

while(listIterator.hasNext()){
    System.out.print(listIterator.next()+" ");
}

while(listIterator.hasPrevious()){
    System.out.print(listIterator.previous()+" ");
}

```
```
1 2 3
3 2 1
```

### ListIterator의 주요 메서드

```void add(E e)```
 
해당 리스트에 전달된 요소를 추가한다.

단, List가 아니라 ListIterator에서는 추가함과 동시에 커서도 이동한다는 점 잊으면 안된다.

```boolean hasNext()```

리스트를 순방향으로 순회할 때 다음 요소가 존재한다면 true

없으면 false를 반환한다.

```boolean hasPrevious()```

리스트를 역방향으로 순회할 때 이전 요소가 존재한다면 true

없으면 false를 반환한다.

```E next()```

리스트의 오른쪽 원소를 반환하고 커서를 오른쪽으로 옮김

```int nextIndex()```

다음 next()메서드를 호출 하였을 때 반환될 요소의 인덱스를 반환한다.

```E previous()```

이전 요소를 반환하고 커서를 왼쪽으로 이동시킴

```int previousIndex()```

다음에 previous() 메서드를 호출 하였을 때 반환될 요소의 인덱스를 반환한다.

```void remove()```

next()나 previous() 메서드에 의해 반환된 가장 마지막요소를 리스트에서 제거한다.

즉 커서 오른쪽 요소를 항상 제거한다고 생각하면 편하다.

```void set(E e)```

next()나 prevous() 메서드에 의해 반환된 가장 마지막요소를 전달된 객체로 대체한다.


### 편한 이해방식

커서가 달린 타자연습을 한다고 생각하면 편하다.

우리가 글씨하나를 추가하면 글씨오른쪽에 커서가 자동으로 이동하고

커서는 항상 리스트의 요소들 사이에 존재하고

단, 지울때 우리는 백스페이스 버튼으로 커서 왼쪽요소를 지우지만

여기서는 remove() 메서드에 의해 항상 오른쪽 요소가 제거되므로

왼쪽 요소를 지우고싶다면 previous()를 호출한 후 제거해야 한다.

출처 : [TCP스쿨 Iterator](http://www.tcpschool.com/java/java_collectionFramework_iterator)