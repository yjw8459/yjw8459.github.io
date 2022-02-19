---
layout: post
title: Spring Validation
description: >
  Spring Validation 정리
sitemap: false
categories:
  - Posts
---

![](https://images.velog.io/images/yjw8459/post/17eb3bfe-fb5d-44f9-90e8-45c3719038cb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.13.11.png)


이번 포스트에서는 **Spring Validation**에 대해서 정리해보고자 한다.

## Spring Validation
애플리케이션에서 데이터의 유효성 검사는 애플리케이션 전체에서 발생한다.
빈번하게 발생하는 데이터 유효성 검사는 결국 다음과 같은 문제를 가지고 있다.

>#### 1. 애플리케이션 전체 계층에 데이터 유효성 검사가 분산되어 있다.
#### 2. 로직이 전체 애플리케이션에 분산되어 있어서 코드 중복을 야기한다.
#### 3. 비즈니스 로직 사이사이에 섞여있어 디버깅이 어렵고 애플리케이션이 복잡해진다.
##### 위와 같은 문제 때문에 데이터 유효성 검사 로직은 수정, 관리하기 어렵고 오류가 발생할 확률이 커진다.

![](https://images.velog.io/images/yjw8459/post/17eb3bfe-fb5d-44f9-90e8-45c3719038cb/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.13.11.png)
이런 문제점을 최소화하고자 Java에서는 **_Bean Validation이라는 데이터 유효성 검사 프레임워크_**를 제공한다. 
Bean Validation은 위와 같은 문제를 해결하기 위해 다양한 제약을 도메인 모델(Entity)에 어노테이션으로 정의할 수 있도록 한다. **어노테이션을 이용한 유효성 검사를 필요한 객체에 직접 정의하는 방법으로 애플리케이션 전체 계층에서 이용되는 유효성 검사 로직의 문제를 해결**한다.


## Annotation
```
@AssertFalse : false 값만 통과 가능

@AssertTrue : true 값만 통과 가능


@DecimalMax(value=) : 지정된 값 이하의 실수만 통과 가능

@DecimalMin(value=) : 지정된 값 이상의 실수만 통과 가능

 
@Digits(integer=,fraction=) : 대상 수가 지정된 정수와 소수 자리수보다 적을 경우 통과 가능


@Future : 대상 날짜가 현재보다 미래일 경우만 통과 가능

@Past : 대상 날짜가 현재보다 과거일 경우만 통과 가능


@Max(value) : 지정된 값보다 아래일 경우만 통과 가능

@Min(value) : 지정된 값보다 이상일 경우만 통과 가능


@NotNull : null 값이 아닐 경우만 통과 가능

@Null : null일 겨우만 통과 가능


@Pattern(regex=, flag=) : 해당 정규식을 만족할 경우만 통과 가능


@Size(min=, max=) : 문자열 또는 배열이 지정된 값 사이일 경우 통과 가능


@Valid : 대상 객체의 확인 조건을 만족할 경우 통과 가능


@Email : 이메일 형식


@URL : URL 형식
```

![](https://images.velog.io/images/yjw8459/post/84854d1d-7a91-491e-bdf5-8a07f4714d3e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.16.11.png)
위의 정리된 어노테이션은 사진처럼 Entity에 어노테이션으로 지정하고 사용한다.

## AutoConfiguration

Spring에서는 ``ValidationAutoConfiguration``클래스를 통해서 LocalValidatorFactoryBean과 MethodValidationPostProcessor을 자동으로 설정한다.

- LocalValidatorFactoryBean 
	Spring에서 Valdator를 사용하기 위한 클래스
- MethodValidationPostProcessor
	메서드 파라미터 또는 리턴 값을 검증하기 위해 사용되는 클래스
    
#### AutoConfiguration으로 Spring에서 바로 Validation을 사용할 수 있다.

## 유효성 검증

![](https://images.velog.io/images/yjw8459/post/b4c448b9-101f-4b1b-8a3d-278b6c975f0e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.51.16.png)

![](https://images.velog.io/images/yjw8459/post/29c6b5ef-b943-482f-801e-b1fc45f5ff5b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.45.49.png)
Product에 값을 입력하고 유효성 검사를 검증하는 코드이다.

![](https://images.velog.io/images/yjw8459/post/4b463991-3d80-468c-bb6b-393569513acf/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.48.55.png)

```
ConstraintViolationImpl{interpolatedMessage='"테스트입니다."', propertyPath=name, rootBeanClass=class yjw.technical.annotation.Product, messageTemplate='{yjw.technical.annotation.Product}'}
ConstraintViolationImpl{interpolatedMessage='과거 또는 현재의 날짜여야 합니다', propertyPath=saleStartAt, rootBeanClass=class yjw.technical.annotation.Product, messageTemplate='{javax.validation.constraints.PastOrPresent.message}'}
ConstraintViolationImpl{interpolatedMessage='0보다 커야 합니다', propertyPath=productNo, rootBeanClass=class yjw.technical.annotation.Product, messageTemplate='{javax.validation.constraints.Positive.message}'}
```

첫 번째 로그 "테스트입니다."의 경우는 직접 설정한 메세지이고 나머지 메세지는 Spring Validation에서 자동 설정하는 검증 메세지이다. 
**Product.name**처럼 메시지를 직접 설정할 수 있는데, "이름이 올바르지 않습니다."로 문자열을 직접 설정하거나 공통 메시지를 Properties로 직접 관리할 수 있다. 


![](https://images.velog.io/images/yjw8459/post/5ea488b7-4594-4ae7-9761-3f5ea1c8b741/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%204.55.21.png)
Product.name의 경우는 직접 ``ValidationMessage.properties``파일에 지정한 메시지가 출력되도록 한 예제이다.
다국어를 사용할 경우 ValidationMessage_ko_KR.properties처럼 언어 별 메세지 프로퍼티 파일을 따로 관리할 수도 있다. 

## 사용자 정의 제약검증자

위 사진에서 ``@ProductConstraint``는 내가 정의한 제약검증자이다.

>#### ProductConstraint(제약자)
![](https://images.velog.io/images/yjw8459/post/62910e52-5604-4079-862f-802d40a6ce07/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.05.35.png)
**@Target** : 사용자가 만든 어노테이션이 부착될 수 있는 타입을 지정하는 것이다.
<br>
**@Retention** : 어노테이션이 메모리에 유지되는 시간을 지정한다.
<br>
**@interface** : interface 앞에 간단히 @를 붙여주면 어노테이션 인터페이스가 만들어진다. annotation은 자신의 element를 가질 수 있다
<br>
message()는 유효값 검증 시 메세지 기본 값을 지정한다.
groups는 상황별 validation 제어를 위해서 사용한다. 예를 들어 Insert, Update할 때 Validation을 구분해서 실행하고 싶을 때 사용한다.
payload는 심각도를 나타내는데, 더욱 디테일하게 그룹을 나눌 경우 사용한다.
<br>


>#### ProductCOnstraintValidator(검증자)
![](https://images.velog.io/images/yjw8459/post/6bde1359-e8a2-45ff-b91b-665d973a1567/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.06.55.png)
먼저, initialize를 살펴보면 제약선언한 ProcuctConstraint 객체가 매개변수로 전달된다. 
그리고 isValid()를 살펴보면 검증대상 객체가 매개변수로 전달되는데 isValid()메서드 안에서 전달된 객체의 유효성 검사를 진행한다. 
#### Product(검증대상)


## 예외처리 
Spring Validation은 유효성 검사 실패 시, ViolationException이 발생된다.
![](https://images.velog.io/images/yjw8459/post/4cafa55f-c588-4d50-8c75-22340a030eca/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202022-01-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.37.44.png)
위와 같이 예외 상황에 대한 처리도 가능하다.
ConstraintViolationException은 500에러로 발생되고 Response되는데 이를 400에러(Bad Request)로 변경처리하는 예제이다.

그 밖에 메서드 파라미터 ex)RequestBody, @ModelAttribute를 데이터 바인딩 할 때 오류가 있을 경우 MethodArgumentNotValidException이 발생하는데 이에 대한 예외처리로도 사용될 수 있다.





