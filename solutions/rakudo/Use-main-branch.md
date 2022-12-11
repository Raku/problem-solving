# Switching Rakudo Primary Developement branch to `main`

Following the decisions mentioned in the following two Raku Steering Council
minutes:

- [Oct 30, 2021](https://github.com/Raku/Raku-Steering-Council/blob/9ccb0d4554fc7b715ac5ecf4a146ab3fc257025c/minutes/20211030.md?plain=1#L47)
- [Oct 15, 2022](https://github.com/Raku/Raku-Steering-Council/blob/9ccb0d4554fc7b715ac5ecf4a146ab3fc257025c/minutes/20221015.md?plain=1#L5)

`RakuAST` branch is getting renamed to `main` and that is then used as the
primary developement branch.

[Rakudo PR #5123](https://github.com/rakudo/rakudo/pull/5123) implements the
principal merging of `master` and `rakuast` branches.

[This comment](https://github.com/rakudo/rakudo/pull/5123#issuecomment-1345059392)
contains the list of actions necessary to finalize the switch.

Solves [problem solving ticket #298](https://github.com/Raku/problem-solving/issues/298).
