Commit and Release Policy
=========================

Problem
-------

Problems this solution strives to solve:

- Breakages that surface first during the release process or shortly after happen often.
    - Our CI does not cover all environments we want to support.
    - Our CI does not cover the release process.
    - Our CI does not cover rakudo installations. (Tests are run in the build directory.)
- Our CI is often not able to give clear feedback on new commits.
    - The master branch has errors that are ignored for longer periods of time.
    - There are flappers that sometimes make correct commits fail.
    - Our CI sometimes fails for reasons unrelated to our tests or code. (e.g. GitHub being unavailable)
- There is no clear idea which environments we want to support.


Goals
-----

- Make our CI reliable enough for people to care about its results.
- Clarify which environments we currently support and define how to support other environments.
- Make sure our code stays working for the defined set of environments we want to support.
- Create ways to better deal with test flappers and unrelated failures.
- Make sure the CI tests do not fail for the master development branch.


Pull Request only policy
------------------------

To make sure our master branch stays clean (i.e. don't introduce commits that break tests) we prohibit committing directly to the master branch of our three primary repositories:

- [MoarVM](https://github.com/MoarVM/MoarVM) `master` branch
- [NQP](https://github.com/Raku/nqp) `master` branch
- [Rakudo](https://github.com/rakudo/rakudo) `master` branch

One _must_ create PRs which _must_ be tested successfully before merging them into the `master` branches.

If a PR fails the CI tests and external factors are suspected (e.g. a flapper) one _can_ re run the CI tests. Merging to the master branch is only possible once a PR was tested cleanly at least once.

At the beginning this policy will not be enforced by software.
After a period of one month a GitHub [branch restriction](https://docs.github.com/en/github/administering-a-repository/about-protected-branches) and a _loose_ [PR status check](https://docs.github.com/en/github/administering-a-repository/about-required-status-checks) will be added to the MoarVM, NQP and Rakudo repositories. This will prohibit committing directly to the master branch and only allow merging a PR after a successful test. A _loose_ PR status check does not require the tests to be run (again) against the latest state of master prior to merge.


Supported environments
----------------------

In general we want to support a wide variety of environments. Reasons for _not_ supporting a platform are:

- It is not sensible to use Raku in the environment. (e.g. microcontrollers with only a few megabytes of RAM)
- There are no use-cases that apply to a wider audience. (e.g. old architectures that are not used widely anymore (PowerPC Macintosh))

For an environment to be officially supported it needs to be covered by our CI. Without a CI we can not give any guarantees about the working state of Raku in that environment. So the list of supported platforms is currently mainly limited by what our CI covers.

Configurations we currently test via our CI (only looking at Azure, ignoring Travis and CircleCI, which are bound to be shut down):

```
moar, non-reloc|reloc, x86-64, Windows 10,   MSVC,                dyncall
moar, non-reloc|reloc, x86-64, MacOS 10.15,  clang 10.0,          dyncall
moar, non-reloc|reloc, x86-64, Ubuntu 18.04, gcc 7.3.0|clang 6.0, dyncall|libffi, glibc-2.27
```

Platforms without CI, but for which releases are built:
```
moar, reloc,           x86-64, CentOS 6,     gcc 4.4.7,           dyncall,        glibc-2.12
```

With the above in mind we can claim to support:

- MoarVM
- on x86-64
- Windows 10
    - MSVC
- MacOS
    - clang
- Linux
    - glibc >= 2.12
    - gcc >= 4 and clang

This list will be added to the Rakudo repository in `docs/supported-environments.md`.

To make an environment a supported environment, full CI coverage of that platform (automatically running and passing tests on that platform in MoarVM, NQP and Rakudo repositories) is a prerequisite.


Improvements to our CI pipeline
-------------------------------

Implement a Software to orchestrate the testing process. The software acts as an intermediate between changes in GitHub and the CI services. Supported CI services will be Azure and Open Build Service.

A rough list of what the software does:

- Watch GitHub for changes. For each change a source tarball compatible with our source release files is created and offered for download. CI services are triggered to test this source tarball.
- Status notifications are reported in GitHub.
- Build logs are retrieved and saved.
- Build artifacts are retrieved and saved.
- Watch GitHub PR comments for command words.
    - `{merge on success}` will cause the software to automatically merge the PR should the CI tests be successful. If the tests are unsuccessful a comment is added stating that automatic merging did not happen because the tests failed.
    - `{re-test}` causes the CI pipelines to be triggered to re-test the PR.
- Scan failed CI build logs for known flappers. If a flapper is identified as the only failure the test is re-run automatically once.

By adding OBS we will be able to cover a much broader set of environments. By creating source tarballs first and testing those the CI services are decoupled from GitHub. This should reduce failures caused by GitHub being unavailable. By not only relying on GitHub and CI hooks but also polling for changes, lost notifications are automatically detected and dealt with. By saving the build logs it is possible to look at the test results of prior tests on re-runs (not all CI services support that). By providing a bot that listens for commands in GitHub comments and by automatically scanning for flappers the usability of the CI increases.

