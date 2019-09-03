# The Path to Raku

This document describes the steps to be taken to effectuate a rename of
`Perl 6` to `Raku`, as described in issue #81.  It does not pretend to be
complete in scope or in time.  To change a name of a project that has been
running for 19+ years will take time, a lot of effort and a lot of
cooperation.  It will affect people in foreseen and unforeseen ways.

## Language changes

### .perl

The `.perl` method should be deprecated in 6.e, and removed in 6.f/g.  It
should be replaced by a `.raku` method.  This could be implemented by a
global search/replace of `.perl` to `.raku` (both in method names and in
calls) in `src/core`, `src/Perl6` and `lib`.  A new `Mu.perl` method should
issue a DEPRECATED message, and then call `Mu.raku`.

The ClassHOW metamodel class should be adapted so that any non-core modules
that have a `.perl` method, will get installed as `.raku`, and a wrapped
version with a `DEPRECATED` message to be installed as the `.perl` method.

Renaming to `.code` currently clashes with `CallFrame.code`, visually clashes
with the `.codes` method, and it being a generic name, probably also clashes
with CPAN and DarkPan Perl 6 modules.

`$*RAKU` and the `Raku` class will be the replacement of `$*PERL` and the
`Perl` class, which initially will just be aliases.  At a language boundary
switch, they will become actual type declarations.

### Pragmas

The `use isms <Perl5>` should **NOT** be changed to `use isms <Perl>`.
Similarly `use Foo:from<Perl5>` and the `:lang` parameter to `EVAL` should
not be changed as to allow more flexibility should the Perl 5 community
decide on a name change as well.

### Versioning

Because the next language release (6.e) may not coincide with
the rename, no changes to versioning of the language need to be done.  The
next release *will* however be a Raku release, but should be otherwise
completely compatible with previous releases.

A new *language* version would be the opportunity to make the name change
more widely known with associated marketing efforts.  But this will *not*
be with the next (monthlyish) release.

Given we are no longer forced to have "6" in the version, there are now
more options to do language versioning properly, and this aspect will
need to be discussed separately at a later time.

### NQP

The acronym for NQP is Not Quite Perl.  It stays that way, so no changes
to documentation are needed.  Generally, the MoarVM / NQP construct should
not matter a lot for the average user.

## Documentation changes

Wherever `Perl 6` is mentioned in the documentation, this should be changed
to `Raku`.  A mention of `Perl 6` in the glossary should remain, and maybe
in the documentation of the `.perl` (to be renamed to `.raku`) method.

