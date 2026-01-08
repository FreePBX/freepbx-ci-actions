# freepbx-ci-actions
FreePBX CI GitHub Actions.

## Background
Starting in 2026, FreePBX will begin using GitHub Actions to trigger multiple
processes, initially including **language translations via Weblate**, and
potentially soon:
* code linting
* unit testing
* security scanning
* PR verifying
* cherry picking
...and other basic quality assurance and automatable management tasks.

## Setup Each Module to Use Shared Actions
To utilize these features, the **freepbx-ci-actions** repo needs to be used by
each module needing some or all of these services. This can be done by copying
files beginning with the word **Module** from the `.github/workflows/`
directory from **freepbx-ci-actions** into the respective FreePBX module's
`.github/workflows/` directory. *But don't copy the* **Shared** *files --
those stay in* **freepbx-ci-actions** *as work is centralized there.*

There are a few other repo-level settings to consider. See the **Actions** and
**Webhooks** pages from another working module *and copy them.*

## How Translations Work
The Weblate server is notified via POST Webhook from GitHub whenever there are
changes to a module's repository. The Weblate server then looks at the release/*
branches for new strings to translate, which it does automatically if there are
already translations for those strings from other modules. Users can also update
the Weblate server translations directly through that specialized web interface.

All of the translations are batched by the Weblate server for pushes to GitHub.
When the Weblate server uploads new translations into GitHub, it should place
them into the weblate/* branches, which will automatically make a PR, lint,
and then merge the PR into the respective release/* branches **hands-free.** :-)

## Handling Translation Failures
If it fails, then manual intervention to correct the issue(s) is required.
One should carefully inspect the errors to determine the best course of action.
In the case of linting problems, it may be sufficient to manually merge the PR.
But if there's a bad push from the Weblate server, then an issue should be
raised in the [issue tracker](https://github.com/FreePBX/issue-tracker/) as
this could indicate a more sever problem with the server, shared actions, etc.
