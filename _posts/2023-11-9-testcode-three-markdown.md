---
layout: post
title: Junit5와 AssertJ를 활용한 테스트 코드 작성기 - 03
subtitle: 백준1919 문제를 TDD로 풀기
categories: Java
tags: [Java, 테스트]
---

#### 백준1919 애너그램 만들기

##### 문제 

> - 두 영어 단어가 철자의 순서를 뒤바꾸어 같아질 수 있을 때, 그러한 두 단어를 서로 애너그램 관계에 있다고 한다. 예를 들면 occurs 라는 영어 단어와 succor 는 서로 애너그램 관계에 있는데, occurs의 각 문자들의 순서를 잘 바꾸면 succor이 되기 때문이다.
> 
> - 한 편, dared와 bread는 서로 애너그램 관계에 있지 않다. 하지만 dared에서 맨 앞의 d를 제거하고, bread에서 제일 앞의 b를 제거하면, ared와 read라는 서로 애너그램 관계에 있는 단어가 남게 된다.
>
> - 두 개의 영어 단어가 주어졌을 때, **두 단어가 서로 애너그램 관계에 있도록 만들기 위해서 제거해야 하는 최소 개수의 문자 수**를 구하는 프로그램을 작성하시오. 문자를 제거할 때에는 아무 위치에 있는 문자든지 제거할 수 있다.

---

##### 입력

> - 첫째 줄과 둘째 줄에 영어 단어가 소문자로 주어진다. 각각의 길이는 1,000자를 넘지 않으며, 적어도 한 글자로 이루어진 단어가 주어진다.

---

##### 출력

> - 첫째 줄에 답을 출력한다.


##### 입출력 예시

|입력 (문자열 2개)|출력 (정수형)|
|---|---|
|aabbcc<br>xxyybb|8|

---

#### **1. 기능 분석**

    - 애너그램을 인위적으로 만드려고 하기보다 그것이 성립되기 위한 최소 조건을 구한다는 관점으로 접근한다.
    - 두 개의 영단어에서 중복되지 않는 알파벳의 개수를 구하면 그만큼의 철자를 제거했을 때 애너그램이 성립된다.

**기능1:** 두 개의 영단어를 읽는다. -> 두 문자열을 입력받는 것으로 테스트 코드는 작성하지 않는다.

**기능2:** 한 영단어에 대하여 알파벳 개수를 담고있는 리스트(이하, 알파벳 리스트) 생성을 수행한다.

- 함수명: MakeAlphabetCountVectorForSingleString

|입력 (String)|출력 (List\<int>)|
|---|---|
|aabbcc|[2,2,2,...,0]|

**기능3:** 두 알파벳 리스트를 비교하여 중복되지 않는 철자의 개수의 합을 구한다.

- 함수명: AbsoluteSumForSubstractionOfTwoVectors

|입력 (List\<int>, List\<int>)|출력 (int)|
|---|---|
|[2,0,2,2]<br>[2,1,2,1]|2|

---

#### **2. 알파벳 리스트 생성 함수 테스트 코드 작성**


