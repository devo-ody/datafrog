# datafrog

Datafrog is a lightweight Datalog engine intended to be embedded in other Rust programs.

Datafrog has no runtime, and relies on you to build and repeatedly apply the update rules.
It tries to help you do this correctly. As an example, here is how you might write a reachability
query using Datafrog (minus the part where we populate the `nodes` and `edges` initial relations).

```rust
extern crate datafrog;
use datafrog::Iteration;

fn main() {

    // Create a new iteration context, ...
    let mut iteration = Iteration::new();

    // .. some variables, ..
    let nodes_var = iteration.variable::<(u32,u32)>("nodes");
    let edges_var = iteration.variable::<(u32,u32)>("edges");

    // .. load them with some initial values, ..
    nodes_var.insert(nodes.into());
    edges_var.insert(edges.into());

    // .. and then start iterating rules!
    while iteration.changed() {
        // nodes(a,c) <-  nodes(a,b), edges(b,c)
        nodes_var.from_join(&nodes_var, &edges_var, |_b, &a, &c| (c,a));
    }

    // extract a `Vec<(u32,u32)>` containing the final results.
    let reachable = variable.complete();
}
```