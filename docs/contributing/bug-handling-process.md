
Eclipse Oniro aims to build a secure system from the ground up by applying
industry best practices for development quality. As with every software project,
bugs occur. This document explains how we handle them.

## How to Report a Bug

If you think you have found a bug in our distribution, please file a bug
report in our [bug
tracker](https://github.com/eclipse-oniro4openharmony/manifest/issues) and in the project that you think is the source of the issue. Use the provided template:

- The module affected
- Actions that reproduce the bug (steps to reproduce)
- The result you see (actual result)
- The result you expect (expected behavior)
- Frequency (always, sometimes, or one-time issue)
- Tested version (image name and version, platform)
- Any known workaround for the issue (include a link to the workaround or mitigation steps)
- Do you have a fix for this issue?

Developers review the reported issues and perform triage (see below).
When a fix is available, the ticket is updated with the details of the
solution.

## Bug Triage

During triage, developers assess the bug and set its severity and domain. At the
end of this process, the bug will:

- Be classified as a security issue, normal bug, feature request, or
  be rejected if the feature works as planned or the bug cannot be
  reproduced.
- Have its severity set. Please refer to the documentation of severity
  levels below.
- Have its domain set. The bug tracker will include the latest list.

If the bug is classified as a security vulnerability, the engineer
assessing the issue will create a new ticket in the private security bug
tracker and the discussion will continue in the security bug tracker
from that point. Please refer to the CVE Process for details.

If the report is confirmed as a bug, the developer assigns one of four severity
levels: critical, normal, minor, or low.

!!! note
    _Critical_ severity bugs make a feature unusable or cause major data
    loss or hardware damage. They have no workaround or require a complex one.
    _Normal_ severity bugs make a feature hard to use, but there is a
    workaround (including another feature to use instead of the desired
    one). _Minor_ severity bugs cause the loss of a non-critical feature (such as
    missing or incorrect logging). _Low_ severity bugs cause minor
    inconveniences (like a typo in the user interface or in the
    documentation).

The bug can originate in a package developed by the project, or from one
we use from an upstream source. The process of handling a bug report
will change between those two cases:

### When the Issue is in the Code Developed by the Project

If the bug originates in code maintained directly by the project, we handle it
in the project bug tracker.

### When the Issue Originates from Upstream Code

If the issue was identified in upstream code, we report an upstream
issue in a way appropriate to the upstream project. We store the
reference to the upstream bug report in our bug tracker. Depending on
the bug severity, we might decide to develop and maintain a fix locally.
However, we strongly prefer to upstream the fix first, and then get it
with a regular upstream code update.

We also update maintained packages periodically from upstream sources,
regardless of the bugs filed in our system. Our goal
is to update to the latest stable version of the package.

## Detailed Workflow

### Bug Sources

Bugs may come from the project's own findings, including QA, or from partners,
community members, and security researchers. The project team may learn about
an issue through channels such as Matrix and discussion forums.
Issues coming from different sources are centralized in the bug tracker,
which also provides a unified identifier for every issue.

### Acknowledgement and Bug Triage

After the bug is entered, a developer will perform triage. The process
starts from acknowledging the issue and then consists of verifying all
the information provided by the bug reporter to reproduce the issue. The
developer performing triage might ask additional questions. Then they
assign severity and domain to the issue in the bug tracker. They also
check which versions are affected and might modify the severity level
set by the reporter. Any project member, or the bug reporter, who
disagrees with the assignment might comment on the issue.

If there is a fix available from the reporter, the developer also
verifies if the fix is correct and matches the IP policy. If the fix is
judged acceptable, the process might skip to the Releasing step.

We aim at the first answer of the triage (either finishing triage, or
additional questions to the reporter) in three working days for critical
bugs and seven days for other bugs. In case of a critical bug, the
person performing triage informs the maintainers of the affected
subsystem.

### Prioritizing and Fixing

Bugs with the severity attached enter the prioritization process. It
includes a weekly meeting when the team reviews bugs entered or modified
during the last week: those during the process of triage, and those with
the triage finished. For the bugs with triage finished, the team sets
the priority and might assign it to a developer.

Bug fixes should follow the same contribution guidelines as any
other contribution. The best practice is to develop a fix for the bug in
a separate branch. Fixes for related bugs are possible in the same
branch.

### Releasing

When a bug fix is available in a branch, the developer creates a pull
request. After acceptance, the change is merged into the main branch.
The developer in charge of the bug verifies with the release manager to
which branches the change should be backported.

If the bug comes from an upstream project, developers upstream the bug
fix. If the upstream is delayed, the Project might ship a local fix.
However, we aim at upstreaming all fixes.

During patch development and upstream submission, the developer updates the
documentation when appropriate and adds a
notification to the release notes. Our release notes contain: links to
bugs fixed in the release, links to CVEs fixed in the release (publicly
known) and a list of CVEs fixed that are still under embargo.

If the bug is critical, the project may publish a separate bug-fix release.
Otherwise, the fix lands
in the next bugfix release.
