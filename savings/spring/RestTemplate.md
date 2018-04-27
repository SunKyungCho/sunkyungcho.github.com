# RestTemplate

도대체 어떻게 사용하는지 옵션을 좀 자세히 봐보자.
### GET

파리미터를 같이 보낼때. 그냥 문자열을 만들어주거나 여러 방법이 있겠지만, `UriComponentsBuilder`를 사용하는게 깔끔한것 같다.
```java
String url = "http://www.kobis.or.kr/kobisopenapi/webservice/rest/movie/searchMovieList.json";
UriComponentsBuilder uri = UriComponentsBuilder.fromUriString(url)
		.queryParam("key", "###############");

RestTemplate restTemplate = new RestTemplate();
String response = restTemplate.getForObject(uri.toUriString(), String.class);

```
이렇게 하면 문자열로 `response`를 받아온다.
그럼 객체로 바로 받고 싶다면 어떻게 하면 될까? 영화진흥원에서 주는 데이터는 이런 형태로 값이 넘어온다. 이걸 객체 리스트 형태로 받아보자.
```javascript
{
  movieListResult: {
    totCnt: 63884,
    source: "영화진흥위원회",
    movieList: [
      {
        movieCd: "20183931",
        movieNm: "떡갈나무가 되는 법",
        movieNmEn: "Ways to Become a Big Tree",
        prdtYear: "2018",
        openDt: "",
        typeNm: "단편",
        prdtStatNm: "기타",
        nationAlt: "한국",
        genreAlt: "애니메이션",
        repNationNm: "한국",
        repGenreNm: "애니메이션",
        directors: [
          {
            peopleNm: "정예지"
          }
        ],
        companys: [ ]
      },
      {
        movieCd: "20183581",
        movieNm: "케이크메이커",
        movieNmEn: "The Cakemaker",
        prdtYear: "2017",
        openDt: "",
        typeNm: "장편",
        prdtStatNm: "개봉예정",
        nationAlt: "독일",
        genreAlt: "드라마,멜로/로맨스",
        repNationNm: "독일",
        repGenreNm: "드라마",
        directors: [
          {
            peopleNm: "오피르 라울 그라이저"
          }
        ],
        companys: [ ]
      }
    }
  }
}
```




### WebClient
비동기 및 반응형을 지원하는 WebClient가 스프링 5.0에서부터 제공되는데 이건 좀 나중에 보자.
* 비동기. 반응형. 즉 요청을 하고 응답이 오기 전까지 기다리지 않는다.
* Spring 5.0에서 부터 제공된다.


retrieve() : 심플하게 response()를 받을 수있다.
exchange() : 좀더 디테일하게 컨트롤을 원하는 경우.


### RestTemplate
* 응답이 올때가지 기다린 후 다음처리.

물론 AsyncRestTemplate도 존재한다. 차이는 ?



private String movieCd; private String movieNm; private String movieNmEn; private String prdtYear; private String openDt; private String typeNm; private String prdtStatNm; private String nationAlt; private String genreAlt; private String repNationNm; private String repGenreNm; private String directors; private String peopleNm; private String companys; private String companyCd; private String companyNm;
