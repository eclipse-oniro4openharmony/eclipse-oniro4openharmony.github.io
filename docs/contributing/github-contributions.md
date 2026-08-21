
Submit contributions as [pull requests](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) to the relevant repository in the [Eclipse Oniro for OpenHarmony organization](https://github.com/eclipse-oniro4openharmony).
The project uses a fork-based pull-request workflow. After creating an account, fork the repository, create a branch for your changes, and open a pull request against the original repository. For general instructions, see GitHub's documentation on [working with forks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks).

## Git Setup

Clone your fork locally, enter its directory, and configure your identity:

```bash
$ git config --local user.email <your_eclipse_account_email>
$ git config --local user.name <your_eclipse_full_name>
```

To push or pull repositories using SSH, [add an
SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) to your profile.

## Commit Guidelines

!!! note
    If you are new to `git`, start by reading the official [Getting Started Document](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup).

At its core, contributing to the project means _wrapping_ your work as
`git` commits. How we handle this has an impact on rebasing,
cherry-picking, back-porting, and ultimately exposing consistent
documentation through its logs.

To achieve this, we maintain the following commit guidelines:

- Each commit should be able to stand by itself providing a building
  block as part of the pull request (PR).
  - A good balance of granularity with scoped commits helps to
    handle backports (for example, cherry-picks) and improves the
    ability to review smaller chunks of code one commit at a time.
- Changes that were added on top of changes introduced in the PR,
  should be squashed into the initial commit.
  - For example, a PR that introduced a new feature and, as a separate
    commit, fixed a build error.
    The latter commit should be squashed into the initial commit.
  - For example, a PR introducing a new documentation chapter and
    adding, as a separate commit, some typo fixes. The latter
    commits should be squashed into the initial commit.
  - There is a small set of exceptions to this rule. All these
    exceptions apply when a PR, even if it
    provides multiple commits in the same scope (for example, to the
    same new feature), each commit has a specific purpose.
    - For example, a line-formatting change followed by a chapter
      addition change in the same documentation file.
    - Also, it can be the case of two functional changes that are
      building blocks in the same scope.
    - Another example where commits are not to be squashed is when
      having a commit moving the code and a commit modifying the
      code in the new location.
- Remove trailing spaces and tabs, and ensure that each file ends with a newline.
- Avoid _merge_ commits as part of your PR. Your commits should be
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
    For example, fixing an issue for a specific part of the code, would
    use the part name as the _scope_.
  - The _title_ (the part after the _scope_) starts with a capital
    letter.
  - The entire _subject_ line shouldn't exceed 80 characters (same
    text wrapping rule for the commit body).
- Separate the commit _body_ from the _subject_ with an empty line.
  line.
  - The commit _body_ is optional but highly recommended. Provide a
    clear, descriptive text block that accounts for all the changes
    introduced by a specific commit.
  - The commit _body_ must not contain more than 80 characters per
    line.
- The commit message will have the commit message _trailers_ separated
  by a new line from the _body_.
  - Each commit requires at least a _Signed-off-by_ trailer line.
    See more as part of the `/contributing/dco` document.
  - All _trailer_ lines are to be provided as part of the same text
    block - no empty lines in between the _trailers_.

Additional commit message notes:

- Avoid using special characters anywhere in the commit message.
- Be succinct but descriptive.
- Have at least one _trailer_ as part of each commit: _Signed-off-by_.
- You can have Git add the _Signed-off-by_ trailer automatically by using
  its `-s` argument.
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

### Creating pull requests

Once your changes have been pushed to your fork, you are ready to
prepare a pull request.

1.  Open your repository in a web browser.
2.  Click **Contribute**, then **Open pull request**. Add a clear description and
    create the pull request. Alternatively, open your fork's webpage. A message
    should indicate that you recently pushed a branch and provide an **Open pull
    request** button.
3.  Before merging, it has to be reviewed and approved by repository
    maintainers. Read their review and add any required changes to your
    pull request.
4.  After you update your pull request, the maintainers will run pipelines to
    verify that your changes do not break the project. If the checks pass and the
    maintainers approve the changes, your work is merged into the
    main project. Remember that each commit of the pull request should
    be a minimum, self-contained building block.
