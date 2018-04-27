# ElasticSearch Analyzer 알아보자.


## Analyzer
Analyzer는 Character filter, Tokenizer, Token filter 이 3가지가 포함된 패키지이다. 엘라스틱에서 기본제공되는 Analyzer나 사용자가 직접 커스텀한 Analyzer나 이 기본3가지가 필요하다.


### 1. Character filters
원본 문자열에 문자를 추가하거나 삭제하거나 변경하는 기능을 제공한다. 예를 들어 힌두어 숫자(٠‎١٢٣٤٥٦٧٨‎٩‎) 들을 우리가 흔히사용하는 숫자로 변경을(123456789)로 변경을 하거나 문자열에서 HTML을 문법을 제거하거나 하는 일을 할 수 있다.
순서에 따라 0개 이상을 Character filter를 적용할 수 있다.

#### 1-1. HTML STRIP CHAR filter
문장에서 HTML을 제거하는 필터 이다.

| Parameters | description     |
| :------------- | :------------- |
| escaped_tags | 특정 태그만 삭제한다. 기본값 html태그 전체 삭제 |

#### 1-1. PATTERN REPLACE CHAR FILTER
자바의 정규표현식을 사용하여 문장의 내용을 변경한다.

| Parameters | description     |
| :------------- | :------------- |
| pattern | 자바 정규 표현식을 사용하여 작성한다. |
| replacement | 변경될 문자열의 정보를 입력한다. |
| flag | 자바 정규 표현식의 플래그 |


### 2. Tokenizer
토크나이저는 문자열을 어떤 기분에 의해서 잘라주는 기능 제공한다.
예를 들어 `whitespace` tokenizer는 "Quick brown fox!"라는 문자열을 [Quick, brown, fox]왁 같은 형태로 잘라준다.

Analyzer는 꼭 한개의 Tokenizer를 갖고 있어야 한다.

#### 2-1. STANDARD TOKENIZER
Elasticsearch에서 일반적으로 사용하는 Tokenizer 대부분의 기호를 만나면 토큰으로 나눈다.

| Parameters | description     |
| :------------- | :------------- |
| max_token_length | 최대 토큰 길이를 초과하는 경우 해당 간격으로 토큰을 분할 한다. 기본값 255 |

#### LETTER Tokenizer
문자가 아닌 다른 문자를 만나면 토큰을 반환한다. 원하지 않는 문자열이 분해가 될 수 있다.

#### LOWER CASE TOKENIZER
Standard Tokenizer와 유사하지만 lowercase token filter를 사용하여 토큰을 소문자로 변경한다.

#### WHITESPACE TOKENIZER
공백을 만나면 텍스트를 토큰화 한다.
| Parameters | description     |
| :------------- | :------------- |
| max_token_length | 최대 토큰 길이를 초과하는 경우 해당 간격으로 토큰을 분할 한다. 기본값은 255 |

#### UAX URL EMAIL TOKENIZER
Standard Tokenizer와 URL, Email 주소를 인식하여 토큰으로 분리한다.
| Parameters | description     |
| :------------- | :------------- |
| max_token_length | 최대 토큰 길이를 초과하는 경우 해당 간격으로 토큰을 분할 한다. 기본값은 255 |

#### NGRAM TOKENIZER
 Ngram이 지정된 문자의 목록 중 하나를 만날때마다 단어를 자른다. N그램은 특정 길이의 연속된 문자를 자른다. 자동완성을 만들시 유용하게 사용한다.
 | Parameters | description     |
 | :------------- | :------------- |
 | min_gram | n 그램 할 문자의 최소길이. 기본 1 |
 | max_gram | N그램 할 문자의 최대 갈이. 기본 2 |
 | token_chars | 토큰에 포함할 문자열 <br> options : <br> - letter(문자)<br>  - digit(숫자) <br> - whitespace(공백) <br> - punctuation(구두점) <br> - symbol(특수기호) |

#### EDGE_NGRAM TOKENIZER
지정된 문자의 목록 중 하나를 만날때 마다 시작부분을 고정시켜 단어를 자르는 방식으로 사용하는 Tokenizer이다. 해당 Tokenizer 역시 자동완성을 만들 시 사용한다.
| Parameters | description     |
| :------------- | :------------- |
  | min_gram | n그램 할 문자의 최소 길이 |
  | max_gram | N그램 할 문자의 최대 길이. |

