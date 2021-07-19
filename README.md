# 🦋 Problem Solving

`Raku/problem-solving` repository is used for working on all issues
that require discussion and/or consensus. This document describes the
process in more detail.

## The Problem-Solving Process
### Step 1: Reporting a problem

Anyone is welcome to report a problem by creating a new issue. The issue should *only*
contain the description of the actual problem (X of the
[XY](https://en.wikipedia.org/wiki/XY_problem)). The issue you
create should start with a short description of the problem,
followed by any additional details, if needed.

This repository has broad scope, but not all problems belong
here – only problems where consensus needs to exist but doesn't yet.
For example, if everyone already agrees on the proper approach to 
solving a problem, then that issue isn't appropriate for this repo 
(even if a few technical questions about the implementation still 
need to be worked out).  As a more concrete example, bugs in the 
Rakudo compiler should be reported in
[rakudo repository](https://github.com/rakudo/rakudo/issues).

If someone opens an issue that doesn't belong in Problem Solving,
then anyone with sufficient GitHub permissions to do so should close
the issue and either direct the user to the correct place to address 
this issue (e.g., the Rakudo repo) or open an issue themselves.

### Step 2: Initial proposed solution

Anyone (including the person who opened the issue) can
submit an initial proposal as a comment in reply to the issue. 

To do so, start a comment with “Initial proposal:” and provide a short and
clear description of the solution you're suggesting. Include just enough 
details to paint the general picture and refrain from writing too much 
(which will be required for the next step, but not now).

Good solutions may resolve more than one problem: if so, link all other
related problems that will be affected by your solution.

By proposing a solution, you are _typically_ volunteering to implement 
that solution.  If you know that you won't be able to work on a 
full solution, please **say so when proposing that solution**. Having a 
hero who will carry a solution though to implementation is often 
essential, but it is also  important to hear out good heroless ideas.

### Step 3: Discussion of the proposal

After the initial proposal, everyone is encouraged to discuss the idea and point
out any flaws, implementation details, suggested changes, etc. that 
they might see.  (Of course, both before and during step 3, people 
can discuss the problem generally rather than a particular solution.)

In step 3, anyone can comment, but two people have an especially 
important role to play: the person who wrote the original proposal
and the _assignee_ for the issue.

#### Assignees

Issues are assigned to corresponding devs (see
[the list](#labels-and-responsible-devs)). The assignee's job is
to help drive the process, provide vision, and assist everyone
involved. Assignees can provide feedback, ask for clarifications,
suggest changes and so on as they see fit. 

Assignees can provide feedback on an initial solution, provide guidance
about what the full solution should include (e.g. what should be
covered in the document, additional requirements, etc). They can also 
reject initial proposals at step 3 (though this shouldn't happen often).

If an assignee is pleased with the initial proposal, they can ask to
submit a fullblown solution.  Alternatively, if the person who suggested 
the solution feels they have received enough feedback, they can decide to
submit a full solution.  Either way, submitting a solution moves us to the
next step.


### Step 4: Submitting a full solution

After a problem/solution has been discussed sufficiently, someone should
submit a Pull Request with a detailed proposal that would solve the problem.  
This PR should add a document to the "solutions" directory in this repo.
Typically, the solution will come after a user has officially proposed an
initial solution (as described in Step 2, above) and, typically, the PR would
be written by the same user who wrote that initial solution.

However, neither of these part of the above is _absolutely_ required.  For 
example, if someone writes an initial solution but doesn't have time (or no
longer has time) to work on implementing the solution, it's fine for someone 
who does have time to submit the PR.  Similarly, if an initial solution has 
been discussed without anyone _formally_ writing an "initial solution" comment,
someone who was involved in that discussion can step forward and draft a PR. 
(This can sometimes happen when one user has a partial solution and other
users add to it without anyone drafting a formal initial solution.)  That said,
it's still better to have a written initial solution and for the PR to be written
by the same person.

The PR should act as a documentation for the solution and should provide
all details that are required to implement the solution. Keep your document
consistent with other files in the solutions directory (naming, directory
structure, markup and so on).

At this point, anyone can provide feedback on the full solution and, if
desired, the PR author can revise the PR based on that feedback.


### Step 5: Solution resolution

Once someone has submitted a PR, it can be resolved in one of 4 ways:

1. Consensus acceptance
2. Speedy acceptance
3. Acceptance without consensus
4. Non-acceptance

#### Consensus acceptance

The most common way for a proposal to be accepted is for the discussion to
proceed to a point where a consensus exists in favor of the solution.  A 
consensus does not require _unanimity_, but it should be clear that the
Raku community as a whole supports the solution.  In particular, if 14 days
pass between the time when the PR (or the latest revision of the PR, if it's 
edited) was proposed and none of the [reviewers](#reviewers) (listed below) 
have objected, then the proposer of the PR can use their judgement to determine
whether consensus exists.  However, if any of the reviewers objects to the PR's 
solution or 14 days have not yet passed, then consensus does *not* support
that solution.

#### Speedy acceptance

If all reviewers approve a solution, then it can be accepted even if 14 days
have not passed.

#### Acceptance without consensus

In **very** rare cases, a PR's solution can be accepted even when the Raku
community cannot reach consensus.  When the Problem Solving process was first
adopted, the way Raku solved problems when consensus couldn't be reached was by
an action by the Benevolent Dictator for Life (Larry Wall).  After the [Raku 
Steering Council Code](https://github.com/Raku/Raku-Steering-Council/blob/main/papers/Raku_Steering_Council_Code.md) 
was adopted, the process for resolving deadlock shifted to the RSC as 
described in that document.  In either case, this power should be exercised 
only as a last resort and in extremely exceptional cases.

#### Non-acceptance

If a solution is not accepted by any of the methods described above, then 
it is not accepted.  Arguably, this isn't a resolution at all – the problem
remains unsolved and the issue stays open.  But that particular solution
has failed to gain acceptance (though, of course, a different solution or 
even a revised version of the same solution could later be accepted).


## Edge cases and other notes

* If any of the merged solutions needs an adjustment, the process should
  start from the beginning. That is, an issue should be filed stating
  the problem with the current solution, and the process continues as
  normal. PRs are allowed to change, modify and shadow existing
  solutions.
* Assignees are allowed to call for a “shortcut” to any problem, in
  which case the solution is applied directly without going through
  the whole process.
* Non-functional changes to existing solutions automatically go
  through a shortcut (typos, grammar, formatting, etc.), just submit
  a PR right away.
* If a shortcut receives any criticism from the corresponding
  development team or other affected parties, it can be reverted and
  the full formal process should begin.
* Passing the assignee status is allowed provided that the receiving
  party agrees. One-time assignees are allowed through this process.
* People are allowed to be assigned to their own PRs.



## Labels and responsible devs

File a `meta` issue if you want to create a new label or if you want
to be added as a responsible dev.

* `meta` – changes to the `problem-solving` repo and this document
  * [@codesections](https://github.com/codesections)
* `language` – changes to the [Raku language](https://github.com/perl6/roast/)
  * –
* `rakudo` – big changes to [Rakudo](https://github.com/rakudo/rakudo/)
  * –
* `moarvm` – big changes to [MoarVM](https://github.com/MoarVM/MoarVM)
  * –
* `documentation` – big changes to
  [Raku documentation](https://github.com/Raku/doc/) and other learning
  resources
  * [@JJ](https://github.com/JJ)
* `unicode` – Unicode and encoding/decoding
  * [@samcv](https://github.com/samcv)
* `infrastructure` - servers, hosting, cloud, monitoring, backup and automation
  * [@rba](https://github.com/rba)
  * [@maettu](https://github.com/maettu)
* `fallback` – if no other label fits
  * [@codesections](https://github.com/codesections)

## Reviewers

File a `meta` issue if you want to be added to this list.

* [@codesections](https://github.com/codesections)
* [@FCO](https://github.com/FCO)
* [@JJ](https://github.com/JJ)
* [@lizmat](https://github.com/lizmat/)
* [@maettu](https://github.com/maettu)
* [@masak](https://github.com/masak)
* [@MasterDuke17](https://github.com/MasterDuke17)
* [@rba](https://github.com/rba)
* [@samcv](https://github.com/samcv)
* [@timo](https://github.com/timo)
* [@tony-o](https://github.com/tony-o)
* [@vrurg](https://github.com/vrurg)
