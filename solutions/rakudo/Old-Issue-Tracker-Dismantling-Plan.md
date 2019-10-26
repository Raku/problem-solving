# Old Issue Tracker Dismantling Plan

[RT](https://rt.perl.org/) was previously used for most of the issue
tracking in the project. Nowadays we use github instead, and RT will
soon become read-only. This document describes the plan for taking
care of the existing RT tickets.

1. All existing tickets will be migrated to github using the tools
developed for the similar transition in the Perl project (big thanks
to [@toddr](https://github.com/toddr) and others!).

  + After migration, links to RT tickets will redirect to appropriate
  github issues.

  + Majority of open tickets on RT are about Rakudo, and the rest are
  related to variety of repos
  ([roast](https://github.com/perl6/roast/),
  [problem-solving](https://github.com/perl6/problem-solving/), etc.).

  + Due to github limitations it is not possible to move tickets
  across organizations, meaning that if we place the repo in `rakudo/`
  organization, it will be relatively hard to move tickets to
  appropriate repos.
  + **Therefore, the repo will be in the `perl6` organization
  ([`perl6/old-issue-tracker`](https://github.com/perl6/old-issue-tracker))**.

2. At least one
[squashathon](https://github.com/rakudo/rakudo/wiki/Monthly-Bug-Squash-Day)
will be devoted to going through open tickets in the created repo and
moving/closing them as required. Rakudo tickets will be simply marked
with a `rakudo` label and will stay in the same repo. Easy
contribution process (no requirement to sign a CLA) in the `perl6/`
organization means that more people will be able to help.

3. After only rakudo tickets are left in the repo, the whole
repository will be transferred to `rakudo/old-issue-tracker`. Links to
RT will keep working either through a double redirect, or the redirect
from RT will be tweaked to point to `rakudo/` organization directly.

