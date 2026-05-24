# ncurses Minesweeper

Terminal game of Minesweeper, implemented in C with ncurses.

Minesweeper is a logic game where mines are hidden in a grid of squares. The
object is to open all the safe squares in the shortest time possible. Use
the arrow keys to move and <SPACE> to select. Read the Help page for more
information.

Click to watch a video demo on YouTube:
[![Minesweeper demo](http://img.youtube.com/vi/g7InqPoMShA/maxresdefault.jpg)](http://www.youtube.com/watch?v=g7InqPoMShA "Minesweeper demo")

## Coursework context

This repository is used as a coursework project for building a CI/CD pipeline
for containerizing a legacy application.

The original project is a terminal C application that is built with `gcc`
through a `Makefile`. As part of the coursework, the project was extended with:

- a `Dockerfile` for containerizing the application;
- a `.dockerignore` file for excluding unnecessary files from the Docker build context;
- a GitHub Actions workflow for automatic Docker image builds on repository changes.

Original repository:

```text
https://github.com/JoshuaS3/ncurses-minesweeper
```

Fork with coursework changes:

```text
https://github.com/WhiteAnge1/ncurses-minesweeper
```

## Legacy application details

The application can be considered legacy-style in the context of this coursework because:

- it is a terminal application written in C;
- it is built using a traditional `Makefile`;
- the compiler is defined as `gcc`;
- the application depends on the `ncurses` library;
- the project is not originally prepared as a containerized application.

The relevant `Makefile` settings are:

```makefile
CC := gcc -std=c11
CLIBS := -lncurses
```

## Compiling and Linking

Should be functional on all systems with an ncurses library. PDCurses may be
dropped in and linked on Windows, although this hasn't been tested. Might work
on WSL or Cygwin.

Requirements:

```text
build-essential libncurses-dev
```

Compiling and linking:

```bash
make compile build
```

**Binary executable deposited at `bin/minesweeper`.** You can copy this to
`/usr/local/bin/minesweeper` to run the game as `minesweeper` from any location
in your shell (given `/usr/local/bin` is in your path).

If you're contributing source code to this repository, install `clang-format
clang-tidy` and use `make` to target the linter programs. (`clang-format` is
a bit finicky; make sure you're running version 10.0.0 at least, or it will
yell at you about unsupported configuration in `.clang-format`.)

## Docker

The application was containerized with Docker.

The Dockerfile uses a multi-stage build:

1. The first stage installs build dependencies and compiles the application.
2. The second stage contains only the runtime dependency and the compiled binary.

Build the Docker image locally:

```bash
docker build -t legacy-minesweeper:local .
```

Run the container:

```bash
docker run --rm -it legacy-minesweeper:local
```

The container starts the compiled application:

```bash
./minesweeper
```

## CI/CD with GitHub Actions

The repository contains a GitHub Actions workflow:

```text
.github/workflows/docker-build.yml
```

The workflow is triggered by repository changes, including `push` events to the
`master` branch.

The workflow performs the following steps:

1. Checks out the repository.
2. Builds the Docker image.
3. Inspects the resulting Docker image.

This verifies that the legacy C application can be automatically built inside a
Docker container whenever changes are pushed to the repository.

## Program structure

* Entry point at `src/main.c`
* State structs at `src/state.h`
* Rendering logic loop at `src/draw/draw.c`
* Game logic handler (input) at `src/game/game.c`
* Game renderer at `src/draw/game.c`

All header files correspond to a similarly named source file except
`src/draw/pages.h`, which encapsulates multiple sources in the same directory,
and `src/state.h`, which provides struct definitions for game state data.

## TODO

* Rewrite Options screen controls
* Rewrite game board renderer

## Copyright and Licensing

This package is copyrighted by [Joshua 'joshuas3'
Stockin](https://joshstock.in/) and licensed under the [MIT License](LICENSE).

A form of the following should be present in each source or header file.

```txt
ncurses-minesweeper Copyright (c) 2021 Joshua 'joshuas3' Stockin
<https://joshstock.in>
<https://git.joshstock.in/ncurses-minesweeper>

This software is licensed and distributed under the terms of the MIT License.
See the MIT License in the LICENSE file of this project's root folder.

This comment block and its contents, including this disclaimer, MUST be
preserved in all copies or distributions of this software's source.
```

<<https://joshstock.in>> | [josh@joshstock.in](mailto:josh@joshstock.in) | joshuas3#9641