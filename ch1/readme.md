# Ch. 1 Getting Started

> 다음 문서는 맥 기준으로 작성되었습니다.

## 설치

### `rustup` 설치
```bash
$ curl https://sh.rustup.rs -sSf | sh
```

### 환경변수 설정

- 파일의 위치는 ~/.bash_profile
- oh-my-zsh 유저라면 ~/.zshrc

```bash
export PATH="$HOME/.cargo/bin:$PATH"
```

### 문서 보기

- 다음 명령어를 통해 로컬에서 `rust` 관련 문서를 참고할 수 있다.

```bash
$ rustup doc
```

## Hello world 찍기

### main.rs

- `rust` 소스코드 파일의 확장명은 `.rs`이다.

```rust
fn main() {
    println!("Hello, world!");
}
```

### 컴파일 및 실행

```bash
$ rustc main.rs
$ ./main
Hello, world!
```

### 코드 분석

- `main` 함수는 `rust` 프로그램의 실행 시작 함수이다. 요청 인자와 응답 인자 모두 없다.
- `rust`의 들여쓰기 스타일은 탭이 아니라 4번의 스페이스이다.
- `println!`은 `rust macro`라고 부른다. `!`에 대한 내용은 19장에 나온다. 지금 알아야할 것은 `!`가 함수가 아니라 매크로를 호출한다는 것이다.
- `"Hello, world!"`를 `println!`의 요청인자로 넘겼고 화면에 `"Hello, world!"`가 뿌려졌다.
- 줄의 끝은 `;`으로 끝난다.

## Cargo

- `cargo`는 `rust`의 빌드 시스템이고 패키지 매니저이다.
- `cargo`는 코드를 빌드하고 라이브러리를 설치하는데 사용된다.