# 디미터 법칙

## 디미터 법칙이란?

LoD(Law of Demeter) 또는 principle of least knowledge은 소프트웨어, 특히 객체 지향 프로그램 개발을 위한 설계 지침입니다. 일반적인 형태에서 LoD는 느슨한 결합의 특수한 case입니다.

## 세 가지 간결한 요약 권장 사항

1. 각 유닛은 다른 유닛에 대한 제한된 지식만 가지고 있어야 합니다. 즉, 현재 유닛과 "밀접한" 관련이 있는 유닛만 있어야 합니다.
   1. 각 유닛은 친구들과만 대화해야 합니다. 낯선 사람과 이야기하지 말아야 합니다.
   2. 직계 친구에게만 이야기합니다.
2. 기본 개념은 "정보 은닉"의 원칙에 따라 주어진 객체가 다른 것(하위 구성 요소 포함)의 구조나 속성에 대해 가능한 한 적게 가정해야 한다는 것입니다.
3. 이는 모듈이 정당한 목적에 필요한 정보와 리소스만 소유하도록 지시하는 최소 권한 원칙의 결과로 볼 수 있습니다.