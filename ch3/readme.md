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

## 3.2 Data Types

## 3.3 How Functions Work

## 3.4 Comments

## 3.5 Control Flow



