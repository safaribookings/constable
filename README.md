# constable

One decorator for lazy debugging. Supports **3.8+**

- Capture function args and result

- Monitor variable state at each assignment op

- Measure execution time

- Any of the above can be turned on/off.


```sh
$ pip install constable
```


:exclamation: **Do not use** (use at your own risk) it in mission critical environments. The code relies on modifying the AST to insert prints and using `exec`. 


### Monitoring functions

```python

import constable

@constable.trace()
def add(a=1, b=2):
    return a + b

add(1, 2)

```

Output - 

```
executing: add(a = 1, b = 2)
execution time: add(a = 1, b = 2) -> 0.00014877 seconds
```

### Tracing variable assignments 

```python

import constable

@constable.trace(['a', 'b'])
def do_something(a=5, b=6):
    a = a + b
    c = a
    a = "Experimenting with the AST"
    b = c + b
    a = c + b
    return a

do_something(1, 2)

```

Output -

```
executing: do_something(a = 1, b = 2)
debug: do_something: a = 3
debug: do_something: a = Experimenting with the AST
debug: do_something: b = 5
debug: do_something: a = 8
execution time: do_something(a = 1, b = 2) -> 0.00009584 seconds
```

### API

The `trace` function is the decorator to add prints to the AST.

```python

def trace(
    variables=None,
    args=True,
    result=False,
    max_len=None,
    verbose=False,
    time=True,
    use_spaces=False
):
    """
    An experimental decorator for tracing function execution using AST.

    Args:
        variables (list, optional): Variables to trace. Traces all if None. Default is None.
        args (bool, optional): Whether to print function arguments. Default is True.
        result (bool, optional): Whether to print function return value. Default is False.
        max_len (int, optional): Max length of printed values. Truncates if exceeded. Default is None.
        verbose (bool, optional): Whether to print detailed trace info. Default is False.
        time (bool, optional): Whether to print execution time. Default is True.
        use_spaces (bool, optional): Whether to add empty lines for readability. Default is False.

    Returns:
        function: Decorator for function tracing.
    """

```
<hr>
