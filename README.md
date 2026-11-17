# Neural Networks Things · Computational Classnotes

Wolfram Language notebooks on the mathematics of neural networks, from S-shaped activation functions and universal approximation to feedforward networks trained by hand and recurrent layers built from scratch, with Python editions of the S Shape Functions notebooks and the BasicRecurrentLayer tutorial. Each notebook is self-contained: it explains the ideas, states the results, and checks every formula with code you can run.

## Contents

### [S Shape Functions Theory](s-shape-functions-theory/)

*Sigmoidal functions, Cybenko's universal approximation theorem, and Kolmogorov's superposition theorem.*
Dara O Shayda · Noor Hoshino

S-shaped functions rise from 0 to 1 around a threshold. The notebook follows them from biology to neural networks:

- six biological S-curves: ion-channel gating, hemoglobin, a fruit-fly gene switch, diffusion fronts, population growth and visual neurons;
- Cybenko's universal approximation theorem and how its proof works;
- worked demonstrations in one and two dimensions;
- Kolmogorov's superposition theorem, the answer to Hilbert's 13th problem, and how it relates to Cybenko's;
- an appendix on continuity.

| File | What it is |
|---|---|
| [`S-Shape-Functions-Theory.nb`](s-shape-functions-theory/S-Shape-Functions-Theory.nb) | The Wolfram notebook (Wolfram 15) |
| [`S-Shape-Functions-Theory.pdf`](s-shape-functions-theory/S-Shape-Functions-Theory.pdf) | Printed version, A4 |
| [`S-Shape-Functions-Theory.md`](s-shape-functions-theory/S-Shape-Functions-Theory.md) | Markdown edition with LaTeX equations and figures |
| [`S-Shape-Functions-Theory.ipynb`](s-shape-functions-theory/S-Shape-Functions-Theory.ipynb) | Python edition, a Jupyter notebook (Python 3.14), evaluated |

- **Notebook online:** https://www.wolframcloud.com/obj/ccn2/Published/S_Shape_Functions_Theory.nb
- **CCN Node:** https://discuss.compclassnotes.com/t/s-shape-functions-theory/522
- **CCN Wiki:** https://wiki.compclassnotes.com/books/neural-networks/page/s-shape-functions-theory

#### Solved exercises

The seven exercises of section 8, each stated and solved with derivations, numerical checks and plots: the staircase with arctan and algebraic sigmoids, a non-monotone sigmoid, the polynomial activation t², x y z as a Kolmogorov superposition, BasicRecurrentLayer as a Cybenko sum, gradients with respect to the weights and training with them, and convergence rates predicted from the modulus of continuity.

| File | What it is |
|---|---|
| [`S-Shape-Functions-Solved-Exercises.nb`](s-shape-functions-theory/S-Shape-Functions-Solved-Exercises.nb) | The Wolfram notebook, evaluated (Wolfram 15) |
| [`S-Shape-Functions-Solved-Exercises.pdf`](s-shape-functions-theory/S-Shape-Functions-Solved-Exercises.pdf) | Printed version, A4 |
| [`S-Shape-Functions-Solved-Exercises.md`](s-shape-functions-theory/S-Shape-Functions-Solved-Exercises.md) | Markdown edition with LaTeX equations, tables and the Wolfram figures |
| [`S-Shape-Functions-Solved-Exercises.ipynb`](s-shape-functions-theory/S-Shape-Functions-Solved-Exercises.ipynb) | Python edition, a Jupyter notebook (Python 3.14, PyTorch for Problem 5), evaluated |

### [Feedforward Neural Networks](feedforward-neural-networks/)

*The algebra and the computing: a first network built from ordinary functions.*
Dara O Shayda · Noor Hoshino

A feedforward network is a chain of simple steps: multiply by a matrix, add a vector, apply an S-shaped function, repeat. The notebook builds one step by step and shows both the algebra and the computing:

- one learning step by hand on the smallest network with every part, with every number written out;
- one neuron computing AND and OR; a layer as one matrix product; XOR and why it needs a hidden layer;
- losses, backpropagation (derived, and checked against `D` and finite differences) and gradient descent, all in matrix form with the actual numbers of each example;
- training shown with Manipulates and animations: XOR, Celsius to Fahrenheit, a 4-8-1 network that learns to balance a stick on a moving cart, and a 1-6-1 network that learns the force law of a pendulum and keeps it swinging;
- the same networks with `NetChain` and `NetTrain`, and a diagram of every network with its trained weights.

