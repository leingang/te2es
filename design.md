# te2es design doc

## Synopsis

```bash
  te2es [options] infile
```

### Argument

`infile` a TextExpander property list file (leave empty for stdin)

### Options

  * `-o`, `--output` output file (default: send output to stdout)
  * Maybe some options to filter snippets

## Installation

  * Clone repo
  * `pip install -e .`

## TextExpander snippets

Each TextExpander snippet group gets its own property list file. Mac 
property lists are an XML application. As XML applications go, they 
are not that fun to parse: each key-value pair consists of one `<key>`
element followed by one value element of varying type.

Luckily, there's a [python library](https://docs.python.org/3/library/plistlib.html) for that.

From reading the property list files, it appears that each snippet's `plainText` key gives the
replacement text.

### Snippet variables

  * `%%`: literal percent character
  * `%|`: insert the cursor here (see [Special Codes in Snippets](https://textexpander.com/learn/using/snippets/advanced-snippet-elements/special-codes))
  * `%m`, `%d`, `%y` etc.: [datetime variables](https://textexpander.com/learn/using/snippets/advanced-snippet-elements/advanced-date-time)
  * `%clipboard`: contents of the clipboard
  * `%filltext:name=<name>%` for [fill-ins](https://textexpander.com/learn/using/snippets/advanced-snippet-elements/advanced-fill-ins)
  * `%snippet:<snippet>%` insert another snippet by name
  * `%key:<key>%` emulate keypress (e.g., `tab`)