#### KEYWORD TOKENIZER
텍스트를 하나의 토큰으로 만든다.
  | Parameters | description     |
  | :------------- | :------------- |
  | buffer_size | 텀을 버퍼로 읽어 드릴 문자 수를 지정 기본값은 256 |

#### PATTERN TOKENIZER
  Java의 정규 표현식을 사용하여 토큰을 분리하는 Tokenizer이다. 잘못된 정규식은 형태소 분석을 느리게 할 수 있다.

  | Parameters | description     |
  | :------------- | :------------- |
  | pattern | 자바 정규식을 사용하여 작성한다. |
  | flags | 자바 정규식의 플래그를 나타낸다. |
  | group | 토큰에서 추출할 그룹을 설정한다. |

#### PATH HIERARCHY TOKENIZER
  결로를 분리하고 N그램 처럼 토큰화 한다. 각 경로를 분리하는 것이 목적이다.


  | Parameters | description     |
  | :------------- | :------------- |
  | delimiter | Path로 사용할 문자열을 정 |
  | replacement | Delimiter의 해당 문자열을 변경한다. |
  | buffer_size | 텀을 최대로 읽을 수 있는 문자열의 개수이다. 기본값 1024 |
  | reverse | true로 정의시 역순으로 path를 정의한다. |
  | skip | 초기에 건너띌 개수 |

### Token filter
토큰 필터는 tokenizer에 의해 잘라진 토큰들에 대해 뭔가 추가하거나 삭제 변경하는 기능을 갖는다. 예를 들어 `lowercase` 토큰 필터는 토큰들을 소문자로 변경하고, `stop` 토큰필터는 불용어에 해당하는 토큰들을 삭제할 것이다.
순서에 따라 0개 이상의 토큰 필터를 적용할 수 있다.


#### ASCII FOLDING TOKEN FILTER
  아스키 코드에 해당하는 127개의 알파벳, 숫자, 기호가 없는 경우 해당 문자를 ASCII요소로 변경하는 토큰 필터

#### LENGTH TOKEN FILTER
  Length 스트림에 대해 해당 조건보다 길거나 짧은 단어를 제거하는 형식의 토큰 필터이다.
  | Parameters | description     |
  | :------------- | :------------- |
  | min | 최소 문자열 값보다 길이가 작은 토큰을 제거. 기본값 0 |
  | max | 최대 문자열 값보다 길이가 큰 토큰은 제거 기본값 Integer.Max_VALUE |

#### LOWERCASE TOKEN FILTER
해당 전체 문자열을 소문자로 변환하는 필터이다.
#### UPPERCASE TOKEN FILTER
해당 전체 문자열을 대문자로 변환하는 필터이다.
#### NGRAM TOKEN FILTER
  ngram으로 글자를 잘라 사용하는 필터이다.
  | Parameters | description     |
  | :------------- | :------------- |
  | min_gram | 기본값 1. |
  | max_gram | 기본값 2. |

#### EDGE_NGRAM TOKEN FILTER
첫 글자를 기준으로 ngram하는 토큰 필터이다.
| Parameters | description     |
| :------------- | :------------- |
| min_gram | 기본값 1. |
| max_gram | 기본값 2. |

#### PORTER STEM TOKEN FILTER
  영어에서 변형된 단어 부터 접사 등을 제거하고 해당 단어의 어간을 분리하는 하는 필터 이다. 예를 들어 “stemmer”, “stemming”, “stemmed”의 어간은 “stem”의 토큰으로 출력되며, “fishing”, “fished”, “fisher”는 “fish”로 출력된다.
  | Parameters | description     |
  | :------------- | :------------- |
  | min_gram | 기본값 1. |
  | max_gram | 기본값 2. |

#### STOP TOKEN FILTER
   불용어로 등록할 사전을 구축하여 사용하는 필터를 의미 한다. 인덱스로 만들고 싶지 않거나 검색이 되지 않게 하고 싶은 단어를 등록하여 해당 단어에 대한 불용어 사전을 구축한다.

  | Parameters | description     |
  | :------------- | :------------- |
  | stopwords | 불용어를 매핑에 직접 등록하여 사용한다. |
  | stopwords_path | 불용어 사전이 존재하는 경로를 지정한다. 해당 경로는 Elasticsearch 서버가 있는 config 폴더 안에 생성한다. |
  | ingnore_case | true로 지정할 경우 모든 단어를 소문자로 변경하여 저장한다. 기본값 false. |

