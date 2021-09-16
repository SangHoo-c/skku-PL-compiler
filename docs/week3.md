## week 3



### CISC / RISC

- RISC : execution time same 
= pipeline architect 
= fetch - decode -execute - store



4 ~ 6 가 optimal 

왜 100 개의 stage 를 하지 않을까? 

=> overhead, 



### inline function

**함수**를 사용하면, 다음과 같은 이점이 있다. 

- 함수 내부 코드 재사용 가능
- 코드 수정 용이
- 함수 이름을 통해 의미하는바 전달 용이
- ''매크로'' 와 다르게 인자가 함수 매개 변수와 일치하는지 확인 (type checking)
- 프로그램 디버깅 용이 



**But**, 함수가 호출될 때마다 발생하는 ***오버헤드*** 존재. 



**Why?**, CPU 가 함수의 반환 위치를 알 수 있도록 현재 명렁어의 주소를 저장해야하기 때문. 

**SO**, C++ 에서 inline function 을 사용하여 내부에서 작성된 코드의 속도와 함수의 장점을 결합한다! 

- inline 키워드 ***(compiler 가 알아서 해주긴 함)***  함수를 inline-function 으로 처리하도록 요청 
- compiler 가 코드를 compile 하면 모든 inline-function 은 **in-place** 확장된다. 
- 함수 호출 == 자체의 내용 복사본으로 대체 

**THEN,** compile 된 코드가 커질 수 있지만, ***함수 오버헤드가 제거된다!!!***



```C++
inline int min(int x, int y){
  return x > y ? y : x;
}

int main(){
  std::cout << min(5, 6) << '\n';
  std::cout << min(3, 2) << '\n';
	return 0
}

... 이런 형태로 compile 된 assembly 코드가 생성된다. 
  
int main(){
  std::cout << (5 > 6 ? 6 : 5) << '\n';
  std::cout << (3 > 2 ? 2 : 3) << '\n';
	return 0
}
```









### C 언어 빌드 과정 

- **(Compiler)** Preprocess
- **(Compiler)** Compile
- **(Compiler)** Assemble
- **(Linker)** Link





#### Preprocess

- 헤더파일을 포함 / 매크로 생성

```C
# include <stdio.h>			// 헤더 파일
# define MAX_NUM = 100	// 메크로
```



#### Compile

- C 언어 코드 => 어셈블리어로 변환 
- 전처리된 소스 코드를 어셈블리어로 변환 

> compiler : 프로그램 Runtime 전에 전체 소스 코드를 검사하여 machine code 로 변환한다. 



#### Assemble

- 어셈블리어를 기계어 (binary) 로 변환 
- assemble 의 결과로 binary 파일 (object파일)  을 받는다. 
- assemble 을 마친 후, 얻은 binary 파일은 직접 실행 불가!! 
  => 사용자 라이브러리 포함하고 있지 않기 때문,  linking 을 통해 연결해야한다. 



#### Link

- 여러개의 object file 을 하나의 실행 파일로 만드는 작업
- binary 파일들이 서로 연결된다.
- link 의 결과로 실행파일이 생성된다. 









### Subtype 

Subtype 이란? Large programming 할때 좋은점 있다

- space saving 
- long 의 subtype 은 short 가 아니다.
- 32 bit 중에 16 bit 를 사용하면 subtype 라고 한다. 없는걸 define
- subtype 의 장점 ( time space 감소, error checking 가능 )



### Polymorphism

다형성, 다른 종류의 data types 의 값들을 동일한 interface 로 사용할 수 있게 해주는 컴퓨터 언어의 특징.

- (ad-hoc) overloading
- (ad-hoc) coercion
- (Universal) parametric 
- (Universal) subtype

**Ad-hoc** : 서로 다른 code 가 각각의 type 에 대해서 사용되는 것 

**Universal** : 동일한 code 가 여러 type 에 대해서 사용되는 것 



#### ad-hoc polymorphism

- 어떤 argument 가 적용되느냐에 따라서 다르게 행동하는 함수 
- Function overloading / operator overloading 

```C++
// overloading : 지나치게 적재 
// 함수의 이름을 동일하게 지어주더라도 
// 인자의 타입 / 개수 / 등으로 각각의 함수를 구별, 자동으로 적절한 함수를 찾아줌 

void print(int x) { ... }
void print(float x) { ... }
void print(string x) { ... }

print(5);
print(4.13);
print("hello");

// 컴파일러가 알아서 인자의 타입 or 개수를 보고 적절한 함수를 찾아 바인딩 해준다. 
```



#### coercion polymorphism

- 실수 (float) 인자를 받는 함수를 정의하게 되면 다음과 같이 정수를 넣어도 문제가 없다. 

```C++
void func(float x) { ... }

f(5); 		// ok

/*
	coercion (코어션) 이 암묵적 (implicit) 으로 일어나기 때문

	1. 5 라는 숫자, 자동으로 실수형으로 type conversion
	2. 함수의 인자로 전달 
*/
```



> 그렇다면, 이런 문제는 overloading 으로 해결할 수 있는것 아닌가?! 

> 그렇다! 하지만, 여러개의 조합을 고려해야하기 때문에 ... 적절히 섞어써야한다. 

```C++
3 + 5
3 + 5.0
3.0 + 5
3.0 + 5.0
  
1. int/int , int/float, float/int, float/float 케이스 오버로딩
2. int/int, float/float 오버로딩, int + float 의 경우, int 를 float 으로 코어선 
```





#### parametric (인자) polymorphism

