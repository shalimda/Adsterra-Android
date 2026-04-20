# Workflow Android Library

`workflow` is an Android library module that manages multiple `WebView` nodes and processes URL lists with configurable timing and layout options.

---

## What It Provides

- Multi-node `WebView` orchestration through `WorkflowManager`
- Custom container view (`WorkflowContainer`) with XML attributes
- Vertical / horizontal / grid node layout support
- URL queue from:
  - manual links passed at runtime
  - remote JSON source (merged with manual links)
- Node lifecycle handling with delays and completion tracking
- Optional custom User-Agent support
- Cleanup helpers for node reset between rounds

---

## Requirements

| Component         | Version     |
|------------------|-------------|
| Android minSdk   | API 24+     |
| compileSdk       | 35          |
| targetSdk        | 35          |
| Java             | 11          |
| AndroidX WebKit  | 1.11.0+     |

---

## Installation

Place `workflow.aar` inside your project's `app/libs/` folder, then add the dependency:

#### Groovy DSL (`build.gradle`)
```groovy
dependencies {
    implementation files("libs/workflow.aar")
    implementation "androidx.webkit:webkit:1.11.0"
}
```

#### Kotlin DSL (`build.gradle.kts`)
```kotlin
dependencies {
    implementation(files("libs/workflow.aar"))
    implementation("androidx.webkit:webkit:1.11.0")
}
```

---

## Permissions

Add the following to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

---

## XML Usage (`WorkflowContainer`)

`WorkflowContainer` is a custom view that handles WebView creation and layout internally.

### Supported Attributes

| Attribute                    | Type      | Description                              |
|-----------------------------|-----------|------------------------------------------|
| `skypr_webViewCount`        | int       | Number of WebView nodes to create        |
| `skypr_redirectWaitMs`      | int       | Wait time (ms) after redirect/load       |
| `skypr_roundDelayMs`        | int       | Delay (ms) between URL rounds            |
| `skypr_enableLogging`       | boolean   | Enables WebView debugging flag           |
| `skypr_autoStart`           | boolean   | Auto-starts workflow on attach           |
| `skypr_customUserAgent`     | string    | Custom User-Agent string                 |
| `skypr_webViewOrientation`  | enum      | `vertical`, `horizontal`, or `grid`      |
| `skypr_webViewSpacing`      | dimension | Spacing between WebView nodes            |
| `skypr_containerBackground` | color     | Background color of the container        |

### Example Layout

```xml
<com.skypper.workflow.WorkflowContainer
    android:id="@+id/workflowContainer"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:skypr_webViewCount="4"
    app:skypr_redirectWaitMs="7000"
    app:skypr_roundDelayMs="10000"
    app:skypr_autoStart="false"
    app:skypr_webViewOrientation="grid"
    app:skypr_webViewSpacing="8dp"
    app:skypr_containerBackground="@android:color/white" />
```

---

## Usage

### Java

```java
WorkflowContainer container = findViewById(R.id.workflowContainer);
container.initialize();

List<String> links = new ArrayList<>();
links.add("https://example.com/page1");
links.add("https://example.com/page2");

container.startWorkflow(links);
```

### Kotlin

```kotlin
val container = findViewById<WorkflowContainer>(R.id.workflowContainer)
container.initialize()

val links = listOf(
    "https://example.com/page1",
    "https://example.com/page2"
)

container.startWorkflow(links)
```

---

## Direct `WorkflowManager` Usage

If you prefer to manage WebViews manually in your layout, use `WorkflowManager` directly.

### Java

```java
WebView[] nodes = new WebView[] {
    findViewById(R.id.webView1),
    findViewById(R.id.webView2),
    findViewById(R.id.webView3),
    findViewById(R.id.webView4)
};

WorkflowManager manager = new WorkflowManager(this, nodes, 7000, 10000);
manager.prepareNodes();
manager.startWorkflow(java.util.Arrays.asList(
    "https://example.com/page1",
    "https://example.com/page2"
));
```

### Kotlin

```kotlin
val nodes = arrayOf(
    findViewById<WebView>(R.id.webView1),
    findViewById<WebView>(R.id.webView2),
    findViewById<WebView>(R.id.webView3),
    findViewById<WebView>(R.id.webView4)
)

val manager = WorkflowManager(this, nodes, 7000, 10000)
manager.prepareNodes()
manager.startWorkflow(listOf(
    "https://example.com/page1",
    "https://example.com/page2"
))
```

---

## Lifecycle Notes

- Call `initialize()` before `startWorkflow()` when using `WorkflowContainer`.
- Call `stopWorkflow()` when the host screen should halt processing.
- Ensure `INTERNET` permission is declared in `AndroidManifest.xml`.

---

## Build Notes

- Release minification is enabled for the library.
- Consumer ProGuard rules are exported via `consumerProguardFiles`.

---

## License

Copyright (c) 2026 White Skypper. All rights reserved.

This library is provided for **use only** under a proprietary license.
You may **not** modify, decompile, reverse engineer, or redistribute the source code in any form without explicit written permission from the author.

> Unauthorized modification or redistribution is strictly prohibited.
