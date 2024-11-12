## 규칙 17.4 반환 타입이 void가 아닌 함수의 모든 종료 경로에는 표현식이 포함된 명시적인 return 문이 있어야 합니다.
Rule 17.4 All exit paths from a function with non-void return type shall have an explicit return statement with an expression

### [근거]
return 문에 주어진 표현식은 함수가 반환할 값을 제공합니다. void가 아닌 함수가 값을 반환하지 않는데 호출 함수가 반환 값을 사용하는 경우, 동작이 정의되지 않습니다. 이를 방지하기 위해 void가 아닌 함수에서는 다음을 보장해야 합니다:

- 모든 return 문에 표현식이 있어야 하며,
- 함수의 끝에 도달하기 전에 항상 return 문을 만나야 합니다.
참고: C99에서는 void가 아닌 함수의 모든 return 문이 값을 반환해야 한다고 규정하고 있습니다.

### [예제]
```c
int32_t absolute(int32_t v) {
    if (v < 0) {
        return v;
    }

    /*
     * 비준수 - 값을 반환하지 않고 함수의 끝에 도달할 수 있음
     */
}

uint16_t lookup(uint16_t v) {
    if ((v < V_MIN) || (v > V_MAX)) {
        /* 비준수 - 반환 값이 없음. C99에서는 제약 사항 */
        return;
    }
    return table[v];
}

```
첫 번째 함수 absolute에서는 v가 0 이상일 경우 반환 값 없이 함수의 끝에 도달할 수 있어 비준수입니다. 두 번째 함수 lookup에서는 v가 범위를 벗어날 경우 return 값이 없어 비준수입니다.

### 원문
```

Rule 17.4 All exit paths from a function with non-void return type shall have an explicit return statement with an expression

[Rationale]
The expression given to th e return statement provides the value that the function returns. If a non
void function does not return a value but the calling function uses the returned value, the behaviour 
is undefi ned. This can be avoided by ensuring that, in a non-v oid function:

 • Every return statement has an expression, and
 • Control cannot reach the end of the function without encountering a return statement.

 Note: C99 constrains every return statement in a non-void function to return a value.

[Example]
 int32_t absolute ( int32_t v )
 {
  if ( v < 0 )
  {
    return v;
  }

  /*
   * Non-compliant - control can reach this point without
   * returning a value
   */
 }

 uint16_t lookup ( uint16_t v )
 {
  if ( ( v < V_MIN ) || ( v > V_MAX ) )
  {
    /* Non-compliant - n o value returned. Constraint in C99 */
    return;
  }
  return table[ v ];
 }

```
