## 규칙 22.4 읽기 전용으로 열린 스트림에 쓰기를 시도해서는 안 됩니다.
Rule 22.4 There shall be no attempt to write to a stream which has been opened as read-only
### [근거]
읽기 전용 스트림에 쓰기를 시도할 경우 동작이 정의되지 않으므로, 이는 안전하지 않다고 간주됩니다.

### [예제]
```c
#include <stdio.h>

void fn(void) {
    FILE *fp = fopen("tmp", "r");
    (void)fprintf(fp, "What happens now?");   /* 비준수 */
    (void)fclose(fp);
}

```
위 예제에서, 파일 "tmp"는 읽기 전용으로 열렸지만, fprintf를 사용해 해당 스트림에 쓰기를 시도하고 있으므로 비준수입니다.


### 원문
```

Rule 22.4 There shall be no attempt to write to a stream which has been opened as read-only

[Rationale]
The Standard does not spec ify the behaviour if an attempt is made to write to a read-only stream. For 
this reason it is considered unsafe to write to a read-only stream.

[Example]
 #include <stdio.h>
 void fn ( void )
 {
  FILE *fp = fopen ( "tmp", "r" );
  ( void ) fprintf ( fp, "What happens now?" );   /* Non-compliant */
  ( void ) fclose ( fp );
 }
```
