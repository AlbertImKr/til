---
description: 외래키 매핑
---

# mappedBy

### JPA에서 리스트에 `@OneToMany` 만 적용

```java
 @OneToMany
 private List<Section> sections = new ArrayList<>();
```

### Hibertnate는 중간을 중간 테이블을 생성한다

```sql
Hibernate: 
    insert 
    into
        section
        (id, distance, down_station_id, line_id, up_station_id) 
    values
        (default, ?, ?, ?, ?)
Hibernate: 
    insert 
    into
        line
        (id, color, name) 
    values
        (default, ?, ?)
Hibernate: 
    insert 
    into
        line_sections
        (line_id, sections_id) 
    values
        (?, ?)
```

### JPA에서 리스트에 `@OneToMany` 에서 `mappedBy`적용

```java
@OneToMany(mappedBy = "line")
private List<Section> sections = new ArrayList<>();
```

### 이 매핑은 왜래 키(Foreign Key)를 사용하여 중간 테이블이 생성하지 않는다

```sql
Hibernate: 
    insert 
    into
        section
        (id, distance, down_station_id, line_id, up_station_id) 
    values
        (default, ?, ?, ?, ?)
Hibernate: 
    insert 
    into
        line
        (id, color, name) 
    values
        (default, ?, ?)
```
