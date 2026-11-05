# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed
- Docs and examples use the placeholder 965XXXXXXXX instead of a sample mobile number.

### Docs
- ERR028 messages (English and Arabic) no longer claim a fixed 15-second wait: duplicate SMS protection is opt-in per account with a configurable window (10 seconds by default)

### Added
- Requests to kwtSMS carry the integration name and version (no personal data) so kwtSMS can see which integrations are in use.
- Initial extension scaffold
- Product Requirements Document (PRD)
- Implementation Plan
- Competitive Analysis
- Marketplace Submission Guide
