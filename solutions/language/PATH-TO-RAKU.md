# The Path to Raku

This document describes the steps to be taken to effectuate a rename of
`Perl 6` to `Raku`, as described in issue #81.  It does not pretend to be
complete in scope or in time.  To change a name of a project that has been
running for 19+ years will take time, a lot of effort and a lot of
cooperation.  It will affect people in foreseen and unforeseen ways.

## Language changes

## .perl

The `.perl` method should be deprecated in 6.e, and removed in 6.f/g.  It
should be replaced by a `.raku` method.  This could be implemented by a
global search/replace of `.perl` to `.raku` (both in method names and in
calls) in `src/core`, `src/Perl6` and `lib`.  A new `Mu.perl` method should
issue a DEPRECATED message, and then call `Mu.raku`.

Renaming to `\.code` currently clashes with `CallFrame.code`, and visually
clashes with the `\.codes` method.  Renaming `CallFrame.code` to
`CallFrame.codeobj` and `\.codes` to `\.codepoints` appears to be an option
to be discussed further.

## Versioning

Because the next language release (6.e) will not coincide with
the rename, no changes to versioning of the language need to be done.
Given we are no longer forced to have "6" in the version, there are now
more options to do language versioning properly, and this aspect will
need to be discussed separately.

## Documentation changes

Wherever `Perl 6` is mentioned in the documentation, this should be changed
to `Raku`.  A mention of `Perl 6` in the glossary should remain, and maybe
in the documentation of the `.perl` (to be renamed to `.raku`) method.

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
backlog again.  Further down the road, it would probably be a wise idea to
make IRC channel logging one of the infrastructure tasks.

## External references

Many places on the Internet refer to `Perl 6`, e.g. Wikipedia.  These
references will need to be changed to `Raku`, with a small explanation of
the name change.

Many sites, such as Reddit and StackOverflow, use implicit / explicit `perl6`
tags.  These will need to be changed or have a `raku` tag added, possibly
with cooperation of the administrators.

Sites such as PerlMonks appear to be really `Perl 5` focused, and could
possible make that clear in their description, or change their description
to specifically include `Raku`.  It would look like the `/r/perl` Reddit
description can be changed to indicate that only `Perl 5` questions are
on topic there.

## Technical changes

All technical changes should make sure that all existing scripts continue
to work without change in the foreseeable future (with optional DEPRECATED
messages where appropriate).

### Executables

As some packagers have already done, the executable should be called `raku`.
With symlinks / hard links added as appropriate to keep the old executable
names working.  If at all technically possible, running a script using the
`perl6` as the executor, should provide a DEPRECATED warning at some point.

Since `rakudo` is the name of the implementation, the main executable that
is created in the build process should have that name.  `raku` and `perl6`
should be symlinks.

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
marketing result.  This will need a new marketing repo, with `Raku` marketing
materials.  Announcement should be coordinated, e.g. by a blog post on
`opensource.com` or similar outlets, followed up by links on the various
social media outlets, such as Twitter, Facebook, Hacker News, Reddit, etc.

Specific attention should be given for the announcement of Raku in non-latin
script countries, such as India, Japan, China, Taiwan.  Raku with its more
than excellent Unicode support, should be able to make a big splash for
developers in those regions.

## Ecosystem changes

From the standpoint of users, there should not be any change: `zef` should
continue to do what it does.  The information about what is in the ecosystem,
is not related to the "perl 6" name at the moment, so does not need any
change either.

On PAUSE, Perl 6 distributions are automatically uploaded to a "Perl6"
subdirectory, but this is completely transparent to both the author as well
as anything else that needs to look at that.  So for the foreseeable future,
no changes will be needed there.

Should PAUSE decide to no longer support `Raku` modules in its system, then
alternatives will need to be found and/or implemented.

## Effects on modules in ecosystem

Many modules either mention "perl6-" in their repo name, or mention "Perl 6"
in their documentation.  These will have to be scanned and Pull Requests
will need to be made for them.

## Effects on running sub-projects

Projects, such as Comma and Cro do not need any notifications, but other
sub-projects may need to get advance notice.

## Effects on the Perl community

There is a (small) part of the Perl community that welcomes Perl 6 leaving
that community.  But in general, it appears that Perl community members would
like to have channels open between members that only do `Perl 5`, and members
that only do `Raku`.

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