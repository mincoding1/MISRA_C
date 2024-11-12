## 규칙 22.6 스트림이 닫힌 후에는 FILE에 대한 포인터 값을 사용해서는 안 됩니다.
Rule 22.6 The value of a pointer to a FILE shall not be used after the associated stream has been closed
### [근거]
표준에 따르면, 스트림이 닫힌 후 FILE 포인터의 값은 불확정 상태가 됩니다.

### [예제]
```c
#include <stdio.h>

void fn(void) {
    FILE *fp;
    void *p;
    fp = fopen("tmp", "w");
    if (fp == NULL) {
        error_action();
    }
    fclose(fp);
    fprintf(fp, "?");   /* 비준수 */
    p = fp;             /* 비준수 */
}

```
위 예제에서 fclose(fp); 이후 fp는 더 이상 유효한 포인터가 아닙니다. 따라서 fprintf(fp, "?");와 p = fp;는 각각 닫힌 스트림에 대해 포인터 값을 사용하려고 시도하므로 비준수입니다.


### 원문
```

Rule 22.6 The value of a pointer to a FILE shall not be used after the associated stream has been closed

[Rationale]
The Standard  states that the value of a FILE pointer is indeterminate after a close operation on a 
stream

[Example]
#include <stdio.h>
 void fn ( void )
 {
  FILE *fp;
  void *p;
  fp = fopen ( "tm p", "w" );
  if ( fp == NULL )
  {
    error_action ( );
  }
  fclose ( fp );
  fprintf ( fp, "?" );   /* Non-compl iant */
  p = fp;                /* Non-compliant */
 }


```
