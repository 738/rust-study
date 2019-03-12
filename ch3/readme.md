# Ch 3. Common Programming Concepts

## 3.1 Variables and Mutability

* 챕터 2에서 말했듯이 변수는 기본적으로 불변한다.

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

위 코드를 작성한 후, `cargo check`를 하게 되면

```bash
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         -
  |         |
  |         first assignment to `x`
  |         help: make this binding mutable: `mut x`
3 |     println!("The value of x is: {}", x);
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable

error: aborting due to previous error

For more information about this error, try `rustc --explain E0384`.
error: Could not compile `variables`.
```

위와 같이 `cannot assign twice to immutable variable 'x'` 컴파일 에러가 나오게 된다.

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

* `mut` 키워드를 사용해서 변수를 선언하면 값을 변경할 수 있다.

```bash
$ cargo run
   Compiling variables v0.1.0
    Finished dev [unoptimized + debuginfo] target(s) in 0.42s
     Running `target/debug/variables`
The value of x is: 5
The value of x is: 6
```

### 변수와 상수의 차이점

* 불변한 변수처럼 상수는 값이 바뀔 수 없다. 하지면 그 둘과는 차이점이 있다.
* 상수를 선언하다면 자료형이 표기되어야 한다.
* 첫째로, `const`에는 `mut` 키워드를 사용할 수 없다. 상수는 항상 불변하다.
* 상수는 글로벌 스코프를 포함해 어떤 스코프에서나 선언될 수 있다.
* 마지막 차이점은 상수는 상수 표현식에서만 정해진다. 함수 호출 혹은 런타임에 계산되는 값의 결과가 될 수 없다.

```rust
const MAX_POINTS: u32 = 100_000;
```

### 섀도잉

* 챕터 2에서 보았듯이, 변수를 선언하면 다시 그 변수를 선언할 수 있다. 새로 선언한 변수는 예전에 선언했던 변수의 값을 가린다.

```rust
fn main() {
    let x = 5;

    let x = x + 1;

    let x = x * 2;

    println!("The value of x is: {}", x);
}
```

* 섀도잉은 `mut`으로 변수를 선언하는 것과는 다르다. `let`을 사용해서 재할당하지 않으면 컴파일 타임 에러가 날 것이기 때문이다.
* `let`을 사용함으로써 약간의 변형만을 수행할 수 있다. 이 변형들이 끝나면 변수가 불변하도록 할 수 있다.
* `mut`과 섀도잉의 다른 차이점은 `let` 키워드를 다시 사용할 때 새 변수를 효율적으로 생성하기 때문에 같은 이름을 재사용하면서 값의 자료형을 바꿀 수 있다.

```rust
let spaces = "   ";
let spaces = spaces.len();
```

* 첫째줄의 spaces 문자열 자료형이고, 둘째줄의 spaces 숫자 자료형이다.
* 섀도잉은 `spaces_str`이나 `spaces_num`과 같은 다른 이름을 나오게 하는 것을 막아준다. 대신에 우리는 더 간단한 `spaces`라는 이름을 재사용할 수 있다. 그러나 아래와 같이 `mut`을 사용하면 컴파일 타임 에러가 발생하게 된다.

```rust
let mut spaces = "   ";
spaces = spaces.len();
```

```bash
error[E0308]: mismatched types
 --> src/main.rs:3:14
  |
3 |     spaces = spaces.len();
  |              ^^^^^^^^^^^^ expected &str, found usize
  |
  = note: expected type `&str`
             found type `usize`
```             

#### 요약

* 변수는 기본적으로 불변이다.
* 변수와 상수의 차이점
  * 상수는 항상 불변이고 변수는 `mut`과 섀도잉을 사용하여 변하게 할 수 있다.
* `mut`과 섀도잉의 차이점
  * `mut`을 사용하면 `let`을 사용하지 않고 변수에 값을 재할당할 수 있다. 자료형을 변경할 수 없다.
  * 섀도잉은 `let`으로 변수를 다시 선언하는 것인데, 같은 이름을 사용하면서 자료형을 변경할 수 있다.

## 3.2 Data Types

* 타입은 크게 스칼라, 컴파운드 둘로 나눌 수 있다.
* Rust에서 사용되는 값은 모두 타입을 가진다. 어떤 형태의 데이터인지 명시해서 Rust가 데이터를 어떻게 다룰지 알 수 있게 해야 한다.
* Rust는 타입이 고정된 언어이다. 모든 변수의 타입이 컴파일 시에 반드시 정해져있어야 한다.

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

* 타입이 명시되어있지 않다면 다음 에러가 발생한다.

```bash
error[E0282]: type annotations needed
 --> src/main.rs:2:9
  |
2 |     let guess = "42".parse().expect("Not a number!");
  |         ^^^^^
  |         cannot infer type for `_`
  |         consider giving `guess` a type
```

### 스칼라 타입

* *스칼라*는 하나의 값으로 표현되는 타입이다. Rust는 정수형, 부동소수점 숫자, 불리언, 그리고 문자 네 가지의 스칼라 타입을 가지고 있다.

#### 정수형

