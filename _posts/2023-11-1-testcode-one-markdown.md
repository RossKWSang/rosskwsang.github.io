---
layout: post
title: Junit5와 AssertJ를 활용한 테스트 코드 작성기 - 01
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


#### **1. build.gradle에 의존성 추가**

```gradle
dependencies {
    testImplementation 'org.assertj:assertj-core:3.22.0'
    testImplementation 'org.junit.jupiter:junit-jupiter:5.8.2'
}
```

build.gradle을


#### **2. 간단한 기능과 테스트 코드 작성**

![트리구조](/assets/images/tree_dir.png)

의존성 설정이 완료되면 다음과 같이 디렉터리와 클래스를 생성한다.

각각의 테스트 코드는 build.gradle에 인식되도록 test항목에 추가한다.

```gradle
test {
    useJUnitPlatform()
    include 'com/example/utils/StringSplitterTest.class'
    include 'com/example/utils/ParenthesisRemoverTest.class'
    include 'com/example/utils/CharacterExtractorTest.class'
}
```


#### **3. 문자열 분리 기능 구현 및 테스트**

* 기능 클래스 : StringSplitter.java
* 테스트 클래스 : StringSplitterTest.java
* 요구사항 명세 
  * 문자열을 일정한 delimeter에 따라 split하여 리스트로 반환하는 함수 작성
  * 테스트 항목 01 : "1,2"를 ` , ` 로 split했을 때 1과 2로 잘 분리되는지 확인
  * 테스트 항목 02 : "1"을 ` , `로 split했을 때 1만을 포함하는 배열이 반환되는지 확인


---

**<center>StringSplitter.java</center>**
```java
package com.example.utils;

public class StringSplitter {
    private StringSplitter() {
        throw new IllegalStateException("유틸리티 클래스에 대한 잘못된 접근: 초기화");
    }
    public static String[] splitString(String input, String delimiter) {
        return input.split(delimiter);
    }
}
```
---

**<center>StringSplitterTest.java</center>**
```java
package com.example.utils;

import com.example.utils.StringSplitter;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.assertThat;

import java.util.function.BiFunction;

public class StringSplitterTest {
    @Test
    @DisplayName("문자열 2개를 split한 결과를 테스트")
    public void testSplitStringWithCommaDelimiter() {
        String input = "1,2";
        String delimiter = ",";

        BiFunction<String, String, String[]> splitter = StringSplitter::splitString;
        String[] result = splitter.apply(input, delimiter);

        assertThat(result)
                .contains("1")
                .contains("2");
    }

    @Test
    @DisplayName("문자열 1개를 split한 결과를 테스트")
    public void testSplitStringWithNoDelimiter() {
        String input = "1";
        String delimiter = ",";

        BiFunction<String, String, String[]> splitter = StringSplitter::splitString;
        String[] result = splitter.apply(input, delimiter);

        assertThat(result).containsExactly("1");
    }
}

```
---

#### **4. 문자열 좌우말단 소괄호 제거 기능 구현 및 테스트**

* 기능 클래스 : ParenthesisRemover.java
* 테스트 클래스 : ParenthesisRemoverTest.java
* 요구사항 명세 
  * "(1,2)" 값이 주어졌을 때 String의 substring() 메소드를 활용해 ` () ` 을 제거하고 "1,2"를 반환


---

**<center>ParenthesisRemover.java</center>**
```java
package com.example.utils;

public class ParenthesisRemover {
    private ParenthesisRemover() {
        throw new IllegalStateException("유틸리티 클래스에 대한 잘못된 접근: 초기화");
    }
    public static String remove(String input) {
        return input.substring(1, input.length() - 1);
    }
}

```
---

**<center>ParenthesisRemoverTest.java</center>**
```java
package com.example.utils;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import java.util.function.Function;

import static org.assertj.core.api.Assertions.assertThat;

public class ParenthesisRemoverTest {
    @Test
    @DisplayName("소괄호를 적절히 제거했는지 테스트")
    public void testSplitStringWithCommaDelimiter() {
        String input = "(1,2)";

        Function<String, String> remover = ParenthesisRemover::remove;
        String result = remover.apply(input);

        assertThat(result)
                .isEqualTo("1,2");
    }
}
```
---

#### **5. 문자열에서 인덱싱된 문자(Char) 추출 기능 구현 및 테스트**

* 기능 클래스 : ParenthesisRemover.java
* 테스트 클래스 : ParenthesisRemoverTest.java
* 요구사항 명세 
  * "abc"값이 주어졌을 때 String의 charAt() 메서드를 활용해 특정 위치의 문자를 가져오는 학습 테스트
  * 문자를 가져올 때 위치 값을 벗어나면 StringIndexOutOfBoundsException이 발생하도록 함
  * JUnit의 @DisplayName을 활용하여 테스트 메소드의 의도를 드러냄


---

**<center>CharacterExtractor.java</center>**
```java
package com.example.utils;

public class CharacterExtractor {
    private CharacterExtractor() {
        throw new IllegalStateException("유틸리티 클래스에 대한 잘못된 접근: 초기화");
    }

    public static char extract(String input, int index) {
        return input.charAt(index);
    }
}
```
---

**<center>CharacterExtractorTest.java</center>**
```java
package com.example.utils;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import java.util.function.BiFunction;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class CharacterExtractorTest {
    @Test
    @DisplayName("적절한 철자를 뽑았는지 테스트 & 문자열 인덱스를 벗어나는 경우 예외처리")
    public void testCharacterExtractor() {
        String input = "abc";

        BiFunction<String, Integer, Character> extractor = CharacterExtractor::extract;

        assertThat(extractor.apply(input, 0))
                .isEqualTo('a');

        assertThat(extractor.apply(input, 1))
                .isEqualTo('b');

        assertThat(extractor.apply(input, 2))
                .isEqualTo('c');

        assertThrows(StringIndexOutOfBoundsException.class, () -> input.charAt(4));
    }
}
```

#### **6. 테스트 결과 검토**

다음과 같은 메시지가 나오면 테스트가 적절히 수행된 것
```cmd
> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE
> Task :compileTestJava
> Task :processTestResources NO-SOURCE
> Task :testClasses
> Task :test
BUILD SUCCESSFUL in 461ms
3 actionable tasks: 2 executed, 1 up-to-date
오후 10:34:49: Execution finished 'test'.
```

다음과 같은 메시지가 나오면 테스트에 실패한 케이스가 존재
```cmd
> Task :compileJava
> Task :processResources NO-SOURCE
> Task :classes
> Task :compileTestJava
> Task :processTestResources NO-SOURCE
> Task :testClasses
> Task :test FAILED

Expected :'d'
Actual   :'c'
...
1 test completed, 1 failed
FAILURE: Build failed with an exception.
```
