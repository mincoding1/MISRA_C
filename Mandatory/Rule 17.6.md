## 규칙 17.6 배열 매개변수 선언에서 대괄호 [] 사이에 static 키워드를 포함해서는 안 됩니다.
Rule 17.6 The declaration of an array parameter shall not contain the static keyword between the [ ]

### [근거]
C99 언어 표준은 프로그래머가 배열 매개변수에 특정 최소 개수의 요소가 포함되어 있음을 컴파일러에 알릴 수 있는 메커니즘을 제공합니다. 일부 컴파일러는 이를 활용해 일부 프로세서에서 더 효율적인 코드를 생성할 수 있습니다.

그러나, 프로그래머가 보장한 최소 개수의 요소를 충족하지 못하고, 요소 수가 지정된 최소 값보다 적을 경우, 동작이 정의되지 않습니다.

일반적인 임베디드 응용 프로그램에서 사용되는 프로세서는 프로그래머가 제공한 추가 정보를 활용하는 데 필요한 기능을 제공하지 않는 경우가 많습니다. 따라서 최소 요소 개수를 충족하지 못하여 프로그램이 실패할 위험이 성능 향상 가능성보다 큽니다.

### [예제]

이 규칙을 준수하는 C99 언어 기능 사용 예시는 없습니다. 다음 예제는 이를 사용함으로써 발생할 수 있는 정의되지 않은 동작을 보여줍니다.
```c
/* 비준수 - 배열 선언자에 static 사용 */
uint16_t total(uint16_t n, uint16_t a[static 20]) {
    uint16_t i;
    uint16_t sum = 0U;

    /* a의 요소가 20개 미만이면 정의되지 않은 동작 */
    for (i = 0U; i < n; ++i) {
        sum = sum + a[i];
    }

    return sum;
}

extern uint16_t v1[10];
extern uint16_t v2[20];

void g(void) {
    uint16_t x;

    x = total(10U, v1); /* 정의되지 않음 - v1은 10개의 요소만 가지며 최소 20개가 필요 */
    x = total(20U, v2); /* 정의되었지만 비준수 */
}

```
위 예제에서 total 함수의 매개변수 a[static 20]는 a가 최소 20개의 요소를 가져야 한다고 가정합니다. 그러나 v1은 10개의 요소만 가지므로 정의되지 않은 동작을 초래합니다. v2의 경우 정의된 동작이지만 여전히 비준수입니다.



### 원문
```
Rule 17.6 The declaration of an array parameter shall not contain the static keyword between the [ ]

[Rationale]
The C99 language standard provides a mechanism for the programmer t o inform the compiler that an 
array parameter contains a specifi ed minimum number of elements. Some compilers are able to take 
advantage of this information to generate more effi  cient code for some types of processor.

If the guarantee made by the programmer is not honoured, and the number of elements is less than 
the minimum specifi ed, the behaviour is undefined.

The processors used in typical embedded applications are unlikely to provide the facilities required 
to take advantage of the additional information provided by the programmer. The risk of the program 
failing to meet the guaranteed minimum number of elements outweighs any potential performance 
increase.

[Example]
There is no use of this C99 language feature that is compliant with this rule. The examples show some 
of the undefi ned behaviour that can arise from its use. 

/* Non-compliant - uses static in array declarator */
 uint16_t total ( uint16_t n, uint16_t a[ static 20 ] )
 {
  uint16_t i;

  uint16_t sum = 0U;

  /* Undefined behaviour if a has fewer than 20 elements */
  for ( i = 0U; i < n; ++i )
  {
    sum = sum + a[ i ];
  }

  return sum;
 }

 extern uint16_t v1[ 10 ];
 extern uint16_t v2[ 20 ];

 void g ( void )
 {
  uint16_t x;

  x = total ( 10U, v1 );   /* Undefined - v1 has 10 elements but needs
                            *             at least 20                   */
  x = total ( 20U, v2 );   /* Defined but non-compliant                 */
 }

```
