Scala Language Reference as Pandoc Markdown - Notes
===================================================

General
-------

- All files must be saved as UTF-8: ensure your editors are configured
  appropriately.
- Use of the appropriate unicode characters instead of the latex modifiers
  for accents, etc. is necessary. For example, é instead of \'e. Make use of
  the fact that the content is unicode, google the necessary characters if
  you don't know how to type them directly.
- Leave two empty lines between each section, regardless of level of nesting.
  Leave two empty lines at the end of every markdown file that forms a part
  of the main specification when compiled.

Conversion from LaTeX - Guidelines
----------------------------------

### Chapter conversion Checklist

#. Convert all `\section{...}`
#. Convert all `\subsection{...}`
#. Convert all `\subsubsection{...}`
#. Convert all `{\em ...}`
#. Convert all `\lstlisting`
#. Convert all `\lstinline`
#. Convert all `\sref{sec:...}`
#. Convert all `\begin{itemize}`
#. Convert all `\begin{enumerate}`
#. Convert all `\example`
#. Convert all `\code`
#. Convert all `\footnote`
#. Convert all single quote pairs
#. Convert all double quote pairs
#. Look for manually defined enumerated lists (1. 2. 3. etc)
#. Remove `%@M` comments
#. Convert all extra macros (`\commadots`, etc)


### Code

Code blocks using the listings package of form

    \begin{lstlisting}
    val x = 1
    val y = x + 1
    x + y
    \end{lstlisting}


can be replaced with pandoc code blocks of form

    ~~~~~~~~~~~~~~{#ref-identifier .scala .numberLines}
    val x = 1
    val y = x + 1
    x + y
    ~~~~~~~~~~~~~~

Where `#ref-identifier` is an identifier that can be used for producing links
to the code block, while `.scala` and `.numberLines` are classes that get 
applied to the code block for formatting purposes. At present we propose to
use the following classes:

- `.scala` for scala code.
- `.grammar` for EBNF grammars.

It is important to note that while math mode is supported in pandoc markdown
using the usual LaTeX convention, i.e. $x^y + z$, this does not work within 
code blocks. In most cases the usages of math mode I have seen within
code blocks are easily replaced with unicode character equivalents. If
a more complex solution is required this will be investigated at a later stage.

#### Inline Code

Inline code, usually `~\lstinline@...some code...@` can be replaced with
the pandoc equivalent of

    `...some code...`{<type>}

where `<type>` is one of the classes representing the language of the
code fragment.

### Macro replacements:

- While MathJAX just support LaTeX style command definition, it is recommended
  to not use this as it will likely cause issues with preparing the document
  for PDF or ebook distribution.
- `\SS` (which I could not find defined within the latex source) seems to be
  closest to `\mathscr{S}`
- `\TYPE` is equivalent to `\boldsymbol{type}'
- As MathJAX has no support for slanted font (latex command \sl), so in all
  instances this should be replaced with \mathit{}
- The macro \U{ABCD} used for unicode character references can be
  replaced with \\uABCD.
- The macro \URange{ABCD}{DCBA} used for unicode character ranges can be
  replaced with \\uABCD-\\uDBCA.
- The macro \commadots can be replaced with ` , … , `.
- There is no adequate replacement for `\textsc{...}` (small caps) in pandoc 
  markdown. While unicode contains a number of small capital letters, it is
  notably missing Q and X as these glyphs are intended for phonetic spelling,
  therefore these cannot be reliably used. For now, the best option is to
  use underscore emphasis and capitalise the text manually, `_LIKE THIS_`.
- `\code{...}` can be replaced with standard in-line verbatim markdown,
  `` `like this` ``.


### Unicode Character replacements

- The unicode left and right single quotation marks (‘ and ’) 
  have been used in place of ` and ', where the quotation marks are intended
  to be paired. These can be typed on a mac using Option+] for a left quote
  and Option+Shift+] for the right quote.
- Similarly for left and right double quotation marks (“ and ”) in
  place of ". These can be typed on a mac using Option+[ and Option+Shift+].

### Enumerations

Latex enumerations can be replaced with markdown ordered lists, which have
syntax

    #. first entry
    #. ...
    #. last entry


Finding rendering errors
------------------------

- MathJAX errors will appear within the rendered DOM as span elements with
  class `mtext` and style attribute `color: red` applied.

