# Search categories

The Pod6 markup language provides a mechanism to "index" text via the `X` code.
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

This feature is implemented via the markup index entries introduced above.
Most of the index entries in the documentation sources follow this pattern:
a "name" (optional), "category" (optional) and a "term".

It is a subset of the Pod6 syntax:

```
X<name|category, term>
# The same as
X<text|entry-level1, entry-level2>
```

* The text specifies what is rendered on a page where an index entry is placed (or nothing to render if absent)
* The category is used to group search results from a common domain. If none is given, the default value
  is 'Reference'
* The term specifies what the user has to use as a search query to navigate to this index entry location

While the Pod6 indexing syntax is quite powerful, one of its main
consumers, the documentation, suffers from this power.

The term categories shown in the search are driven by the sources, which means
that if e.g. a contributor makes a mistake and writes:

```
X<text|entry term,category>
```

Then the `category` term will be indexed under the `entry term` category,
even if the search category `entry term` does not make any sense to the consumer.

In the past, as the documentation emerged with no guidelines for this section,
people had to come up with search categories ad-hoc, sometimes even confusing
the exact indexing syntax in different ways and the list of search "categories"
is now known to be inconsistent and in some cases just incorrect.

On top of this, a notation where the "category" is set using parentheses was introduced:

```
X<text|entry (category)>
```

There are two disadvantages:

* Existing documentation tools do not recognize this ad-hoc notation, resulting in confusing
  results for the reader
* Having this fixed, two different syntax cases for the same things further complicates
  the area we want to have as straightforward as we can

Overall, absence of strict guidelines for terms indexing in the documentation
resulted in various inconsistencies and this can be improved.

# Solution

To address two problems described in the last paragraph, I propose to follow these steps:

1. Introduce guidelines for indexing.

To specify an index entry, the `X<>` markup code is resticted to be one of:

```
X<|term>
X<|$category,term>
X<text|term>
X<text|$category,term>
X<text|$category-1,term-1;...;$category-N,term-N>
```

where `$category` strictly refers to one of the supported categories described in this
document (including possible future extensions), see the next step.

2. Establish a list of supported search categories, see Appendix A.

3. Write a test for the documentation sources to gather all present index entries
   and check them against the approved categories list. The test passes when
   all index entries adhere to the approved categories.

4. Write a test checking index entries do not contain parentheses categories syntax

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
- [ ] `Subroutines`
- [ ] `Methods`
- [ ] `Terms`
- [ ] `Operators`
- [ ] `Adverbs`
- [ ] `Traits`
- [ ] `Phasers`
- [ ] `Syntax` (various bits of language syntax explained at meta-level)
- [ ] `Regex` (everything related to regex)
- [ ] `Control flow` (everything related to control flow)
- [ ] `Raku` (syntax bits such as quoting, literals, identifiers)
- [ ] `Variables`
- [ ] `Reference` (default category for general reference)
- [ ] `Language` (language-related topics)
- [ ] `Programs` (program writing-related topics)
- [ ] `Foreign` (for terms from other languages and migration guides)
