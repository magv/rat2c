# RAT2C

rat2c converts one or multiple rational expressions (given as
files) into a single C function, optimizing their evaluation
via [FORM] as described in [arXiv:1310.7007] and [arXiv:1707.06453],
and merging common subexpressions. The resulting C code is
printed to the standard output.

The input files can contain arbitrary arithmentic expressions
using operators `+`, `-`, `*`, `/`, and `^`, and function
calls given as `name(expr1,expr2,...)`. Variable and function
names should match `[a-zA-Z_][a-zA-Z0-9_]*`.

Each part of the expression enclosed into parentheses is
optimized separately via FORM, so fewer parentheses is
better.

The power operation will be formatted as `pow(x,n)` in the
output. Fractional numbers will become `quo(num,den)`. These
two functions should be appropriately defined by the user.

[FORM]: https://www.nikhef.nl/~form/maindir/maindir.html
[arXiv:1310.7007]: https://arxiv.org/abs/1310.7007
[arXiv:1707.06453]: https://arxiv.org/abs/1707.06453

## Usage

`rat2c` [`-a` *names*] [`-f` *name*] [`-t` *type*] [`-O` *level*] *file* *...*

## Options

* `-a` *names*

    Use this list of variables as the function argument.

* `-f` *name*

    Use this function name (default: `function`).

* `-t` *type*

    Use this type for variables (default: `double`).

* `-O` *level*

    Use this FORM optimization level (default: `4`).

## Example

    $ echo 'x+y*x^2+z*x^3' | rat2c -
    void
    function(
        double *result,
        const double x,
        const double y,
        const double z)
    {
        double tmp1 = x*z;
        double tmp2 = y+tmp1;
        tmp1 = x*tmp2;
        tmp2 = 1+tmp1;
        result[0] = x*tmp2;
    }