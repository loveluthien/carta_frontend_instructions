---
name: changelog
description: Write CARTA frontend changelog entries.
---

# Write CARTA Frontend Changelog
Changelog file is `CHANGELOG.md` in the root directory of the CARTA frontend project.

## Skill Description
CARTA frontend changelog is for users to understand what has changed. The description should be as simple as possible and should not include the technical details. The new entry should be summarized in one or two sentences categorized by type (`### Added`, `### Changed`, `### Fixed`) under `## [Unreleased]` section.

## When to use
When user requests to write a changelog. 

## Format
- The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).
- Entry style: `description ([#issue_number](issue_link))`.
- More than one issue, separate them with commas: `description ([#issue_number1](issue_link1), [#issue_number2](issue_link2))`.

## Find Associated Issue
- Each entry must associate to at least one issue.
- Find associated issues in the chat history, branch name with `git status`, or pull request description.
- Ask the user to provide issue numbers or links.
- Fetching the issues in `https://github.com/CARTAvis/carta-frontend/issues`.

## Entry content
- Categorize the entry by type: `### Added` for new features, `### Changed` for changes, and `### Fixed` for bug fixes based on the issue description.
- For bug fixes, describe the issue what was fixed. Example: `Fixed a bug where the frontend would crash when loading large FITS files ([#123](issue_link))`.
- For new features, describe the feature. Example: `Added support for displaying spectral line data in the widget ([#456](issue_link))`.
- For changes, describe the change and its impact on users. Example: `Changed the default color scheme to improve visibility of faint features in astronomical images ([#789](issue_link))`.