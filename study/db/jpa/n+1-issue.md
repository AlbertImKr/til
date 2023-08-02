---
description: N+1 문제
---

# N+1 Issue

## 설정

### Member Class

```java
@Entity
@Table(name = "member")
public class Member {
    ...
    @Embedded
    private Favorites favorites;
    ...
}
```

### Favorites Class

```java
@Embeddable
public class Favorites {
    ...

    @OneToMany(mappedBy = "member", cascade = {CascadeType.PERSIST, CascadeType.MERGE}, orphanRemoval = true)
    private List<Favorite> favoriteCollection;

    ...
}
```

### FavoriteData Class

```java
@Table(name = "favorite")
@Entity
public class FavoriteDataResponse {

    ... 
    
    @JsonIgnore
    @ManyToOne(cascade = CascadeType.PERSIST)
    @JoinColumn(name = "member_id")
    private Member member;
    @ManyToOne
    @JoinColumn(name = "source_id")
    private StationResponse source;
    @ManyToOne
    @JoinColumn(name = "target_id")
    private StationResponse target;
    
    ...
```

### StationData Class

```java
@Entity
@Table(name = "station")
public class StationData {

    ...
}
```

### FavoriteDataRepository Class

```java
public interface FavoriteDataRepository extends JpaRepository<FavoriteData, Long> {

    List<FavoriteData> findAllByMember(Member member);
}
```

## 문제

### 테스트 결과

#### 하위 Station 만큼 퀴리 N개  및 favoritedate 테이블 조회  퀴리 1개 총 N+1 개 발생한다.

```sql
------------ 즐겨 찾기 전체 조회 요청 ------------
....
Hibernate: 
    select
        favoriteda0_.id as id1_0_,
        favoriteda0_.member_id as member_i2_0_,
        favoriteda0_.source_id as source_i3_0_,
        favoriteda0_.target_id as target_i4_0_ 
    from
        favorite favoriteda0_ 
    where
        favoriteda0_.member_id=?
Hibernate: 
    select
        stationres0_.id as id1_4_0_,
        stationres0_.name as name2_4_0_ 
    from
        station stationres0_ 
    where
        stationres0_.id=?
Hibernate: 
    select
        stationres0_.id as id1_4_0_,
        stationres0_.name as name2_4_0_ 
    from
        station stationres0_ 
    where
        stationres0_.id=?
Hibernate: 
    select
        stationres0_.id as id1_4_0_,
        stationres0_.name as name2_4_0_ 
    from
        station stationres0_ 
    where
        stationres0_.id=?
------------ 즐겨 찾기 전체 조회 요청 ------------
```

## 해결

### 방법1:  JPQL

#### 조인을 사용해서 조회시 station 까지 가져온다

```java
public interface FavoriteDataRepository extends JpaRepository<FavoriteData, Long> {

    @Query("select favorite,source,target,member_T "
            + "from FavoriteData favorite "
            + "join StationData source "
            + "on favorite.source.id = source.id "
            + "join StationData target "
            + "on favorite.target.id = target.id "
            + "join Member member_T "
            + "on favorite.member.id = member_T.id "
            + "where favorite.member = :member")
    List<FavoriteData> findAllByMember(Member member);
}
```

#### 테스트 결과&#x20;

* 하나의 쿼리 문제 해결

```sql
------------ 즐겨 찾기 전체 조회 요청 ------------
Hibernate: 
    select
        favoriteda0_.id as id1_0_0_,
        stationdat1_.id as id1_4_1_,
        stationdat2_.id as id1_4_2_,
        member3_.id as id1_2_3_,
        favoriteda0_.member_id as member_i2_0_0_,
        favoriteda0_.source_id as source_i3_0_0_,
        favoriteda0_.target_id as target_i4_0_0_,
        stationdat1_.name as name2_4_1_,
        stationdat2_.name as name2_4_2_,
        member3_.age as age2_2_3_,
        member3_.email as email3_2_3_,
        member3_.password as password4_2_3_,
        member3_.role as role5_2_3_ 
    from
        favorite favoriteda0_ 
    inner join
        station stationdat1_ 
            on (
                favoriteda0_.source_id=stationdat1_.id
            ) 
    inner join
        station stationdat2_ 
            on (
                favoriteda0_.target_id=stationdat2_.id
            ) 
    inner join
        member member3_ 
            on (
                favoriteda0_.member_id=member3_.id
            ) 
    where
        favoriteda0_.member_id=?
------------ 즐겨 찾기 전체 조회 요청 ------------
```

### 방법2:  EntityGraph

#### EntityGraph의 attributePaths를 사용해서 한번 가져온다

```java
public interface FavoriteDataRepository extends JpaRepository<FavoriteData, Long> {

    @EntityGraph(
        attributePaths = {"source","target","member"}
    )
    List<FavoriteData> findAllByMember(Member member);
}
```

#### 테스트 결과

```sql
------------ 즐겨 찾기 전체 조회 요청 ------------
Hibernate: 
    select
        favoriteda0_.id as id1_0_0_,
        member1_.id as id1_2_1_,
        stationdat2_.id as id1_4_2_,
        stationdat3_.id as id1_4_3_,
        favoriteda0_.member_id as member_i2_0_0_,
        favoriteda0_.source_id as source_i3_0_0_,
        favoriteda0_.target_id as target_i4_0_0_,
        member1_.age as age2_2_1_,
        member1_.email as email3_2_1_,
        member1_.password as password4_2_1_,
        member1_.role as role5_2_1_,
        stationdat2_.name as name2_4_2_,
        stationdat3_.name as name2_4_3_ 
    from
        favorite favoriteda0_ 
    left outer join
        member member1_ 
            on favoriteda0_.member_id=member1_.id 
    left outer join
        station stationdat2_ 
            on favoriteda0_.target_id=stationdat2_.id 
    left outer join
        station stationdat3_ 
            on favoriteda0_.source_id=stationdat3_.id 
    where
        favoriteda0_.member_id=?
------------ 즐겨 찾기 전체 조회 요청 ------------
```

## 기타

#### 멤버 조회시 FetchType Lazy 전략 사용

```java
@Entity
@Table(name = "member")
public class Member {

    ...
    @Basic(fetch = FetchType.LAZY)
    @Embedded
    private Favorites favorites;
    
    ...
}
```
