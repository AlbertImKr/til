# RestAssured

## 상황

* 팀 웹 서비스 API  프로젝트 진행하면서 Spring MockMvc를 사용하여 테스트를 진행해왔습니다.

### **MockMvc의 한계**

* MockMvc는 Spring MVC의 컨트롤러 테스트에 특화되어 있지만, 실제 HTTP 요청과 응답의 흐름을 완전히 모방하지는 못합니다.
* 복잡한 설정과 상대적으로 **낮은 가독성**으로 인해 테스트 코드의 **유지 관리**가 어렵습니다.

### 예시 코드

```java
@Sql(statements = "insert into organization (title) values ('eojjeogojeojjeogo')")
@DisplayName("마일 스톤 수정 요청하면 200 코드를 응답한다")
@Test
void updateMilestone() throws Exception {
        // given
        long milestoneId = milestoneService.create(ORGANIZATION_TITLE, createMileStoneRequest);
        MilestoneRequest changedMilestoneRequest = new MilestoneRequest(TEST_TITLE, TEST_DESCRIPTION,
                TEST_DUE_DATE);
        // when
        mockMvc.perform(patch("/api/" + ORGANIZATION_TITLE + "/milestones/" + milestoneId)
                        .content(objectMapper.writeValueAsString(changedMilestoneRequest))
                        .contentType(MediaType.APPLICATION_JSON)
                        .accept(MediaType.APPLICATION_JSON)
                )
                // then
                .andDo(print())
                .andExpect(status().isOk());
}
```

만약에서 milestoneService의 create의 메서드가 변경을 하면 테스트코드도 대응하게 변경해야 한다.

## 해결 방법

* **MockMvc** 대신 **RestAssured**와 **Cucumber**를 사용하여 BDD 방식으로 개선하려고 검토했습니다.

### **RestAssured와 Cucumber의 비교**

#### **RestAssured의 장점**

* **실제 HTTP 테스트**: RestAssured는 실제 HTTP 요청과 응답을 테스트하여 실제 환경에 더 가까운 테스트를 가능하게 합니다.
* **간결하고 명확한 API**: API 테스트에 최적화된 구문을 제공하여 테스트 코드의 가독성과 유지 관리를 향상시킵니다.
* **API 테스트에 특화**: 복잡한 API 요청 및 데이터 포맷을 쉽게 처리할 수 있어 API 테스트에 매우 적합합니다.

```java
given().
        config(RestAssured.config().xmlConfig(xmlConfig().declareNamespace("test", "http://localhost/"))).
when().
         get("/namespace-example").
then().
         body("foo.bar.text()", equalTo("sudo make me a sandwich!")).
         body(":foo.:bar.text()", equalTo("sudo ")).
         body("foo.test:bar.text()", equalTo("make me a sandwich!"));
```

**Cucumber의 장점**

* **BDD 접근법**: Cucumber는 자연 언어로 테스트 시나리오를 작성하여, 비기술적 이해관계자도 테스트 과정을 쉽게 이해하고 참여할 수 있습니다.
* **팀 내 커뮤니케이션 강화**: BDD 접근법은 개발자, 테스터, 비즈니스 이해관계자 간의 명확한 커뮤니케이션을 촉진합니다.
* **사용자 중심의 테스트**: 사용자의 경험과 요구사항에 기반하여 테스트를 설계하고 구현할 수 있습니다.

```java
Scenario: Breaker guesses a word
  Given the Maker has chosen a word
  When the Breaker makes a guess
  Then the Maker is asked to score
```

## 내가 선택한 방법과 이유

* RestAssured를 사용하여 실제 HTTP 테스트를 테스트 할수 있고 간결하고 명확한 API를 사용하여 가독성을 높일 수 있고 테스트 코드를 유지 보수가 쉽다고 생각하여 선택했습니다.

## 결과

* 테스트 Step를 한글 메서드를 통합하고 테스트의 가독성르 향상되였고 비지니스 로직의 변경 따라 변경이 적어졌습니다.

```java
@DisplayName("내 관심상품에 담은 후, 해당 관심상품 삭제")
@Test
void shouldRemoveProductToInterestedProductsList() {
    출력_필드_추가("member_removeLikeProduct", spec);

    // given
    var id = 상품을_등록한다(ayaanAccessToken, 1).jsonPath().getString("id");
    관심상품에_담는다(id, albertAccessToken);

    // when
    var response = 관심상품에_제거한다(id, albertAccessToken, spec);

    // then
    관심상품은_담은_응답_검증(response);
}
```





