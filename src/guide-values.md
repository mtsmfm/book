# Random values

Now that we have a way of producing random data, how can we convert it to the
type of value we want?

This is a trick question: we need to know both the *range* we want and the type
of *distribution* of this value (which is what the [`next`](guide-dist.md) section
is all about).

## The `Rng` trait

For convenience, all generators automatically implement the [`Rng`] trait,
which provides short-cuts to a few ways of generating values. This has several
convenience functions for producing uniformly distributed values:

-   [`Rng::gen`] generates an unbiased (uniform) random value from a range
    appropriate for the
    type. For integers this is normally the full representable range
    (e.g. from `0u32` to `std::u32::MAX`), for floats this is between 0 and 1,
    and some other types are supported, including arrays and tuples.
    
    This method is a convenience wrapper around the [`Standard`] distribution,
    as documented in the [next section](guide-dist.html#uniform-distributions).
-   [`Rng::gen_range`] generates an unbiased random value with given bounds
    `low` (inclusive) and `high` (exclusive)
-   [`Rng::fill`] and [`Rng::try_fill`] are optimised functions for filling any byte or
    integer slice with random values

It also has convenience functions for producing non-uniform boolean values:

-   [`Rng::gen_bool`] generates a boolean with the given probability
-   [`Rng::gen_ratio`] also generates a boolean, where the probability is defined
    via a fraction

Finally, it has a function to sample from arbitrary distributions:

-   [`Rng::sample`] samples directly from some [distribution](guide-dist.md)

Examples:

```rust
# extern crate rand;
use rand::Rng;
let mut rng = rand::thread_rng();

// an unbiased integer over the entire range:
let i: i32 = rng.gen();

// a uniformly distributed value between 0 and 1:
let x: f64 = rng.gen();

// simulate rolling a die:
let roll = rng.gen_range(1..7);
```

Additionally, the [`random`] function is a short-cut to [`Rng::gen`] on the [`thread_rng`]:
```rust
if rand::random() {
    println!("we got lucky!");
}
```

[`Rng`]: ../rand/rand/trait.Rng.html
[`Rng::gen`]: ../rand/rand/trait.Rng.html#method.gen
[`Rng::gen_range`]: ../rand/rand/trait.Rng.html#method.gen_range
[`Rng::sample`]: ../rand/rand/trait.Rng.html#method.sample
[`Rng::gen_bool`]: ../rand/rand/trait.Rng.html#method.gen_bool
[`Rng::gen_ratio`]: ../rand/rand/trait.Rng.html#method.gen_ratio
[`Rng::fill`]: ../rand/rand/trait.Rng.html#method.fill
[`Rng::try_fill`]: ../rand/rand/trait.Rng.html#method.try_fill
[`random`]: ../rand/rand/fn.random.htm
[`thread_rng`]: ../rand/rand/fn.thread_rng.html
[`Standard`]: ../rand/rand/distributions/struct.Standard.html
