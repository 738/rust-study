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

## 3.3 How Functions Work

## 3.4 Comments

## 3.5 Control Flow



