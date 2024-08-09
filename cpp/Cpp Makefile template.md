
```Makefile
CXX=g++
CXXFLAGS=-Wall
SOURCES=src/main.cpp
TARGET=bin/main

default: all

all: build

build:
	@if [ ! -d bin ]; then \
		mkdir bin; \
	fi
	$(CXX) -o $(TARGET) $(SOURCES) $(CXXFLAGS);

clean:
	rm -v $(TARGET)
	
```
