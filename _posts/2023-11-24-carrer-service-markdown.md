---
layout: post
title:
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

| 입력 (문자열 2개) | 출력 (정수형) |
| ----------------- | ------------- |
| aabbcc<br>xxyybb  | 8             |

---

#### **1. 기능 분석**

    - 애너그램을 인위적으로 만드려고 하기보다 그것이 성립되기 위한 최소 조건을 구한다는 관점으로 접근한다.
    - 두 개의 영단어에서 중복되지 않는 알파벳의 개수를 구하면 그만큼의 철자를 제거했을 때 애너그램이 성립된다.

**기능1:** 두 개의 영단어를 읽는다. -> 두 문자열을 입력받는 것으로 테스트 코드는 작성하지 않는다.

**기능2:** 한 영단어에 대하여 알파벳 개수를 담고있는 리스트(이하, 알파벳 리스트) 생성을 수행한다.

- 함수명: MakeAlphabetCountVectorForSingleString

| 입력 (String) | 출력 (List\<int>) |
| ------------- | ----------------- |
| aabbcc        | [2,2,2,...,0]     |

**기능3:** 두 알파벳 리스트를 비교하여 중복되지 않는 철자의 개수의 합을 구한다.

- 함수명: AbsoluteSumForSubstractionOfTwoVectors

| 입력 (List\<int>, List\<int>) | 출력 (int) |
| ----------------------------- | ---------- |
| [2,0,2,2]<br>[2,1,2,1]        | 2          |

#### **2. 알파벳 리스트 생성 함수**

