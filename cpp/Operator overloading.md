
```cpp
class X
{
public:
	X& operator += (const X& rhs)
	{
		return *this;
	}

	friend X operator+(X lhs, const X& rhs)
	{
		lhs += rhs;
		return lhs;
	}

	inline bool operator==(const X& lhs, const X& rhs) 
	{ 
		return cmp(lhs,rhs) == 0; 
	}
	inline bool operator!=(const X& lhs, const X& rhs) 
	{ 
		return cmp(lhs,rhs) != 0; 
	}
	inline bool operator< (const X& lhs, const X& rhs) 
	{ 
		return cmp(lhs,rhs) <  0; 
	}
	inline bool operator> (const X& lhs, const X& rhs) 
	{ 
		return cmp(lhs,rhs) >  0; 
	}
	inline bool operator<=(const X& lhs, const X& rhs) 
	{ 
		return cmp(lhs,rhs) <= 0; 
	}
	inline bool operator>=(const X& lhs, const X& rhs) 
	{ 
		return cmp(lhs,rhs) >= 0; 
	}

	std::ostream& operator<<(std::ostream& out, const Fraction& f)
	{
	   return out << f.num() << '/' << f.den();
	}
};
```
