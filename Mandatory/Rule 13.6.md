## 규칙 13.6 sizeof 연산자의 피연산자는 잠재적인 부작용을 포함하는 표현식을 포함해서는 안 됩니다.
Rule 13.6 The operand of the sizeof operator shall not contain any expression which has potential side effects

### [추가 설명]
sizeof 연산자의 피연산자로 나타나는 표현식은 일반적으로 평가되지 않습니다. 이 규칙은 그러한 표현식이 실제로 평가되는지 여부에 관계없이 부작용을 포함해서는 안 된다고 규정합니다. 이 규칙에서는 함수 호출을 부작용으로 간주합니다.

### [근거]
sizeof 연산자의 피연산자는 표현식이거나 타입을 지정할 수 있습니다. 피연산자가 표현식을 포함할 경우, 해당 표현식이 실제로 평가되지 않는 경우가 대부분이기 때문에 프로그래밍 오류가 발생할 수 있습니다. C90 표준에서는 sizeof 피연산자로 나타나는 표현식이 런타임에 평가되지 않는다고 명시하고 있습니다. C99에서는 가변 길이 배열 타입을 포함하는 경우 배열 크기 표현식이 필요에 따라 평가될 수 있으며, 평가 없이 결과를 결정할 수 있다면 평가 여부가 명시되지 않습니다.

### [예외]
sizeof(V) 형태에서, V가 가변 길이 배열이 아닌 volatile 한정자를 가진 lvalue인 경우는 허용됩니다.

### [예제]
```c
volatile int32_t i;
int32_t j;
size_t s;

s = sizeof(j);         /* 준수 */
s = sizeof(j++);       /* 비준수 */
s = sizeof(i);         /* 준수 - 예외 허용 */
s = sizeof(int32_t);   /* 준수 */

```
이 예제에서, 마지막 sizeof 표현식은 가변 길이 배열 크기 표현식이 타입의 크기에 영향을 미치지 않을 수 있음을 보여줍니다. 피연산자는 "v를 매개 변수 타입으로 가지는 함수에 대한 n개의 포인터로 이루어진 배열" 타입입니다. 피연산자가 가변 길이 배열 타입을 가지고 있기 때문에 평가됩니다. 그러나 n개의 함수 포인터 배열의 크기는 함수 포인터 타입의 매개변수 목록에 영향을 받지 않습니다. 따라서 volatile 한정자를 가진 객체 v는 평가될 수도, 평가되지 않을 수도 있으며, 그에 따른 부작용이 발생할 수도, 발생하지 않을 수도 있습니다.
```c
volatile uint32_t v;

void f(int32_t n) {
    size_t s;

    s = sizeof(int32_t[n]);                         /* 준수 */
    s = sizeof(int32_t[n++]);                       /* 비준수 */
    s = sizeof(void (*[n])(int32_t a[v]));          /* 비준수 */
}

```
위 예제에서, sizeof(int32_t[n])는 준수하지만, sizeof(int32_t[n++])와 sizeof(void (*[n])(int32_t a[v]))는 각각 인수 평가가 필요하거나 v의 평가 여부가 불확실하므로 비준수입니다.


### 원문
```

Rule 13.6 The operand of the sizeof operator shall not contain any expression which has potential side effects

[Amplification]
Any expressions appearing in the operand of a sizeof operator are not normally evaluated. This rule 
mandates that the  evaluation of any such expression shall not contain side eff ects, whether or not it is 
actually evaluated.

A function call is considered to be a side eff ect for the purposes of this rule

[Rationale]
The operand of a sizeof operator may be either an expression or may specify a type. If the operand 
contains an expression , a possible programming error is to expect that expression to be evaluated 
when it is actually not evaluated in most circumstances.

The C90 standard states that expressions appearing in the operand are not evaluated at run-time.

In C99, expressions appearing in the operand are usually not evaluated at run-time. However, if the 
operand contains a variable-length array type then the array size expression will be evaluated if
necessary. If the result can be determined without evaluating the array size expression then it is 
unspecifi ed whether it is evaluated or not.

[Exception]
An expression of the form sizeof ( V ), where V is an lvalue with a volatile qualifi ed type that is not 
a variable-length array, is permitted.

[Example]
 volatile int32_t i;
         int32_t j;
         size_t  s;

 s = sizeof ( j );         /* Compliant             */
 s = sizeof ( j++ );       /* Non-compliant         */
 s = sizeof ( i );         /* Compliant - exception */
 s = sizeof ( int32_t );   /* Compliant             */

In this example the fi nal sizeof expression illustrates how it is possible for a variable-length array size 
expression to have no eff ect on the size of the type. The operand is the type “array of n pointers 
to function with parameter type array of v int32_t”. Because the operand has variable-length 
array type, it is evaluated. However, the size of the array of n function pointers is unaff ected by the 
parameter list for those function pointer types. Therefore, the volatile-qualifi ed object v may or may 
not be evaluated and its side eff ects may or may not occur.

 volatile uint32_t v;

 void f ( int32_t n )
 {
  size_t s;

  s = sizeof ( int32_t[ n ] );                         /* Compliant     */
  s = sizeof ( int32_t[ n++ ] );                       /* Non-compliant */
  s = sizeof ( void ( *[ n ] ) ( int32_t a[ v ] ) );   /* Non-compliant */
 }


```
