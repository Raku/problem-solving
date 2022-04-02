# Solution – Multiple docs websites

Allow anyone to set up an alternate frontend for the Raku documentation and
serve that frontend via a raku.org subdomain so long as it is built from the
raku/doc source.

## Details

The first section of the raku/doc README currently reads:

```md
# Official Documentation of Raku™

[![artistic](https://img.shields.io/badge/license-Artistic%202.0-blue.svg?style=flat)](https://opensource.org/licenses/Artistic-2.0)
[![CircleCI](https://circleci.com/gh/Raku/doc/tree/master.svg?style=svg)](https://circleci.com/gh/Raku/doc/tree/master)
[![test](https://github.com/Raku/doc/actions/workflows/test.yml/badge.svg)](https://github.com/Raku/doc/actions/workflows/test.yml)

An HTML version of this documentation can be found
at [https://docs.raku.org/](https://docs.raku.org/) and also
at [`rakudocs.github.io`](https://rakudocs.github.io) (which is
actually updated more frequently).
This is currently the recommended way to consume the documentation.
```

This solution document calls for it being changed to:


```md
# Official Documentation of Raku™

[![artistic](https://img.shields.io/badge/license-Artistic%202.0-blue.svg?style=flat)](https://opensource.org/licenses/Artistic-2.0)
[![CircleCI](https://circleci.com/gh/Raku/doc/tree/master.svg?style=svg)](https://circleci.com/gh/Raku/doc/tree/master)
[![test](https://github.com/Raku/doc/actions/workflows/test.yml/badge.svg)](https://github.com/Raku/doc/actions/workflows/test.yml)

This repo contains the source code of the Raku documentation.  The official
version HTML version of this documentation can be found
at [https://docs.raku.org/](https://docs.raku.org/), which is
currently the recommended way to consume the documentation.  Alternate versions
of the documentation can be found at:

   * docs.preview.raku.org – updated more frequently

If you would like to add your own alternate version to this list, please submit
a PR as described in the [`CONTRIBUTING`](/CONTRIBUTING.md#Adding-an-alternate-version).

```

(`docs.preview.org` should proxy to the site currently at `rakudocs.github.io`.)

And add the following section to `CONTRIBUTING.md`:

```md
## Adding an alternate version

Anyone can submit an alternate frontend for the Raku documentation so long as it
is built from the source in this repo.  To do so, please submit a PR with the
name of your site and the base URL at which it is avalible.  Once that PR is
merged, the URL `docs.$your-site-name.raku.org` will proxy to your site and your
site will

To add your own presentation of the documentation in this repo, please submit a
PR with the name of your site and its base URL.  When merged,
`docs.$your-site-name.raku.org` will mirror to your site and your site will be
added to the list in the `README` for this repo.

While it's not necessary for your site to render *all* of the content from this
repo, it should probably render a large fraction – if it doesn't, it might make
sense for your site to be a seperate Raku resouce rather than an alternate
documentation site.  Similarly, your site should not use any non-minimal amount
of content that is *not* in this repo (though it's fine for you to submit PRs to
this repo that your version will render but others will not).  The purpose
behind both of these restrictions is to prevent fragmentation of the effort that
goes into maintaing the Raku documentation's _source_ while still allowing
experimentation in the documentation's _presentation_.

If an alternate version of the documentation gains enough widespread aproval in
the Raku community, that version may be promoted to the official version.  If
so, the current official version will be made avalible as an alternate version.
```
