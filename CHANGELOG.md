# Changelog

## [Table of Contents]
- [Unreleased](#unreleased)
- [2.0.0](#200---2021-06-07)
- [1.1.1](#111---2021-06-07)
- [1.1.0](#110---2021-05-03)
- [1.0.0](#100---2021-05-02)

## [Unreleased][]
### Added
- Open Key support (in addition to Camelot Keys and Standard Notation). Methods to apply movements on the wheel are notation agnostic, as long as you work with the key object (Camelot Key or Open Key), so they work in both cases the same.
- Added 2 methods to transalte object keys between Camelot notation and Open one: translateObjectCamelotToOpen & translateObjectOpenToCamelot.
### Changed
- getKeyNotationObject() has been split into 2 versions, one for Camelot Keys (getKeyNotationObjectCamelot) and another for Open Keys (getKeyNotationObjectOpen), they work the same as previously and output their respective key object.
- getKeyNotation() has been split into 2 versions, same comment applies (getKeyNotationCamelot and getKeyNotationOpen). They convert the key object to a string ('7A').
- Pattern creation: random shuffling is now up to x1000 times faster for big sizes.
### Removed
### Fixed

## [1.1.1] - 2021-06-07
### Added
### Changed
### Removed
### Fixed
- Pattern creation: not really random due to using sort + random method. Using an array shuffle now instead.

## [1.1.0] - 2021-05-03
### Added
- Method for pattern creation from harmonic mixing.
- Method for pattern application to keys.
### Changed
### Removed
### Fixed
- keyNotation for self mapped keys was wrong.

## [1.0.0] - 2021-05-02
### Added
- First release.
### Changed
### Removed
### Fixed

[Unreleased]: https://github.com/regorxxx/Camelot-Wheel-Notation/compare/v2.0.0...HEAD
[2.0.0]: https://github.com/regorxxx/Camelot-Wheel-Notation/compare/v1.1.1...v2.0.0
[1.1.1]: https://github.com/regorxxx/Camelot-Wheel-Notation/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/regorxxx/Camelot-Wheel-Notation/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/regorxxx/Camelot-Wheel-Notation/compare/16b9932...v1.0.0