#### WORD DELIMITER TOKEN FILTER
  * 단어를 특정 조건에 의해 분할 할때 사용하는 필터이다. 예를 들면 아래와 같은 작업을 할 수 있다.
  ```
  “wi-fi” -> [“wi”,”fi”]
  “ElasticSearch” -> [“Elastic”,”Search”]
  “Elasticsearch6.1” -> [“Elasticsearch”,”6.1”]
  ```

| Parameters    | description     |
| :------------- | :------------- |
|    generate_word_parts    | 단어를 대문자기준으로 분리 한다. “ElasticSearch” ⇒ [“Elastic”,”Search”] 기본값은 true   |
| generate_number_parts | 숫자를 특수기호를 기준으로 분리한다. "500-42" ⇒ "500" "42".  기본값은 true |
| catenate_words | 영문 단어를 특수기호를 기준으로 합친다. "wi-fi" ⇒ "wifi". 기본값은 false |
| catenate_numbers | 숫자를 특수기호를 기준으로 합친다. "500-42" ⇒ "50042". 기본값은 false |
| catenate_all | 모든단어를 특수기로흘 기준으로 합친다. “wi-fi-4000" ⇒ "wifi4000" 기본값은 false |
| split_on_case_chage | position정보가 같은 단어를 생성하고 원본을 유지한다. Power-Shot=>[PowerShot,Power-Shot] 기본값은 false |
| preserve_original  | 원본을 유지한다. 500-42" ⇒[ "500-42", "500", "42"] 기본값은 false |
| split_on_numerics | 숫자를 기준으로 분리한다. J2se ⇒[ "j","2","se"] 기본값은 true |
| stem_elnglish_possessive | Stemming 알고리즘을 기준으로 토큰을 분리한다. "O’Neil’s" ⇒ ["O", "Neil"].  기본값은 true |
| protected_words | Elasticsearch의 config 파일안에 파일을 만들어 변형/분리하지 않을 단어에 대한 정의한다. |


#### STEMMER TOKEN FILTER
Stemming 알고리즘을 사용하여 단어를 원본으로 변형하는 필터.

| Parameters | description     |
| :------------- | :------------- |
|name | english, light_english, minimal_english, possessive_english, porter2, lovins 등 다른 나라의 언어도 사용가능 |

#### KEYWORD MARKER TOKEN FILTER
Stemming 하기전에 사용하는 필터로 특정 단어를 stemming하지 않도록 지정할 수 있다.

| Parameters | description     |
| :------------- | :------------- |
| keywords | 보호할 단어의 목록을 지정한다. |
| keyword_path | 파일로 관리할 경우의 경로 Elasticsearch 서버의 config폴더 밑에 생성한다. |
| keyword_pattern | 텍스트의 패턴을 정규 표현식으로 나타낼 수 있다. |
| ignore_case | 모든 단어를 소문자로 변경한다. 기본값은 false |

#### KEYWORD REPEAT TOKEN FILTER
Stemming 하기전에 사용하는 필터로 특정 단어를 stemming 한 결과와 하지 않은 원본 결과를 같이 같이 보여준다.

#### SYNONYM TOKEN FILTER
동의어를처리할수있는필터이다.
| Parameters | description     |
| :------------- | :------------- |
| synonyms | 동의어로 사용할 단어 등록을 한다. |
| synonyms_path | 파일로 관리할 경우의 경로 Elasticsearch 서버의 config폴더 밑에 생성한다. |

#### REVERSE TOKEN FILTER
문자를 반대로 뒤집는다. 자동완성의 중간일치 단어를 검색되게 할 때 사용한다.


#### PATTERN CAPTURE TOKEN FILTER
정규표현식으로 나오는 데이터를 전체 출력한다.

#### TRIM TOKEN FILTER
앞뒤 공백을 제거해 주는 필터이다.



# 참고
[https://www.elastic.co/guide/en/elasticsearch/reference/6.2/analysis.html](https://www.elastic.co/guide/en/elasticsearch/reference/6.2/analysis.html)
