# Assertion Util

## Description

- 자연스럽게 읽을 수 있고, 검증 로직과 Exception을 커스터마이징 할 수 있는 간단한 custom Assertion  을 만들어보았습니다.
- `Assertion`  한 개의 파라미터에 대한 단일 검증 
- `BiAssertion` : 두 개의 파라미터에 대한 단일 검증
- `Assertions` : 한 개의 파라미터를 받아 복수 번의 검증

```java
// 검증 Predicate
// BagValidator class
boolean isBag(String param);

// 검증을 통화 못할 시 Throw할 Exception
InvalidBagException

// (custom) Assertion을 활용한 검증
Assertions.with(bagToBeValidated)
		.setValidation(bagValidator::isBag)
		.validateOrThrow(InvalidBagException::new);
```

```java
// 검증 Predicate
// BagValidator class
boolean isBagOwner(String person, String bag);

// 검증을 통화 못할 시 Throw할 Exception
BagTheifException

// (custom) Assertion을 활용한 검증
Assertions.with(personHavingBag, bag)
		.setValidation(bagValidator::isBagOwner)
		.validateOrThrow(BagTheifException::new);
```

## Reason

- 단순히 predicate만 가지고는 검증을 위해 `if` 문을 사용해야 했는데 보통 **부정**일 때 예외를 던지는 케이스가 많아 구현 방법이 여러가지로 나뉘게 되었습니다.

```java
// 구현 방법 1.
if(!isBag(someBag)) {
		throw new InvalidBagException();
}

// 구현 방법 2.
if(isNotBag(someBag)) {
		throw new InvalidBagException();
}

// 구현 방법 3.
if (isBag(someBag)) {
		return 
}
throw InvalidBagException()
```

- 이 여러가지 방법을 하나로 통일하면서 검증이라는 흐름이 잘 읽힐 수 있기 위해 만들게 되었습니다.
- 또한 검증을 위해서 부정의 의미를 가지는 predicate를 작성하지 않아도 됩니다.