| File | What it is |
|---|---|
| [`Feedforward-Neural-Networks.nb`](feedforward-neural-networks/Feedforward-Neural-Networks.nb) | The Wolfram notebook, evaluated (Wolfram 15); animations and Manipulates are shown as one frame until their cells are run |
| [`Feedforward-Neural-Networks.pdf`](feedforward-neural-networks/Feedforward-Neural-Networks.pdf) | Printed version, A4 |
| [`Feedforward-Neural-Networks.md`](feedforward-neural-networks/Feedforward-Neural-Networks.md) | Markdown edition with LaTeX equations and the Wolfram outputs |
| [`videos/`](feedforward-neural-networks/videos/) | The animations as MP4 videos, and a recording of the 3D Unity 6 version of the cart-pole and the learned pendulum |

- **Unity 6 version:** the cart-pole and the learned pendulum in 3D, with the notebook's trained networks and training inside Unity: https://github.com/lossofgenerality/neural-networks-unity
- **Unity 6 video (4K, YouTube):** https://youtu.be/lIUtEO9m79M

### [BasicRecurrentLayer](basic-recurrent-layer/)

| File | What it is |
|---|---|
| [`BasicRecurrentLayer-Tutorial.nb`](basic-recurrent-layer/BasicRecurrentLayer-Tutorial.nb) | A short tutorial on Wolfram's built-in `BasicRecurrentLayer`: the recurrence, checking the layer by hand, stateful evaluation, and a small sequence classifier |
| [`BasicRecurrentLayer-Tutorial.ipynb`](basic-recurrent-layer/BasicRecurrentLayer-Tutorial.ipynb) | Python edition of the tutorial, a Jupyter notebook (Python 3.14, PyTorch `nn.RNN`), evaluated |
| [`BasicRecurrentLayer-FromScratch.nb`](basic-recurrent-layer/BasicRecurrentLayer-FromScratch.nb) | The same layer built from ordinary math functions: the full matrix form of the recurrence, a check against the built-in layer, backpropagation through time derived and checked with `D`, and training by hand-written gradient descent |

## Using the notebooks

- Open them in Mathematica or Wolfram Desktop, version 14 or later; S Shape Functions Theory was saved with Wolfram 15.
- Evaluate from the top. In S Shape Functions Theory, run the settings cell **c.0** first: it sets the font sizes and figure widths used by every figure and table. In the solved exercises, run **c.0** and **c.0.1** first; after that each problem runs on its own.
- Every code cell starts with an index comment such as `(*c.7*)`, so cells can be referred to by number.
- Each notebook keeps its variables in a private context, so notebooks open in the same session do not interfere with each other.
- The small organism photos in section 0 of S Shape Functions Theory are loaded from Wikimedia Commons when their cells run, so those cells need an internet connection.

## Using the Python editions

Current version of the Python editions: **1.0.0** (16 November 2026).

The `.ipynb` notebooks are Python ports of the Wolfram notebooks, with the same text, the same numbering of sections, equations, figures, tables and theorems, and the same cell indices, written as `# c.7`. They use only free, open-source software: NumPy, SciPy, SymPy, Matplotlib, NetworkX, ipywidgets, Pillow, requests and PyTorch. GitHub shows them with their outputs; there are two ways to run them.

### In the browser, with nothing to install

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/lossofgenerality/neural-networks-things?quickstart=1)

Click the button and then **Create codespace**. GitHub starts a small Linux machine with Python 3.14 and the libraries of `uv.lock`; the first start takes a few minutes. The main notebook opens by itself: click **Run All** at the top of the notebook, and if you are asked for a kernel, choose `.venv`. Personal GitHub accounts include a free monthly allowance of Codespaces time, far more than these notebooks need. Stop the codespace when you are done (github.com/codespaces → ··· → Stop codespace).

### On your own computer

Install [uv](https://docs.astral.sh/uv/) and, in the folder of this repository:

```
uv sync            # Python 3.14 and the exact library versions of uv.lock, in .venv
uv run jupyter lab
```

Open a notebook and choose Kernel → Restart Kernel and Run All Cells.

### Documentation

- [Running the Python environment: a guide for programmers](docs/python-environment-guide.md): the tools, when to use each, macOS and GitHub, common tasks and troubleshooting.
- [Python port: setup log](docs/python-setup-log.md): everything installed for the Python port, step by step, with the problems met and how they were fixed.

### Notes

- The slider of Figure 2 works only in a running notebook.
- Random numbers come from NumPy and PyTorch, so the values that depend on random weights (section 5 of the main notebook; Problems 3, 5 and 6 of the exercises; the weights, data and accuracy of the BasicRecurrentLayer tutorial) differ slightly from the Wolfram runs, and the text of the Python editions gives the values of the Python run. Everything else agrees with the Wolfram results.
- In the BasicRecurrentLayer tutorial, PyTorch's `nn.RNN` has two bias vectors where Wolfram's layer has one; the notebook fixes the second at 0, so the layer and its parameter count (18) match Wolfram's.

## License

© Computational Classnotes · CC BY-NC-SA 4.0. See [LICENSE.md](LICENSE.md). The photos keep their own licences, listed there.