* 정수형은 소수점이 없는 숫자이다. `u32`는 부호가 없는 32비트 변수임을 나타낸다.

| Length  | Signed | Unsigned |
|---------|--------|----------|
| 8-bit   | i8     | u8       |
| 16-bit  | i16    | u16      |
| 32-bit  | i32    | u32      |
| 64-bit  | i64    | u64      |
| 128-bit | i128   | u128     |
| arch    | isize  | usize    |

* `isize`와 `usize`는 프로그램이 동작하는 컴퓨터 환경에 따라 달라진다. 64-bit 아키텍처이면 64-bit를, 32-bit 아키텍처면 32-bit를 사용한다.
* 다음 표처럼 정수형 리터럴을 사용할 수 있다.

| Number literals | Example     |
|-----------------|-------------|
| Decimal         | 98_222      |
| Hex             | 0xff        |
| Octal           | 0o77        |
| Binary          | 0b1111_0000 |
| Byte (u8 only)  | b'A'        |

* 일반적인 경우, Rust의 기본값인 `i32`를 사용하는게 가장 좋다.

#### 부동소수점 타입

* Rust에는 부동소수점 숫자를 위한 두 가지 기본 타입도 있다. Rust의 부동소수점 타입은 `f32`와 `f64`로 각기 32bit와 64bit의 크기를 갖는다.
* 기본 타입은 `f64`인데, 최신의 CPU 상에서 `f32`, `f64` 둘 다 대략 비슷한 속도를 내면서도 더 정밀한 표현이 가능하기 때문이다.

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

* 부동소수점 숫자는 IEEE-754 표준에 따라 표현된다. 

#### 수학적 연산

```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;

    // remainder
    let remainder = 43 % 5;
}
```

#### 불리언 타입

```rust
fn main() {
    let t = true;

    let f: bool = false; // with explicit type annotation
}
```

#### 문자 타입

* Rust의 `char`는 가장 근본적인 알파벳 타입이다.
* String이 큰 따옴표를 쓰는 것에 반해 `char` 타입은 작은 따옴표를 쓴다.

```rust
fn main() {
   let c = 'z';
   let z = 'ℤ';
   let heart_eyed_cat = '😻';
}
```

### 복합 타입

* 복합 타입은 다른 타입의 다양한 값을 하나로 묶을 수 있다. Rust는 튜플과 배열, 두 개의 복합 타입을 가지고 있다.

#### 튜플

```rust
fn main() {
    let tup: (i32, f64, u8) = (500, 6.4, 1);
}
```

* 변수 `tup`에는 튜플 전체가 bind된다.

```rust
fn main() {
    let tup = (500, 6.4, 1);

    let (x, y, z) = tup;

    println!("The value of y is: {}", y);
}
```

* 위와 같이 `tup`을 세 개의 분리된 변수인 `x`, `y`, `z`에 *구조해제*하여 할당할 수 있다.

```rust
fn main() {
    let x: (i32, f64, u8) = (500, 6.4, 1);

    let five_hundred = x.0;

    let six_point_four = x.1;

    let one = x.2;
}
```

* `.` 뒤에 색인을 넣어서 튜플의 요소에 접근할 수 있다.

#### 배열

* 튜플과는 다르게 배열의 모든 요소는 타입이 같아야 한다.
* 고정된 길이를 갖는다.

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
}
```

* 벡터 타입처럼 가변적이지 않다. 벡터 타입은 유사 집합체로 표준 라이브러리에서 제공되며, 확장 또는 축소가 가능하다.

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

* 위와 같이 배열의 요소에 접근할 수 있다.

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];
    let index = 10;

    let element = a[index];

    println!("The value of element is: {}", element);
}
```

```bash
$ cargo run
   Compiling arrays v0.1.0 (file:///projects/arrays)
    Finished dev [unoptimized + debuginfo] target(s) in 0.31 secs
     Running `target/debug/arrays`
thread '<main>' panicked at 'index out of bounds: the len is 5 but the index is
 10', src/main.rs:6
note: Run with `RUST_BACKTRACE=1` for a backtrace.
```

* 컴파일 시에는 아무런 에러도 발생시키지 않았지만 프로그램의 실행 간에 에러가 발생했다.
* 색인을 사용하여 배열에 접근하려고 하면 Rust는 지정된 색인이 배열 길이보다 작은지 확인하고 색인이 길이보다 길면 *패닉*한다.
* Rust의 안전 원칙이 동작하는 첫번째 예이다. Rust는 메모리 접근을 허용하고 계속 진행하는 대신 즉시 종료하여 이러한 종류의 오류로부터 사용자를 보호한다.

#### 요약

* Rust에는 크게 Scala, Compound 타입 두 종류의 타입이 있다.
* Scala 타입에는 정수형, 부동소수점, 불리언, 문자 타입이 있고 Compound 타입에는 튜플, 배열이 있다.

## 3.3 How Functions Work

```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```

* Rust에서는 변수나 함수의 네이밍 규칙으로 *snake case*를 사용한다.
* Rust는 함수의 위치를 신경쓰지 않는다. `another_function` 함수가 `main` 함수 뒤에 있어도 되고 앞에 있어도 상관없다.

