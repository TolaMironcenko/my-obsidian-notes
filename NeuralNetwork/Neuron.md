
## Neuron sigmoid function

$$ e = 2.71828 $$

$$ f(x) = {1 \over 1 + {e^{-x}}} $$

## Neuron struct in c++

```cpp
#include <iostream>
#include <cmath>

struct Neuron {
	double value;
	double error;
	double e = 2.71828;
	void act() {
		value = (1 / (1 + exp(-value)));
	}
};

int main() {
	Neuron *n1 = new Neuron;
	n1->value = 1234567890;
	n1->act();
	printf("neuron double value = %f\n", n1->value);
	return 0;
}

```

## Neuron struct in c

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

struct Neuron {
	double value;
	double error;
};

void Neuron_act(struct Neuron *n) {
	n->value = (1 / (1 + exp(-n->value)));
}

int main () {
	struct Neuron *n1 = malloc(sizeof *n1);
	n1->value = 10;
	Neuron_act(n1);
	printf("neuron double value = %f\n", n1->value);
}
```

