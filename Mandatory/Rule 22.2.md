## 규칙 22.2 메모리 블록은 표준 라이브러리 함수로 할당된 경우에만 해제되어야 합니다.
Rule 22.2 A block of memory shall only be freed if it was allocated by means of a Standard Library function

### [추가 설명]
표준 라이브러리 함수 중 메모리를 할당하는 함수는 malloc, calloc, realloc입니다.

메모리 블록은 그 주소가 free에 전달될 때 해제되며, realloc에 전달될 때도 해제될 가능성이 있습니다. 해제된 메모리 블록은 더 이상 할당된 것으로 간주되지 않으므로, 이후 다시 해제할 수 없습니다.

### [근거]
할당되지 않은 메모리를 해제하거나, 동일한 할당된 메모리를 여러 번 해제하면 정의되지 않은 동작이 발생합니다.

### [예제]
```c
#include <stdlib.h>

void fn(void) {
    int32_t a;

    /* 비준수 - a는 할당된 메모리를 가리키지 않음 */
    free(&a);
}

void g(void) {
    char *p = (char *)malloc(512);
    char *q = p;

    free(p);

    /* 비준수 - 할당된 블록이 두 번째로 해제됨 */
    free(q);

    /* 비준수 - 할당된 블록이 세 번째로 해제될 가능성 있음 */
    p = (char *)realloc(p, 1024);
}

```
위 예제에서:

- fn 함수에서 &a는 할당된 메모리를 가리키지 않기 때문에 free(&a)는 비준수입니다.
- g 함수에서 p와 q가 동일한 메모리를 가리키고 있으며, p가 이미 free(p)로 해제된 후 free(q)로 다시 해제되므로 비준수입니다.
- p가 realloc에 전달되어 재할당되면, 원래의 메모리 블록이 해제될 가능성이 있어 비준수입니다.




### 원문
```

Rule 22.2 A block of memory shall only be freed if it was allocated by means of a Standard Library function

[Amplification]
The Standard Library functions that allocate memory are malloc, calloc and realloc.

A block of memory is freed when its address is passed  to free and potentially freed when its address is 
passed to realloc. Once freed, a block of memory is no longer considered to be allocated and therefore 
cannot subsequently be freed again

[Rationale]
Freeing non-allocated memory, or freeing the same allocated memory more than once leads to 
undefi ned behaviour.

[Example]
 #include <stdlib.h>

 v oid fn ( void )
 {
  int32_t a;

  /* Non-compliant - a does not point to allocated storage     */
  free ( &a );
 }

 void g  ( void )
 {
  char *p = ( char * ) malloc ( 512 );
  char *q = p;

  free ( p );

  /* Non-compliant - allocated block freed a second time       */
  free ( q );

  /* Non-compliant - allocated block may be freed a third time */
  p = ( char * ) realloc ( p, 1024 );
 }
 
```
