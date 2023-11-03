---
layout: post
title: Junit5와 AssertJ를 활용한 테스트 코드 작성기 - 02
subtitle: 
categories: Java
tags: [Java, 테스트]
---

#### JUnit5와 AssertJ를 활용한 간단한 기능구현과 테스트 코드 작성기



> ##### Version
> * JUnit5 : 5.8.2
> * AssertJ : 3.22.0
> * Gradle : 6.8.1

---


#### **1. build.gradle에 테스트 코드 추가**

**Junit5와 AssertJ를 활용한 테스트 코드 작성기 - 01**에서 작성한 테스트 코드 이외에도 ***SetTest.class*** 와 ***ParsingCalculatorTest.class***를 추가하고 빌드

**<center>build.gradle</center>**

```gradle
test {
    useJUnitPlatform()
    include 'com/example/utils/StringSplitterTest.class'
    include 'com/example/utils/ParenthesisRemoverTest.class'
    include 'com/example/utils/CharacterExtractorTest.class'
    include 'com/example/utils/SetTest.class'
    include 'com/example/utils/ParsingCalculatorTest.class'
}
```


#### **2. 셋(Set)을 구현한 이후 몇가지 JUnit 기능 활용 테스트**

* 테스트 클래스 : SetTest.java
* 요구사항 명세 
  * 클래스내 numbers라는 셋을 선언하고 setUp 메서드에 셋에 몇가지 숫자를 추가함 해당 메서드는 @BeforeEach Annotation을 가지고 있기 때문에 각각의 개별 테스트 이전에 한번 씩 실행됨

  * 테스트 항목 01 : Set의 size() 메소드를 활용해 Set의 크기를 확인하는 학습테스트 구현
  * 테스트 항목 02 : Set의 contains() 메소드를 활용해 1,2,3의 값이 존재하는지 확인하는 학습테스트를 구현, JUnit의 ParameterizedTest를 활용하여 중복코드를 제거할 것
  * 테스트 항목 03 : 존재하지 않는 경우에 대하여 테스트가 가능하도록 구현, 예를들어 1,2,3 값은 contains 메소드 실행결과 true, 4,5 값을 넣으면 false 가 반환되는 테스트를 하나의 Test Case로 구현

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

* 기능 클래스 : ParsingCalculator.java
* 테스트 클래스 : ParsingCalculatorTest.java
* 요구사항 명세 
  * 쉼표(,) 또는 콜론(:)을 구분자로 가지는 문자열을 전달하는 경우 구분자를 기준으로 분리한 각 숫
자의 합을 반환 (예: “” => 0, "1,2" => 3, "1,2,3" => 6, “1,2:3” => 6)
앞의 기본 구분자(쉼표, 콜론)외에 커스텀 구분자를 지정할 수 있다. 커스텀 구분자는 문자열 앞부
분의 “//”와 “\n” 사이에 위치하는 문자를 커스텀 구분자로 사용한다.
예를 들어 “//;\n1;2;3”과 같이 값을 입력할 경우 커스텀 구분자는 세미콜론(;)이며, 결과 값
은 6이 반환되어야 한다.
문자열 계산기에 숫자 이외의 값 또는 음수를 전달하는 경우 RuntimeException 예외를 throw
한다.


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
