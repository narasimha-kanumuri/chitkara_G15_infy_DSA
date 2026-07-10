# Recursion Programs

```c
int fun(int x) {
    if (x > 1) {
        fun(x / 2);
        fun (x / 2);
    }
    printf("%d ", x);
    return 0;
}
fun(4);
```

```c
int fun(int x){
    if (x > 100) {
        return x - 10; ;
    }
    return fun(fun( x + 11));
}

printf("%d", fun(95));
```
