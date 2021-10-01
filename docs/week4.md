p.26 , p.27

### floating point precision

real number 를 컴퓨터가 이해할 수 있는 근사값으로 표현하는 방법 
=> sign bit, exponent big, mantissa bit 로 구성되어 있다. 

- 정보 손실 없이 얼마나 많은 유의한 자릿수를 나타낼 수 있는지 
- 제한된 메모리 때문에 부동 소수점 자리가 특정 수 의 유의한 숫자만 저장가능 
- 실제로 저장되는 숫자는 원하는 숫자와 정확히 일치할 수 없다. 





p.29

### enumeration

enum ( in C ) : 정수형 상수에 이름을 붙여서 코드를 이해하기 쉽게 해준다. 

```C
const int Sunday = 0;
const int Monday = 1;
const int Tuesday = 2;
...

// => enum 을 사용해서 더 편하게 정의 가능 하다. 

#include <stdio.h>

enum DayOfWeek {    // 열거형 정의
    Sunday = 0,         // 초깃값 할당
    Monday,						// 아래의 값들은 1 씩 증가하면서, 자동할당된다. 
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday
};

int main()
{
    enum DayOfWeek week;    // 열거형 변수 선언
    week = Tuesday;    // 열거형 값 할당
    printf("%d\n", week);   // 2: Tuesday의 값 출력

    return 0;
}
  
```





p.34 

### reference counting 

메모리를 제어하는 방법 중 하나로 Garbage Collection 의 한 방식이다. 

어떤 한 동적 단위(객체, Object)가 참조값을 가지고 이 단위 객체가 참조(참조 복사)되면 참조값을 늘리고 참조한 다음 더이상 사용하지 않게 되면 참조값을 줄이면 된다. 보통 참조값이 0이 되면 더이상 유효한 단위 객체로 보지 않아 메모리에서 제거한다.

약점으로는 매번 참조할 때마다 참조값을 검사해야 하므로 많은 수의 단위 객체를 사용하게 되면 그 검사에 대한 부하가 커진다. 또한 참조하는 단위 객체 사이에 서로 참조하게 되면 **순환 참조 오류**로 인해 잘못된 참조 파괴가 생기거나 또는 단위 객체가 고아(orphaned)가 될 수 있다.

강한 참조 / 약한 참조가 있는데, 일반적으로 강한 참조를 말하며, 순환 참조 오류를 해결하기 위해서 약한 참조를 사용한다. 

- 메모리에 할당된 객체에 참조중이 개수를 세는 것
- 특정 객체에 얼마나 많은 참조가 있는지 **숫자**로 세는 것 

<br/>

<br/>

코드로 확인해보자. 

```C
Object * obj1 = new Object(); // RefCount(obj1) starts at 1
Object * obj2 = obj1;         // RefCount(obj1) incremented to 2 as new reference is added
Object * obj3 = new Object(); 

obj2->SomeMethod();
obj2 = NULL;                  // RefCount(obj1) decremented to 1 as ref goes away
obj1 = obj3;                  // RefCount(obj1) decremented to 0 and can be collected
```

<br/>

순환 참조를 해결하기 위해 구현한 GC 를 'Automatic Garbage Collection' 이라 한다.

자세한 내용은 정리한 포스트로 대체하겠다. [python 의 메모리 관리](https://github.com/SangHoo-c/lsh-tech-blog/blob/master/Python/Memory-Management-2.md#collect_generations)

<br/>

<br/>

