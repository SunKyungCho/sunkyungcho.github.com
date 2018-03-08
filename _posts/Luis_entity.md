# LUIS Entity 정리

엔티티란 사용자에 의해 입력된 문장에서 중요한 단어를 뜻한다. 엔티티를 구성해 놓고 여러 intent(의도)에서 사용할 수 있다.

## Entity Types

### Prebuilt
흔히 사용되는 대표적인 컨셉들(숫자, E-mail, 날짜, 등)을 미리 생성해논 타입이다. 안타깝게도 한글은 준비되어 있지 않다.

### List()
연관된 단어들을 대표한다. 각 리스트에는 하나 이상으로 구성되어 있다. 이건 머신러닝되지 않는다. 그리고
are best used for a known set of variations on ways to represent the same concept
가장 많이 사용되어 진다/ 아는 셋 변화하는 것들중
### Simple

### Hierarchical(계급, 계층)

### Composite(합성)
