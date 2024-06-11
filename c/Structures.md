
```c
typedef struct SomeStructure {
// some data
} SomeStructure;

int main() {
	SomeStructure *s1;
	return 0;
}
```

```c
struct SomeStructure {
// some data
};

int main() {
	struct SomeStructure s1;
	return 0;
}
```

# Ternary operator

```c
int a = 10, b = 20, c;
c = (a < b) ? a : b;
```

