# Concurrent

<!--}}}-->

* Lightweight tasks
* No shared mutable state
* Fast asynchronus messaging

<!--vvv-->

## Lightweight tasks
## No shared mutable state

We saw this previously:

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

<!--vvv-->

## Fast asynchronus messaging

```rust
extern mod extra;
use extra::comm::DuplexStream;

fn do_task(channel: &DuplexStream<int, int>) {
   let mut value = 0;
   loop {
          value = channel.recv();
          if(value == -1) { break; }
          value += 1;
          channel.send(value);
   }
}

fn main() {
let(from, to) = DuplexStream();

   do spawn {
      do_task(&to);
   }

    do spawn {
        for num in range(0, 5) {
            from.send(num);
            println(from.recv().to_str());
        }
       from.send(-1);
    }
    println("Tasks spawned!");
}
```

