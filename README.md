# Tutorial Algebraic Cryptanalysis

This repo is made for the tutorial "Where did my RAM go? Using algebraic cryptanalysis in practice" in the Croatia summer school 2025.
In this repo you can find the slides for the guiding presentations as well as the exercises that accompany the tutorial.
This tutorial aims to give students a starting understanding of algebraic cryptanalysis, the common methods used therein, and how to estimate the complexity of such attacks.


## Installation

Follow the installation steps for Conda at [Miniforge](https://github.com/conda-forge/miniforge)


Run the following command to create a virtual environment containing jupyter, sage and python.
```console
conda create -n summer-school python=3.12 sage jupyter
```

Activate the environment using
```console
conda activate summer-school
```

Now you can open jupyter lab to edit the code in the browser using
```console
jupyter lab
```
and clicking the link in the terminal output.

Alternatively you can link your favorite IDE to this python environment