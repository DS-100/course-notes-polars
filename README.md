# course-notes-polars

[![Jupyter Book (via myst) GitHub Pages Deploy](https://github.com/DS-100/course-notes-polars/actions/workflows/deploy.yml/badge.svg)](https://github.com/DS-100/course-notes-polars/actions/workflows/deploy.yml) [![Accessibility Checks](https://github.com/DS-100/course-notes-polars/actions/workflows/a11y.yml/badge.svg)](https://github.com/DS-100/course-notes-polars/actions/workflows/a11y.yml)

The Data 100 course notes, using **Polars** instead of pandas. Built with
[Jupyter Book (MyST flavor)](https://jupyterbook.org/en/stable/intro.html).

## Structure

Only two things need to be edited.
- `myst.yml`: Configuration information about the textbook. Modify this file for things like:
    - changing the logo or favicon;
    - adding or removing launch buttons;
    - changing information about the book.
    - table of contents (section, chapter numbering and order)
- `content/`: Content of the textbook. All the notebooks with section and chapter content go here. Modify these files to change the content of the sections.

## Maintaining the Textbook

### One-time Setup
Follow these steps the first time you set up a computer to modify and maintain the textbook.
1. Create a local copy of this repo by running `git clone https://github.com/DS-100/course-notes-polars.git` from the command line in whichever folder you want to contain the textbook.
2. Next, you need to install all the required packages. Either of the commands `pip install -r requirements.txt` or `conda install --file requirements.txt` should work. If you have a Windows device, it's preferable to run this in an Anaconda Prompt terminal. This should install the packages used to build the textbook and execute the notebooks. If you use pip to install the packages, I recommend using a python virtual environment:
```
python3 -m venv venv # create virtual environment
source venv/bin/activate # activate the virtual environment
pip install -r requirements.txt # as above
```
If you use a virtual environment, make sure to activate it before updating the textbook by first running `source venv/bin/activate` at the top level of this repo. You can deactivate the environment with `deactivate`. If you choose to use a virtual environment not named venv, ensure that you add this to the `.gitignore` so it is NOT tracked by git.

### Updating the Textbook
These steps detail the process you should go through every time you update the textbook. Remember to activate your virtual environment first if you are using one.
1. **Pull:** `cd` into `course-notes-polars/`, your local copy of the repository and `git pull origin main` to collect any updates which may have been pushed to the remote copy by other collaborators.
2. **Update:** Make any changes you wish to make. This should (likely) only consist of changes to `myst.yml` and the files in `content/`.
    - If you added new sections or chapters, update the `toc` in `myst.yml` to ensure your changes are included.
3. **Build and Check:** `cd` into the top level directory of this repository and run `jupyter book start` to build the book and serve it on localhost. View what the textbook looks like with any changes you've made. Make sure nothing is broken and the changes are as you want them.
4. **Push:** Stage any changes you made (i.e. using `git add [file]`, `git add -u`, `git add .`, etc.), commit your changes with `git commit -m "[description]"` (please include a useful description of any changes you made), and push to the repository with `git push origin main`. Deployment will happen automatically via GitHub Actions.

### Committed outputs are what ships

**CI does not execute the notebooks.** The deploy workflow runs `jupyter-book build --html` with no
`--execute`, so the site publishes whatever output is already committed in each `.ipynb`. If you
change a code cell, re-run it and commit the new output, or the page will show new code above an old
result.

### Deployment
The textbook is hosted on GitHub Pages and deployed automatically everytime you push to the main branch via [this workflow](https://github.com/DS-100/course-notes-polars/actions/workflows/deploy.yml).

## Polars/pandas comparison tabs

Many chapters carry side-by-side tab-sets showing the same operation in both libraries. They look
like this in a markdown cell, and the HTML comments are markers — leave them in place:

````
<!-- tab-twins:begin <cell-id> -->
:::::{tab-set}
:::: {tab-item} Polars
:sync: pl
...
::::
:::: {tab-item} pandas
:sync: pd
...
::::
:::::
<!-- tab-twins:end -->
````

Three things to know before editing one:

- **The panes are frozen text.** They do not execute. Editing the code cell above without editing
  its tab leaves the two disagreeing, and nothing will warn you.
- **The code cell a tab describes is tagged `remove-input, remove-output`,** so the reader sees the
  tab instead of a duplicate. The cell still runs and still carries its output.
- **A cell whose output is a figure keeps `remove-input` only,** because a frozen text pane cannot
  carry an image. If you hide such a cell's output, the plot disappears from the page with no error.
- **`:sync:` keys are page-scoped**, so clicking either tab switches every comparison on that page.

`content/polars_1/polars_1.ipynb` additionally has a "Pick a library" tab-set near the top, which
acts as a labelled master switch for the rest of that page.

## Helpful Things
The [Jupyter Book](https://jupyterbook.org/stable/) website has lots of information about Jupyter Book. This textbook uses Jupyter Book v2 built on [MyST](https://mystmd.org/).

If changes you've made aren't showing up the HTML after building, sometimes deleting `_build` and then building again helps. You can also run `jupyter book clean`.

### Images

All images must have alt text, and it must describe the figure rather than repeat its title.
[More on images and figures in MyST.](https://mystmd.org/guide/figures)

Note that MyST needs a space after the colon in a directive option: `:alt: text` is read,
`:alt:text` is silently ignored, and `:alt: ""` passes an automated accessibility check while
telling a screen-reader user nothing.

### Links
Links to the internet should be done as they are usually done in Markdown. However, to cross-link to other pages of the textbook, there is an internal linking system that should be used instead (since it is robust to file structure changes). This system is described [here](https://mystmd.org/guide/cross-references#targeting-headers).

For example, Section 12.4 Exercise 3 of the [Stat 88 Textbook](https://github.com/stat88/textbook) contains a link to Section 12.2.
1. The flag `(ch12.2)=` was added *before* the primary header of the notebook.
```
(ch12.2)=
## The Distribution of the Estimated Slope ##
```
2. The link referencing Section 12.2 is created like so:
```
**3.**
Refer to the regression of active pulse rate on resting pulse rate in [Section 12.2](ch12.2). Here are the estimated values again, along with some additional data.
```

### Hidden Cells

[Use tags to hide cells and/or their output](https://mystmd.org/guide/interactive-notebooks#notebooks-cell-visibility)

To hide code behind a click, put it in a dropdown [directive](https://mystmd.org/guide/directives) in
a markdown cell:

````{dropdown} Click to see the code
:open: false
```python
import polars as pl
import numpy as np
import urllib.request
import os.path
import zipfile
```
````

Then put the code you want to execute (and see its output on the website) in a code cell with the
`remove-input` tag:

```{code-cell} python
:tags: [remove-input]
print("This will show output with no input!")
```

The dropdown and the code cell are two copies of the same source. Change one and you must change the
other.
