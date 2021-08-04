# jknowrob
A Jupyter Kernel for [KnowRob](https://github.com/knowrob/knowrob).

Inspired by [ayceesrk/jupyter-swi-prolog](https://github.com/kayceesrk/jupyter-swi-prolog)

## Supported environments

Only **python3** is supported

## Installation

1. Install [SWI-Prolog](http://www.swi-prolog.org).
2. Install jknowrob `pip install git+https://github.com/sasjonge/jupyter-knowrob.git`
3. Change directory to your jupyters kernel directory. Typically `~/.local/share/jupyter/kernels`.
4. `mkdir jknowrob && cd jknowrob`
5. Install kernel spec: `wget https://raw.githubusercontent.com/sasjonge/jupyter-knowrob/master/kernel.json`
6. Restart jupyter
7. Profit
