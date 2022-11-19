# COMP7607 Course Project

Per [requirement](./REQUIREMENT.md), this repository contains the code (modified from [princeton-nlp/blindfold-textgame@f0dbf32](https://github.com/princeton-nlp/blindfold-textgame/tree/f0dbf32cb76563982291c51d6db9d6691889c55d)) for a project based on the paper _Reading and Acting while Blindfolded: The Need for Semantics in Text Game Agents_ ("the original paper" hereafter) ([arXiv](https://arxiv.org/abs/2103.13552) | [Website](https://blindfolded.cs.princeton.edu) | [Code](https://github.com/princeton-nlp/blindfold-textgame)).

For instructions on setting up the environment and training models, head to the [instructions section](#instructions) at the end.

Pre-trained models with raw training progress logs are available [here](./raw_results). Aggregated results are presented in the [results section](#results).

## Introduction

The authors of the original paper argued that existing agents (baseline model) utilize little semantic understanding on game texts by demonstrating that a model (hash input model) which only has access to the hashed texts is able to perform as well as, or even better than, the baseline model, with the reasonable assumption that the hashing process removes any available sematics. However, in the original implementation, the [PyTorch RNG](https://pytorch.org/docs/1.13/notes/randomness.html#pytorch-random-number-generator) was used in place of a cryptographically secure hash function. Thus, it remains unclear whether the "hashing" process (or more accurately, RNG seeding process) itself introduced bias that might have affected the results. This project aims to eliminate this uncertainty by replacing the RNG with cryptographically secure hash functions like `SHA-512` and `SHA3-512`, and trying to reproduce the same results as in the original paper.

## Results

Raw training results are presented in the [raw_results folder](./raw_results) and aggregated here. The training has only been done for `zork1`, which has a maximum score of `350`.

| Model       | Steps  | Loss   | Max Score | Final Score |
| ----------- | ------ | ------ | --------- | ----------- |
| `baseline`  | 100000 | 0.8016 | 118       | **25.0**    |
| `torch_rng` | 100000 | 0.2226 | 50        | **30.0**    |
| `sha2`      | 100000 | 0.6563 | 50        | **45.0**    |
| `sha3`      | 100000 | 0.9146 | 117       | **25.0**    |

It looks like all hash-based models are able to perform at least as well as the baseline model, consistent with the results from the original paper.

## Instructions

The code has been tested with Python `3.9.15` on `Ubuntu 22.04.1`. Other Python versions might not work. Notably, the code does not work on Windows since `jericho`, one of the dependencies, [doesn't support Windows](https://github.com/microsoft/jericho/issues/31).

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