각 기능은 하나의 함수로 코딩되기전 테스트 코드로 작성된다.
의존성은 [이전에 작성한 포스트](https://rosskwsang.github.io/java/2023/11/01/testcode-one-markdown.html) 참고


**<center>MakeAlphabetCountVectorForSingleStringTest</center>**

```Java
    @Test
    @DisplayName(value="인코딩 함수(문자열 -> 벡터) 동작 테스트")
    void makeAlphabetCountVectorForSingleStringTest() {
        Function<String, List<Integer>> stringEncoder = Anagram::makeAlphabetCountVectorForSingleString;
        assertThat(stringEncoder.apply("aabbcc"))
                .isEqualTo(
                        Stream.concat( // 1
                                Stream.of(Arrays.asList(2, 2, 2)).flatMap(List::stream), // 2
                                IntStream.generate(() -> 0).limit(23).boxed() // 3
                        )
                                .collect(Collectors.toList()) // 4
                );
    }
```

해당 테스트는 aabbcc를 [2,2,2,0,0,...,0]으로 바꾸는 함수를 상정하고 작성하였다.
1. 두 개의 스트림을 순서대로 병합한다.
2. 앞단에 들어갈 2,2,2 Array를 스트림으로 변환하고 flat하여 동일한 차원에서 병합되도록 한다. FlatMap을 실행하지 않으면 결과는 [[2,2,2],0,0,...,0]가 된다.
3. IntStream.generate는 정수를 무한히 생성하고, Limit은 생성되는 정수의 개수를 조절한다. boxed()를 실행하여 IntStream을 Stream<Integer>로 변환한다.
4. 병합된 Stream을 List로 변환하여 수집(Collect)한다.

---

#### **3 . 알파벳 리스트 생성 함수 작성 및 테스트 결과**
```Java
    public static List<Integer> makeAlphabetCountVectorForSingleString(String input) {
        List<Integer> result = IntStream.generate(() -> 0).limit(26).boxed().collect(Collectors.toList()); // 1
        input.chars() // 2
                .forEach(ch -> {
                    if (Character.isAlphabetic(ch)) {
                        result.set(ch - 'a', result.get(ch - 'a') + 1); // 3
                    }
                });
        return result;
    }
```
1. 테스트 코드와 동일한 방식으로 0으로 채워진 List<Integer>를 생성
2. 인자로 받은 문자열을 IntStream으로 변환
3. Stream의 값이 알파벳에 해당하면 List<Integer>의 인덱스를 알파벳과 일치('a'를 소거)시키고 1을 추가함

* 테스트 결과는 **성공**

---

#### **4. 리스트 절대차이의 합을 구하는 함수 테스트 코드 작성**

```Java
    @Test
    @DisplayName(value="두 리스느 원소의 차이의 절대값의 합을 구하는 테스트 ")
    void absoluteSumForSubtractionOfTwoVectors() {
        BiFunction<List<Integer>, List<Integer>, Integer> distanceMeasurer = Anagram::absoluteSumForSubtractionOfTwoVectors;
        assertThat(distanceMeasurer.apply(
                Arrays.asList(2, 2, 2),
                Arrays.asList(2, 3, 1)
                )
        )
                .isEqualTo(2);
    }
```
위 테스트는 List를 두 개 받아 벡터 차를 구하여 정확한 값을 출력하는 지 테스트한다. 가령 [2, 2, 2]와 [2, 3, 1]의 차이는 [0, 1, 1]이고 전체를 합했을 때 나올 아웃풋이 2와 일치하는 지 테스트한다.

---

#### **5. 리스트 절대차이의 합을 구하는 함수 작성 및 테스트 결과**

```Java
    public static int absoluteSumForSubtractionOfTwoVectors(List<Integer> vec1, List<Integer> vec2) {

        return IntStream
                .range(0, Math.min(vec1.size(), vec2.size()))
                .mapToObj(i -> Math.abs(vec1.get(i) - vec2.get(i)))
                .mapToInt(Integer::intValue)
                .sum();
    }
```

* 테스트 결과는 **성공**


#### **6. 기능 통합과 문제 풀이 결과**

```Java
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String word1 = br.readLine();
        String word2 = br.readLine();
        System.out.println(absoluteSumForSubtractionOfTwoVectors
                (
                        makeAlphabetCountVectorForSingleString(word1),
                        makeAlphabetCountVectorForSingleString(word2)
                )
        );
    }
```

기능 통합은 main함수에서 다음과 같이 이루어졌으며 백준에 제출한 결과 통과 판정을 받음


#### **7. 회고**
- 문제의 명세를 자세히 보았을 때 작성할 필요없는 조건문을 사용한 경우가 있음
- Stream을 사용하고 있지만 함수형으로 작성이 되고 있는 것 같지는 않음 -> 이 부분에 대한 공부가 필요함 

