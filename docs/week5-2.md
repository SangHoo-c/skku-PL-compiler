## week 5-2



ch3. p2

**subprogram** : fucntion + side effect 

> side effect : state 의 변경사항,  changing something somewhere.
>
> - chaning the value of variable
> - wirting some data to disk 
> - Enabling or disabling a button in the User Interface.

```C
// Any operation which modifies the state of the computer or which interacts with the outside world 
// is said to have a ** side effect ** . 

// n 의 값 ( state of computer ) 을 변경한다 => side effect 
int n = 0;
int next_n() { return n++; }
void set_n(int newN) { n = newN; }      

// output 을 출력한다, => side effect 
int Write(const char* s) { return printf("Output: %s\n", s); }
```



**function** 

- returns a value 
- input 을 받고 output 을 주는것 

**procedure**

-  just executes commands 
- A procedure is a set of commands which can be executed in order.



ch3. p.7
short -circuit expression : 왜 쓰는가 ? 

index 사용 ? No 



ch3. P5.

**postfix / prefix**

```C
#include <stdio.h>

int main(){

    int cnt = 3;
    int res = ++c;			// prefix 
    printf("%d \n", c); // cnt 에 + 1 을 한 후, res 에 cnt 의 값을 넣어준다.
    printf("%d \n", r);


    cnt = 3;				// postfix 
    res = cnt++;    // cnt 가 호출되어 res 에 cnt 값을 넣은 후에, cnt + 1 을 수행한다.

    printf("%d \n", cnt);
    printf("%d \n", res);

    return 0;
}

/*
output
4 
4 
4 
3 
*/
```





ch3. p6.

**lazy evaluation**, 

- 계산의 결과 값이 필요할 때까지 계산을 늦추는 기법 
- 필요할때만 평가되기 때문에, **메모리를 효율적**으로 사용할 수 있음 
- 무한 자료구조를 만들어 사용가능 
- 실행 도중의 오류상태를 피할 수 있음 
- 컴파일러 최적화 가능 

**eager evaluation**

- operands 를 바로 계산 
- 연산을 할 수 있을때 다 한다. 
- 전통적인 프로그래밍 언어에서 사용

```python
# 대부분의 요소가 사용될 것이 확실한 상황이면,
# list 를 통해 미리 연산을 해두는 것이 더 효율적이다. 

def func():
    print("return 1")
    return 1

  
# list comprehension  
print("[let's make one_list !]")
one_list = [func() for x in range(10)]


# lazy evaluation 
print("[let's make one_generator !]")
one_generator = (func() for x in range(10))  # generator 생성

print("[let's print one_generator !]")		# 실제로 값을 출려갛기 전에 func() 가 한번도 실행되지 않았다. 
for one in one_generator:
    print(one)

    
# ref. https://itholic.github.io/python-lazy-evaluation/
```



<br/>

<br/>





ch3. p.11

swithching is faster ? => not always, 

옵션이 작다면, if 가 빠르다. but 옵션이 많다면,  switch 가 훨씬빠르다. 

**jump table** 



Ref. https://modoocode.com/16

ref. [switch vs if-else](https://aahc.tistory.com/6)

