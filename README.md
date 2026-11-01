# NativeWebBridgeSDK

[![CI](https://github.com/infoadmstudio2020-cyber/NativeWebBridgeSDK/actions/workflows/ci.yml/badge.svg)](https://github.com/infoadmstudio2020-cyber/NativeWebBridgeSDK/actions/workflows/ci.yml)
[![Release](https://github.com/infoadmstudio2020-cyber/NativeWebBridgeSDK/actions/workflows/release.yml/badge.svg)](https://github.com/infoadmstudio2020-cyber/NativeWebBridgeSDK/actions/workflows/release.yml)
[![JitPack](https://jitpack.io/v/infoadmstudio2020-cyber/NativeWebBridgeSDK.svg)](https://jitpack.io/#infoadmstudio2020-cyber/NativeWebBridgeSDK)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

**NativeWebBridgeSDK** is the official **Layer 3: Transport & Communication Bridge** component of the **NativeRuntime Android Ecosystem**. It provides a high-performance, bidirectional RPC bridge between HTML5/JavaScript web content and native Android subsystems with automatic JSON envelope serialization, domain security whitelisting, router dispatching, and diagnostic event publishing.

---

## Architecture Role

```
+-----------------------------------------------------------------------------------+
| Layer 4: Master Orchestration (NativeRuntimeSDK)                                  |
+-----------------------------------------------------------------------------------+
                                        | Assembles / Boots
+-----------------------------------------------------------------------------------+
| Layer 3: Transport & Communication Bridge                                         |
|  - NativeWebBridgeSDK (WebBridge, NativeJavascriptInterface, BridgeEventBus)      |
+-----------------------------------------------------------------------------------+
                                        | Dispatches RPC / Handlers
+-----------------------------------------------------------------------------------+
| Layer 2: Domain Engines                                                           |
|  - Routing: NativeRouterSDK                                                       |
|  - Core Subsystems: NativePlatformEngine, NativeStorageEngine, etc.              |
+-----------------------------------------------------------------------------------+
                                        | Managed Lifecycle / Telemetry
+-----------------------------------------------------------------------------------+
| Layer 1: Core Infrastructure                                                      |
|  - NativeServiceSDK (BaseService, ServiceRegistry)                                |
|  - NativeLoggerSDK (Multi-Sink Logger)                                            |
+-----------------------------------------------------------------------------------+
                                        | Implements Contracts
+-----------------------------------------------------------------------------------+
| Layer 0: Universal Foundation Contracts                                           |
|  - NativeRuntimeContracts (RuntimeState, IApiHandler, NativeApiRequest/Response)  |
+-----------------------------------------------------------------------------------+
```

---

## Consumer Integration Guide

### 1. Configure Repositories
Add the JitPack maven repository to your `settings.gradle` or root `build.gradle`:

```groovy
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven { url 'https://jitpack.io' }
    }
}
```

### 2. Add Dependencies
Include `NativeWebBridgeSDK` alongside the cyber ecosystem dependencies in your module's `build.gradle`:

```groovy
dependencies {
    implementation 'com.github.infoadmstudio2020-cyber:NativeRouterSDK:1.0.0'

    // Ecosystem dependencies
    implementation 'com.github.infoadmstudio2020-cyber:NativeServiceSDK:1.0.1'
    api 'com.github.infoadmstudio2020-cyber:NativeRuntimeContracts:v1.0.1'
    implementation 'com.github.infoadmstudio2020-cyber:NativeLoggerSDK:1.0.1'

    // NativeWebBridgeSDK
    implementation 'com.github.infoadmstudio2020-cyber:NativeWebBridgeSDK:1.0.0'
}
```

---

## Quick Start (Java / Android)

### Initialize the SDK
```java
import com.roni.library.bridge.BridgeConfig;
import com.roni.library.bridge.NativeWebBridgeSDK;
import com.roni.library.bridge.WebBridge;

public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        BridgeConfig config = BridgeConfig.builder()
                .interfaceName("NativeBridge")
                .addAllowedOrigin("https://yourdomain.com")
                .timeoutMs(10000L)
                .debugLogging(true)
                .build();

        NativeWebBridgeSDK.getInstance().init(this, config);
    }
}
```

### Bind WebView in Activity
```java
WebView webView = findViewById(R.id.webView);
WebBridge bridge = WebBridge.getInstance();

// 1. Enable JavaScript
webView.getSettings().setJavaScriptEnabled(true);

// 2. Attach Native JavaScript Interface
webView.addJavascriptInterface(bridge.getJavascriptInterface(), bridge.getConfig().getInterfaceName());

// 3. Connect JavaScript Evaluator
bridge.setWebEvaluator(script -> runOnUiThread(() -> webView.evaluateJavascript(script, null)));

// 4. Inject Bootstrap Client Script
webView.setWebViewClient(new WebViewClient() {
    @Override
    public void onPageFinished(WebView view, String url) {
        super.onPageFinished(view, url);
        view.evaluateJavascript(bridge.getBootstrapScript(), null);
    }
});
```

---

## JavaScript Client API (Web Frontend)

Once loaded, the bridge automatically attaches `window.NativeBridgeClient`:

```javascript
// 1. Asynchronous RPC Call to NativeRouter
window.NativeBridgeClient.call('echo', { message: 'Hello Native!' })
  .then(function(response) {
      console.log('Native response:', response);
  })
  .catch(function(error) {
      console.error('Call failed:', error);
  });

// 2. Synchronous Native Ping Check
const pong = window.NativeBridgeClient.ping();
console.log('Ping status:', pong);

// 3. Listen to Native Broadcast Events
window.NativeBridgeClient.on('system_status', function(data) {
    console.log('Status event received:', data);
});
```

---

## Companion Sample Application

The repository includes a companion testing app under `:app` with:
- **Dashboard Screen**: Ecosystem module status overview & connectivity status
- **Bridge & Router Tester**: Live in-app WebView demonstrating real bidirectional message exchange
- **Service Diagnostics**: Real-time inspection of `NativeServiceSDK` built-in services (`DeviceInfoService`, `BatteryStatusService`, `NetworkStateService`)
- **Integration Test Center**: Automated live test runner executing 7/7 verification checks
- **Runtime Telemetry Console**: Real-time log buffer recording all RPC invocations, events, and errors

---

## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.
