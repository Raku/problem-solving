Based on https://github.com/Raku/problem-solving/issues/455

The problem
===========

We currently generate wrapper .bat scripts for every installed raku script on Windows. Those bat
wrappers don't kick it and never will. To give one example: It's currently impossible to run `zef
install 'ABC:ver<0.6.13>:auth<zef:colomon>'` on Windows. The `<` and `>` characters don't survive
the trip through CMD-land.

Since we'll touch the wrapper scripts, it makes sense to also revamp the wrappers in general.
Here's an overview:

A CURI (CompUnit::Repository::Installation) is a store represented as a folder for installed
modules. It contains everything a module needs, including the actual `something.raku` script. All
contents (including that script) are stored in a way opaque to the user. Running such an installed
script requires calling the following Raku method:

    CompUnit::RepositoryRegistry.run-script('some_script');

Wrappers which basically only execute the above call are installed in the `curi_store/bin`
subfolder. For those wrapper scripts to work their path must be in PATH or they need to be called
with their absolute path. (Sidenote: On Windows there are no shebang lines. There additional `.bat`
files are generated.)

There are three standard CURIs every Rakudo installation brings with it: `core`, `site` and `vendor`
installed into `$prefix/share/perl6/`. In principle the `site` and `vendor` CURIs could be shared
between multiple Rakudo installations, but that doesn't make much sense, as every installation
brings its own set with it.
There can be additional CURIs in arbitrary places. Which CURIs are used is configurable.

Relocatable Rakudo installations have a well defined directory layout. Things (`rakudo` executables,
CURIs, libs, ...) are always in the same place relative to each other.

Currently our wrapper scripts always just use the rakudo executable found in the path. Actually we
want to be smarter than that.

There are different situations we need to consider:

- Standard CURIs, part of a non-relocatable installation: Typical case when Rakudo is installed by
  the distro. The CURIs belong to a specific Rakudo installation. -> Directly use the full path of
  the `rakudo` excutable in the shebang line.
- Standard CURIs, part of a relocatable installation: Different approaches are possible:
  - Use `env` in the shebang line and hope that the PATH points to the Rakudo this CURI belongs to.
    (That's *not* guaranteed.)
  - Use absolute paths and provide a tool to update the shebang lines. One then needs to call that
    script whenever the installation is moved around. (That's what Strawberry Perl does.)
  - Let the wrapper be a shell script or native executable and dynamically determine the `rakudo`
    path based on the location of the wrapper. Something like `$wrapper_path/../../../bin/rakudo`.
- Custom CURIs: Such CURIs don't belong to a specific rakudo installation, so it's difficult to
  classify them as relocatable or non-relocatable. The only options I see are:
  - Rely on `env` and just use the Rakudo that happens to be in the PATH.
  - Provide a tool to set / change the shebang lines. Then it's up to the user to configure the
    wrappers to his liking.

The solution
============

Three key changes:

1. Two files per script

We put two files in the `bin/` folder. A `*.raku` script, not marked executable, and a `sh` script
(POSIX) / `.exe` (Windows).

2. Native executables on Windows

We generate native executables for each script in a CURI on Windows. This is done by integrating
[Devel-ExecRunnerGenerator][1] into the Rakudo core. It's basically a native executable that can be
tuned by attaching a config to the end of the binary file to do what we want it to do. That
executable is compiled as a part of the Rakudo build process. The executable is 16.5KiB.
Potential problems we'll accept: Dynamically creating executables on Windows might be an anti-virus
/ Microsoft security nightmare.

[1]: https://sr.ht/~patrickb/Devel-ExecRunnerGenerator/

3. Finding the right Rakudo

In every case we want to keep our ability to call a script by passing it to some rakudo:
`/some/rakudo /path/to/curi/share/perl6/site/bin/my_script.raku`.

We do the following:

- When building a non-relocatable Rakudo we default to using absolute paths in the standard CURI
  wrappers.
- When building a relocatable Rakudo, we default to using relative paths in the standard CURI
  wrappers.
- Non-standard CURIs default to using the Rakudo found in `PATH`.

CURIs remember in a configuration file how its wrapper scripts should look. The CURI implementation
in Rakudo will respect this configuration and generate wrappers accordingly. The goal here is to
never mix different wrappers in a specific CURI.

This is implemented in rakudo/rakudo#5725 and is released in Rakudo 2025.06.
