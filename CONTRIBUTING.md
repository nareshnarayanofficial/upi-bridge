# Contributing to UPI SMS Bridge

Thank you for your interest in contributing!

## Before You Start

1. Read the [architecture overview](docs/architecture.md)
2. Check existing issues and PRs
3. For large features, open an issue first to discuss the design

## Development Setup

### Android App

Requirements:
- Android Studio Hedgehog or later
- JDK 17
- Android SDK 34 (target), min SDK 26

```bash
cd android
./gradlew assembleDebug
./gradlew test
```

### Node.js Integration

```bash
cd integrations/nodejs
npm install
npm test
```

## Contribution Areas

| Area | Good first issues |
|------|-------------------|
| SMS parsers | Adding a new bank format |
| Tests | Unit tests for parser edge cases |
| Docs | Improving installation instructions |
| Integrations | Adding PHP / Python receivers |

## Adding a New Bank SMS Parser

1. Look at `android/sms/parsers/HdfcSmsParser.kt` as a reference
2. Create `android/sms/parsers/YourBankSmsParser.kt` implementing `SmsParser`
3. Register it in `android/sms/SmsParserRegistry.kt`
4. Add test cases in `android/tests/parser/YourBankSmsParserTest.kt`
5. Document the SMS format in `docs/supported-sms-formats.md`

**Important:** Use only synthetic SMS examples — no real bank messages or account numbers in tests.

## Pull Request Guidelines

- Keep PRs focused on a single concern
- All tests must pass
- New parsers must have at least 5 test cases covering happy path and edge cases
- No floating-point arithmetic for amounts — always use integer paise
- Do not include SMS text or personal data in any logs or test fixtures

## Code Style

- Kotlin: follow official Kotlin style guide
- JavaScript: ESLint with provided config
- Commit messages: `type(scope): description` (e.g., `feat(parser): add Axis Bank SMS format`)

## License

By contributing, you agree your contributions are licensed under the MIT License.
