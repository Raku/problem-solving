# Sunsetting p6c / CPAN ecosystems

This document provides a solution to [issue 316](https://github.com/Raku/problem-solving/issues/316).

## Announcement

The p6c and CPAN ecosystems are to be sunsetted as far as installation of Raku modules is concerned.  This will consist of the following steps:

- Enable the Raku Ecosystem Archive by default in zef immediately
This will allow any installation of a Raku module to look through the [Raku Ecosystem Archive (REA)](https://github.com/raku/REA) if not found on zef, p6c or CPAN ecosystems.

- Disable the p6c / CPAN ecosystems by default in zef immediately
With the REA enabled, the modules that currently reside on p6c and CPAN ecosystems can be downloaded and installed from the REA.

- Migrate the remaining raku-community-modules to zef before July 1st 2022
There are about 95 modules in the raku-community-modules repo that still need migrating to zef ecosystem.  More than 20 have already been moved.  Elizabeth Mattijsen intends to move them all by July 1st, unless someone beats her to it.

- Alert any module authors after July 1st, 2022
Any module authors still not having moved their modules to the zef ecosystem by July 1st 2022, should be informed with a i**single** issue that lists the modules in question.  This could be an issue in the problem solving repo, but another repo would work as well.  The module authors should also be able to indicate they no longer wish to support a module themselves: in that case, a move to the raku-community-modules repo should be considered.

- Stop the ecosystem harvesters on January 1st, 2023
Stop running the ecosystem harvesters of p6c and CPAN.  This will effectively disregard any updates of any modules on those ecosystems.  But any updates with a new version would still become visible in the REA, as that runs its own harvester.

- Offer to move any remaining modules to raku-community-modules
Any authors still having modules in the p6c and CPAN ecosystems, should be offered help in moving their modules to zef and/or the raku-community-modules repo (which would then be followed by a move to the zef ecosystem).

- Stop the REA harvester on July 1st, 2023
Stop running the REA harvester: this would make the p6c and CPAN ecosystems truly sunsetted.

- Security updates on remaining modules
Any module remaining in the p6c and CPAN ecosystem, should be forked in the raku-community-modules repo and re-released with a version bump if a security update to these, clearly abandoned modules, is necessary.
