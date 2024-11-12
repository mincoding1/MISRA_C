## 규칙 9.1 자동 저장 기간을 가진 객체의 값은 설정되기 전에 읽어서는 안 됩니다.
Rule 9.1 The value  of an object with automatic storage duration shall not be read before it has been set

### [추가 설명]
이 규칙에서 배열 요소나 구조체 멤버는 개별 객체로 간주해야 합니다.

### [근거]
표준에 따르면, 정적 저장 기간을 가진 객체는 명시적으로 초기화되지 않는 한 자동으로 0으로 초기화됩니다. 반면, 자동 저장 기간을 가진 객체는 자동으로 초기화되지 않기 때문에 초기화되지 않은 값을 가질 수 있습니다.

참고: 자동 객체의 명시적 초기화가 무시될 수 있는 경우가 있습니다. goto 또는 switch 문을 사용하여 특정 레이블로 점프할 때 이러한 현상이 발생할 수 있습니다. 이 경우 객체가 선언되긴 하지만, 명시적 초기화는 무시됩니다.

```c
void f(bool_t b, uint16_t *p) {
    if (b) {
        *p = 3U;
    }
}

void g(void) {
    uint16_t u;
    f(false, &u);
    if (u == 3U) {
        /* 비준수 - u는 값이 할당되지 않았음 */
    }
}
```
위 코드에서 함수 f는 b가 false일 때 u에 값을 할당하지 않습니다. 따라서 u는 초기화되지 않은 값을 가지며, u == 3U 조건문은 비준수입니다.

다음의 C99 비준수 예제에서, goto 문이 x의 초기화를 건너뜁니다.
```c
{
    goto L1;
    uint16_t x = 10u;
L1:
    x = x + 1u;   /* 비준수 - x는 값이 할당되지 않았음 */
}

```
여기서 goto 문이 x의 선언을 우회하여 초기화가 이루어지지 않고 x = x + 1u로 사용되기 때문에 비준수입니다. 참고: 이 예제는 규칙 15.1도 위반합니다.

### 원문
```
Rule 9.1 The value  of an object with automatic storage duration shall not be read before it has been set

[Amplification]
For the purposes of this rule, an array element or structure member shall be considered as a discrete 
object.

[Rationale]
According to The Standard, objects with static storage duration are automatically initialized to zero 
unless initialized explicitly. Objects with automatic storage duration are not automatically initialized 
and can therefore have indeterminate values. 

Note: it is sometimes possible for the explicit initialization of an automatic object to be ignored. This 
will happen when a jump to a label using a goto or switch statement “bypasses” the declaration of the 
object; the object will be declared as expected but any explicit initialization will be ignored.

[Example]
 void  f ( bool_t b, uint16_t *p )
 {
  if ( b )
  {
    *p = 3U;
  }
 }
 void g ( void )
 {
  uint16_t u;
  f ( false, &u );
  if ( u == 3U )
  {
    /* Non-compliant - u has not been assigned a value */
  }
 }

 In the following non-compliant C99 example, the goto statement jumps past the initialization of x.

 Note: This example is also non-compliant with Rule 15.1.
 {
  goto L1;
  uint16_t x = 10u;
 L1:
  x = x + 1u;   /* Non-compliant - x has not been assigned a value */
 }

```