- generic programming 
- data type 이나 함수를 범용적 (generically) 작성하여 세부 타입에 상관없이 동일하게 처리할 수 있는 것
- Function(a, b) return a + b // 인 함수에서 a, b 가 integer / double / string 이든 동일하게 처리되어야 하는것

```C++
// type 이 다르기 때문에 모든 코드를 다시 작성해야하는 문제를 해결해준다! 
// type 을 인자화하여 그 인자의 값을 결정하는 시점을 객체가 생성될 시점으로 미룬다! 

class Stack<T>{
  
  ...
  public void push(T item){ ... }
  public void pop(){ ... }
}

Stack IntStack = new Stack<int>;
Stack StringStack = new Stack<string>;
```





#### Subtype polymorphism 

- inheritance 
- OOP 에서 사용
- 부모, 자식 관계에서 같은 메소드 호출에 대해 서로 다른 방법으로 응답하는 것  
- 추상 메소드 : 함수의 선언부분만 있고 구현 부분이 없는 함수, 하위 클래스에서 정의

```C++
// 어떤 타입 T 를 요구하는 상황에서 타입 T 뿐 아니라
// 그것의 subtye 을 가지는 객체도 대신 사용할 수 있는 것

class T { ... } 
class T1 extends T { ... }

T t = new T();
T1 t1 = new T1();

void func(T x) { ... }

func(t);		// ok
func(t1);		// ok
```



> Polymorphism opertations 이란?
>
> - 똑같은게 여러개의 형태를 갖고 있다. 
> - 1 + 2 // 1.0 + 2.0 
> - 같은 모습이지만, 다른 코드로 execute 







### Static / Dynamic type 

- 장단점 
- 선언 시점
- Dynamic type language 의 장단점





#### Static 

- compile 시간에 변수의 type 이 결정 
- 프로그래머가 변수의 type 을 직접 명시해야 한다. (C, C++, JAVA)
- compile 시간에 변수의 type 을 체크하므로 사소한 버그들을 쉽게 체크할 수 있다. 
- 컴파일 최적화가 가능, runtime 효율성 향상, 메모리 사용 절약 가능 



#### Dynmaic

- runtime 에 type 이 결정 
- 빠르게 코드를 작성할 수 있다. (python)
- runtime 에 변수의 type 을 판단 (누가??? )
  => ex) python : 인터프리터가 runtime 에 type checking 



```C
// C 는 compile 시에 type checking 을 한다.
// 타입 에러 발생 코드 
int var;
if(0){
  var = "3" + 1;
}
```

```python
# python 은 인터프리터가 runtime 에 type checking 을 하기 때문에 
# 정상 작동 코드 
if 0:
  var = "3" + 1
```





### Volatile 

- volatile 을 사용하여, 컴파일러가 해당 변수를 최적화에서 제외하여 **항상 메모리**에 접근하도록 만든다. 
- FM 대로 메모리에 접근하고 레지스터로 이를 읽어와 메모리로 다시 저장 
- memory map IO 가 안쓰이면서 줄어들었다.



>CPU 와 입출력 기기를 접속하는 방법 
>
>1. Memory Mapped I/O
>2. I/O Mapped I/O





**Memory Mapped I/O**

- 메모리와 I/O 가 하나의 연속된 address 영역에 할당된다.
- 즉, I/O 가 차지하는 만큼 메모리 용량은 감소한다. 
- CPU 입장에서 메모리와 I/O 가 동일한 외부기기로 간주 (read/write ,,, load/store)
- **포트 입출력** 구현시 복잡성 감소 
- 임베디드 시스템, ARM, MIPS, ...
- **BUT** 주소와 데이터 버스를 많이 사용하게 되어, 메인 메모리에 접근하는 것보다 매핑된 장치에 접근하는 것이 느리다. 



> 사용시 I/O 영역변수는 volatile 타입으로 선언해야한다. (컴파일러 최적화 방지)
>
> I/O 영역은 non-cacheable 로 설정해야 한다.





**I/O Mapped I/O**

- 메모리와 I/O 가 별개의 주소 영역에 할당 
- I/O 를 사용하더라도 메모리 용량은 감소하지 않는다. 
- CPU 의 입장에서는 메모리와 I/O 를 구분하여 취급해야하므로 read/write 신호 이외의 I/O 접근하기 위한 신호 필요
- 주로 intel 계열의 CPU 에서 사용 
- 메모리용으로 주소영역 전체를 사용할 수 있다. 

>**PLUS 상식**
>
>ARM Processor 는 **MMI (memory mapped i/o) 방식**
>ARM Assembler 나 C 로 작성할때 실제 메모리 주소를 포인터로 잡아서 값에 접근한다. 
>특히 어셈블리상에서 I/O port 접근 명령어가 따로 존재하지 않고, 
>일반 메모리에 대한 접근인 load/store 명렁어를 사용한다. 





### type conversion / Explicit / Implicit

=> main goal : not to loose info

= 정보손실은 똑같이 일어남
= explicit 사용하면 clear 하다. 

implicit - compile - static -  오류찾기 쉬움

explicit - runtime - dynamic 





### **!!! P.26 계산할 수 있어야!!! : 시험에 나온다.**





### p.35 Structure / elementary data type 

Why system doesn’t provide structure data type? (대신 user-defined)

=> system may provide everything , but cost too much.



By using structure, convenient, efficient

​	

Ordered set P.12

\- 순서대로 지정되어있는것









### Self-modification 이란?

- 실행 중에 자신의 명령어를 바꾸는 코드 





