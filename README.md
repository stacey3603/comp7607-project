# COMP7607 Course Project

Code modified from [princeton-nlp/blindfold-textgame@f0dbf32](https://github.com/princeton-nlp/blindfold-textgame/tree/f0dbf32cb76563982291c51d6db9d6691889c55d).

- [Requirement](./REQUIREMENT.md)

- Paper: _Reading and Acting while Blindfolded: The Need for Semantics in Text Game Agents_

  - [arXiv](https://arxiv.org/abs/2103.13552)
  - [Project website](https://blindfolded.cs.princeton.edu)
  - [Code](https://github.com/princeton-nlp/blindfold-textgame)

## Getting started

The code has been tested with Python `3.9.15`. Other Python version might not work.

```console
$ python3 --version
Python 3.9.15
```

### (Optional) Setting up virtual environment

Although not strictly necessary, to avoid clashes with other packages, it's recommended that you set up and activate a [Python virtual environment](https://docs.python.org/3/tutorial/venv.html).

Create and activate a virtual environment in your home folder:

```console
$ python3 -m venv $HOME/nlp
$ source $HOME/nlp/bin/activate
```

### Installing dependencies

Install dependencies with:

```console
$ pip3 install -r ./requirements.txt
```

### Training models

Train the baseline model:

```console
$ python3 train.py
```

Train the "hash input" model from the original paper:

```console
$ python3 train.py --hash_rep 1 --hash_func torch_rng
```

Train the new SHA-2 model (with `SHA-512`) introduced here:

```console
$ python3 train.py --hash_rep 1 --hash_func sha2
```

Train the new SHA-3 model (with `SHA3-512`) introduced here:

```console
$ python3 train.py --hash_rep 1 --hash_func sha3
```
