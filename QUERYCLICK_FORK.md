# QueryClick fork of labd/wagtail-2fa

**This is a temporary fork.** It exists only because the upstream project, [labd/wagtail-2fa](https://github.com/labd/wagtail-2fa), has two fixes we need that are still waiting to be merged and released. As soon as upstream ships them, we stop using this fork.

## Why it exists
The latest upstream release (1.8.0) does not work for us:

1. **It crashes on Wagtail 8.0.** It imports `UserListingButton` from `wagtail.users.widgets`, which Wagtail 8.0 removed. Fixed by upstream pull request [#285](https://github.com/labd/wagtail-2fa/pull/285) (not merged).
2. **Login fails with `django-otp` 1.7.1 or newer** (ours is 1.7.3), even on Wagtail 7.4. Fixed by upstream pull request [#286](https://github.com/labd/wagtail-2fa/pull/286), reported as [#284](https://github.com/labd/wagtail-2fa/issues/284) (not merged).

## What is different from upstream
Nothing except those two pull requests, merged together (one small template conflict resolved by hand: the OTP form's hidden `otp_device` field was added to the surviving template). No other code, dependencies or behaviour have been changed. To see exactly what differs:

https://github.com/labd/wagtail-2fa/compare/master...QueryClick:wagtail-2fa:combo

At the time of writing that is 9 files, about 117 lines added and 184 removed, roughly 70 of the added lines being tests.

## How we use it
Our sites install it from a **full commit hash**, never from a branch name, so the code that runs in production cannot change unless someone deliberately edits our `requirements.txt`:

```
wagtail-2fa @ git+https://github.com/QueryClick/wagtail-2fa.git@<full commit hash>
```

## Rules for this repository
- Do not commit anything here except re-merging upstream changes. No features, no "quick fixes".
- Any change to the `combo` branch must be reviewed by a second person, because this is security code.
- After any change, update the commit hash in the site `requirements.txt` files and re-run the 2FA tests (see `2fa-rollout.md` in the project notes).
- Write access to this repository should be limited to people who deploy the sites.

## How to retire this fork (the goal)
1. Check upstream: has a release newer than 1.8.0 been published to PyPI whose changelog includes Wagtail 8.0 support and the `django-otp` 1.7.1 login fix?
2. If yes, in each site's `requirements.txt` replace the `wagtail-2fa @ git+...` line with `wagtail-2fa==<that version>`, reinstall, and re-run the 2FA checks (enrolment, login with a wrong and a right code, recovery).
3. Archive this repository on GitHub and leave this note in place.

Keep an eye on https://github.com/labd/wagtail-2fa/pulls and https://pypi.org/project/wagtail-2fa/ . Upstream security fixes will NOT reach us automatically while we use this fork.

## Contact
Chris Liversidge, QueryClick / Corvidae AI.
