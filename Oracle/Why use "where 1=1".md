# Why use "where 1=1"? 

### 00. 목차
  01. [ 왜 우리는 Oracle에 조건문 1=1을 쓰는 것일까? ](https://github.com/hongcoding94/I-Wonder-Development/new/main#01-%EC%99%9C-%EC%9A%B0%EB%A6%AC%EB%8A%94-oracle%EC%97%90-%EC%A1%B0%EA%B1%B4%EB%AC%B8-11%EC%9D%84-%EC%93%B0%EB%8A%94-%EA%B2%83%EC%9D%BC%EA%B9%8C)
  02. [ 왜 1=1일까? ](https://github.com/hongcoding94/I-Wonder-Development/new/main#02-%EC%99%9C-11%EC%9D%BC%EA%B9%8C)
  03. [ 참조내용 ](https://github.com/hongcoding94/I-Wonder-Development/new/main#03-%EC%B0%B8%EC%A1%B0%EC%9E%90%EB%A3%8C)

---

### 01. 왜 우리는 Oracle에 조건문 1=1을 쓰는 것일까?
##### WHERE 1=1를 사용하는 이유  
   - 쿼리를 작성하다 조건문을 넣을 때 조건 구분이 불명확할 때 의미 없는 1=1 조건을 걸고 이후 `<if test="조건">`을
    통해서 AND로 나머지 조건 쿼리를 작성하면된다.

     현재 글쓴이가 개발 중에 의미 없는 1=1를 넣는 사례는 정적인 쿼리에서는 사용하지 않으며 동적인 쿼리에서 구분이 불명확할 때에만 사용을 하지 않고 있습니다.
     
##### WHERE 1=1를 지양해야하는 이유
  - SELECT문인 경우 데이터 변경이 혹은 삭제가 이루어 지지 않기 때문에 문제가 없음.
  - UPDATE문, DELETE문의 `if조건`이 들어가야하는 상황에 WHERE 1=1을 사용했을 경우 조건이 없기 때문에 데이터 전체를 대상으로 **변경 또는 삭제될수 있기 때문**(초급 개발자가 흔히 할 수 있습니다.)<br/>
    
    때문에 Java와 쿼리에서 `NULL` 체크를 반드시 해줘야한다.<br/>
    ~~개발계 or 테스트계 데이터는 상관이 없지만 운영에 반영이 되었다면 끔찍한 결과를 초래할 수 있다.~~


> [!NOTE]
> 본 코드 작성법은 `Oracle Query`을 사용하며 `MyBatis`를 사용하여 작성한 코드이며 이해하기 쉽게 설명하기 위한 내용입니다.

`예시 코드`
```
 A. 정적쿼리
  SELECT *
  FROM USER_ A01
  WHERE 1=1  -- <- 의미 없는 표현 방식 : 명확한 조건이 있는데 사용하였음
    AND A01.USER_TYPE = #{userType}
    ORDER BY USER_NM

 B. 동적쿼리 Ⅰ
    SELECT * 
    FROM USER_ A01
    WHERE 1=1 -- <- 명확한 조건이 없음으로 무의미한 1=1 생성
      <if test = '!("msater".(userType) and "dev".(userType))'>
        AND A01.USER_TYPE = #{userType}
      </if>
      ORDER BY USER_NM

 C. 동적쿼리 Ⅱ
    SELECT *
    FROM USER_ A01
    WHERE USER_NM = #{userNm} -- <- 명확한 조건이 있음으로 1=1은 제거
      <if test = '!("msater".(userType) and "dev".(userType))'>
        AND A01.USER_TYPE = #{userType}
      </if>
      ORDER BY USER_NM 
```

위 내용에서 정적쿼리에서 사용한 'WHERE 1=1' 위에서 설명했듯이 의미 없으며 해당 부문에서 명확한 조건이 들어갔을 때에는 WHERE 1=1를 제거하고 명확한 조건문을 넣어주면 되는 것이다.

동적 쿼리에서는 명확한 조건이 없다면 1=1로 해당 로직에 대한 가독성을 키워주는 역활을 할 뿐만 아니라 해당 로직에 대한 결과값이 달라질 수 있기 때문에 1=1를 사용할 때에는 'Oracle SQL Developer'등등으로
쿼리를 돌려보고 사용할 것을 추천한다.

---
### 02. 왜 1=1일까?
0=0, 1=1, 2=2, 3=3 다양하게 할 수 있는데 왜? 1=1로 사용해 왔던 것인가?
현재의 컴퓨터가 도입되기 전 2진법으로 사용시절에 Query를 사용할 때 0은 false 1은 true로 사용하였기 때문에 `1=1은 참(true)을 의미로`으로 사용하였으며
오늘날에는 개발자들끼리의 암묵적으로 사용되고 있습니다.

---
### 03. 참조자료

  참조 자료 없음

<hr>
<div align="center">
  Warning(주의) : <b>제가 잘못 이해한 이슈들이 있을 수 있습니다. </b><br>
  내용에 문제가 있다면 이슈를 통해 알려주시거나 <br>
  피드백을 주신다면 감사하겠습니다.
</div>
