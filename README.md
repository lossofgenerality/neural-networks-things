# Neural Networks · Computational Classnotes

Wolfram Language notebooks on the mathematics of neural networks, from S-shaped activation functions and universal approximation to recurrent layers built from scratch. Each notebook is self-contained: it explains the ideas, states the results, and checks every formula with code you can run.

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

### [BasicRecurrentLayer](basic-recurrent-layer/)

| File | What it is |
|---|---|
| [`BasicRecurrentLayer-Tutorial.nb`](basic-recurrent-layer/BasicRecurrentLayer-Tutorial.nb) | A short tutorial on Wolfram's built-in `BasicRecurrentLayer`: the recurrence, checking the layer by hand, stateful evaluation, and a small sequence classifier |
| [`BasicRecurrentLayer-FromScratch.nb`](basic-recurrent-layer/BasicRecurrentLayer-FromScratch.nb) | The same layer built from ordinary math functions: the full matrix form of the recurrence, a check against the built-in layer, backpropagation through time derived and checked with `D`, and training by hand-written gradient descent |

## Using the notebooks

- Open them in Mathematica or Wolfram Desktop, version 14 or later; S Shape Functions Theory was saved with Wolfram 15.
- Evaluate from the top. In S Shape Functions Theory, run the settings cell **c.0** first: it sets the font sizes and figure widths used by every figure and table. In the solved exercises, run **c.0** and **c.0.1** first; after that each problem runs on its own.
- Every code cell starts with an index comment such as `(*c.7*)`, so cells can be referred to by number.
- Each notebook keeps its variables in a private context, so notebooks open in the same session do not interfere with each other.
- The small organism photos in section 0 of S Shape Functions Theory are loaded from Wikimedia Commons when their cells run, so those cells need an internet connection.

## License

© Computational Classnotes · CC BY-NC-SA 4.0. See [LICENSE.md](LICENSE.md). The photos keep their own licences, listed there.
