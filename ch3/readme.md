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

### 문자 타입

* Rust의 `char`는 가장 근본적인 알파벳 타입이다.
* String이 큰 따옴표를 쓰는 것에 반해 `char` 타입은 작은 따옴표를 쓴다.

```rust
fn main() {
   let c = 'z';
   let z = 'ℤ';
   let heart_eyed_cat = '😻';
}
```

## 3.3 How Functions Work

## 3.4 Comments

## 3.5 Control Flow



