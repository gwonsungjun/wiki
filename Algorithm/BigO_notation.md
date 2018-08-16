# Big O Notation (대문자 O 표기법)

## 기본 개념
- 시간 복잡도(Time Complexity) - 알고리즘의 수행시간 분석결과 (cpu 사용량, 실제로는 명령어의 실행 횟수만을 고려)
- 공간 복잡도(Space Complexity) - 알고리즘의 메모리 사용량에 대한 분석결과
- 점근적 표기법에서 점근적이란? - 가장 큰 영향을 주는 항만 계산 한다는 의미
    - 최상의 경우 : 오메가 표기법 (Big-Ω Notation)
        - f(n) = Ω(g(n))이 성립하려면
        - f(x) > C * g(x)를 만족하는 상수 C가 존재해야 한다.
    - `최악의 경우 : 빅오 표기법 (Big-O Notation)`
        - f(n) = O(g(n))이 성립하려면
        - f(x) <= C * g(x)를 만족하는 상수 C가 존재해야 한다.
    - 평균의 경우 : 세타 표기법 (Big-θ Notation)
        - f(n) = θ(g(n))이 성립하려면
        - f(n) = Ω(g(n))이 참이고
        - f(n) = O(g(n))이 참이어야 한다. (둘다 참일 경우만)

## 빅오 표기법 : 알고리즘 실행 시간(얼마나 빠른지)을 나타내는 방법
- 빅오표기법은 알고리즘이 동작하기 위해 필요한 연산 횟수를 나타낸다.
- 최악의 실행 시간을 나타낸다.
    - 예를 들어 전화번호부에서 Adit이라는 이름을 찾는다 했을때 한 번에 찾는다면 O(1)이 된다. 하지만 최악의 경우 전화번호부에서 모든 이름을 확인해야 하므로 O(n).
- 이렇게 하면 입력 데이터의 크기가 늘어날 때 알고리즘의 실행 속도가 얼마나 증가하는지 알 수 있다.

## O(1)

```java
public void printFirstItem(int[] arrayOfItems){
    System.out.println(arrayOfItems[0]);
}
```
- 하나의 index에 접근, 따라서 O(1)
- other example : stack(push, pop), Access hash table(하나의 key에만 접근하므로)
- Tip
    - nO(1) = O(1)
    - 2*O(1) = 10 * O(1) = O(1)
        - 2*O(1) = O(1) : Sysout을 두번해도 큰영향을 주지 않는다는 뜻.
    - Big O Notation에서 상수(n)는 무시된다. : 알고리즘 퍼포먼스에 큰 영향을 끼치지 않으므로.

## O(log n) - 로그 시간
- Binary Search(이진 탐색) Tree Access, Search, Insertion, Deletion
- Denote n = 8, log 8 = log 2^3 = 3
- Tip
    - nO(log n) = O(log n)
    - 2 * O(log n) = 10 * O(log n) = O(log n)

## O(n) - 선형 시간

```java
public void printAllItems(int[] arrayOfItems){
    for(int item : arrayOfItems){
        System.out.println(item);
    }
}
```

- 아이템의 갯수만큼 Access(단순 탐색)
- other example : traverse tree, traverse linked list
- Tip
    - nO(n) = O(n)
    - 2 * O(n) = 10 * O(n) = O(n)

## O(nlog n)
- Quick Sort, Merge Sort, Heap Sort
- Denote n = 4, log 8 = 2 * log 2^2 = 4
- Tip
    - nO(nlog n) = O(nlog n)
    - 2 * O(nlog n) = 10 * O(nlog n) = O(nlog n)

## O(n^2)

```java
public void printAllPossibleOrderdPairs(int[] arrayOfItems){
    for (int firstItem : arrayOfItems){
        for (int secondItem : arrayOfItems){
            int[] orderPair = new int[] {firstItem, secondItem};
            System.out.println(Arrays.toString(orderPair));
        }
    }
}
```

- outer loop, inner loop : 배열에 10개의 item이 있다면 총 100번 accessing
- other example : Insertion sort, Bubble sort, Selection sort
- Tip
    - nO(n^2) = O(n^2)
    - 2 * O(n^2) = 10 * O(n^2) = O(n^2)

## O(n!)
- 외판원 문제(traveling salesperson problem)와 같이 정말 느린 알고리즘.


## Links
- [빅오 표기법 - 허민석님 영상](https://www.youtube.com/watch?v=OVRLHJC95Gs)
- [Hello Coding  그림으로 개념을 이해하는 알고리즘](https://book.naver.com/bookdb/book_detail.nhn?bid=11823284)
- [바보코딩 알고리즘](https://www.youtube.com/watch?v=Chcl71vEkRg)