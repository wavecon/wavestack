<!-- SPDX-License-Identifier: CC-BY-4.0 -->
<!-- Copyright (C) 2023 Wavecon GmbH -->

The Wavestack project welcomes all contributions.

Please make sure that you adhere to the following requirements:

- Git commits are signed
- Commit messages adhere to the guidelines detailed below
- You agree to the Developer Certificate of Origin (DCO) and include a
  suitable sign-off in the git commit message


## Getting started

The handbook is created using [Hugo][hugo], which is an open-source
static site generator that takes Markdown as input. We use the
[Docsy][docsy] theme that has been specifically designed for technical
documentation.

If you have not used Hugo or Docsy before, we recommend to familiarise yourself
with the following articles:

- https://gohugo.io/getting-started/
- https://gohugo.io/content-management/
- https://www.docsy.dev/docs/adding-content/

The specific Markdown dialect used by Hugo is [CommonMark][commonmark]
as implemented by the [goldmark][goldmark] library. The CommonMark
project provides a full specification of the markup language and a
handy tutorial for learning it. These can be found on:

- https://commonmark.org/help/
- https://commonmark.org/help/tutorial/
- https://spec.commonmark.org/0.30/

Contributions are accepted via [git][git]. Everything you ever wanted
to know about git (and more!) can be found on:

- https://git-scm.com/book/en/v2

and we would recommend to read at least the following:

- [Chapter 1 - Getting Started][git-scm-getting-started]
- [Chapter 2 - Git Basics][git-scm-git-basics]
- [Chapter 3 - Git Branching][git-scm-git-branching]
- [Chapter 5 - Distributed Git][git-scm-distributed-git]
- [Chapter 7.6 - Git Tools - Rewriting History][git-scm-rewriting-history]

## Editor Setup

You can use [editorconfig][editorconfig] to automatically configure a
subset of editor settings to defaults defined for this project.

Editorconfig comes [already pre-installed][editorconfig-pre-installed]
for a number of environments or can be easily added as [a
plugin][editorconfig-plugin].

## Code of Conduct

We would like our community to remain open and welcoming to everyone
and kindly ask you to adhere to the [CNCF Code of Conduct][cncf-coc].

## Developer Certificate of Origin (DCO)

By contributing to this project you are agreeing to the [Developer
Certificate of Origin (DCO)](lf-dco) which is a document created by
the Linux Foundation that is:

> a per-commit sign-off made by a contributor stating that they agree
> to the terms published at https://developercertificate.org/ for that
> particular contribution.

We therefore require you to include a sign-off in every commit, by
which you certify that your contribution adheres to the rules of the
[DCO](DCO):

`Signed-off-by: Jane Doe <jdoe@example.com>`

The sign-off line can be automatically added to the commit message if
you call `git commit` with `-s`.

## Scope of changes

When contributing or planning a change, you should make sure that the
feature you are working on addresses an actual need and is in line
with the direction of this project.

Whilst many of your changes will be small, you might still want to
discuss them with the maintainers or your colleagues before you start
your work to ensure that your contribution is likely to get accepted.

We also encourage you to keep commits and merge requests small as they
will be easier to review as a result.

### Commits

Each commit in the history of the project tells a story. At best they
express the evolution of a feature in semantically distinct units in a
way that can be easily followed. Strive for breaking up your commits
into units that can be applied or reverted independently.

Keep in mind that the git log serves as *permanent* record of your
contribution and by breaking up larger changes into smaller steps you
make it easier for everyone to follow along and understand your
changes.

### Merge requests

Merge requests are easiest to review if they are small and focused on
a single problem or feature. Be kind to your reviewers and create a
new merge request for every semantically different change you want to
make.

Strive for making every merge request useful in its own right. If a
commit introduces a bugfix or feature that has value outside the scope
of the specific merge request you are working on then it should be in
a separate MR as that allows it to be merged faster and deployed
independently.

Sometimes your work is not yet finished, but you still want to gather
initial feedback. You can create a [draft merge
request][gitlab-draft-mr] in that situation by adding `[Draft]` at the
beginning of the merge request title. Doing so prevents it from being
merged accidentally, while simultaneously allowing others to leave
comments and reviews.

