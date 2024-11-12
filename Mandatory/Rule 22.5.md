## 규칙 22.5 FILE 객체에 대한 포인터는 역참조되어서는 안 됩니다.
Rule 22.5 A pointer to a FILE object shall not be dereferenced
### [추가 설명]
FILE 객체에 대한 포인터는 직접적으로 또는 memcpy나 memcmp와 같은 함수를 통해 간접적으로 역참조해서는 안 됩니다.

### [근거]
표준(C90 섹션 7.9.3(6), C99 섹션 7.19.3(6))에 따르면, 스트림을 제어하는 데 사용되는 FILE 객체의 주소는 중요한 의미를 가지며, 객체의 복사본은 동일한 동작을 보장하지 않을 수 있습니다. 이 규칙은 이러한 복사가 발생하지 않도록 보장합니다.

FILE 객체를 직접 조작하는 것은 스트림 지정자로서의 사용과 호환되지 않을 수 있으므로 금지됩니다.

### [예제]
```c
#include <stdio.h>

FILE *pf1;
FILE *pf2;
FILE f3;

pf2 = pf1;      /* 준수 */
f3 = *pf2;      /* 비준수 */

```
위 예제에서, pf2 = pf1는 포인터 할당으로 준수입니다. 그러나 f3 = *pf2는 FILE 객체를 역참조하여 복사하려고 하므로 비준수입니다.

다음 예제에서는 FILE *가 pos라는 멤버를 가진 완전한 타입이라고 가정합니다:
```
pf1->pos = 0;   /* 비준수 */
```
이 예제에서 pf1->pos = 0는 FILE 객체를 직접적으로 조작하려고 하므로 비준수입니다.





### 원문
Rule 22.5 A pointer to a FILE object shall not be dereferenced

[Amplification]
A pointer to a FILE object shall  not be dereferenced directl y or indirectly (e.g. by a call to memcpy or 
memcmp).

[Rationale]
The Standard (C90 Section 7.9.3(6), C99 Section 7.19.3(6) ) states that the address of a FILE object 
used to control a stream may be signifi cant and a copy of the object may not give the same behaviour. 
This rule ensures that such a copy cannot be made.

The direct manipulation of a FILE object is prohibited as this may be incompatible with its use as a 
stream designator.

[Example]
#include <stdio.h>

FILE *pf1;
FILE *pf2;
FILE  f3;

pf2 = pf1;      /* Compliant     */
f3 = *pf2;     /* Non-compliant */

The following example assumes that FILE * specifi es a complete ty pe with a member named pos:

pf1->pos = 0;   /* Non-compliant */

```
