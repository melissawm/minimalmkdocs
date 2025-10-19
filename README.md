# minimalmkdocs

MkDocs is what is called a documentation generator. This means that it takes a bunch of source files in plain text, and generates a bunch of other awesome things, mainly HTML. For our use case you can think of it as a program that takes in plain text files in Markdown format, and outputs HTML.

Markdown -> MkDocs -> HTML

So as a user of MkDocs, your main job will be writing these text files. This means that you should be minimally familiar with Markdown.

This repo contains a very simple example of how to set up and use MkDocs to generate Python documentation.

See the rendered version of the documentation at [https://minimalmkdocs.readthedocs.io](https://minimalmkdocs.readthedocs.io)

This repo is heavily inspired by [minimalsphinx](https://minimalsphinx.readthedocs.io) ([repo](https://github.com/melissawm/minimalsphinx)), another minimal example, but using Sphinx instead of MkDocs.

## Basic instructions

0. Install [uv](https://docs.astral.sh/uv/getting-started/installation/) on your machine. Depending on your operating system, the instructions may vary. uv can help us manage our virtual environments and dependencies, including Python itself. If you are creating a project from scratch, you can run

   ```bash
   uv init
   ```

1. Add the basic dependencies to your project. I recommend adding mkdocs itself, and whatever theme you wish to use. We are using [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) in this example, and adding it is enough to get started (it will automatically pull in MkDocs, Markdown, Pygments and Python Markdown Extensions.)

   ```bash
   $ uv add --group docs mkdocs-material
   ```

   This will install Python and all required packages in a virtual environment located in `.venv/`. To see the list of dependencies, check the "docs" dependency group configuration in the `pyproject.toml` file. To install these dependencies later, run:

   ```bash
   $ uv sync --all-groups
   ```

2. Initiate MkDocs

   If you are starting a project from scratch, you can do

   ```bash
   $ uv run mkdocs new .
   ```

   to get an initial file structure and configuration for your documentation. The final message should say:

   > INFO    -  Writing config file: ./mkdocs.yml

   > INFO    -  Writing initial docs: ./docs/index.md

   (For this repo, this initial configuration has already been run, so you don't need to do it again.)

3. You can now check the `mkdocs.yml` file to see some default configuration options, and the `docs/` folder for the initial documentation files. Make sure to add your own site name, url and set the theme to Material. Your `mkdocs.yml` file should look something like this now:

```yaml
site_name: Minimal MkDocs

site_url: https://github.com/melissawm/minimalmkdocs
theme:
  name: material
```

To build the documentation site, you can run

```bash
$ uv run mkdocs serve
```

This will start a local server, usually at `http://127.0.0.1:8000`. You can open this address in your browser to see the rendered documentation. Any changes you make to the Markdown files will automatically be reflected in the browser.

Now let's create some documents!

## Adding narrative content

You can add narrative content to your documentation by creating Markdown files in the `docs/` folder. You can then link these files in the `mkdocs.yml` configuration file under the `nav:` section to create a navigation structure for your documentation site. We will add a Quickstart page to our documentation in this way. Your updated `mkdocs.yml` file should look like this:

```yaml
site_name: Minimal MkDocs

site_url: https://github.com/melissawm/minimalmkdocs
theme:
  name: material

nav:
  - Home: index.md
  - Quickstart: quickstart.md
```

### Markdown extensions: admonitions and other extra Markdown features

For some of the extra features we want to add to our docs, we will use MkDocs plugins such as the [Python Markdown Admonition Extension](https://python-markdown.github.io/extensions/admonition/) and the [PyMdown Tabbed Extension](https://facelessuser.github.io/pymdown-extensions/extensions/tabbed/). These extensions are already included in the Material for MkDocs installation, but we have to enable them in our `mkdocs.yml` file. To do this, we add the following lines to our configuration:

```yaml
markdown_extensions:
  - admonition
  - pymdownx.tabbed:
      alternate_style: true
  - pymdownx.superfences
```

(The [SuperFences extension](https://squidfunk.github.io/mkdocs-material/setup/extensions/python-markdown-extensions/#superfences:~:text=The-,SuperFences,-extension%20allows%20for) allows for arbitrary nesting of code and content blocks inside each other, including admonitions, tabs, lists and all other elements.)

Now you can add admonitions to your Markdown files using the following syntax:

```markdown
!!! warning

    This is a warning admonition.
```

And you can create tabbed content using the following syntax:

```markdown
=== "Tab 1"
    Markdown **content**.

    Multiple paragraphs.

=== "Tab 2"
    More Markdown **content**.

    - list item a
    - list item b
```

### Plugins: including other Markdown files

You can also include other Markdown files inside your documents using the [mkdocs-include-markdown-plugin](https://github.com/mondeja/mkdocs-include-markdown-plugin).

To use this plugin, first install it by adding it to your `docs` dependency group:

```bash
$ uv add --group docs mkdocs-include-markdown-plugin
```

Then, enable it in your `mkdocs.yml` file by adding the following lines:

```yaml
plugins:
  - include-markdown
```

Now you can include other Markdown files in your documents using the following syntax:

```markdown
{𠂽% include-markdown "path/to/other/file.md" %𠂽}
```

## Documenting a Python module

We'll create a very simple Python module to test our setup. It's a starter Pokédex, containing just the three starter Pokémon from the Kanto region.

When documenting Python code, it is common to write *docstrings*. MkDocs supports the inclusion of docstrings from your modules with a plugin called [mkdocstrings](https://mkdocstrings.github.io/). It works for multiple languages, but we will only add the Python language support. First, add it to your `docs` dependency group:

```bash
$ uv add --group docs mkdocstrings-python
```

Then, enable it in your `mkdocs.yml` file by adding the following lines:

```yaml
plugins:
  - mkdocstrings:
      default_handler: python
```

You can then document whole classes or even modules automatically, using member options for the auto directives, like

```
# Reference

::: my_library.my_module.my_class
```

### Griffe for parsing docstrings

mkdocstrings uses [Griffe](https://griffe.github.io/) under the hood to parse docstrings. Griffe supports multiple docstring styles, including Google, NumPy and reStructuredText. You can specify the docstring style in your `mkdocs.yml` file by adding the following lines:

```yaml
plugins:
  - mkdocstrings:
      default_handler: python
      handlers:
        python:
          options:
              docstring_style: google
```

### doctests

With Sphinx, it is possible to run tests embedded in the documentation using the `doctest` extension. MkDocs does not have a built-in way to run doctests, but you can use the [mktestdocs plugin](https://github.com/koaning/mktestdocs) to achieve similar functionality.

### Internal and external anchors and references 

The [mkdocs-autorefs](https://mkdocstrings.github.io/autorefs/) plugin can be used to create internal and external references and anchors in your documentation. This allows you to link to other sections of your documentation or to external resources easily.

To use this plugin, add it to your `mkdocs.yml` file:

```yaml
plugins:
  - autorefs
```

Now, you can do the following:
- Use `[title][anchor]` to create a link to an internal anchor.
- Use `[title](#section-id)` to link to a section within the same document.
- Use `[title](external-url)` to link to an external URL.
- Use `[object][]` to link to documented objects within your project.

To link to other project's documentation, you can use the `inventories` option of the `mkdocstrings` plugin. For example, to link to the Python standard library documentation, you can add the following lines to your `mkdocs.yml` file:

```yaml
plugins:
  - mkdocstrings:
      handlers:
        python:
          inventories:
            - url: https://numpy.org/doc/stable/objects.inv
```

Now, you can link to NumPy objects, for example, using the syntax `[object][]`. For example, to link to the `array` class in NumPy, you can use `[numpy.array][]` to get [numpy.array][].
