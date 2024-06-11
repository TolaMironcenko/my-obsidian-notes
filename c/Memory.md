
# Malloc()

```c
int *ptr;
int n = 10;
ptr = (int*)malloc(n * sizeof(int));
```

# Calloc()

```c
int *ptr;
int n = 10;
ptr = (int*)calloc(n, sizeof(int));
```

# Realloc()

```c
int *ptr;
int n = 10;
ptr = (int*)calloc(n, sizeof(int));
n = 20;
ptr = (int*)realloc(ptr, n * sizeof(int));
```

# Free()
```c
free(ptr);
```

