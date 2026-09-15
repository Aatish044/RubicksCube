# Rubicks Cube Solver

A C++ project that explores different Rubik's Cube representations and search algorithms for solving scrambled cubes efficiently. The repository contains multiple cube models, several solver strategies, and a pattern-database-based heuristic to improve search performance.

## Overview

This project focuses on the trade-offs between:

- cube representation design
- search strategy
- memory usage
- heuristic pruning
- performance and implementation complexity

The codebase includes:

- three different cube models:
  - 3D array representation
  - 1D array representation
  - bitboard-based representation
- multiple solvers:
  - DFS
  - BFS
  - IDDFS
  - IDA*
- a corner pattern database for heuristic guidance
- CMake-based build configuration

## Project Structure

```text
RubicksCube/
├── CMakeLists.txt
├── main.cpp
├── README.md
├── Databases/
│   └── cornerDepth5V1.txt
├── Model/
│   ├── RubiksCube.cpp
│   ├── RubiksCube.h
│   ├── RubiksCube1dArray.cpp
│   ├── RubiksCube3dArray.cpp
│   └── RubiksCubeBitboard.cpp
│   └── PatternDatabase/
│       └── PatternDatabase.h
├── PatternDatabases/
│   ├── CornerDBMaker.cpp
│   ├── CornerDBMaker.h
│   ├── CornerPatternDatabase.cpp
│   ├── CornerPatternDatabase.h
│   ├── NibbleArray.cpp
│   ├── NibbleArray.h
│   ├── PatternDatabase.cpp
│   ├── PatternDatabase.h
│   ├── PermutationIndexer.h
│   ├── math.cpp
│   └── math.h
├── Solver/
    ├── BFSSolver.h
    ├── DFSSolver.h
    ├── IDAstarSolver.h
    └── IDDFSSolver.h

```

## Features

- Multiple cube encoding styles for comparison and experimentation
- Random scrambling support
- Move generation and move inversion
- Solver benchmarking with different strategies
- Pattern database support for corner states
- IDA* search using heuristic pruning
- Compact bitboard representation for faster state handling

## Solver Design

This project compares multiple ways to solve a Rubik's Cube by combining different cube representations with search strategies.

### DFS
Depth-first search explores one move path deeply before backtracking. It is straightforward to implement and useful for understanding the cube state space, but it becomes inefficient for deeper scrambles because the branching factor grows quickly.

### BFS
Breadth-first search guarantees the shortest solution in terms of move count, but it stores a very large number of states in memory. This makes it useful as a reference algorithm, though not practical for larger depth searches.

### IDDFS
Iterative deepening depth-first search avoids the memory blow-up of BFS while still searching in increasing depth limits. It is a good middle ground when you want an exact solver without the full memory cost of breadth-first exploration.

### IDA*
Iterative Deepening A* is the most important solver in this project. Instead of expanding every state blindly, it uses a heuristic to estimate how far a cube is from being solved. The corner-pattern database gives an admissible estimate based on the corner configuration, allowing the solver to focus on promising branches and prune inefficient ones.

This is the key idea behind the project: combine a compact cube representation with informed search so the solver can handle realistic scramble depths much more efficiently than naive exhaustive search.

## Pattern Database

The project includes a corner pattern database implementation designed to estimate how many moves are needed to solve the cube's corner configuration. This heuristic helps the IDA* solver decide which branches are worth exploring and which can be skipped. In other words, the solver does not just search blindly; it uses precomputed knowledge about corner states to guide the decision-making process.

## Build Instructions

This project uses CMake.

### Windows (PowerShell)

```powershell
cmake -S . -B build
cmake --build build --config Release
.\build\Release\rubiks_cube_solver.exe
```

### Linux / macOS

```bash
cmake -S . -B build
cmake --build build
./build/rubiks_cube_solver
```

## Running the Project

The example execution is driven from `main.cpp`. The file contains test code and sample usage for:

- cube creation
- cube shuffling
- solver execution
- database loading
- IDA* solving

## Important Note About the Database

The current `main.cpp` uses a hardcoded database path:

```cpp
string fileName = "C:\\Users\\user\\CLionProjects\\rubiks-cube-solver\\Databases\\cornerDepth5V1.txt";
```

This path is project-specific and may not exist on your machine. If you want to run the database-backed solver locally, update that path to the correct location of `Databases/cornerDepth5V1.txt` in your environment.

## Example Usage

After building the project, you can run the executable and test the cube solvers through the code in `main.cpp`. The project is designed more as a research/algorithmic implementation than a polished end-user CLI.

## Current Status

This repository is best described as a prototype / research project:

- solver logic is implemented
- multiple cube representations are included
- pattern-database support exists
- build setup is provided via CMake
- the project is not yet a production-ready GUI or CLI tool

## Future Improvements

Potential next steps include:

- adding a proper command-line interface
- supporting configurable database paths cleanly
- creating automated tests
- benchmarking each solver representation
- improving database generation and loading reliability
- adding documentation for algorithm trade-offs and complexity

---

Built as a Rubik's Cube algorithm and optimization experiment in C++.