## Commit message guidelines

Git commit messages tell a similar story to your merge requests or
commits. But while the commits themselves focus on the *how* of
changes, commit messages elaborate on the *why* and their context.

Peter Hutterer [motivates this well][phutterer-on-commit-msg]:

> Re-establishing the context of a piece of code is wasteful. We can’t
> avoid it completely, so our efforts should go to reducing it [as
> much] as possible. Commit messages can do exactly that and as a
> result, a commit message shows whether a developer is a good
> collaborator.

One of the most important aspects of well written commit messages is
that, once messages are of consistently high quality, they will
actually be read often. That is, they will become another important
tool in toolbox. A tool that can be used to understand the context of
every single line of code you find in the code base.

A typical commit message has two parts:

```
Summary

Commit body, that can span several lines...
...
...
...
```

[Chris Beams][cbeams] defined [seven rules of a great commit
message][cbeams-seven-rules]:

1. Separate subject from body with a blank line
1. Limit the subject line to 50 characters
1. Capitalize the subject line
1. Do not end the subject line with a period
1. Use the imperative mood in the subject line
1. Wrap the body at 72 characters
1. Use the body to explain what and why vs. how

and even provides an example of a good commit message:

```
Summarize changes in around 50 characters or less

More detailed explanatory text, if necessary. Wrap it to about 72
characters or so. In some contexts, the first line is treated as the
subject of the commit and the rest of the text as the body. The
blank line separating the summary from the body is critical (unless
you omit the body entirely); various tools like `log`, `shortlog`
and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you
are making this change as opposed to how (the code explains that).
Are there side effects or other unintuitive consequences of this
change? Here's the place to explain them.

Further paragraphs come after blank lines.

 - Bullet points are okay, too

 - Typically a hyphen or asterisk is used for the bullet, preceded
   by a single space, with blank lines in between, but conventions
   vary here

If you use an issue tracker, put references to them at the bottom,
like this:

See:
- #456
- https://www.example.com/some/relevant/link

Signed-off-by: Jane Doe <jdoe@example.com>
```

If you like to learn more about this, you can read the following
articles:

- [Chris Beams - How to Write a Git Commit Message][cbeams-commit-msg]
- [Peter Hutterer - On commit messages][phutterer-on-commit-msg]
- [Tim Pope - A Note About Git Commit Messages][tpope-note-commit-msg]
- [Git SCM Book - Distributed Git - Contributing to a
  Project][git-scm-contribute]
- [Erlang - Writing good commit messages][erlang-otp-commit-msg]

<!-- References -->

[cbeams-commit-msg]: https://cbea.ms/git-commit/
[cbeams-seven-rules]: https://cbea.ms/git-commit/#seven-rules
[cbeams]: https://cbea.ms
[cncf-coc]: https://github.com/cncf/foundation/blob/main/code-of-conduct.md#contributor-code-of-conduct
[commonmark]: https://commonmark.org/
[docsy]: https://www.docsy.dev/
[editorconfig]: https://editorconfig.org/
[editorconfig-pre-installed]: https://editorconfig.org/#pre-installed
[editorconfig-plugin]: https://editorconfig.org/#download
[erlang-otp-commit-msg]: https://github.com/erlang/otp/wiki/writing-good-commit-messages
[git-scm-contribute]: https://git-scm.com/book/en/v2/Distributed-Git-Contributing-to-a-Project
[git-scm-distributed-git]: https://git-scm.com/book/en/v2/Distributed-Git-Distributed-Workflows
[git-scm-getting-started]: https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control
[git-scm-git-basics]: https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository
[git-scm-git-branching]: https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell
[git-scm-rewriting-history]: https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History
[gitlab-draft-mr]: https://docs.gitlab.com/ee/user/project/merge_requests/drafts.html
[goldmark]: https://github.com/yuin/goldmark
[hugo]: https://gohugo.io/
[phutterer-on-commit-msg]: https://who-t.blogspot.com/2009/12/on-commit-messages.html
[tpope-note-commit-msg]: https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
[lf-dco]: https://wiki.linuxfoundation.org/dco
