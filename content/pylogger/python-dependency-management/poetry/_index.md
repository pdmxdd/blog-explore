---
title: "Poetry"
date: 2022-03-20
draft: false
hidden: false
weight: 105
---

## Poetry Documentation

When you are working with a new tool there is no bette resource than the official documentation. If I'm dealing with an open source tool I like to view both the remote git repo (like GitHub, GitLab, or BitBucket) and the official docs.

### Poetry Resources

- [Poetry GitHub Repo](https://github.com/python-poetry/poetry)
- [Poetry Official Documentation](https://python-poetry.org/docs/)
- [Poetry Website Homepage](https://python-poetry.org/)

## Installation

The GH repository provides a simple command for unix based systems:

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

### Breakdown

`curl -sSL https://install.python-poetry.org`: a simple curl request for whatever is served at https://install.python-poetry.org.

- `-s`: silent mode
- `-S`: show error (`--show-error`)
- `-L`: location (`--location`) if resource has a new location refire request to new location

Let's fire off this request to see the contents of the python file at the web location. I used `curl -sSL https://install.python-poetry.org | bat`.

It's a 900 line file, but it begins with a comprehensive comment:

![python poetry install file](pictures/python-poetry-install-file.png)

Unsurprisingly the `curl` request is simply downloading a python file into STDIN and passing it to the python3 interpreter via the pipe operator (`|`).

## Actual Installation

Executing:

![Install Poetry](pictures/install-poetry.png)

Well, that was simple.

![Poetry Version](pictures/poetry-version.png)

## Usage

### Create a New Poetry Project

Let's create a new pylogger project using poetry:

![Create new project with Poetry](pictures/create-poetry-project.png)

Alright we created a new project called `pylogger-poetry` right next to the original `pylogger` project.

I like the automatic `tests` directory and am very interested in `pyproject.toml` (you can clearly see the Rust/Cargo inspiration).

### Personal Customizations

I would prefer the `README.rst` to be in markdown rather than restructured text, but that's an easy fix.

![Rename README](pictures/poetry-rename-readme.png)

It doesn't automatically configure the directory as a local git repository, but again that's a minor concern we can fix.

![Poetry Initialize Git](pictures/poetry-initialize-git.png)

### Add Dependency

It's pretty simple to add a new depedency to the project with the `poetry add [dependency]` command.

![Poetry Add pynput](pictures/poetry-add-dependency.png)

Ok, first reaction: **20.8** seconds seems excessive.

According to the documentation Poetry will **exhaustively** resolve dependency issues, which is very much appreciated. However, 20.8 seconds is a bit extreme for a project that only has one relatively light library.

I do like that a lock file was automatically written (so any subsequent environment creations will be almost immediate) and the `pyproject.toml` was automatically updated.

![Pyproject TOML](pictures/pyproject-toml.png)

However, the project is ready to go. With a simple adding of a remote repo, staging, committing, and [pushing](https://github.com/pdmxdd/pylogger-poetry) this project is ready for development.

## Adding Code

To make things simple I'm just adding a typical hello world.

After creating `pylogger_poetry/main.py` I add:

```python
if __name__ == "__main__":
    print("hello world")
```

## Running Code

### `poetry run python3 main.py`

![poetry run python3 main.py](pictures/poetry-run-python-main.png)

### `poetry shell` && `python3 main.py`

![poetry shell && python3 main.py](pictures/poetry-shell-python-main.png)

### `source /home/paul/.cache/pypoetry/virtualenvs/...`

![source activate && python3 main.py](pictures/virtualenv-python-main.png)

I don't personally like that the virtual environment is stored in `$HOME/.cache/pypoetry/virtualenvs/`, but looking at the [Poetry Configuration Documentation](https://python-poetry.org/docs/configuration/#virtualenvsin-project) you can easily set a global variable to force virtual environments to be created in a `.venv` directory in your project directory. I am very supportive of tools that give you the ability to customize details like this.

## Final Thoughts

I quite like the experience of Poetry. 

I have a few **minor** complaints:

- README defaults to rst instead of md (personal preference)
- no automatic git repo initialization (personal preference)
- long dependency resolution window

I really **like**:

- the `pyproject.toml`
- the `poetry.lock` file
- discrete default project directory & tests directory
- you can configure poetry to create the virtual environment inside of the project directory

I am currently ambivalent towards the `poetry shell`, and `poetry run python file.py`. 

I can see how the abstraction over the isolated virtual environment allows a developer to focus on development instead of managing virtual environments which is certainly a benefit. However, it is still the responsibility of the developer to understand their virtual environment and I fear that beginners may never dive deeper into the underlying concepts when using poetry. Poetry isn't hiding this information. It clearly prints out where the virtual environment lives.

Again, this is a nitpicky, personal complaint that wouldn't stop me from using the tool.

Overall I feel the **pros far outweigh the cons**.

Based on this first experience I will happily start using Poetry for my Python projects.