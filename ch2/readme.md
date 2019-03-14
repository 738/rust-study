# Ch 2. Programming a Guessing Game

## 새 프로젝트 생성

```bash
$ cargo new guessing_game
$ cd guessing_game
```

## 추측하기 구현

```rust
// src/main.rs
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin().read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

### `use`로 라이브러리 가져오기

* 기본적으로 `rust`는 [the prelude](https://doc.rust-lang.org/std/prelude/index.html) 안의 모든 프로그램의 스코프에 단지 몇 개의 타입만 지원한다. 사용하고자 하는 타입이 **the prelude**에 없다면 `use` 문장을 사용해서 명시적으로 타입을 가져와야 한다.
* `std::io` 라이브러리는 사용자의 입력을 받는 것 등 많은 유용한 기능을 제공한다.

### 변수에 값 저장하기

```rust
let foo = 5; // immutable
let mut bar = 5; // mutable
```

* 기본적으로 `let`은 상수, `mut`을 붙이면 변할 수 있다.

```rust
let mut guess = String::new();
```

* `String::new`는 `String` 타입의 새로운 인스턴스를 반환하는 함수이다.
* `::new`의 `::` 문법은 `String` 타입에 관련되어 있는 함수(associated function)라는 뜻이다. associtated function는 타입 위에 구현되어있다. 이 경우에는 `String` 객체가 아니라 `String` 자체에. 몇몇 언어들은 static method라고도 부른다.
* `new` 함수는 새로운 빈 `String`을 생성한다.
* `let mut guess = String::new();` 라인은 새로운 빈 `String` 인스턴스와 연결된 가변변수를 생성한다.

### `Result` 타입으로 잠재된 실패 다루기

* `io::Result` 인스턴스는 `expect`라는 메소드를 가지고 있다. `io::Result` 인스턴스의 값이 `Err`일 경우 `expect` 메소드는 프로그램의 작동을 멈추게 하고 메시지를 출력하도록 한다.

### `println!` 변경자 (placeholder)를 이용한 값 출력

```rs
println!("You guessed: {}", guess);
```

* `{}`는 변경자로써 값이 표시되는 위치를 나타낸다.

## 비밀번호 생성

### 크레이트(Crate)를 사용하여 모듈 가져오기

* `rand` 코드를 사용하기 전에 `Cargo.toml`의 의존 리스트에 `rand` 크레이트를 추가한다.
* Cargo는 버전을 명시하는 표준에 해당하는 [Semantic Versioning(semver)](https://semver.org/)을 이용한다.
* Cargo는 [Crates.io](https://crates.io/) 데이터의 복사본인 레지스트리(registry)에서 모든 것들을 가져온다. Crates.io는 Rust 생태계의 개발자들이 다른 사람들도 이용할 수 있도록 Rust 오픈소스를 공개하는 곳이다.

```toml
[dependencies]

rand = "0.3.14"
```

### 임의의 숫자 생성

## 비밀번호와 추리값 비교

## 반복문을 이용하여 여러 번의 추리 허용

### 정답 이후에 종료하기

### 잘못된 입력값 처리하기

#### 요약
