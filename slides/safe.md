# Safe

<!--}}}-->

* No null pointers
* Default immutability
* No shared mutable state

<!--vvv-->

## No null pointers

Unlike C there is no way to dereference a null pointer.

```rust
fn main() {
    let x = (); // the unit type, also called nil, is the closest you can get to null
    println(x.to_str());
}
```


<!--vvv-->

## Default immutability

```rust
fn main() {
   let x = 1;
   x = 2;
   println(x.to_str());
}
```

```bash
$ rustc immutable.rs
immutable.rs:3:4: 3:5 error: re-assignment of immutable variable `x`
immutable.rs:3     x = 2;
                   ^
immutable.rs:2:8: 2:9 note: prior assignment occurs here
immutable.rs:2     let x = 1; 
                       ^
error: aborting due to previous error
```

<!--vvv-->

## Default immutability

```rust
fn main() {
   let mut x = 1;
   x = 2;
   println(x.to_str());
}
```

```bash
$ rustc mutable.rs
mutable.rs:2:8: 2:13 warning: value assigned to `x` is never read,
                            #[warn(dead_assignment)] on by default
mutable.rs:2     let mut x = 1; 
                  ^~~~~
$ ./mutable
2
```

<!--vvv-->

## No shared mutable state

```rust
fn main() {
    for num in range(0, 5) {
        let mut x = 1; 
        do spawn {
            x = x + num;
            println(format!("{}", x));
       }
    }
}
```

```bash
$ rustc shared.rs
shared.rs:5:12: 5:13 error: cannot assign to immutable captured outer variable in a heap closure
shared.rs:5             x = x + num;
                        ^
shared.rs:5:16: 5:17 error: mutable variables cannot be implicitly captured
shared.rs:5             x = x + num;
                            ^
error: aborting due to 2 previous errors
```

<!--vvv-->

## No shared mutable state

```rust
fn main() {
    for num in range(0, 5) {
        let x = 1; 
        do spawn {
            println(format!("{}", x + num));
       }
    }
}
```

```bash
$ rustc shared.rs
$ ./shared
4
5
2
1
3
```
