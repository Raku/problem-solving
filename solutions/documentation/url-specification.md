# URL Specification v1.0.0

## Table of contents

- [Introduction notes](#introduction-notes)
- [URL generation](#url-generation)
- [Examples](#examples)

## Introduction notes

### Pod block classification

Before write an URL specification, we need to know where those URLs point to. In the documentation, we define documents by using:

~~~perl6
=begin pod

Docs!

=end pod
~~~

This is usually called a *pod block*. In `Documentable`, these blocks are represented by a `Documentable::Primary` object. To avoid depending on the logical distribution of these files in a directory structure and in order to make a more granular classification of these pod blocks, we use additional *metadata*:

~~~perl6
=begin pod :kind("Type") :subkind("class") :category("basic")

Docs!

=end pod
~~~

The metadata is formed by three different values:

- `kind`: An [enum](https://github.com/Raku/Documentable/blob/9563911c93fee5c7fe83f4de5a2e9ee58167bd87/lib/Documentable.pm6#L4-L6) with six different values, depending of the documentation you are writing:

  - Type: Pod blocks related to 'Types'.
  - Language: Pod blocks related to 'Language'.
  - Programs: Pod blocks related to 'Programs'.
  - Syntax: Pieces of documentation with this kind of header: `=headN X<>` ([example](https://github.com/Raku/doc/blob/1be6eefaac9fa27341207be56c85a4729dcaa570/doc/Language/experimental.pod6#L17)) or with headers matching the `syntax` token of [this grammar](https://github.com/Raku/Documentable/blob/master/lib/Documentable/Heading/Grammar.pm6).
  - Routine: Pieces of documentation with headers matching `routine` or `operator` tokens of the same grammar.
  - Reference: created by `X<>` elements.

- `subkind`: classification purposes (this value does not affect urls)
- `category`: classification purposes (this value does not affect urls)

The last three are set by `Documentable` and should not be used in the metadata. For the creation of URLs, we are only interested in the first three, so I will only refer to those in the next paragraphs.

*Side note:* have in mind that in the same `.pod6` file, can appear more than one pod block, so you can write things like:

~~~perl6
=begin pod :kind("Type") :subkind("class") :category("basic")

Docs for a type!

=end pod

=begin pod :kind("Type") :subkind("class") :category("basic")

Docs for another type!

=end pod
~~~

### Pod block names

We need to give each and every pod block a *meaningful* name, to show it if necessary. These names depend on the pod block `Kind` value.

- `Kind::Type`: the last word of `=TITLE` is taken as name. So, if we have a pod block with `=TITLE class X::Some::Class`, the name will be set to `X::Some::Class`.
- `Kind::Language` and `Kind::Programs`: `=TITLE` is converted to string. So, if we have a pod block with `=TITLE Experimental features`, the name will be set to `Experimental features`.

## URL generation

### Pod block URLs

Each and every pod block gets a standalone HTML page, so every pod block needs an URL. This URL will depend on `Kind` classification:

- `Kind::Type`: the URL for those blocks will be formed as: `/${kind.lc}/{$name}`.
- `Kind::Language` and `Kind::Programs`: the URL for those blocks will be formed as `/{kind}/{$filename}`, where filename is the filename with the extension stripped out. So the URL for [experimental.pod6](https://github.com/Raku/doc/blob/master/doc/Language/experimental.pod6#L17) will be `/language/experimental`.

See [this example](#get-all-urls-from-primary-objects).


### Secondary URLs

In a pod block, we can have a lot of different documented methods, subs, etc. Lot of the time, we do not want all that information, but only a little part. How we define those *little parts of documentation*? We use headers. But not all headers are valid, they need to follow one of these two rules:

- Match [this grammar](https://github.com/Raku/Documentable/blob/master/lib/Documentable/Heading/Grammar.pm6).
- Math `=headn X<>` format.

Each and every one of these subsets of a pod block is represented by a `Documentable::Secondary` object. These objects also get a name, specified by the same grammar. They also get an HTML page, but not in the same way as primary objects. First, they are grouped by `name`, forming a single pod block. This is made in order to have the routines, subs and methods with the same named grouped in the same page, to help the final user. Now, to form the URL, the intuitive way would be: `"/${kind.lc}/${name}`, but this is not correct in some cases.

As you may know, `Raku` accepts a huge range of symbols, so the name `attribute` can be a little bit weird sometimes (from a URL perspective). For this reason, `name` needs to be slightly altered to generate valid URLs. This alteration is made by [good-name](https://github.com/Raku/Documentable/blob/9563911c93fee5c7fe83f4de5a2e9ee58167bd87/lib/Documentable.pm6#L55-L75) sub. This function makes these replacements:

~~~
/   => $SOLIDUS
%   => $PERCENT_SIGN
^   => $CIRCUMFLEX_ACCENT
#   => $NUMBER_SIGN
' ' => _
~~~

See [this example](#get-all-urls-from-secondary-objects) and [this one](#classification-of-secondary-objects-by-name).

## Examples

##### Get all URLs from Primary objects

~~~perl6
use Documentable:ver<2.0.0>;
use Documentable::Registry:ver<2.0.0>;

my $registry = Documentable::Registry.new(
    :topdir("doc"),
    :dirs(DOCUMENTABLE-DIRS),
    :!verbose,
);

$registry.compose;

say $registry.documentables.map({.url});
~~~

##### Get all URLs from Secondary objects

~~~perl6
use Documentable:ver<2.0.0>;
use Documentable::Registry:ver<2.0.0>;

my $registry = Documentable::Registry.new(
    :topdir("doc"),
    :dirs(DOCUMENTABLE-DIRS),
    :!verbose,
);

$registry.compose;

say $registry.definitions.map({.url});
~~~

#### Classification of secondary objects by name

~~~perl6
use Documentable:ver<2.0.0>;
use Documentable::Registry:ver<2.0.0>;

my $registry = Documentable::Registry.new(
    :topdir("doc"),
    :dirs(DOCUMENTABLE-DIRS),
    :!verbose,
);

$registry.compose;

my %routine-documents = $registry.lookup("routine", :by<kind>).categorize({.name});
my %syntax-documents = $registry.lookup("syntax", :by<kind>).categorize({.name});

# you can check them! (very long output)
say %routine-documents<⊅>;
~~~