각 기능은 하나의 함수로 코딩되기전 테스트 코드로 작성된다.
의존성은 [이전에 작성한 포스트](https://rosskwsang.github.io/java/2023/11/01/testcode-one-markdown.html) 참고

**<center>MakeAlphabetCountVectorForSingleStringTest</center>**

```Java
    @Test
    void test1() {
        // given
        String input = "aabbcc";

        // when
        List<Integer> result = MakeAlphabetCountVectorForSingleString(input);

        // then
        assertThat(result).isEqualTo(Arrays.asList(2, 2, 2, 1));
    }
```

```Java
    @Test
    void test2() {
        // given
        String input = "aaaaa";

        // when
        List<Integer> result = MakeAlphabetCountVectorForSingleString(input);

        // then
        assertThat(result).isEqualTo(Arrays.asList(5));
    }

```

**<center>MakeAlphabetCountVectorForSingleString</center>**

```Java

```

- 테스트 클래스 : SetTest.java
- 요구사항 명세
  - 클래스내 numbers라는 셋을 선언하고 setUp 메서드에 셋에 몇가지 숫자를 추가함 해당 메서드는 @BeforeEach Annotation을 가지고 있기 때문에 각각의 개별 테스트 이전에 한번 씩 실행됨
  - 테스트 항목 01 : Set의 size() 메소드를 활용해 Set의 크기를 확인하는 학습테스트 구현
  - 테스트 항목 02 : Set의 contains() 메소드를 활용해 1,2,3의 값이 존재하는지 확인하는 학습테스트를 구현, JUnit의 ParameterizedTest를 활용하여 중복코드를 제거할 것
  - 테스트 항목 03 : 존재하지 않는 경우에 대하여 테스트가 가능하도록 구현, 예를들어 1,2,3 값은 contains 메소드 실행결과 true, 4,5 값을 넣으면 false 가 반환되는 테스트를 하나의 Test Case로 구현

---

**<center>셋업</center>**

```java
public class SetTest {
    private Set<Integer> numbers;

    @BeforeEach
    void setUp() {
        numbers = new HashSet<>();
        numbers.add(1);
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
    }
```

---

**<center>테스트 항목 01</center>**

```java
    @Test
    @DisplayName("위 정의된 Set의 크기가 3이 맞는지 확인 테스트")
    void testSetSize() {
        assertThat(numbers.size())
                .isEqualTo(3);
    }
```

**<center>테스트 항목 02</center>**

```java
    @ParameterizedTest
    @ValueSource(ints = {1, 2, 3})
    void testSetContainsCertainValues(int input) {
        assertTrue(numbers.contains(input));
    }
```

**<center>테스트 항목 03</center>**

```java
    @ParameterizedTest
    @CsvSource(value = {"1:true", "2:true", "3:true", "4:false", "5:false"}, delimiter = ':')
    void testSetContainsCertainValuesOrNot(String integerInput, String isContainedInput) {
         int integer = Integer.parseInt(integerInput);
         boolean isContained = Boolean.parseBoolean(isContainedInput);
         assertEquals(numbers.contains(integer), isContained);
    }
```

---

#### **3. 문자열 파싱 이후 덧셈을 수행하는 계산기 기능 구현 및 테스트**

- 기능 클래스 : ParsingCalculator.java
- 테스트 클래스 : ParsingCalculatorTest.java
- 요구사항 명세
  - 쉼표(,) 또는 콜론(:)을 구분자로 가지는 문자열을 전달하는 경우 구분자를 기준으로 분리한 각 숫자의 합을 반환 (예: “” => 0, "1,2" => 3, "1,2,3" => 6, “1,2:3” => 6)
  - 앞의 기본 구분자(쉼표, 콜론)외에 커스텀 구분자를 지정할 수 있음
  - 커스텀 구분자는 문자열 앞부분의 “//”와 “\n” 사이에 위치하는 문자를 커스텀 구분자로 사용
  - 예를 들어 “//;\n1;2;3”과 같이 값을 입력할 경우 커스텀 구분자는 세미콜론(;)이며, 결과 값은 6이 반환됨
  - 문자열 계산기에 숫자 이외의 값 또는 음수를 전달하는 경우 RuntimeException 예외를 throw

---

**<center>ParsingCalculator.java</center>**

```java
package com.example.utils;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;
import java.util.stream.Collectors;

public class ParsingCalculator {
    private ParsingCalculator() {
        throw new IllegalStateException("유틸리티 클래스에 대한 잘못된 접근: 초기화");
    }

    public static String joinList(String[] list) {
        return Arrays.stream(list)
                .collect(Collectors.joining("|"));
    }

    public static String extractCustomSeparator(String str) {
        Pattern pattern = Pattern.compile("//(.*)\n");
        Matcher matcher = pattern.matcher(str);

        if (matcher.find()) {
            return "[^/\n" + joinList(matcher.group(1).split("")) + "]";
        } else {
            return "[^,|:]";
        }
    }

    public static int parseAndSum(String str) {

        String delimiter = extractCustomSeparator(str);

        Pattern pattern = Pattern.compile(delimiter);
        Matcher matcher = pattern.matcher(str);

        List<String> tokens = new ArrayList<>();
        while (matcher.find()) {
            tokens.add(matcher.group());
        }

        int sum = 0;
        for (String token : tokens) {
            try {
                int number = Integer.parseInt(token);
                if (number < 0) {
                    throw new RuntimeException("음수 값은 허용되지 않습니다.");
                }
                sum += number;
            } catch (NumberFormatException e) {
                throw new RuntimeException("숫자 이외의 값은 허용되지 않습니다.");
            }
        }
        return sum;
    }
}
```

---

**<center>ParsingCalculatorTest.java</center>**

```java
package com.example.utils;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import java.util.function.Function;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

class ParsingCalculatorTest {

    int parseAndSum(String input) {
        Function<String, Integer> calculator = ParsingCalculator::parseAndSum;
        return calculator.apply(input);
    }

    @Test
    @DisplayName("공백문자를 넣을 경우 0을 출력")
    void emptyString() {
        assertEquals(0, parseAndSum(""));
    }

    @Test
    @DisplayName("단일 문자 테스트")
    void singleNumber() {
        assertEquals(1, parseAndSum("1"));
    }

    @Test
    @DisplayName("복수 문자 테스트")
    void multipleNumbers() {
        assertEquals(6, parseAndSum("1,2,3"));
    }

    @Test
    @DisplayName("복수 문자 테스트")
    void multipleNumbersWithCustomDelimiter() {
        assertEquals(6, parseAndSum("//.:\n1.2:3"));
    }

    @Test
    @DisplayName("음수 포함시 런타임 에러")
    void negativeNumber() {
        assertThrows(RuntimeException.class, () -> parseAndSum("1,-2,3"));
    }

    @Test
    @DisplayName("숫자가 아닌 문자를 포함시 런타임 에러")
    void nonNumericString() {
        assertThrows(RuntimeException.class, () -> parseAndSum("a,b,c"));
    }
}
```
