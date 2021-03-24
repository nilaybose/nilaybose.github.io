---
layout: post
title: Complete code coverage for Java Pojo
date: 2021-01-26 21:00:00 +0530
description: Complete code coverage for Java Pojo
img: junit_pojo.png
fig-caption: 
tags: [junit, unit-testing, code coverage, java, pojo]
---

#### Why code coverage of java pojo is necessary.

- Sometimes team feels that 85% or 90% is good enough coverage, as 10% belongs to java value objects and other non-critical codes.
- Anything less than 99% coverages is not ideal, as developers or team many miss critical business codes which may fall within 10% missed coverages.
- This java pojo tester will give code coverages for the pojo, so that developers/ team can achieve 95+% coverages and team has no exception.

#### Import the java pojo utility
```
git clone git@github.com:nilaybose/javaUnitTesting.git
import as a maven project
```

### Features

- 100% code coverages
- Ability to test java immutable objects without setters.
- Able to scan and do code coverages of all constructors.
- Able to do code coverages of Equals and Hash code methods and check missed implementations.
- Ability to test interface/ abstract class as class field types

#### Examples
- Refer the class [PojoTester](https://github.com/nilaybose/javaUnitTesting/blob/master/src/test/java/bose/edu/junit/util/PojoTester.java)
 
#### Examples Code coverage without Equals and Hash 
- Source [GenericProduct](https://github.com/nilaybose/javaUnitTesting/blob/master/src/main/java/bose/edu/junit/day1/GenericProduct.java)
- Test [TestGenericProduct](https://github.com/nilaybose/javaUnitTesting/blob/master/src/test/java/bose/edu/junit/day1/TestGenericProduct.java)
```java
    @Test
    @DisplayName("Pojo setter getter test")
    public void test() {
        assertThat("Validation passes", 
        PojoTester.validate(GenericProduct.class), is(true));
    }
```
- Code coverage
![Code Coverage](/assets/img/pojo_wohe.png)

#### Examples Code coverage with Equals and Hash 

- Source [GenericProductWithHashEquals](https://github.com/nilaybose/javaUnitTesting/blob/master/src/main/java/bose/edu/junit/day1/GenericProductWithHashEquals.java)
- Test [TestGenericProductWithHashEquals](https://github.com/nilaybose/javaUnitTesting/blob/master/src/test/java/bose/edu/junit/day1/TestGenericProductWithHashEquals.java)
```java
    @Test
    @DisplayName("Pojo setter getter test")
    public void test() {
        assertThat("Validation passes",
        PojoTester.validateWithEqualsAndHashcode(
        GenericProductWithHashEquals.class, null), is(true));
    }
```
- Code coverage
  ![Code Coverage](/assets/img/pojo_whe.png)
  
#### Examples ignore fields in Setter getter tests/ Equality, hashcode test
- There are many methods of validate we can use to ignore fields in setter/ getter tests or Equals/ Hash Code test
```java
   validateWithEqualsAndHashcode(Class<?> clazz,
                                 Map<Class<?>, Supplier<?>> customMappers,
                                 Set<String> ignoreFields,
                                 Set<String> ignoredEqualsAndHash) {
```
- Below code snippet will not match the **name** field for the class **GenericProductWithHashEquals**
```java
    PojoTester.validate(
            GenericProductWithHashEquals.class, null, Sets.newLinkedHashSet("name"));

    PojoTester.validateWithEqualsAndHashcode(
    GenericProductWithHashEquals.class, //Object under test
    null, // No custom mapper
    Sets.newLinkedHashSet("name"), // No setter check of name field
    Sets.newLinkedHashSet("name"))); // No equals/ hash code check for name field
```

#### Custom Mapper
- We use suppliers to create the field type of the objects to initialize and tests
- We can pass in a custom mapper if the field type is interface, abstract class or any types not defined in Pojo Tester
- If the field of class has SortedSet, which is not defined in default suppliers, we can pass as custom mapper

```java
        Map<Class<?>, Supplier<?>> customMappers = new HashMap<>();
        customMappers.put(SortedSet.class, 
                () -> { TreeSet s = new TreeSet(); s.add("abc"); return s;});

        PojoTester.validateWithEqualsAndHashcode(
                GenericProductWithHashEquals.class,
                customMappers, 
                Sets.newLinkedHashSet("name"), // No setter check of name field
                Sets.newLinkedHashSet("name")); // No equals/ hash code check for name field
```


**Note** - if you find this helpful, please like my project in github