### 함수 매개변수

```rust
fn main() {
    another_function(5);
}

fn another_function(x: i32) {
    println!("The value of x is: {}", x);
}
```

* 각 매개변수마다 타입을 지정해야 한다.

### 구문과 표현식

* `let y = 6;`는 구문이다. 함수 정의도 또 하나의 구문이다.

```rust
fn main() {
    let x = (let y = 6);
}
```

```bash
$ cargo run
   Compiling functions v0.1.0 (file:///projects/functions)
error: expected expression, found statement (`let`)
 --> src/main.rs:2:14
  |
2 |     let x = (let y = 6);
  |              ^^^
  |
  = note: variable declaration using `let` is a statement
```

* `let y = 6` 구문은 반환값이 없으므로 x에 바인딩할 값이 없다.
* Rust 코드의 대부분은 표현식이며 이는 어떤 값을 산출한다. `5+6`은 `11`을 산출하는 표현식이다.
* 함수와 매크로를 호출하는 것은 표현식이다.

```rust
fn main() {
    let x = 5;

    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);
}
```

* 새로운 범위를 생성하는데 사용하는 블록 `{}`은 표현식이다.
* 위 예제의 블록에서 `4`를 산출한다. 이 값은 `let` 구문의 일부로 `y`에 바운드된다.
* 표현식은 종결을 나타내는 세미콜론을 사용하지 않는다. 만약 세미콜론을 표현식 마지막에 추가하면, 이는 구문으로 변경되고 반환값이 아니게 된다.

### 반환값을 갖는 함수

* 반환값의 타입이 `->` 뒤에 명시되어야 한다.
* 대부분의 함수들은 암묵적으로 마지막 표현식을 반환한다.

```rust
fn five() -> i32 {
    5
}

fn main() {
    let x = five();

    println!("The value of x is: {}", x);
}
```

```rust
fn main() {
    let x = plus_one(5);

    println!("The value of x is: {}", x);
}

fn plus_one(x: i32) -> i32 {
    x + 1;
}
```

```rust
error[E0308]: mismatched types
 --> src/main.rs:7:28
  |
7 |   fn plus_one(x: i32) -> i32 {
  |  ____________________________^
8 | |     x + 1;
  | |          - help: consider removing this semicolon
9 | | }
  | |_^ expected i32, found ()
  |
  = note: expected type `i32`
             found type `()`
```

* 위와 같이 `plus_one` 함수의 마지막에 표현식이 아닌 구문으로써 `;`을 붙이게 되면 `mismatched types` 에러가 발생한다.
* 구문은 값을 산출하지 않고 `()`처럼 빈 튜플로 표현된다.

#### 요약

* 변수와 함수의 네이밍 기본 관습은 `snake_case`이고 함수의 위치는 상관 없다.
* 매개변수와 반환값에 타입을 지정해야 한다.
* 구문은 산출되는 값이 없고 표현식은 산출되는 값이 있다. 따라서 표현식 뒤에는 `;`이 붙지 않는다.
* 새로운 범위를 생성하는데 사용하는 블록 `{}`은 표현식이다.

## 3.4 Comments

* 주석은 다른 프로그래밍 언어들과 비슷하게 `//` 를 사용해서 주석으로 표시한다.

## 3.5 Control Flow

### `if` 표현식

```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

* `if` 문안에 조건식은 `bool` 타입이어야 한다.
* 너무 많은 `else if`의 코드는 이해하기 어려우므로 Rust에서 `match`라는 분기 생성자를 지원한다. (6장에서 다룬다.)

### `let` 구문에서 `if` 사용하기

* `if`가 표현식이기 때문에 `let` 구문의 우측에 사용할 수 있다.
* 또한 `if`, `else if`, `else` 표현식에서 산출하는 값이 모두 같은 타입이어야 한다.

```rust
fn main() {
    let condition = true;
    let number = if condition {
        5
    } else {
        6
    };

    println!("The value of number is: {}", number);
}
```

### 반복문과 반복

* Rust에서 제공하는 반복문은 크게 세가지다: `loop`, `while`, `for`

#### `loop` 무한반복

* `loop`을 사용해서 무한반복에 빠지게 할 수 있다.

```rust
fn main() {
    loop {
        println!("again!");
    }
}
```

### `while` 조건부반복

```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{}!", number);

        number = number - 1;
    }

    println!("LIFTOFF!!!");
}
```

### `for` 콜렉션반복

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a.iter() {
        println!("the value is: {}", element);
    }
}
```

```rust
fn main() {
    for number in (1..4).rev() {
        println!("{}!", number);
    }
    println!("LIFTOFF!!!");
}
```

#### 요약

* `if`문 조건식 안에는 `bool` 타입이어야 한다.
* `if`문 표현식들 안에는 모두 같은 타입이 있어야 한다.
* `loop`을 사용하면 쉽게 무한반복에 빠뜨릴 수 있다.
* `while`을 사용하면 조건부 반복문을 구현할 수 있다.
* `for`을 사용하면 콜렉션 반복문을 구현할 수 있다.
