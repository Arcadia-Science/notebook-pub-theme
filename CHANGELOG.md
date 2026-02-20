# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2026-02-20

### Added

- Syntax highlighting theme (`arcadia-light.theme`) for code blocks
- Link to syntax highlighting theme builder in README

### Changed

- Updated fonts to more closely match Stacks
- Loaded fonts via dedicated `fonts.html` include instead of inline in `main.css`
- Refactored CSS across article, frontmatter, sidebar, and navbar styles
- Changed default PR reviewer to Robert-Roth; schedule shifted to 9AM PST

### Fixed

- Fixed `.csl-entry` styling

## [1.0.6] - 2026-02-04

### Changed

- Gate Quarto setup behind version check for faster no-op runs
- Added pull vs push model rationale to documentation

## [1.0.5] - 2026-02-04

### Changed

- Reverted `--arcadia-pewter` to original color (#3b6179)

## [1.0.4] - 2026-02-04

### Changed

- TEST: Changed `--arcadia-pewter` to deep red (#8B0000) for testing automated theme updates

## [1.0.3] - 2026-02-03

### Added

- Reusable GitHub workflows for automated theme updates

## [1.0.2] - 2026-02-03

### Fixed

- Bumped version in `_extension.yml` (was missing from v1.0.1)

## [1.0.1] - 2026-02-03

### Fixed

- Renamed extension directory from `arcadia-science` to `Arcadia-Science` for Linux filesystem compatibility

## [1.0.0] - 2026-02-02

### Added

- Initial release
- Dumped all styling from the [notebook pub template](https://github.com/Arcadia-Science/notebook-pub-template) into this extension
