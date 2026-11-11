# A constructive heuristic for the Container Pre-marshalling Problem (CPMP)

Implementation of a Filling-Emptying Greedy algorithm (**FEG**) for the CPMP. It is based on two procedures: (1) *filling* ordered stacks by moving directly badly placed containers into ordered stacks such that the containers become well placed and *emptying* one small stack, by moving its elements into other stacks, in order to have more available slots for well placing containers.


### Compilation

Enter to the `src` folder and compile the C++ code:

````
g++ Greedy.cpp Layout.cpp Bsg.cpp main_cpmp.cpp -o feg -std=c++11
````

### Execution

The command of exceution is:
````
.\feg H INSTANCE nb [--GreedyVariant] [--BSvariant]
````

where:
* `H` is the maximum height of stacks
* `nb` is the number of beams of the Beam Search strategy. If `nb=0`, then only the greedy algorithm is applied
* `GreedyVariant` (`FEG` or `FERG`). Correspond to the subyacent greedy algorithm variant.
* `BSvariant` (`single_moves` or `compound_moves`). Correspond to the beam search variant.


Example:
````
.\feg 12 ..\Instancias\CVS\10-6\data10-6-10.dat 0 --FERG
120 0.001
````

The output of the method (`120 0.001` in the example) corresponds to the minimum number of steps found by the strategy and the spend CPU time in seconds.

### Solving random instances

The algorithm also allows one to generate and solve random instances. The command is:
````
.\feg H --random S N nb [--GreedyVariant] [--BSvariant]
````
where `S` is the number of stacks and `N`, the number of containers.

Example:
````
.\feg 5 --random 3 6 1000 --FEG --compound_moves

Generating a random instance: S=3, N=6, H=5
[59 ]
[35 ]
[70 31 49 27 ]
2       0
State-Action 2
[59 ]
[35 27 ]
[70 31 49 ]
2,0
State-Action 1
[59 ]
[35 ]
[70 31 49 27 ]
2,1
````


---

## Notes on this fork

This fork of [rilianx/cpmp](https://github.com/rilianx/cpmp) is the version of the solver used for the
FRG and BS-FRG baselines of the paper *"A Size-Independent Self-Attention Model for the Container
Pre-Marshalling Problem"* ([OscarRojasG/CPMP-Framework](https://github.com/OscarRojasG/CPMP-Framework)).
It differs from the original (commit `a495076`) only in `src/main_cpmp.cpp`:

- `k`, the number of destination stacks tried for each single move in the beam search, is read from the
  command line again (it was fixed to 0);
- the optional last argument `--moves` prints the sequence of moves of the solution.

`Bsg.*`, `Greedy.*` and `Layout.*` are unchanged. A `Makefile` was added.

### Build

```bash
cd src
make            # produces ./frg (Linux and macOS)
```

### Command line

The command line of the current code is the one below, not the one described in the sections above:

```
./frg H instance a k max_steps beams <flag> seed [--moves]
```

- `H`: maximum height of the bay.
- `a`: parameter of the heuristic; `k`: number of destinations considered for single moves.
- `max_steps`: step limit of the greedy.
- `beams`: 0 for the greedy (FRG); number of candidates for the beam search (BS-FRG).
- `<flag>`: `--compound`, `--mixed` or `--no-assignment`. A flag must always be given, also with `beams = 0`.
- `seed`: seed of the random tie-breaking.

The output is the number of moves and the CPU time in seconds; with `--moves`, a second line
`MOVES o,d o,d ...` lists the moves (origin and destination stack, 0-indexed).

Settings of the paper, on an instance of the CV group 3-3 (`H = 5`):

```bash
./frg 5 ../Instancias/CVS/3-3/data3-3-1.dat 1.2 2 100 0 --compound 2        # FRG
./frg 5 ../Instancias/CVS/3-3/data3-3-1.dat 1.2 3 100 1000 --mixed 2        # BS-FRG, 1,000 candidates
./frg 5 ../Instancias/CVS/3-3/data3-3-1.dat 1.2 3 100 10000 --mixed 2       # BS-FRG, 10,000 candidates
```
