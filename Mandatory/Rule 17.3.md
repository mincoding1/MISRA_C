## 규칙 17.3 함수는 암시적으로 선언되어서는 안 됩니다.
Rule 17.3 A function shall not be declared implicitly

### [근거]
함수 호출이 프로토타입이 있는 상태에서 이루어지면, 제약 조건에 의해 전달된 인수의 개수가 매개변수의 개수와 일치하고, 각 인수가 해당 매개변수에 할당될 수 있음이 보장됩니다.

함수가 암시적으로 선언된 경우, C90 컴파일러는 해당 함수의 반환 타입을 int로 가정합니다. 암시적 함수 선언에는 프로토타입이 포함되지 않으므로, 컴파일러는 함수 매개변수의 개수와 타입에 대한 정보를 가지지 못합니다. 그 결과, 인수 전달과 반환 값 할당에서 부적절한 타입 변환이 발생할 수 있으며, 그 외에도 정의되지 않은 동작이 발생할 수 있습니다.

### [예제]
함수 power가 다음과 같이 선언되었다고 가정합니다:
```c
extern double power(double d, int n);
```
그러나 다음 코드에서 이 선언이 보이지 않는다면 정의되지 않은 동작이 발생합니다.

```c
void func(void) {
    /* 비준수 - 반환 타입과 두 인수 타입이 잘못되었습니다 */
    double sq1 = power(1, 2.0);
}

```
위 코드에서 power 함수는 double과 int 타입의 매개변수를 기대하지만, 호출에서는 int와 double 타입의 인수를 전달하고 있습니다. 이 경우 선언이 보이지 않으면 컴파일러는 타입 불일치 문제를 감지하지 못해 잘못된 동작이 발생할 수 있습니다.


### 원문
```

Rule 17.3 A  function shall not be declared implicitly

[Rationale]
Provided that a function call is made in the presence of a prototype, a constraint ensures that the 
number of arguments matches the number of parameters and that each argument can be assigned 
to its corresponding parameter.

If a function is declare d implicitly, a C90 compiler will assume that the function has a return type of int. 
Since an implicit function declaration does not provide a prototype, a compiler will have no information 
about the number of function parameters and their types. Inappropriate type conversions may result 
in passing the arguments and assigning the return value, as well as other undefi ned behaviour.

[Example]
If the function power is declared as: 

extern double power ( double d, int n );

but the declaration is not visible in the following code then undefi ned behaviour will occur.

 void func ( void )
 {
  /* Non-compliant - return type and both argument type s incorrect */
  double sq1 = power ( 1, 2.0 );
 }
```
