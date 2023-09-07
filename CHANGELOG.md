Changelog
=========

[1.1.1] - 2023-09-07
--------------------

### Other Changes

- ci: Add markdownlint, test_converting_readme, and build_docs workflows (#46)

  - markdownlint runs against README.md to avoid any issues with
    converting it to HTML
  - test_converting_readme converts README.md > HTML and uploads this test
    artifact to ensure that conversion works fine
  - build_docs converts README.md > HTML and pushes the result to the
    docs branch to publish dosc to GitHub pages site.
  - Fix markdown issues in README.md
  
  Signed-off-by: Sergei Petrosian <spetrosi@redhat.com>

- docs: Make badges consistent, run markdownlint on all .md files (#47)

  - Consistently generate badges for GH workflows in README RHELPLAN-146921
  - Run markdownlint on all .md files
  - Add custom-woke-action if not used already
  - Rename woke action to Woke for a pretty badge
  
  Signed-off-by: Sergei Petrosian <spetrosi@redhat.com>

- ci: Remove badges from README.md prior to converting to HTML (#48)

  - Remove thematic break after badges
  - Remove badges from README.md prior to converting to HTML
  
  Signed-off-by: Sergei Petrosian <spetrosi@redhat.com>

- docs: Make supported versions and README consistent (#49)

  - Add Postgresql version 15 into README
  
- ci: fix mode of vars/main.yml for ansible-test (#50)


[1.1.0] - 2023-07-19
--------------------

### New Features

- feat: Enable support for Postgresql 15 (#44)

### Bug Fixes

- fix: facts being gathered unnecessarily (#43)

### Other Changes

- ci: Add pull request template and run commitlint on PR title only (#40)
- ci: Rename commitlint to PR title Lint, echo PR titles from env var (#41)
- ci: ansible-lint - ignore var-naming[no-role-prefix] (#42)

[1.0.4] - 2023-05-23
--------------------

### Other Changes

- docs: Consistent contributing.md for all roles - allow role specific contributing.md section
- docs: add Collection requirements section
- test: ensure facts for tests_include_vars_from_parent.yml

[1.0.3] - 2023-05-03
--------------------

### Other Changes

- test: add cleanup to all tests - parameterize path names

[1.0.2] - 2023-04-27
--------------------

### Other Changes

- test: check generated files for ansible_managed, fingerprint
- ci: Add commitlint GitHub action to ensure conventional commits with feedback

[1.0.1] - 2023-04-14
--------------------

### Other Changes

- Improve readme (#27)
- Fix typos in README

[1.0.0] - 2023-04-06
--------------------

### New Features

- New role for managing PostgreSQL
