# te2es design doc

## Synopsis

```bash
  te2es [options] infile
```

### Argument

`infile` a TextExpander property list file (leave empty for stdin)

### Options

  * `-o`, `--output` output file (default: send output to stdout)
  * OR `-s`, `--save` output file name (default: use the group name from the file)
  * Maybe some options to filter snippets

## Installation

  * Clone repo
  * `pip install -e .`

## TextExpander snippets

01234567890123456789012345678901234567890123456789012345678901234567890123456789

Each TextExpander snippet group gets its own property list file. Mac property
lists are an XML application. As XML applications go, they are not that fun to 
parse: each key-value pair consists of one `<key>` element followed by one value 
element of varying type. Luckily, there's a [python library](https://docs.python.org/3/library/plistlib.html)
for that.

Each property list file is a dictionary. The `name` key gives the name of the
snippet group, and the `snippetPlists` key gives a list (array) of dictionaries,
one for each snippet.

### Snippet properties

  * `abbreviation`: keystrokes which trigger the snippet
  * `plainText`: replacement text
  * `label`: human-readable string for dialogs
  * `creationDate`, `modificationDate`: metadata
  * `uuidString`: identifier
  * `snippetType`: an integer. Don't know what this does yet. Seems to be
     related to the snippet's engine.
    * `0`: plain, just copy text.
    * `2`: AppleScript? Don't want to import these.
    * `3`: Shell?


### Snippet variables

These are used in the `plainText` field.

  * `%%`: literal percent character
  * `%|`: insert the cursor here (see [Special Codes in Snippets](https://textexpander.com/learn/using/snippets/advanced-snippet-elements/special-codes))
  * `%m`, `%d`, `%y` etc.: [datetime variables](https://textexpander.com/learn/using/snippets/advanced-snippet-elements/advanced-date-time)
  * `%clipboard`: contents of the clipboard
  * `%filltext:name=<name>%` for [fill-ins](https://textexpander.com/learn/using/snippets/advanced-snippet-elements/advanced-fill-ins)
  * `%snippet:<snippet>%` insert another snippet by name
  * `%key:<key>%` emulate keypress (e.g., `tab`)

## Espanso matches

See also [Matches Basics](https://espanso.org/docs/matches/basics/).


In the Espanso config directory lives a directory called `match`. Each file in
that directory is a YAML file with matches in it.

The YAML document is a dictionary at the top level with a `matches` key whose 
value is a list of match dictionaries.

```yaml
# espanso match file

# For a complete introduction, visit the official docs at: https://espanso.org/docs/

# You can use this file to define the base matches (aka snippets)
# that will be available in every application when using espanso.

# Matches are substitution rules: when you type the "trigger" string
# it gets replaced by the "replace" string.
matches:
  # Simple text replacement
  - trigger: ":espanso"
    replace: "Hi there!"

  - trigger: ":br"
    replace: "Best Regards,\nJon Snow"
...
```

Use `$|$` to indicate the cursor positioning.

You can refer to previously defined matches with
[Nested matches](https://espanso.org/docs/matches/basics/#nested-matches).

Fill-ins should become [Forms](https://espanso.org/docs/matches/forms/).




