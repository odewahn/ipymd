# IPython notebook & Markdown

Combining the best of IPython notebook and Markdown for git-friendly technical book writing.

**tl; dr**: this **experimental** module replaces the JSON-based `.ipynb` format by Markdown `.md` documents (this is exactly Markdown, not a variant). You loose all metadata and code outputs, but you keep Markdown text and code. This is useful when you write technical documents, blog posts, books, etc.

## Rationale

### IPython notebook

Pros:

* Excellent UI for executing code interactively *and* writing text

Cons:

* nbformat not really git-friendly (JSON, contains the code output by default)
* cannot easily edit in a text editor


### Markdown

Pros:

* Simple ASCII format to write code and text
* Can be edited in any text editor
* Git-friendly

Cons:

* No UI to execute code interactively

## Workflow

* Write contents (text and code) in a Markdown `document.md`
    * either in the notebook UI, as usual, with Markdown cells and code cells (useful when working on code)
    * or in a text editor, using Markdown (useful when working on text)
* Only the Markdown cells and input code cells are saved in the file
* Collaborators can work on the Markdown document using GitHub (branches, pull requests...), they don't need IPython. They can do everything from the GitHub web interface.


## CAVEATS

**WARNING**: this is an experimental module, there is a risk for data loss, so be careful!

* DO NOT RENAME YOUR .MD NOTEBOOKS IN THE NOTEBOOK UI


## Installation

0. You need IPython 3.0 (or latest master) (with PR #7278) and mistune.
1. Put this repo in your PYTHONPATH.
2. Add this in your `ipython_notebook_config.py` file:

    ```python
    c.NotebookApp.contents_manager_class = 'ipymd.MarkdownContentsManager'
    ```

3. Now, you can open `.md` files in the notebook.

### Atlas

There is also experimental support for O'Reilly Atlas (thanks to @odewahn, see [this presentation](http://odewahn.github.io/publishing-workflows-for-jupyter/#1)).

* Math equations are automatically replaced by `<span class="math-tex" data-type="tex">{equation}</span>` tags.
* Code inputs are replaced by `<pre data-code-language="{lang}" data-executable="true" data-type="programlisting">{code}</pre>` tags.

This is handled transparently in the notebook UI, i.e. you can keep writing math in `$$` and code in code cells or Markdown code blocks.

To use the Atlas format, put this in your config file:

```python
c.NotebookApp.contents_manager_class = 'ipymd.AtlasContentsManager'
```

## Using Docker

You can also run this using Docker on a Mac (via boot2docker) or Linux by building the image and then running the app in the container.  

* Building the image

```
docker build -t odewahn/ipymd .
```

Be sure to change then username `odewahn` to your username.

* Running the app in the container

```
$ cd your/notebook/directory
$ docker run -it -p 8888:8888 -v $(pwd):/usr/data odewahn/ipymd
```

The `-v` option will map your current local directory on your host to the `/usr/data` directory on the container.  Note that this volume mapping is currently not supported on Windows, although I'd expect that this will change soon.

Then, open your browser:

* On a Mac, go to `192.168.59.103:8888`
* On Ubuntu, go to `127.0.0.1:8888`


  
