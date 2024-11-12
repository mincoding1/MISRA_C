## 규칙 19.1 객체는 겹치는 객체에 할당되거나 복사되어서는 안 됩니다.
Rule 19.1 An object shall not b e assigned or copied to an overlapping object

### [근거]
메모리에서 일부가 겹치는 두 객체가 생성되고, 하나가 다른 객체에 할당되거나 복사되면 동작이 정의되지 않습니다.

### [예외]
다음 경우는 동작이 잘 정의되어 있으므로 허용됩니다:

정확히 겹치고 타입이 호환되는 두 객체 간의 할당 (타입 한정자는 무시).
memmove 함수와 같은 표준 라이브러리 함수를 사용해 객체를 부분적으로 또는 완전히 겹치게 복사.

### [예제]

다음 예제는 또한 union을 사용하므로 규칙 19.2를 위반합니다.
```c
void fn(void) {
    union {
        int16_t i;
        int32_t j;
    } a = {0}, b = {1};

    a.j = a.i;   /* 비준수 */
    a = b;       /* 준수 - 예외 1 */
}

#include <string.h>

int16_t a[20];

void f(void) {
    memcpy(&a[5], &a[4], 2u * sizeof(a[0])); /* 비준수 */
}

void g(void) {
    int16_t *p = &a[0];
    int16_t *q = &a[0];

    *p = *q;   /* 준수 - 예외 1 */
}
```
위 코드에서:

- a.j = a.i;는 메모리 내 일부가 겹치므로 비준수입니다.
- a = b;는 예외 1에 해당하여 준수합니다.
- memcpy(&a[5], &a[4], ...)는 메모리 복사에 겹침이 발생하여 비준수입니다.
- *p = *q;는 정확히 겹치므로 예외 1에 해당하여 준수합니다.


### 원문
```
Rule 19.1 An object shall not b e assigned or copied to an overlapping object

[Rationale]
The behaviour is undefi ned when two objects are created which have some overlap in memory and 
one is assigned or copied to the other.

[Exception]
The following are permitted because the behaviour is well-defined:
 1. Assignment between two objects that overlap exactly and have compatible types (ignoring 
    their type qualifiers)
 2. Copying between objects that overlap partially or completely using The Standard Library 
    function memmove

[Example]
This example also violates Rule 19.2 because it uses unions. 

void fn ( void )
 {
  union
  {
    int16_t i;
    int32_t j;
  } a = { 0 }, b =  { 1 };

  a.j = a.i;                /* Non-compliant           */
  a = b;                    /* Compliant - exception 1 */
 }

 #include <string.h>

 int16_t a[ 20 ];

 void f ( void )
 {
  memcpy ( &a[ 5 ], &a[ 4 ], 2u * sizeof ( a[ 0 ] ) ); /* Non-compliant */
 }

 void g ( void )
 {
  int16_t  *p = &a[ 0 ];
  int16_t  *q = &a[ 0 ];

  *p = *q;   /* Compliant - exception 1 */
 }

```
