---
description: 번역
---

# JVM Internals

## Overview

이 글에서는 JVM(Java Virtual Machine)의 내부 아키텍처에 대해 설명합니다.다음 다이어그램은 Java Virtual Machine 사양 Java SE 7 Edition을 준수하는 일반적인 JVM의 주요 내부 구성 요소를 보여줍니다

<figure><img src="../../../.gitbook/assets/JVM_Internal_Architecture.png" alt=""><figcaption></figcaption></figure>

## Table of contents

이 다이어그램에 표시된 구성요소는 각각 아래의 두 색션으로 설명됩니다. 첫 번째 섹션은 각 스레드에 대해 생성되는 구성 요소를 다루고 두 번째 섹션은 스레드와 독립적으로 생성되는 구성 요소를 다룹니다

### [Threads](threads/)

* [JVM System Threads](threads/#jvm-system-threads)
* [Per Thread](threads/#per-thread)
  * [program Counter(PC)](threads/#program\_counter)
  * [Stack](threads/#stack)
  * [Native Stack](threads/#native-stack)
  * [Stack Restrictions](threads/#stack-restrictions)
  * [Frame](threads/frame.md)
    * [Local Vairable Array](threads/frame.md#local-vairable-array)
    * [Operand Stack](threads/frame.md#operand\_stack)
    * [Dynamic Linking](threads/frame.md#dynamic\_linking)

### [Shared Between Threads](./#shared-between-threads)

* [Heap](shared-between-threads/#heap)
* [Memory Management](shared-between-threads/#memory-management)
* [Non-Heap Memory](shared-between-threads/#non-heap-memory)
* [Just In Time(JIT) Compliation](shared-between-threads/#just-in-time-jit-compliation)
* [Method Area](shared-between-threads/#method-area)
* [Class File Structure](shared-between-threads/#class-file-structure)
* [Classloader](shared-between-threads/#classloader)
* [Faster Class Loading](shared-between-threads/#faster-class-loading)
* [Where is The Method Area](shared-between-threads/#where-is-the-method-area)
* [Classloader Reference](shared-between-threads/#classloader-reference)
* [Run Time Constant Poll](shared-between-threads/#constant\_pool)
* [Exception Table](shared-between-threads/#exception-table)
* [Symbol Table](shared-between-threads/#symbol-table)
* [Interned Strings(String Table)](shared-between-threads/#interned-strings-string-table)



## Reference

> [https://blog.jamesdbloom.com/JVMInternals.html](https://blog.jamesdbloom.com/JVMInternals.html)



