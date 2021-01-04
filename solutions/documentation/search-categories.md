# Search categories

The Pod6 markup language provides a mechanism to "index" text via the `X` formatting code.
The code consists of the optional text part that is formatted into the document if given
and a number of obligatory, possibly multi-level (separated with a comma),
index entries separated with a semicolon:

```
# Common
X<text|entry>
# Variations
X<|entry-level-1, entry-level-2>
X<|entry1;entry-level1, entry-level2;entry2>
```

The [documentation for Raku](https://docs.raku.org) website supports a search feature:
the user can look up terms by text to navigate to relevant documentation pieces describing
the term.

On the website the search results (a set of search entries) are presented
as a list of items divided into *categories*.

Both forming a set of the search entries for the website and assigning 
categories to entries is based upon the Documentable module code which
extracts the search anchors from the index entries, headings and pages.

At the time Documentable module was written, a lot of legacy code
was simply migrated into the module and as a result the existing
system for the indexing remains to be unpredictable and bizarre
to the documentation writer.

Currently, in Documentable there are 6 (six) different code paths where the
category is set for different cases of index entries, headings and pages,
and each has its own arbitrary way of deciding the category.

More so, in the past, as the documentation emerged with no guidelines
for indexing entries at all, people had to come up with index formatting
in an ad-hoc manner, sometimes confusing its syntax in different ways
and as the result the state of indexed entries and usability of the search suffers.

This document describes a solution chosen for the Raku language
documentation sources to solve its specific range of issues in this
particular field and may or may not apply to other usages of Pod6.

# Currently existing ways to categorize search entries

Current Raku documentation organization layout consists of six types of pages,
these include primary pages:

* Explaining various aspects of the language itself, so assigned kind `Language`
* Describing existing types included in the language, so assigned kind `Type`
* Describing various topics around working with the language, so assigned kind `Program`

As well as secondary pages, generated based on the primary pages:

* Explaining routines (subroutines, methods, submethods) provided, so assigned kind `Routine`
* Explaining syntax, so assigned kind `Syntax`
* Describing various concepts the documentation refers to, so assigned kind `Reference`

To assign a category to a search item, these ways currently exist:

* For pages of kind `Type`, `Language`, `Program` pages depend on the page subkind
* For secondary, generated documentation pages (kind `Routine`, `Syntax`) try
  to guess a category based on a subkind or simply take the first page subkind available
  as one
* For entries of kind `Reference` simply take the Kind as a category
* For a Pod6 heading with `X` formatting code inside use the first
  entry part as the category (so for `=head1 X<text|entry1,entry2` an item
  of category `entry1` will be created of hardcoded kind `Syntax`)
* `X` formatting codes are also indexed for the second time, where a
  dubious code path instead of calculating the category uses
  a list of subkinds instead (which is reduced to just the first item down the line,
  which is most likely to become `'reference'`)
* Headings are parsed for items like `variable`, `token`, `sub`, `method` etc.
  and for matching headers search entries are created automatically with
  categories being simply hardcoded in the grammar's actions.

Some of the code paths above simply hardcode some arbitrary categorization
while others are driven by the sources (the content of indexing formatting code itself),
which means that if e.g. a contributor makes a mistake and writes:

```
X<text|entry term,category>
```

Then the `category` term will be indexed under the `entry term` category,
even if the search category `entry term` does not make any sense to the consumer.

# Solution

To address the issues described above, I propose to follow these steps:

1. Introduce strict guidelines for indexing.

To specify an index entry, the `X<>` markup code is restricted to be one of the
following forms:

```
X<|$category,term>
X<text|$category,term>
X<text|$category-1,term-1;...;$category-N,term-N>
```

where the `$category-1`, ... `$category-N` variables *explicitly* set the category
for the corresponding term indexed and strictly refer to one of the supported
categories described in this document (including possible future extensions), see the next step.

Sticking to the syntax described here implies two important things:

* No implicit category setting (setting a category manually becomes mandatory)
* Sub-categories (e.g. `X<|Cat,Sub1,Sub2,Term>`) are forbidden

Please, do note that in the example above names such as `$category`, `$category-1` are
placeholders for an actual category name.

Valid examples are:

```
X<|Syntax,does>
X<|Language,\ (container binding)>
X<Subroutines|Syntax,sub>
X<|Variables,$*PID>
X<Automatic signatures|Variables,@_;Variables,%_>
X<Typing|Language,typed array;Syntax,[ ] (typed array)>
X<Attributes|Language,Attribute;Foreign,Property;Foreign,Member;Foreign,Slot>
```

2. Establish a list of supported search categories, see Appendix A for the suggested one.

3. Write a test for the documentation sources to gather all present index entries
   and check them against the approved categories list. The test passes when
   all index entries adhere to the approved categories.

4. Fix the `Documentable` module indexing bits to make them both clearer
   and adhere to the scheme suggested in this document.

5. Gradually adapt current documentation index entries to pass both tests.


# Outcomes

* An organized list of categories makes it easier for the user to understand
* A solid state of search categories makes it easier for the contributor to index items into correct
  categories
* Possible separate documentation search frontends get a much cleaner set of categories to search by

# Appendix A: Search Categories

This list is made with a couple of styling rules in mind, those are:

* Titlecase over lowercase (`Subroutines` over `routines`)
* Plural over singular (`Subroutines` over `routine`)
* Categorize both Raku language standard library API (`Types`, `Subroutines`) as well
  as language-related topics and terms (`Language`, `Syntax`) grouped

The existing as for 05.01.2021 ad-hoc list of search categories is:

- [ ] `class`
- [ ] `role`
- [ ] `enum`
- [ ] `module`
- [ ] `Language`
- [ ] `programs`
- [ ] `sub`
- [ ] `prefix`
- [ ] `listop`
- [ ] `infix`
- [ ] `Routine`
- [ ] `postfix`
- [ ] `postcircumfix`
- [ ] `method`
- [ ] `submethod`
- [ ] `routine`
- [ ] `trait`
- [ ] `term`
- [ ] `twigil`
- [ ] `variable`
- [ ] `syntax`
- [ ] `regex`
- [ ] `regex quantifier`
- [ ] `parameter`
- [ ] `quote`
- [ ] `matching adverb`
- [ ] `regex adverb`
- [ ] `substitution adverb`
- [ ] `hyper`
- [ ] `Phasers`
- [ ] `Asynchronous Phasers`
- [ ] `Python`
- [ ] `constant`
- [ ] `hash (Basics)`
- [ ] `rakudoc`
- [ ] `control flow`
- [ ] `:sym<>`
- [ ] `scalar (Basics)`
- [ ] `statement (Basics)`
- [ ] `string literal (Basics)`
- [ ] `TOP`
- [ ] `topic variable (Basics)`
- [ ] `variable interpolation (Basics)`
- [ ] `declarator`
- [ ] `:cached`
- [ ] `eager (statement prefix)`
- [ ] `gather (statement prefix)`
- [ ] `identifier`
- [ ] `classes`
- [ ] `lazy (statement prefix)`
- [ ] `macros`
- [ ] `pack`
- [ ] `react (statement prefix)`
- [ ] `sink (statement prefix)`
- [ ] `supply (statement prefix)`
- [ ] `<sym>`
- [ ] `->`
- [ ] `is default (Variable)`
- [ ] `try (statement prefix)`
- [ ] `with orwith without`
- [ ] `Reference`

Most of them are absorbed during solution's step 3 into the new standard list.

The standard list is:

- [ ] `Types`
- [ ] `Modules`
- [ ] `Routines` (a common category for something existing as a method and a subroutine)
- [ ] `Subroutines`
- [ ] `Methods`

- [ ] `Terms`
- [ ] `Adverbs`
- [ ] `Traits`
- [ ] `Phasers`
- [ ] `Asynchronous Phasers`
- [ ] `Pragmas`
- [ ] `Variables`

- [ ] `Control flow` (everything related to control flow)
- [ ] `Regexes` (everything related to regex)

- [ ] `Operators` (a common category for something existing as an operator with different application)
- [ ] `Listop operators`
- [ ] `Infix operators`
- [ ] `Metaoperators`
- [ ] `Postfix operators`
- [ ] `Prefix operators`
- [ ] `Circumfix operators`
- [ ] `Postcircumfix operators`

- [ ] `Tutorial`
- [ ] `Foreign` (for terms from other languages and migration guides)
- [ ] `Syntax` (legacy, various bits of language syntax explained at meta-level)
- [ ] `Reference` (legacy, default category for general reference)
- [ ] `Language` (legacy, language-related topics)
- [ ] `Programs` (legacy, program writing-related topics)

# Appendix B: Updates to the list of supported search term categories

The suggested list of categories is proven to be able to cover all
existing index entries for the existing language documentation at the
time of writing.  In case of issues arising, updating it is possible
by starting a discussion with other maintainers and providing the
reasoning behind the change for one of the reasons:

* The current list does not include an important category which
  absolutely does not fit into any of the existing ones
* The current list contains a category hindering the understanding of
  the language in any way
* The current list creates cases where name clashing happens.
  For example, say the `Asynchronous Phasers` category is not stated
  and then both `QUIT` (asynchronous phaser) and `QUIT` ("normal" phaser)
  meaning two different things fall into the same category `Phasers`
  and result in a confusion

But not for one of the following reasons:

* A matter of style or preferences (e.g. `Subroutine` vs `Subroutines`
  or `routine`)
* Overspecializing categories (e.g. splitting `Infix operators` into
  `Set infix operators`, `Compare infix operators` etc.)
