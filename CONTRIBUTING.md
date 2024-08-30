# Contributing to Vulnrichment

Thank you for your interest in making the [Vulnrichment](https://github.com/cisagov/vulnrichment) project better!
Here are some basic guidelines to ensure that we all have the best experience possible with incorporating any
changes you might be suggesting.

## Opening an issue

In many cases, you may not have a specific fix in mind, and that's okay! Just file an
[issue](https://github.com/cisagov/vulnrichment/issues), describing the problem you've spotted as best you can,
ideally with screenshots and any other details you can provide, along with what you expected to happen.

## Pull Requests

Patches accepted! Please note, though, that there's a bit of a wrinkle with submitting PRs against CVE JSON files
in Vulnrichment repository. Because these files are generated through a backend process here at CISA, your changes
are quite likely to be overwritten the next time the corpus of enriched CVEs are generated, and your specific
PR'ed commit may be lost. However, we will make every effort to merge your PR in the normal GitHub way, so even in that
case, your contribution will be recognized by GitHub and your account will be listed as a contributor. Occasionally,
though, such merges can create conflicts which we'd rather avoid. This is especially true if you are submitting a PR
that affects a large number of CVEs. In these cases, the PR is still helpful to see what you're trying to accomplish,
but the ultimate change may be a systemic change on our backend processing, and your original PR may be closed without
merging, even though the backend generator has been updated to effect your change on subsequent runs.

As for anything that does not touch a CVE JSON file, such as changes to documentation and other assets, please feel
free to submit those PRs in the usual way, and expect those to be merged if the changes are acceptable to this
repository's maintainers.

For more detail and discussion on how we handle PRs, please see [Issue #78](https://github.com/cisagov/vulnrichment/issues/78).

## Thank You

Again, thanks so much for taking the time to contribute to [Vulnrichment](https://github.com/cisagov/vulnrichment).
This project's maintainers depend on external, third-party input to make sure we're delivering the best experience
we can when it comes to enriching CVE data, and we most definitely couldn't do it without people like you.
