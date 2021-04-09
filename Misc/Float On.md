# angstromCTF 2021

* **Category:**  Misc
* **Points:** 130pts
* **Author:** [Movement](https://github.com/MovementGH)

## Challenge

>I cast my int into a double the other day, well nothing crashed, sometimes life's okay.
>
>We'll all float on, anyway: float_on.c.
>
>Float on over to /problems/2021/float_on on the shell server, or connect with nc shell.actf.co 21399.
>
>Navigate to the Discord Server and take a look at the banner for #general. There's the flag!

This challenge provides source code. If we look at the source code we see there are multiple stages, each of which takes input from the user to a variable `x` and checks some condition. The only tricky part is that `x` is taken as a uint_64 but tested as a double. It is not converted, however, it is done using a union. This means that the bits will not change, it will just be interpreted differently.

This means that we first need to find a double which works for each condition, and then find the uint64 which has the same bits as that double.

There are 5 stages:
```cpp
DO_STAGE(1, x == -x);
DO_STAGE(2, x != x);
DO_STAGE(3, x + 1 == x && x * 2 == x);
DO_STAGE(4, x + 1 == x && x * 2 != x);
DO_STAGE(5, (1 + x) - 1 != 1 + (x - 1))
```

The first stage is simple. 0 == -0, so if we set the double to 0, we will pass this stage

The second stage requires `x` to not equal `x`. This happens if our double is equal to `NaN`. We can accomplish this by setting the uint to `-1`

The third stage requires both `x+1` and `x*2` to be equal to `x`. To accomplish this, we will take advantage of the `Infinity` state of floating point numbers. If we use a bit calculator to find the bits of a double, and convert that to a uint64, we will find that `16106127360000000000` causes this condition to pass.

The fourth stage requires `x+1` to equal `x`, but `x*2` to NOT equal `x`. To do this, we will make a large enough double that the precision does not extend to the ones digit. This means we can still multiply it by two and have a larger number, but we will not be able to add one. There are a lot of numbers which fit this condition, `9218868437227405312` is one of them.

The fifth and final stage requires `(1+x)-1` to not equal `1+(x-1)`. To accomplish this we will do a similar thing as stage 4, except we have to find the border of where rounding occurs. Basically, we want `x+1` to equal `x` because of rounding, but to be low enough that `x-1` will equal `x-1`. This means that `(1+x)-1` will equal `x-1`, but `1+(x-1)` will equal `x`. `9223372036854775807` works for this stage.