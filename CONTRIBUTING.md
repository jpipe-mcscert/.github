# Contributing to jPipe

This is how work is organized in every repository of the
[`jpipe-mcscert`](https://github.com/jpipe-mcscert) organization. GitHub shows this guide
in any repository that has no contributing guide of its own. A repository that has one
(`CONTRIBUTING.md` or `docs/contributing.md`) links here and adds only what is specific
to it, such as the development setup, the tests to run, or the CI checks. It does not
change the workflow described here.

Open an issue before starting anything larger than a fix, so that we can agree on the
approach before you write the code.

## Branches

| Branch | Role |
|--------|------|
| `main` | Released code. A release is a tag on `main` (`vX.Y.Z`). |
| `dev` | The integration branch, and the default branch on GitHub. New work starts here and comes back here. |
| a work branch | One per piece of work, cut from `dev`: `feat/…`, `fix/…`, `chore/…`, `docs/…`. |

Changes reach `main` only through a pull request, and new work reaches `dev` the same
way. A repository may let maintainers push small changes straight to `dev`; its own guide
says so when it does.

**Main-only repositories.** A repository without a `dev` branch is main-only: work
branches are cut from its default branch and come back to it through a pull request.
Everything else in this guide applies, reading the default branch where it says `dev`.
The Claude plugin marketplace [`jpipe-skills`](https://github.com/jpipe-mcscert/jpipe-skills)
is one, because users install it straight from its default branch and a `dev` branch
there would ship unreleased work. Content repositories (tutorials, examples, demos), the
website and this `.github` repository are others.

**Catalogue repositories are not part of this workflow.** The Scoop bucket
[`scoop-mcscert`](https://github.com/jpipe-mcscert/scoop-mcscert) and the Homebrew tap
[`homebrew-mcscert`](https://github.com/jpipe-mcscert/homebrew-mcscert) are written only by
the release pipelines of the projects they distribute. Nobody modifies them by hand, and
they take no pull requests. A problem in a catalogue is fixed in the release automation
that writes it.

## Making a change

1. **Cut a work branch from `dev`**, named after what it does.
2. **Keep the whole change on that branch**, as one or more commits. If something else
   needs doing along the way, open an issue for it rather than starting a second branch.
3. **Close issues from the commits that resolve them**: end the message with `Closes #N`.
4. **Keep the `CHANGELOG` in step** with what users will notice: features, behaviour
   changes, breaking changes, fixes to a released version. Internal changes stay out.
   Update the documentation too when behaviour changes.
5. **Push.** CI runs on your branch.

## Pull requests

A pull request goes into `dev`. It goes into `main` only for a release, for a hotfix, or
in a main-only repository. It moves through these steps:

1. **Review it locally first**, and address what the review finds.
2. **Open it as a draft.**
3. **Pass the quality gate.** Repositories analyzed by SonarCloud get a `SonarQube` check
   on every push to the pull request. Fix its issues, or justify the ones you keep, until
   the gate passes.
4. **Request one Copilot review.** A pull request gets one round; it is not re-requested
   after the fixes.
5. **Answer every review comment on GitHub.** Say what you changed, and resolve the thread.
   If you disagree or are unsure, reply with your reasoning and leave the thread open for
   the reviewer.
6. **Mark it ready for review.** A maintainer reviews it and merges it.

## Releases

A release is a pull request from `dev` into `main`:

1. Set the version and move the `CHANGELOG`'s `[Unreleased]` notes under the new version
   and date, on a work branch merged into `dev`, or directly on `dev` where the
   repository lets maintainers push there.
2. Open the release pull request from `dev` into `main`, and take it through the steps
   above.
3. A maintainer merges it, then tags the merge commit on `main` (`vX.Y.Z`) and pushes the
   tag. The tag triggers the repository's release workflow.
4. Merge `main` back into `dev`, so that the release commit is on both branches and the
   next release merges cleanly.

**Hotfixes.** A fix to the released version that cannot wait for the next release is cut
from `main` (`fix/…`), comes back into `main` through a pull request, and is released with
a patch tag. Merge `main` back into `dev` afterwards, as for a release.

**Main-only repositories** release from their default branch. Not every merge is a
release: a maintainer sets the version and the `CHANGELOG` through an ordinary pull
request, then tags the resulting commit (`vX.Y.Z`).

Only a maintainer merges a release or a hotfix into `main` and pushes a release tag.

## For AI assistants

Coding assistants working in these repositories follow the same workflow, with these
limits:

- **Stay on the branch the session is on.** Do not create a branch without the
  maintainer's approval. If something seems to need a branch of its own, say so and wait.
  If the session is on `main` or `dev` (or a main-only repository's default branch), ask
  for a work branch before committing anything.
- **Never commit to `main` or `dev`, merge a pull request, or push a tag.** A human merges,
  unless the maintainer explicitly asks the assistant to merge a given pull request. Even
  then, the release pull request into `main` and every tag stay with the maintainer.
- **Open a pull request only when asked**, then follow the steps under
  [Pull requests](#pull-requests) in order and stop once it is ready for review. Report
  the threads left open and why.
