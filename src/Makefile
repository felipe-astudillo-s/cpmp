# Builds the CPMP solver (FRG and BS-FRG) with the default C++ compiler (Linux and macOS).

CXX ?= c++
CXXFLAGS ?= -O2 -std=c++11

SRCS = main_cpmp.cpp Layout.cpp Greedy.cpp Bsg.cpp
BIN = frg

all: $(BIN)

$(BIN): $(SRCS) Layout.h Greedy.h Bsg.h
	$(CXX) $(CXXFLAGS) $(SRCS) -o $(BIN)

clean:
	rm -f $(BIN)

.PHONY: all clean
