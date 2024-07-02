Changelog
=========

[1.3.8] - 2024-07-02
--------------------

### Bug Fixes

- fix: add support for EL10 (#93)

### Other Changes

- ci: ansible-lint action now requires absolute directory (#92)

[1.3.7] - 2024-06-11
--------------------

### Other Changes

- ci: use tox-lsr 3.3.0 which uses ansible-test 2.17 (#87)
- ci: tox-lsr 3.4.0 - fix py27 tests; move other checks to py310 (#89)
- ci: Add supported_ansible_also to .ansible-lint (#90)

[1.3.6] - 2024-04-04
--------------------

### Other Changes

- ci: bump ansible/ansible-lint from 6 to 24 (#84)
- ci: bump mathieudutour/github-tag-action from 6.1 to 6.2 (#85)

[1.3.5] - 2024-02-26
--------------------

### Other Changes

- test: stop and disable service on ostree systems (#82)

[1.3.4] - 2024-02-08
--------------------

### Other Changes

- ci: fix python unit test - copy pytest config to tests/unit (#78)
- docs: fix role var typo (#79)
- test: skip versions tests if ostree (#80)

[1.3.3] - 2024-01-31
--------------------

### Other Changes

- test: tests_versions gather_facts; skip version 16 if not supported (#76)

[1.3.2] - 2024-01-23
--------------------

### Other Changes

- test: add test for versions (#74)

[1.3.1] - 2024-01-16
--------------------

### Bug Fixes

- fix: Enable PostgreSQL stream selection for c9s and RHEL9 (#72)

### Other Changes

- ci: support ansible-lint and ansible-test 2.16 (#70)
- ci: Use supported ansible-lint action; run ansible-lint against the collection (#71)

[1.3.0] - 2023-12-08
--------------------

### New Features

- feat: Enable support for Postgresql 16 (#68)

### Other Changes

- ci: bump actions/github-script from 6 to 7 (#66)
- refactor: get_ostree_data.sh use env shebang - remove from .sanity* (#67)

[1.2.0] - 2023-11-29
--------------------

### New Features

- feat: support for ostree systems (#62)

### Other Changes

- docs: fix error message to include support for Postgresql 15 (#63)

[1.1.2] - 2023-11-06
--------------------

### Other Changes

- Bump actions/checkout from 3 to 4 (#52)
- ci: ensure dependabot git commit message conforms to commitlint (#55)
- ci: use dump_packages.py callback to get packages used by role (#57)
- ci: tox-lsr version 3.1.1 (#59)
- ci: Fix implicit octal values in main.yml (#60)

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
