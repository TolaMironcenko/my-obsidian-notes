
# Func

```cpp
template <typename T> T myMax(T x, T y) {
	return (x > y) ? x : y
}
```

# Class

```cpp
template <typename T> class Array {
private:
	T* ptr;
	int size;
public:
	Array(T arr[], int s);
	void print();
};
```

```cpp
template <class T, class U> class A {
    T x;
    U y;

public:
    A() { cout << "Constructor Called" << endl; }
};
```

# boost::mpl::bool_<>

```cpp
boost::mpl::bool_<true>
std::integral_constant<bool, true>
std::bool_constant<true>
```

# boost::enable_if<>

```cpp
boost::enable_if<typename boost::mpl::bool_<true>>::type
std::enable_if<std::integral_constant<bool, true>::value>::type
```
