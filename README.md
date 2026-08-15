# aiML

Exploratory work — notebooks written to learn machine learning concepts by
building them from scratch rather than reading about them.

Nothing here is a library or a product. It's a sandbox.

## Notebooks

- **`math_of_ml_screensaver.ipynb`** — the math behind ML, worked up from dot
  products and gradient descent to a tiny MLP trained *by hand* (no autograd).
  The learned scalar field becomes a divergence-free flow, and particles running
  through it produce a generative screensaver. A final section scales the same
  idea to a GPU CPPN in PyTorch.
- **`epistemic_vs_aleatoric.ipynb`** — the two kinds of uncertainty in a
  prediction: what the model doesn't know yet, versus what nothing could know.

The `.mp4` and `.png` files are rendered output from the screensaver notebook.

## Running

```bash
python -m venv .venv && source .venv/bin/activate
pip install numpy matplotlib jupyterlab
jupyter lab
```

The GPU section additionally needs `torch` and an NVIDIA GPU. MP4 export uses
`ffmpeg` if it's on PATH, and falls back to GIF if not.
