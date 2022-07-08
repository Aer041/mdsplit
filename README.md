# mdsplit

`mdsplit` is a python command line tool to
**split markdown files** into chapters
**at a given [heading level](https://spec.commonmark.org/0.30/#atx-headings)**.

Each chapter (or subchapter) is written to its own file,
which is named after the heading title.
These files are written to subdirectories representing the document's structure.

**Note:**
- The output is *guaranteed to be identical* with the input
  (except for the separation into multiple files of course).
    - This means: no touching of whitespace or changing `-` to `*` of your lists
      like some viusual markdown editors tend to do.
- Text before the first heading is written to a file with the same name as the markdown file.
- Chapters with the same heading name are written to the same file.

**Limitations:**
- Only [ATX headings](https://spec.commonmark.org/0.30/#atx-headings) 
  such as `# Heading 1`) are supported.
  [Setext headings](https://spec.commonmark.org/0.30/#setext-headings)
  (underlined headings) are not recognised.

## Installation

Either use pip:

    pip install mdsplit
    mdsplit

Or simply download [mdsplit.py](mdsplit.py) and run it (it does not use any dependencies but python itself):

    python3 mdsplit.py

## Usage

**Split by heading 1** and write to an output folder based on the input name 

```bash
mdsplit in.md
```

```mermaid
%%{init: {'themeVariables': { 'fontFamily': 'Monospace', 'text-align': 'left'}}}%%
flowchart LR
    subgraph in.md
        SRC[# Heading 1<br>lorem ipsum<br><br># HeadingTwo<br>dolor sit amet<br><br>## Heading 2.1<br>consetetur sadipscing elitr]
    end
    SRC --> MDSPLIT(mdsplit in.md)
    MDSPLIT --> SPLIT_A
    MDSPLIT --> SPLIT_B
    subgraph in/HeadingTwo.md
        SPLIT_B[# HeadingTwo<br>dolor sit amet<br><br>## Heading 2.1<br>consetetur sadipscing elitr]
    end
    subgraph in/Heading 1.md
        SPLIT_A[# Heading 1<br>lorem ipsum<br><br>]
    end
    style SRC text-align:left
    style SPLIT_A text-align:left
    style SPLIT_B text-align:left
    style MDSPLIT fill:#000,color:#0F0
```

**Split by heading 2**

```bash
mdsplit in.md --max-level 2 --output out
```

```mermaid
%%{init: {'themeVariables': { 'fontFamily': 'Monospace', 'text-align': 'left'}}}%%
flowchart LR
    subgraph in.md
        SRC[# Heading 1<br>lorem ipsum<br><br># HeadingTwo<br>dolor sit amet<br><br>## Heading 2.1<br>consetetur sadipscing elitr]
    end
    SRC --> MDSPLIT(mdsplit in.md -l 2 -o out)
    subgraph out/HeadingTwo/Heading 2.1.md
        SPLIT_C[## Heading 2.1<br>consetetur sadipscing elitr]
    end
    subgraph out/HeadingTwo.md
        SPLIT_B[# HeadingTwo<br>dolor sit amet<br><br>]
    end
    subgraph out/Heading 1.md
        SPLIT_A[# Heading 1<br>lorem ipsum<br><br>]
    end
    MDSPLIT --> SPLIT_A
    MDSPLIT --> SPLIT_B
    MDSPLIT --> SPLIT_C
    style SRC text-align:left
    style SPLIT_A text-align:left
    style SPLIT_B text-align:left
    style MDSPLIT fill:#000,color:#0F0
```

## Development

Install [poetry](https://python-poetry.org)

Prepare virtual environment and download dependencies

    poetry install

Run tests

    poetry run pytest

Release new version

    poetry build
    poetry publish

[Download statistics](https://pypistats.org/packages/mdsplit)

### Potential TODOs

- [test multiple python versions](https://consoledotlog.co.uk/tox-with-pyenv-and-poetry-to-test-projects-with-multiple-python-versions.html)