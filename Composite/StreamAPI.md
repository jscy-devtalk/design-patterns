  # [Java] Stream API
Stream API는 JDK8에서 함수형 인터페이스, 람다식과 함께 추가된 기능이다. 컬렉션, 배열 등의 데이터를 

## 사용하는 이유?
- **가독성 향상 및 유지보수 용이**

Stream API는 함수형 프로그래밍을 지원하기 때문에 람다식, 메서드 참조를 통해 보다 직관적이고 선언적인 코드를 작성할 수 있다. 즉 간결하고 쉬운 코드를 작성하고 성능을 높이는 것을 목적으로 사용한다. 

*물론 병렬 처리나 데이터 처리 연산을 지원한다는 점에서도 의미가 있다. 

**[Stream API 사용 전]**
```java
String[] names = {"Warren Buffet", "Jeff Bezos", "Mark Zuckerberg", "Elon Musk"}
List<String> nameList = Arrays.asList(names);

Arrays.sort(names)
Collections.sort(nameList);

for (String name : names) {
  System.out.println(name);
}

for (String name : namelist) {
  System.out.println(name);
}
```

**[Stream API 사용 후]**
```java
String[] names = {"Warren Buffet", "Jeff Bezos", "Mark Zuckerberg", "Elon Musk"}
List<String> nameList = Arrays.asList(names);

Stream<String> nameStream = nameList.stream();
Stream<String> arrayStream = Arrays.stream(names);

nameStream.sorted().forEach(System.out::println);
arrayStream.sorted().forEach(System.out::println);

// 더 간결한 버전
nameList.stream().sorted().forEach(System.out::println);
Arrays.stream(names).sorted().forEach(System.out::println);
```

## Stream API의 특징
- 원본 데이터를 변경하지 않는다.
- 일회용이다.
- 내부 반복으로 작업을 처리한다.

1. 원본 데이터를 변경하지 않는다.
Stream API는 데이터를 조회하여 별도의 요소들로 Stream 객체를 생성한다. 뒤에 얘기할 중간 연산 작업은 Stream 요소들로 처리한다.

2. 일회용이다.
Stream API는 한번 사용이 끝나면 재사용이 불가능하다. 사용이 끝나는 기준은 최종 연산(소비)이며 최종 연산 시 모든 Stream 요소를 소모한다. 만약 최종연산이 끝난 후 다시 사용하려고 하면 IllegalStateException이 발생하게 된다.

## 2. Stream Pipeline(구조)
스트림 파이프라인은 데이터 소스(생성), 중간 연산(가공), 최종 연산(소비)으로 구성된다. 

각 단계는 메서드 체인(Method Chain)으로 구성되어 요소들을 처리한 후 다음 단계로 전달한다. 

예를 들어 리스트를 Stream API를 사용해 필터링하는 예제를 확인해보자
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
int sum = numbers.stream()  // 데이터 원본이 되는 소스
  .filter(n -> n % 2 == 1)  // 중간 연산: 홀수 필터링
  .mapToInt(n -> n * n)     // 중간 연산: 각 홀수의 제곱
  .sum();                   // 최종 연산: 합계 계산

System.out.println(sum);
```
위처럼 연산이 세미콜론 없이 여러 번 연결되는 것을 연산 파이프라인이라고 하기도 한다. 

### 생성
Stream API를 사용하기 위해 Stream 객체를 생성하는 단계이다. 
Collection, 배열, File 등 다양한 데이터로부터 생성할 수 있다. 

### 가공
원본의 데이터를 입맛에 맞게 데이터를 처리하는 단계이다. 연산 결과는 Stream으로 반환되어 중간연산을 연속적으로 이어갈 수 있다. 
```java
public class StreamTest {
  public static void main(String[] args) {
    List<String> names = Arrays.asList("Jeong", "Sim", "Choi", "Yo");

    List<String> streamNames = names.stream()
        .filter(name -> name.length() < 4) // 길이가 4 초과인 문자열 필터
        .map(String::toUpperCase) // 대문자로 변환
        .distinct() // 중복 객체 제거
        .sorted() // 정렬
        .collect(Collectors.toList());

    System.out.println(streamNames);
  }
}
```

### 소비
가공된 데이터로부터 결과를 만들기 위한 연산이다. Stream 객체 당 1번만 처리할 수 있다. 

## 단점은 없나?
Stream API의 대표적인 단점으로는 컴퓨팅 비용이 있다.
기존의 for-loop방식과 비교했을 때 스트림은 내부적으로 다양한 조건과 상황에 따라서 연산을 처리하기 때문에 생성하는데 적지 않은 비용이 발생한다.

## 정리
Stream은 저장 요소를 하나씩 참조하며, 함수형 인터페이스를 적용하며 반복적으로 처리할 수 있도록 해주는 기능이다.  
이 과정에서 for, if 등으로 인해 만들어지는 불필요한 문장을 걷어낼 수 있어 직관적인 코드를 작성할 수 있다.  
  
단, 남발해서는 안되고 상황에 따라 적절하게, 순서를 고려하여 사용하는 것이 바람직하다.