A mention to `Perl 6` should only occur when a redirect from `docs.perl6.org`
has been detected / or a special URL to redirect to has been followed (e.g.
https://documentation.raku-lang.org/?from=perl6).

## Website changes

Websites that have `perl6` in their name, should redirect to their
equivalent with a 301 (Moved Permanently).  Some way should be devised that
if someone is redirected, that a small explanation of the name change is
offered and that the visitor is indeed at the place they were planning to
be.

## IRC Channels

The IRC channels used by Perl 6 users on freenode (#perl6\*) will need to
be renamed or closed / opened with the new name (#raku\*).  New joins should
be forwarded to the new channels.

Colabti.org will need to be asked to log these channels, so that we can have
backlog again.  Unfortunately, it appears to be too difficult to migrate #perl6
logs, so the same approach that was done when the #perl6-dev channel was
created, will need to be followed.  So searching the logs will need to be
done on the old names.

Further down the road, it would probably be a wise idea to make IRC channel
logging one of the infrastructure tasks.

## External references

Many places on the Internet refer to `Perl 6`.  These references will need
to be changed to `Raku`, with a small explanation of the name change.  This
should be a coordinated effort to avoid duplicity of work, and to make sure
that the explanation of the name change is consistent.

Many sites, such as Reddit and StackOverflow, use implicit / explicit `perl6`
tags.  These will need to be changed or have a `raku` tag added, possibly
with cooperation of the administrators.

Sites such as PerlMonks appear to be really `Perl` (aka `Perl 5`) focused,
and could possible make that clear in their description, or change their
description to specifically include `Raku`.  It would look like the
`/r/perl` Reddit description can be changed to indicate that only `Perl 5`
questions are on topic there.

## Technical changes

All technical changes should make sure that all existing scripts continue
to work without change in the foreseeable future (with optional DEPRECATED
messages where appropriate).

### Executables

As some packagers have already done, the executable should be called `rakudo`,
since `rakudo` is the name of the implementation.  `raku` and `perl6` should
be symlinks.  If at all technically possible, running a script using the
`perl6` as the executor, should provide a DEPRECATED warning at some point.

### Extensions

The extension `.rk` for scripts, `.rkm` for modules, and `.rd` for
documentation (POD6) will become the defacto standards for files containing
`Raku` code or documentation.  The old `.pm`, `.pm6` and `.pod6` extensions
will continue to be supported for 6.e.  In 6.f, the `.pm`, `.pm6` and `.pod6`
extensions should be marked as DEPRECATED, causing a message to be generated
when the module is loaded.

On Windows some more trickery may be involved to mark a script with the
`.rk` extension as executable.

### Testing

Roast continues to be the specification of the language.  Only the name
of the language it specifies, changes.  Internal references to `Perl 6`
will need to be changed.

## Mascot

Camelia will remain the mascot.  So the only thing that should change there
is that it is the mascot of `Raku` rather than `Perl 6`.  The fact that the
wings contain a "P" and a "6" is obscure enough to not be an issue, and could
be seen as a lasting tribute (easter egg) to the origin of "Raku".

## Marketing changes

Renaming `Perl 6` to `Raku` is an event that should be used to get maximum
marketing result.  This will need `Raku` marketing materials.  Announcement
should be coordinated, e.g. by a blog post on `opensource.com` or similar
outlets, followed up by links on the various social media outlets, such as
Twitter, Facebook, Hacker News, Reddit, etc.

Specific attention should be given for the announcement of Raku in non-latin
script countries (such as India, Japan, China, Taiwan) by making sure any
announcements are also translated into appropriate languages for these regions.
Raku, with its more than excellent Unicode support, should be able to make a
big splash for developers in those regions.

## Social Media

Users who want to blog about `Raku` are suggested to always at least mention
`Raku Programming Language` in their blog post, or in the boilerplate of their
blog posts.  Mentioning of `Perl 6` in such blog posts is **discouraged**,
unless the blog post is actually about the renaming process or historical
matters.

When using social media that use hash-tags, users are suggested to use the
`#raku-lang` hash-tag, and **not** use the `#perl6` hash-tag, unless the
post is actually about the renaming process or historical matters.

Of course, the same applies to more official social media usage by the core
development team.

## Ecosystem changes

From the standpoint of users, there should not be any change: `zef` should
continue to do what it does.  The information about what is in the ecosystem,
is not related to the "perl 6" name at the moment, so does not need any
change either.

On PAUSE, Perl 6 distributions are automatically uploaded to a "Perl6"
subdirectory, but this is completely transparent to both the author as well
as anything else that needs to look at that as long as a file `META6.json`
exists in the distribution.  So for the foreseeable future, no changes will
be needed there.

Should PAUSE decide to no longer support `Raku` modules in its system, then
alternatives will need to be found and/or implemented.

## Effects on modules in ecosystem

Many modules either mention "perl6-" in their repo name, or mention "Perl 6"
in their documentation.  The documentation will have to be scanned and Pull
Requests will need to be made / generated for them.  Developers will be asked
to change their repo-name to not include `perl6-` to avoid future confusion.

## Effects on running sub-projects

Projects, such as Comma and Cro do not need any notifications, but other
sub-projects may need to get advance notice.  An inventory of these
sub-projects will need to be made, and their maintainers be notified
individually, rather than through social media.  This is both to stress
the urgency for a change, and to be able to present a "clean slate" to the
general public when announcements about the name change *are* made through
social media.

## Effects on books

Currently printed copies of books will probably need a sticker like "Covers
the new exciting Raku programming language).

Perl 6 books that have been open sourced, can be adapted by the community or
the original author.  Since ebook sales currently outperform printed books
by an order of magnitude, preparing another version of an ebook should be
a relatively small effort, which can actually be distributed among many
individuals using modern source control techniques..

## Effects on the Perl community

There is a (small) part of the Perl community that welcomes Perl 6 leaving
that community.  But in general, it appears that Perl community members would
like to have channels open between members that only do `Perl 5`, and members
that only do `Raku`.

The name change of `Perl 6` to `Raku` is also intended to have a healing
effect on a community that has been effectively split for many years.
It is the hope of the Raku core development team that future events will
continue to cater for both `Perl` as well as `Raku` presentations.

## Effects on user groups

Each Perl user group (Perl Monger group) will have to decide for themselves
what they want to do with this new situation.  More active groups in the
past years, have started using the services of online meeting organizers
such as MeetUp.  To indicate the changed situation, it may be a good idea
to change the names of such groups, e.g. from "Foo Perl Mongers Meeting" to
"Foo Perl Family Meetup" or "Foo Perl Community Meetup".  Should a user
group decide to only focus on `Perl 5` or `Raku` only, then they are of
course open to not change their name, or to change it to something like
"Foo Raku Meetup".  This could coincide with the official announcement,
and maybe special events to celebrate the name change.

## Effects on events

Event organizers as always, are completely free in the naming of their events.
If an event would like to cater for both `Perl` and `Raku` attendees, then
this should be reflected in the name of the event.  An event with just `Raku`
in its name, would appear to cater only for attendees interested in `Raku`.
An event with just `Perl` in its name, would appear to cater for attendees
interested in `Perl 5` only.  Suggestions for naming events are (where "Foo"
is a place / country name):

    The Foo Perl Community Workshop
    The Foo Perl Family Workshop
    The Foo Perl and Friends Workshop
    The Foo Perl and Raku Conference
    The Foo Raku and Friends Workshop

etc. etc.

## Relationship with The Perl Foundation

No changes should be necessary with regards to the relationship with The
Perl Foundation.  But this is mostly up to The Perl Foundation.  A suggestion
would be to make the website of The Perl Foundation more general: a Perl
Family.  With equal attention for Perl, Raku, RPerl and CPerl, and emphasis
on continued development on all projects.

Should The Perl Foundation decide to not want to have anything to do with
`Raku`, only then should an alternate organisational support be discussed.

However, since "Yet Another Society" is doing business as "The Perl
Foundation", maybe it is an idea to create another "doing business as"
called "The Raku Foundation".  Which would make it clear that "The Perl
Foundation" is for Perl 5 only, whereas "The Raku Foundation" would be for
Raku only.  While both are part of the Perl Mindset in the "Yet Another
Society".
