
<!-- README.md is generated from README.Rmd. Please edit that file -->

# piggyback <img src="man/figures/logo.svg" align="right" alt="" width="120" />

<!-- badges: start -->

[![lifecycle](https://img.shields.io/badge/lifecycle-stable-green.svg)](https://lifecycle.r-lib.org/articles/stages.html)
[![R-CMD-check](https://github.com/ropensci/piggyback/workflows/R-CMD-check/badge.svg)](https://github.com/ropensci/piggyback/actions)
[![Coverage
status](https://codecov.io/gh/ropensci/piggyback/branch/master/graph/badge.svg)](https://app.codecov.io/github/ropensci/piggyback?branch=master)
[![CRAN
status](https://www.r-pkg.org/badges/version/piggyback)](https://cran.r-project.org/package=piggyback)
[![Peer Review
Status](https://badges.ropensci.org/220_status.svg)](https://github.com/ropensci/software-review/issues/220)
[![DOI](https://zenodo.org/badge/132979724.svg)](https://zenodo.org/badge/latestdoi/132979724)
[![DOI](http://joss.theoj.org/papers/10.21105/joss.00971/status.svg)](https://doi.org/10.21105/joss.00971)
<!-- badges: end -->

Because larger (&gt; 50 MB) data files cannot easily be committed to
git, a different approach is required to manage data associated with an
analysis in a GitHub repository. This package provides a simple
work-around by allowing larger ([up to 2 GB per
file](https://docs.github.com/en/github/managing-large-files/distributing-large-binaries))
data files to piggyback on a repository as assets attached to individual
GitHub releases. These files are not handled by git in any way, but
instead are uploaded, downloaded, or edited directly by calls through
the GitHub API. These data files can be versioned manually by creating
different releases. This approach works equally well with public or
private repositories. Data can be uploaded and downloaded
programmatically from scripts. No authentication is required to download
data from public repositories.

## Installation

Install from CRAN via

``` r
install.packages("piggyback")
```

You can install the development version from
[GitHub](https://github.com/ropensci/piggyback) with:

``` r
# install.packages("devtools")
devtools::install_github("ropensci/piggyback")
```

## Quickstart

See the [piggyback
vignette](https://docs.ropensci.org/piggyback/articles/intro.html) for
details on authentication and additional package functionality.

Piggyback can download data attached to a release on any repository:

``` r
library(piggyback)
pb_download("iris.tsv.gz", repo = "cboettig/piggyback-tests", dest = tempdir())
#> Warning in pb_download("iris.tsv.gz", repo = "cboettig/piggyback-tests", :
#> file(s) iris.tsv.gz not found in repo cboettig/piggyback-tests
```

Downloading from private repos or uploading to any repo requires
authentication, so be sure to set a `GITHUB_TOKEN` (or `GITHUB_PAT`)
environmental variable, or include the `.token` argument. Omit the file
name to download all attached objects. Omit the repository name to
default to the current repository. See [introductory
vignette](https://docs.ropensci.org/piggyback/articles/intro.html) or
function documentation for details.

We can also upload data to any existing release (defaults to `latest`):

``` r
## We'll need some example data first.
## Pro tip: compress your tabular data to save space & speed upload/downloads
readr::write_tsv(mtcars, "mtcars.tsv.gz")

pb_upload("mtcars.tsv.gz", repo = "cboettig/piggyback-tests")
```

## Git LFS and other alternatives

`piggyback` acts like a poor soul’s [Git
LFS](https://git-lfs.com/). Git LFS is not only expensive, it
also [breaks GitHub’s collaborative
model](https://angryfrenchman.org/github-s-large-file-storage-is-no-panacea-for-open-source-quite-the-opposite-12c0e16a9a91)
– basically if someone wants to submit a PR with a simple edit to your
docs, they cannot fork your repository since that would otherwise count
against your Git LFS storage. Unlike Git LFS, `piggyback` doesn’t take
over your standard `git` client, it just perches comfortably on the
shoulders of your existing GitHub API. Data can be versioned by
`piggyback`, but relative to `git LFS` versioning is less strict:
uploads can be set as a new version or allowed to overwrite previously
uploaded data.

## But what will GitHub think of this?

[GitHub
documentation](https://docs.github.com/en/github/managing-large-files/distributing-large-binaries)
at the time of writing endorses the use of attachments to releases as a
solution for distributing large files as part of your project:

![](man/figures/github-policy.png)

Of course, it will be up to GitHub to decide if this use of release
attachments is acceptable in the long term.

<!--
 When GitHub first came online, it was questioned whether committing binary objects and data to GitHub was acceptable or an abuse of a *source code* repository.  GitHub has since clearly embraced a inclusive notion of "repository" for containing far more than pure source.  I believe attaching data that is essential to replicating an analysis and within the 2 GB file limits enforced by GitHub to be in the same spirit of this inclusive notion, but GitHub may decide otherwise. 
 -->

Also see our [vignette comparing
alternatives](https://docs.ropensci.org/piggyback/articles/alternatives.html).

------------------------------------------------------------------------

Please note that this project is released with a [Contributor Code of
Conduct](https://ropensci.org/code-of-conduct/). By participating in
this project you agree to abide by its terms.

[![ropensci\_footer](https://ropensci.org/public_images/ropensci_footer.png)](https://ropensci.org)
