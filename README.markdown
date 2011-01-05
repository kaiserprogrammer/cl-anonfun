cl-anonfun
==========

cl-anonfun is a Common Lisp library that provides a simple anonymous function notation.

Usage
-----

### fn

    (fn &rest body)

`fn` macro returns `lambda` form of `body`. If special symbols starting with `%` are found in `body`, its symbols will be placed to lambda-list of `lambda` form. Special symbol `%` represents its first argument. Special symbol formed `%<n>` represents its `n`'th argument. Special symbol `%&` represents its rest of arguments.

#### Examples

    (macroexpand '(fn * % %))
    ; => #'(LAMBDA (%) (* % %))
    (funcall (fn * % %) 3)
    ; => 9
    (macroexpand '(fn mapcar %2 %1))
    ; => #'(LAMBDA (%1 %2) (mapcar %2 %1))
    (funcall (fn mapcar %2 %1) '(1 2 3) (fn * % %))
    ; => 1 4 9
    (macroexpand '(fn apply #'+ 1 2 3 %&))
    ; => #'(LAMBDA (&REST %&) (APPLY #'+ 1 2 3 %&))
    (funcall (fn apply #'+ 1 2 3 %&) 4 5)
    ; => 15

### enable-fn-syntax

    (enable-fn-syntax)

By calling this function, you can use special syntax `#%(...)` instead of `fn` macro. Any forms of `fn` macro can be used.

#### Examples

    (enable-fn-syntax)
    (funcall #%(* % %) 3)
    ; => 9

License
-------

Copyright (C) 2011  Tomohiro Matsuyama <<tomo@cx4a.org>>.
Licensed under the LLGPL License.