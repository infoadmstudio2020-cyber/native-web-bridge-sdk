# Changelog

All notable changes to **NativeWebBridgeSDK** are documented in this file.

## [1.0.0] - 2026-11-01

### Added
- **WebBridge Core Engine**: Complete Layer 3 Transport & Bridge implementation connecting Web and Native.
- **Bidirectional RPC Routing**: Seamless integration with `NativeRouterSDK:1.0.0`.
- **Ecosystem Service Integration**: Connected to `NativeServiceSDK:1.0.1` and `NativeRuntimeContracts:v1.0.1`.
- **Diagnostic Logging**: Structured telemetry output via `NativeLoggerSDK:1.0.1`.
- **Event Bus Subsystem**: `BridgeEventBus` supporting wildcard topic subscriptions and native-to-web broadcasting.
- **Message Serialization**: `JsonMessageSerializer` with full envelope parsing and type safety.
- **JavaScript Injection Engine**: `JavaScriptBridgeScript` client generator with Promise-based `call()`, `ping()`, and `on()`.
- **Companion Sample Application**: Multi-panel interactive testbed with live WebView, diagnostics, test runner, and console.
- **Comprehensive Unit Tests**: Full JVM test suite in `NativeWebBridgeSDKTest`.
- **JitPack & CI/CD Support**: Automated GitHub Actions workflows for testing, publishing, and universal artifact releases.
