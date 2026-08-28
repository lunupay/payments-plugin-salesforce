# Changelog

All notable changes to the Lunu Payment Salesforce Commerce Cloud Integration will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [22.1.0] - 2024-10-10

### Fixed
- Fixed undefined `order` variable bug in `processNotifications.js` when handling failed/expired/canceled payments
- Fixed undefined `callbackPaymentStatus` variable in `lunuHelpers.js` logger call

### Changed
- Updated all references from `lunu.io` to `lunupay.com` (excluding API endpoints)
- Changed API documentation URL to `https://docs.lunupay.com/`
- Updated widget URL from `/testing/` to `/sandbox/` for consistency
- Updated package.json metadata (repository URL, author, homepage)
- Improved code quality and consistency

### Added
- Comprehensive README.md with installation instructions, configuration guide, and troubleshooting
- .gitignore file to prevent committing sensitive data and build artifacts
- LICENSE file (ISC License)
- CHANGELOG.md for version tracking
- Keywords in package.json for better discoverability

## [22.0.0] - 2022-03-06

### Added
- Initial release of Lunu Payment integration for Salesforce Commerce Cloud
- Core payment processing functionality
- SFRA compatibility layer
- Business Manager job for processing notifications
- Webhook handling for payment status updates
- Custom object for storing payment notifications
- Site preferences for configuration
- Service definitions for Lunu API integration
- Payment method and processor configuration
- Integration tests for checkout flow
- Documentation (LUNU.pdf)

### Features
- Cryptocurrency payment support (Bitcoin, Ethereum)
- QR code generation (EIP-67, BIP-21)
- Automated order status management
- Real-time payment notifications
- Support for test and production modes
- Secure API authentication
- Customizable payment widget
- Comprehensive logging

## [Unreleased]

### Planned
- Enhanced error handling and retry logic
- Support for additional cryptocurrencies
- Improved admin reporting and analytics
- Multi-language support for payment widget
- Enhanced security features
- Performance optimizations

---

## Version Numbering

This project follows Salesforce Commerce Cloud's SFRA version numbering scheme (YEAR.QUARTER.PATCH):
- Major version: Year (e.g., 22 = 2022, 24 = 2024)
- Minor version: Quarter (1-4)
- Patch version: Incremental updates

## Types of Changes

- `Added` for new features
- `Changed` for changes in existing functionality
- `Deprecated` for soon-to-be removed features
- `Removed` for now removed features
- `Fixed` for any bug fixes
- `Security` for vulnerability fixes



