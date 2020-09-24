# PEP 8

* 원본 링크: https://www.python.org/dev/peps/pep-0008


### 이름 규칙
* 모든 변수와 함수 이름은 소문자로 써 주시고, 여러 단어일 경우 _로 나눠 주세요.
```
# bad
someVariableName = 1
SomeVariableName = 1

def someFunctionName():
    print("Hello")


# good
some_variable_name = 1

def some_function_name():
    print("Hello")
```

* 모든 상수 이름은 대문자로 써주시고, 여러 단어일 경우 _로 나눠주세요.
```
# bad
someConstant = 3.14
SomeConstant = 3.14
some_constant = 3.14


# good
SOME_CONSTANT = 3.14
```

### 의미 있는 이름
```
# bad (의미 없는 이름)
a = 2
b = 3.14
print(b * a * a)


# good (의미 있는 이름)

radius = 2
pi = 3.14
print(pi * radius * radius)
```

```
# bad (의미 없는 이름)
def do_something():
    print("Hello, world!")


# good (의미 있는 이름)
def say_hello():
    print("Hello, world!")
```
    
### 들여쓰기
* 들여쓰기는 무조건 스페이스 4개를 사용하세요.

```
# bad (스페이스 2개)
def do_something():
  print("Hello, world!")


# bad (스페이스 8개)
i = 0
while i < 10:
        print(i)


# good (스페이스 4개)
def say_hello():
    print("Hello, world!")
```    
    
### 함수 정의
* 함수 정의 위아래로 빈 줄이 두 개씩 있어야 합니다. 하지만 파일의 첫 줄이 함수 정의인 경우 해당 함수 위에는 빈 줄이 없어도 됩니다.

```
# bad
def a():
    print('a')
def b():
    print('b')

def c():
    print('c')



# good
def a():
    print('a')


def b():
    print('b')


def c();
    print('c')
```

### 괄호 안
* 괄호 바로 안에는 띄어쓰기를 하지 마세요.

```
# bad
spam( ham[ 1 ], { eggs: 2 } )


# good
spam(ham[1], {eggs: 2})
```

### 함수 괄호
* 함수를 정의하거나 호출할 때, 함수 이름과 괄호 사이에 띄어쓰기를 하지 마세요.

```
# bad
def spam (x):
    print (x + 2)


spam (1)


# good
def spam(x):
    print(x + 2)


spam(1)
```

### 쉼표
* 쉼표 앞에는 띄어쓰기를 하지 마세요.

```
# bad
print(x , y)


# good
print(x, y)
```

### 지정 연산자
* 지정 연산자 앞뒤로 띄어쓰기를 하나씩만 해 주세요.

```
# bad
x=1
x    = 1


# good
x = 1
```

### 연산자
* 기본적으로는 연산자 앞뒤로 띄어쓰기를 하나씩 합니다.
```
# bad
i=i+1
submitted +=1


# good
i = i + 1
submitted += 1
```

* 하지만 연산의 "우선 순위"를 강조하기 위해서는, 연산자 앞뒤로 띄어쓰기를 붙이는 것을 권장합니다.
```
# bad
x = x * 2 - 1
hypot2 = x * x + y * y
c = (a + b) * (a - b)


# good
x = x*2 - 1
hypot2 = x*x + y*y
c = (a+b) * (a-b)
```

### 코멘트
* 일반 코드와 같은 줄에 코멘트를 쓸 경우, 코멘트 앞에 띄어쓰기 최소 두 개를 해 주세요.
```
# bad
x = x + 1# 코멘트


# good
x = x + 1  # 코멘트
```