# CI Patterns

- [  ] Build on every change, commit, branch, merge, and pull request.
- [  ] Use the ticket number as a branch naming convention, add useful information to commit messages, and standardize the version for all applications.
- [  ] Use pre and post actions on commits, merges, and pull requests.
- [  ] Use a new, fresh isolated workspace to build the application and control the allocated resources to avoid impacting other builds.
- [  ] Automatically release and deploy a new version on every commit, branch creation, merge, or pull request; test the build weekly to identify potential issues proactively instead of waiting for a code update.
- [  ] Deploy a hotfix as soon as possible; test the code in staging before moving it to production.
- [  ] Ensure the deliverable is free of any CVE (Critical Vulnerabilities and Exposures); take action immediately when a severity is identified.
- [  ] Check the source for sensitive data and break the pipeline if it is found; dissociate passwords from the source code.
- [  ] Lint and format code automatically to make it more readable; break the pipeline upon receiving bad quality reports.
- [  ] Run a set of tests automatically on each build; run specific tests periodically.
- [  ] Run tests the same way on any platform (laptop, on-premises, cloud, container orchestration platform, etc.) to always have the same result with mocked data.
- [  ] Measure the number of releases per day, the time to build each one, and pipeline status.
- [  ] Automatically notify all teams of a new release and tag or comment a ticket on a build status.
- [  ] Manage, deduplicate, and version the application's configuration into a centralized configuration management tool.

