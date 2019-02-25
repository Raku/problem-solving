# 🦋 Problem Solving

`perl6/problem-solving` repository is used for working on all issues
that require discussion and/or consensus. This document describes the
process in more detail.


## Reporting a problem

Anyone is welcome to create a new issue. The ticket should *only*
contain the description of the actual problem (X of the
[XY](https://en.wikipedia.org/wiki/XY_problem)). It is advised that
you start your ticket with a short description of the problem and
include more details afterwards.

This repository has broad scope, but not all problems belong
here. For example, bugs in the Rakudo compiler should be reported in
[rakudo repository](https://github.com/rakudo/rakudo/issues).

## Initial proposal

Anyone is welcome to submit an initial proposal as a comment. Start
your comment with “Initial proposal:” and follow with a short and
clear description of the approach that can be used to resolve the
problem. Include just enough details to paint the general picture and
refrain from writing too much (which will be required for the next
step, but not now).

Good solutions resolve more than one problem, so link all other
related problems that will be affected by your solution.

If you know that you won't be able to work on a full solution, please
indicate so in your comment. Having a hero is often more important
than having a good idea, but it is also important to hear out good
heroless ideas.


### Assignees and their role

Issues and PRs are labeled and assigned to corresponding devs (see
[the list](#labels-and-responsible-devs)). The task of the assignee is
to drive the process, provide vision and assist everyone
involved. Assignees can provide feedback, ask for clarifications,
suggest changes and so on as they see fit. They can also reject
initial proposals early.

Labels and assignees are managed in different ways (automatically
by a bot, manually, self-assignment, etc).

If an assignee is pleased with the initial proposal, they can ask to
submit a fullblown solution. This moves us to the next step. Assignees
should provide their expectations of the solution (e.g. what should be
covered in the document, additional requirements, etc).


## Submitting a solution

PRs can only be submitted once all previous stages are completed. PRs
should be submitted in `perl6/problem-solving` repository. The
document in the PR should act as a documentation for the solution (as
it will also be read later after it is accepted). This time all
details should be provided.

Assignees will request changes that are necessary to finalize the
solution. Note that this process may require an actual implementation
(if applicable) and other efforts (possibly in other repos) if so is
required by the assignee.

If there are common principles that are used for a particular label,
assignees are welcome to compose a document in order to avoid
repeating themselves.

Note that all people (even non-devs) who have enough experience in
affected areas are welcome to provide feedback. That being said, the
assignee has the final word on decisions in the issue/PR, so they are
free to engage in discussions as they see fit. In rare cases they can
moderate/lock the discussion if so is unavoidable.

Once the assignee is happy with the proposal, the process moves to the
next step.


## Acceptance

A review is requested from all of the reviewers (see
[the list](#reviewers)). This step exists to ensure peer-review and to
notify the devs of the major upcoming changes. By approving a PR the
dev confirms that they reviewed and understood the proposal, and that
they are OK with it. This is also the last chance to express concerns
and present feedback (though it is encouraged to do that before this
step is reached).

It is expected that all reviewers will understand the reasoning of the
solution and in the end all of them will approve the PR. This may
require time, discussion, and minor adjustments, but if it happens
that the discussion doesn't lead to mutual understanding, the BDFL may
step in.

This is the final step. Once the PR is merged, it is expected that
corresponding efforts (e.g. PRs in other repos) are merged too, and
the future work is already outlined. This is the last step.



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
* If a reviewer does not respond in 14 days, they are removed from the
  list of reviewers on that PR.
* Passing the assignee status is allowed provided that the receiving
  party agrees. One-time assignees are allowed through this process.
* People are allowed to be assigned to their own PRs.
* If everything else fails, BDFL can force any change as they
  like, though this should only be used in exceptional cases.


## Labels and responsible devs

File a `meta` issue if you want to create a new label or if you want
to be added as a responsible dev.

* `meta` – changes to the `problem-solving` repo and this document
  * @AlexDaniel
  * @jnthn
* `language` – changes to the Perl 6 language
  * @jnthn
* `rakudo` – big changes to rakudo
  * @jnthn
* `moarvm` – big changes to moarvm
  * @jnthn
* `documentation`
  * @JJ



## Reviewers

File a `meta` issue if you want to be added to this list.

* @AlexDaniel
* @jnthn
* @JJ
* …
