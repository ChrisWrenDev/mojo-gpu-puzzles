# Mojo GPU Puzzles

This repository contains my solutions, experiments, and notes while working through the **Mojo GPU Puzzles**. The goal of this project is to deepen my understanding of GPU programming concepts using the **Mojo programming language** and its GPU capabilities.

## ▚ About Mojo GPU Puzzles

The Mojo GPU Puzzles are a hands-on way to learn:

- GPU execution models (threads, blocks, grids)
- Memory hierarchy (global, shared, registers)
- Parallel patterns (map, reduce, scan, stencil, etc.)
- Performance optimization techniques
- Warp-level primitives and synchronization
- Kernel design and launch configuration

Each puzzle focuses on implementing a small but fundamental GPU pattern correctly and efficiently.

- [https://docs.modular.com/mojo](https://docs.modular.com/mojo/manual/)
- [https://puzzles.modular.com](https://puzzles.modular.com/introduction.html)
- [https://github.com/modular/mojo-gpu-puzzles](https://github.com/modular/mojo-gpu-puzzles)

## ▚ Repository Structure

```text
.
├── puzzles/
│ ├── 01*\*.mojo
│ ├── 02*\*.mojo
│ ├── ...
│
├── notes/
│ ├── puzzle_01.md
│ ├── puzzle_02.md
│ ├── ...
│
└── README.md
```

- `puzzles/` — My Mojo implementations for each puzzle.
- `notes/` — Explanations, performance observations, and lessons learned.
- `README.md` — Project overview (this file).

## ▚ Learning Goals

Through these puzzles, I aim to:

- Understand how GPUs execute parallel workloads.
- Develop intuition for memory coalescing and access patterns.
- Learn to reason about occupancy and throughput.
- Compare CPU vs GPU mental models.
- Build a foundation for high-performance ML and scientific computing.

## ▚ Notes & Reflections

Each puzzle has a corresponding markdown note in the notes/ directory where I document:

- The core idea behind the puzzle
- My implementation approach
- Bugs encountered
- Optimization strategies
- Key takeaways

## ▚ Requirements

- Latest **Mojo** toolchain with GPU support.
- Compatible GPU ([supported GPUs](https://docs.modular.com/max/packages/#gpu-compatibility)).

## ▚ Running a Puzzle

Example:

```sh
# Test solutions on GPU
pixi run tests
# Or a specific puzzle
pixi run tests pXX
# Or manually
pixi run mojo/python solutions/pXX/pXX.{mojo,py}

# Run GPU sanitizers for debugging on NVIDIA GPUs using `compute-sanitizer`
pixi run memcheck  <optional pXX>    # Detect memory errors
pixi run racecheck <optional pXX>    # Detect race conditions
pixi run synccheck <optional pXX>    # Detect synchronization errors
pixi run initcheck <optional pXX>    # Detect uninitialized memory access
# Or run all sanitizer tools
pixi run sanitizers pXX
# Or manually
# Note: ignore the mojo runtime error collision with the sanitizer. Look for `Error SUMMARY`
pixi run compute-sanitizer --tool {memcheck,racecheck,synccheck,initcheck} mojo solutions/pXX/pXX.mojo

# Format code
pixi run format
```
