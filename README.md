# Times Square Demo Notebooks for USDF

These are notebooks that demonstrate Times Square using production-grade datasets at the Rubin USDF.

View online: https://usdf-rsp-dev.slac.stanford.edu/times-square/

## Contributing

### Setting up the repository

Clone this repository:

```bash
git clone https://github.com/lsst-sqre/times-square-usdf
cd times-square-usdf
```

Install the pre-commit hooks:

```bash
pip install pre-commit
pre-commit install
```

### Adding a notebook

All ipynb Jupyter Notebook files in this repository's `main` branch are eligible to be published to Times Square on USDF Dev at https://usdf-rsp-dev.slac.stanford.edu/times-square/.
Setting up a notebook requires two files: the `ipynb` file itself and a `yaml` file that provides metadata for the notebook.
Both files must have the same root name (e.g `my-notebook.ipynb` and `my-notebook.yaml`).
Notebooks can be organized in directories, and those directories are reflected on Times Square.

#### Preparing the notebook

The first code cell of a Times Square notebook must exclusively assign values to parameters.
When Times Square runs a notebook on the Rubin Science Platform, it will replace this code cell with variable assignments requested by the user.
For example, if the notebook uses a `date` variable as a string and a `threshold` variable as a float, the first code cell should look like this:

```python
date = '2023-01-01'
threshold = 0.5
```

The notebook can then use these variables in subsequent cells.

Markdown cells can also use these variables as Jinja variables in the `params` namespace.
For example, a title markdown cell might be:

```markdown
# Report for {{ params.date }}
```

The notebook must be able to run independently from the standard Notebook Aspect kernel.
Your notebook can't load files from this repository, rely on private files in a specific user's directory, or import Python packages that aren't available in the standard Notebook Aspect kernel.

#### Preparing the metadata

The `yaml` file that corresponds to the ipynb file must contain the following keys:

- `title`: The title of the notebook, as it will appear on Times Square.
- `description`: A short description of the notebook, as it will appear on Times Square.
- `authors`: A list of authors for the notebook, as they will appear on Times Square. Each item is a dictionary with `name` and `slack` keys.
- `tags`: A list of tags for the notebook, as they will appear on Times Square.
- `parameters`: A mapping of parameters that the notebook accepts, as they will appear on Times Square.
  The key of each parameter is its Python variable name, and maps to `type`, `description`, and `default` keys.
  The `type` must be one of `string`, `integer`, `number`, or `boolean`.
  More types are coming

For example:

```yaml
title: My Notebook
description: A notebook that does something.
authors:
  - name: An Engineer
    slack: @engineer
tags:
  - demo
parameters:
  date:
    type: string
    description: The date to use.
    default: 2023-01-01
  threshold:
    type: number
    description: The threshold to use.
    default: 0.5
```
