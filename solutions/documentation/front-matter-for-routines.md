# Front matter for routine pages

The [documentation for Raku](https://docs.raku.org) is generated from
the [source](https://github.com/Raku/doc) by processing the primary
content (in pod6 format), and generating secondary content that will
depend on the type of the page. As a matter of fact, there are two
rough kind of pages:

* Tutorial pages, which are free format, but can be indexed.
* Class pages, which have a more or less fixed format, are indexed.

Several types of secondary pages are generated from that, but we'll
focus on *routine* pages. These pages are extracted from those two
kind of pages, but other than that they don't have any kind of
additional content other than the routines (and methods) in
lexicographical order of the classes they are declared for.

This also provokes a series of hacks, needed to index and generate
pages for independent routines, as well as operators, which are
actually routines too. But the bigger problem is that there's no
tutorial-style content for routines, forcing the user to hopscotch
around from one definition to the next to find the real semantics.

Since the Big Summer Split, documentation and its rendering is
decoupled, with documentation (and some stuff that will be eventually
split) in the Raku/doc repo, and Documentable, which is the API, in
the Raku/Documentable repo. I propose to follow these steps to be able
to index and document routines separately:

1. Almost inmediately, we can change the sorting order of the
   routines/methods in the current pages from lexicographical on the
   class/Role name they're defined to "hierarchical" order, with
   routines lower in the hierarchy first. This will help somehow by
   focusing on improving those versions of the routine, while the rest
   of the solution is worked out.
1. Add a new `kind` "Routine" to Documentable
   (see
   [documentation](https://raw.githubusercontent.com/Raku/Documentable/master/docs/Language/document-format.pod6) for
   this.
2. Process that routine in such a way that it has an introduction
   (like Types right now) + collation of all routines and methods with
   the same name.
3. Ensure this works for operators too.
4. Ensure that current URLs are maintained.
4. Write boilerplate for routines that will not have independent
   documentation, so that it defaults to what we have now. Make it reasonable.
4. Split
   the
   [page for independent routines](https://docs.raku.org/language/independent-routines) in
   as many pages for routines to seed the documentation.
   
