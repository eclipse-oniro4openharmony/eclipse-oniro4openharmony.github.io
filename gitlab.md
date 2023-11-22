---
title: Gitlab Contributions
parent: Contribution
layout: default
---

# Overview

handles contributions as [merge
requests](https://docs.gitlab.com/ee/user/project/merge_requests/) to
relevant repositories part of the [GitLab
instance](https://gitlab.eclipse.org/eclipse/oniro-core). The flow for
handling that is classic: fork-based merge requests. This means that
once you have an account, you can fork any repository, create a branch
with proposed changes and raise a merge request against the forked
repository. More generic information you can find on the Gitlab\'s
documentation as part of [\"Merge requests
workflow\"](https://docs.gitlab.com/ee/development/contributing/merge_request_workflow.html).

# Git setup

Clone your fork locally, enter its directory and set:

```bash
$ git config --local user.email <your_eclipse_account_email>
$ git config --local user.name <your_eclipse_full_name>
```

To push and pull over HTTPS with Git using your account, you must set a
password or [a Personal Access
Token](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html)
.

If you want to push or pull repositories using SSH, you have to [add a
SSH key](https://docs.gitlab.com/ee/user/ssh.html) to your profile.

# Commit Guidelines

> 💡 **NOTE：** If you are new to `git`, start by reading the official [Getting Started Document](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup).

At its core, contributing to the project means _wrapping_ your work as
`git` commits. How we handle this has an impact on rebasing,
cherry-picking, back-porting, and ultimately exposing consistent
documentation through its logs.

To achieve this, we maintain the following commit guidelines:

- Each commit should be able to stand by itself providing a building
  block as part of the MR.
  - A good balance of granularity with scoped commits helps to
    handle backports (e.g. cherry-picks) and also improves the
    ability to review smaller chunks of code taking commit by
    commit.
- Changes that were added on top of changes introduced in the MR,
  should be squashed into the initial commit.
  - For example, a MR that introduced a new build system recipe and,
    as a separate commit, fixed a build error in the initial recipe.
    The latter commit should be squashed into the initial commit.
  - For example, a MR introducing a new docs chapter and also
    adding, as a separate commit, some typo fixes. The latter
    commits should be squashed into the initial commit.
  - There is a small set of exceptions to this rule. All these
    exceptions gravitate around the case where an MR, even if it
    provides multiple commits in the same scope (for example, to the
    same build recipe), each of the commits has a very specific
    purpose.
    - For example, a line formating change followed by a chapter
      addition change in the same documentation file.
    - Also, it can be the case of two functional changes that are
      building blocks in the same scope.
    - Another example where commits are not to be squashed is when
      having a commit moving the code and a commit modifying the
      code in the new location.
- Make sure you clean your code of trailing white spaces/tabs and that
  each file ends with a new line.
- Avoid _merge_ commits as part of your MR. Your commits should be
  rebased on top of the _HEAD_ of the destination branch.

As mentioned above, _git log_ becomes informally part of the
documentation of the product. Maintaining consistency in its format and
content improves debugging, auditing, and general code browsing. To
achieve this, we also require the following commit message guidelines:

- The _subject_ line (the first line) needs to have the following
  format: `scope: Title limited to 80 characters`.
  - Use the imperative mood in the _subject_ line for the _title_.
  - The _scope_ prefix (including the colon and the following
    whitespace) is optional but most of the time highly recommended.
    For example, fixing an issue for a specific build recipe, would
    use the recipe name as the _scope_.
  - The _title_ (the part after the _scope_) starts with a capital
    letter.
  - The entire _subject_ line shouldn\'t exceed 80 characters (same
    text wrapping rule for the commit body).
- The commit _body_ separated by an empty line from the _subject_
  line.
  - The commit _body_ is optional but highly recommended. Provide a
    clear, descriptive text block that accounts for all the changes
    introduced by a specific commit.
  - The commit _body_ must not contain more than 80 characters per
    line.
- The commit message will have the commit message _trailers_ separated
  by a new line from the _body_.
  - Each commit requires at least a _Signed-off-by_ trailer line.
    See more as part of the `/contributing/dco`{.interpreted-text
    role="doc"} document.
  - All _trailer_ lines are to be provided as part of the same text
    block - no empty lines in between the _trailers_.

Additional commit message notes:

- Avoid using special characters anywhere in the commit message.
- Be succinct but descriptive.
- Have at least one _trailer_ as part of each commit: _Signed-off-by_.
- You can automatically let `git` add the _Signed-off-by_ by taking
  advantage of its `-s` argument.
- Whenever in doubt, check the existing log on the file (`<FILE>`) you
  are about to commit changes, using something similar to:
  `git log <FILE>`.

Example of a full git message:

```text
busybox: Add missing dependency on virtual/crypt

Since version 1.29.2, the busybox package requires virtual/crypt. Add this
to DEPENDS to make sure the build dependency is satisfied.

Signed-off-by: Joe Developer <joe.developer@example.com>
```

# Contributions to Documentation

In , the documentation usually stays with the respective code
repositories. This means that contributing to documentation is not in
any way different than contributing to code. The processes, contribution
guidelines are to remain the same. The only difference is that
documentation files are to be released under
`Creative Commons License version 4.0`.

Documentation that doesn\'t link directly to one specific repository, is
available in the [docs
repository](https://gitlab.eclipse.org/eclipse/oniro-core/docs).

In terms of file format, the project unifies its documentation as
`ReStructuredText` files. A RestructuredText primer is available as part
of the Sphinx
[documentation](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html).

As a rule of thumb, anything that ends up compiled in the project
documentation is to maintain the RestructuredText file format. Text
files that are not meant to be compiled as part of the project\'s
documentation can be written in
[Markdown](https://daringfireball.net/projects/markdown/). For example,
a repository `README` file can be written in Markdown as it doesn\'t end
up compiled in the project-wide documentation.

## Creating merge requests

Once your changes have been pushed to your fork, you are ready to
prepare a merge request.

1.  Go to your repository in an internet browser.
2.  Create a merge request by clicking `Merge Requests` on left toolbar
    and press `New merge request`. Add an explainable description and
    create a merge request. Alternatively, you can enter the website of
    your fork. You should see a message that you pushed your branch to
    the repository. In the same section you can press
    `Create merge request`.
3.  Before merging, it has to be reviewed and approved by repository
    maintainers. Read their review and add any required changes to your
    merge request.
4.  After you polish your merge request, the maintainers will run the
    pipelines which check if your changes do not break the project and
    approve them. If everything is correct, your work is merged to the
    main project. Remember that each commit of the merge request should
    be a minimum, self-contained building block.
