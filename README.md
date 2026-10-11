# Post Selection RNA Sequence Network Analysis

A workflow for MAFFT alignment, alignment visualization, pairwise difference counts, average-linkage clustering, cluster abundance summaries, and an abundance-weighted radial sequence network.

## Repository contents

- `Post Selection RNA Sequence Network Analysis.ipynb` — analysis workflow.
- `requirements.txt` — Python dependencies, including JupyterLab.
- `.gitignore` — exclusions for sequencing files, private data directories, outputs, checkpoints, and local environments.
- `README.md` — setup and usage instructions.

Supply your own abundance-annotated FASTA file to run the analysis.

## Setup

Use Python 3.10 or newer. From the repository directory, create and activate a local environment:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python -m ipykernel install --user --name rna-sequence-analysis --display-name "Python (RNA sequence analysis)"
```

On Windows, activate with `.venv\Scripts\activate` instead. MAFFT is an external program and is not installed by `requirements.txt`. On macOS with Homebrew installed:

```sh
brew install mafft
```

On other systems, install MAFFT for your platform and make sure the `mafft` command is on PATH in the environment running Jupyter.

## input file

Open the notebook and set `input_file` in the configuration cell:

```python
# input file
input_file = Path("your_sequences.fasta")
```

Replace the placeholder with the path to your FASTA file. Relative paths are resolved from the notebook's working directory. Absolute paths are also supported.

The input file must contain at least two sequences with unique identifiers. Each header's final whitespace-separated field must be a finite, nonnegative normalized abundance expressed as a fraction. For example, the illustrative header `>example_sequence 0.1` represents 10%. Abundances are summed as supplied, without renormalizing a selected subset to 100%.

Start JupyterLab:

```sh
python -m jupyterlab
```

Select the `Python (RNA sequence analysis)` kernel and run all notebook cells in order. The workflow checks that the input file and MAFFT exist. Results are written to `outputs/`, including aligned FASTA, figures, a distance matrix, sequence/cluster assignments, and cluster abundance summaries.

## Parameters and interpretation

- `cluster_cutoff = 10` controls average-linkage hierarchical clusters.
- `network_threshold = 3` connects pairs whose calculated distance is at most 3. Changing this does not change hierarchical cluster membership.
- Node size reflects abundance; node colour reflects cluster membership. Radial placement is for legibility and does not encode distance.

Distances count substitutions and internal gap-versus-base positions, ignoring terminal gaps. They are descriptive difference counts, not evolutionary distance estimates. Gap runs are counted position by position, ambiguity symbols are compared literally, and T/U are not normalized by the distance function. Pairs without any shared nongap positions stop clustering. Cluster IDs are arbitrary and network edges do not establish ancestry.
