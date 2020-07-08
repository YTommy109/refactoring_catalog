# Rust 編

できるのか?

```rust
fn fizzbuzz(value: i32) -> String {
    match (value % 3, value % 5) {
        (0, 0)  => String::from("FizzBuzz"),
        (0, _)  => String::from("Fizz"),
        (_, 0)  => String::from("Buzz"),
        _       => value.to_string()
    }
}

fn main() {
    println!("{}", fizzbuzz(15))
}
```
