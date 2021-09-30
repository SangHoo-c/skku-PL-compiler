### Memory Alignment

> 컴퓨터 메모리에서 데이터가 정렬되고 액세스 되는 방식을 나타낸다. 
>
> **데이터 정렬**, **데이터 구조 패딩 (padding)**, **패킹 (packing)**, 의 세가지 문제로 구성된다. 

컴퓨터 하드웨어의 CPU 는 데이터가 **naturally aligined** (데이터의 주소가 데이터 사이즈의 배수) 될 때, 
메모리에 대한 R/W 을 가장 효율적으로 수행할 수 있다. 

**naturally aliginment** 를 보장하기 위해, structure elements **사이** 또는 **마지막** 요소 뒤에 **padding** 을 삽입해야한다. 





<br/>

***(현대의 프로세서의 메모리 하위 시스템이)*** 메모리에 접근하는 것을 프로세서의 **워드 크기** 에 대해서 분할하고 정렬하는 것으로 제한한다. 

=> 프로세서의 워드 크기가 4 byte 일때, 메모리로 부터 load 명령어시 4 바이트 만큼 읽고 / 기준 주소 또한 4 의 배수이어야 한다는 것



그렇다면, 왜? 워드 단위로 메모리 접근을 제한할까? 
Ref. [memory alignment](https://minusi.tistory.com/entry/%EB%A9%94%EB%AA%A8%EB%A6%AC-%EC%96%BC%EB%9D%BC%EC%9D%B8%EB%A8%BC%ED%8A%B8Memory-Alignment).

- 속도
- 범위 
- 원자성

memory usage effieciency 를 위해서. 

<br/>

메모리룰에 따라서 멤버 순서에 따른 객체 크기가 달라진다. 

> 메모리룰 
>
> 1. 멤버 변수는 변수 사이즈 배수에서 위치가 할당 되어야 한다. 
> 2. object 전세 사이즈는 가장 큰 멤버 변수의 배수가 되어야 한다. 

padding 은 safety & performance 를 위함이다. 

> ...the total size of the structure should be a multiple of the largest **alignment** of any structure member...



```C
struct char_int {
    double a;
    int b;
    int c;
};
struct char_int2 {
    int a;
    // 4B padding here.
    double b;
    int c;
    // 4B padding here.

};

/*
$ ./a.out                                
sizeof(struct_a):16, &struct_a.a:0xed2af9b8, &struct_a.b:0xed2af9c0
sizeof(struct_b):24, &struct_b.a:0xed2af9a0, &struct_a.b:0xed2af9a8
*/

// padding 으로 인해 전체 struct size 의 변화를 알 수 있다. 
```



<br/>

<br/>

p. 49

Records : 다른 타입의 정해진 수의 원소들을 하나로 모은것 (ex. c - 구조체)

Variant Records : 하나의 타입에 여러 다른 형태의 타입이 올 수 있지만, 동시에 여러개의 값을 갖진 않는다.  (ex. c - Union)

하나만 사용하는게 확실하다면, 뭘 사용하는지에 대해서 flag 를 사용한다. 

```C
// Union : 멤버변수끼리 메모리 공유하는 것 
// 메모리를 공유하면 메모리를 아낄 수 있는 장점이 있는 반면 / 다른 멤버변수에 값을 넣으면 기존의 값은 지워진다. 
// 공용체의 전체 크기는 가장 큰 자료형의 크기 
#include <stdio.h>

union data
        {
    int age;
    int score;
    char class;
        };

int main(void)
{
    union data miyeong;
    miyeong.age = 20;
    miyeong.score = 100;		// 나중에 넣은 값만, 메모리에 할당 
    printf("age : %d \n", miyeong.age);
    printf("score : %d \n", miyeong.score);
    printf("miyeong의 크기 : %d \n", sizeof(miyeong));		

    return 0;
}

/*
output 
$ ./a.out     
age : 100 
score : 100 	
miyeong의 크기 : 4 
*/
```



<br/>

<br/>



### Dangling Reference 

**Dangling Pointer** : 메모리에서 할당해제 된 공간을 여전히 가리키고 있는 포인터 

[about dangling pointer](https://thinkpro.tistory.com/67)

- 메모리 접근시 예측 불가능한 동작
- 메모리 접근 불가시 segment fault 
- 잠재적인 보안 위험 

```C
#include <stdio.h>
#include <stdlib.h>

int main(void){
    int *x = (int *)malloc(sizeof(int));
    int *y;

    *x = 20;
    y = x;
    free(x);
    printf("%d", *y);	// 해제된 메모리 공간을 참조하고 있다. 
    return 0;
}

/*
Local variable 'y' may point to deallocated memory
*/
```



<br/>

Dangling Pointer 다루기 

- 여러 포인터가 같은 '동적 메모리' 를 가리키는 것을 피하자. 
- 메모리 해제 후, 포인터를 0 또는 NULL 로 설정하자. 



> ***NULL 포인터란***
>
> 널 포인터는 동적 메모리 할당을 처리할 때 유용하다. 
>
> 동적 메모리 할당과 관련하여 널 포인터는 기본적으로 "이 포인터에 메모리가 할당되지 않았다."를 의미한다.
>
> 

<br/>





p. 52

### Garbage 

메모리 할당 이 되었지만, 프로그램에서 접근할 수 없는 메모리

```C
#include <stdio.h>
#include <stdlib.h>

int main(void){
    int *x = (int *)malloc(sizeof(int));
    x = 0;	// 포인터 변수에 다른 값을 배정하는 경우, x 가 가리키던 메모리 공간이 Garbage 가 된다.  
    *x = 4;	
    printf("%d", *x);
    return 0;
}

/*
output
$ ./a.out     
[1]    3535 segmentation fault  ./a.out

*/
```









### Garbage Collection

프로그램이 더 이상 접근 할 수 없는 메모리를 자동으로 해제시켜 주는 기술 

memory leak 을 잡기위해 GC 를 한다??? => **잘못된 개념** 

> memory leak 이란? 
>
> *a type of resource leak that occurs when a computer program incorrectly manages memory allocations in such a way that memory which is no longer needed is not released.*
>
> 번역하자면, **사용하지 않을** 메모리를 해제하지 않는 현상.
>
> 계속된 memory leak 은 Out Of Memory 를 발생. 

<br/>

결국, memory leak 을 잡기 위해선 사용하지 않는 메모리를 찾아내야 한다. (but, memory leak 은 잡아낼 수 가 없다. [memory leak ref](https://blog.seulgi.kim/2019/04/garbage-collection-and-memory-leak.html))

GC 는 메모리를 전부 해제하는 것을 보장하지 않는다. 

대신, 해제해도 안전한 메모리만 해제하는 것이다. 



> **GC 는 메모리 사용의 효율성이 아닌, 소프트웨어의 안정성을 올리는 도구**









<br/>

<br/>



