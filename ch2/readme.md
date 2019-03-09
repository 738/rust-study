# Ch. 2 Programming a Guessing Game

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

