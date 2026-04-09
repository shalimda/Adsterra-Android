# WorkflowManager

A lightweight Android library for automated sequential WebView loading with proxy support, smart node management, and built-in interaction simulation.

---

## Features

- 🔄 Sequential URL loading across multiple WebViews
- 🌐 Fetch URL lists from a remote JSON source (GitHub, CDN, etc.)
- ➕ Combine remote URLs with custom manual links
- 🧹 Auto cache, cookie & storage cleanup between loads
- 🤖 Simulated user scroll interaction on each page
- 🔁 Smart node cycling with active-state tracking
- 🛡️ Redirect-safe — fires completion logic only once per load

---

## Installation

### 1. Add the AAR file

Place `workflow.aar` inside your project's `app/libs/` folder.

---

### 2. Add dependency

#### Groovy DSL (`build.gradle`)
```groovy
dependencies {
    implementation files('libs/workflow.aar')
    implementation 'androidx.webkit:webkit:1.8.0'
}
```

#### Kotlin DSL (`build.gradle.kts`)
```kotlin
dependencies {
    implementation(files("libs/workflow.aar"))
    implementation("androidx.webkit:webkit:1.8.0")
}
```

---

## Setup

### XML Layout

Add your WebViews to your layout file: Add This Layout For Hidden Location

```xml
<!-- res/layout/activity_main.xml -->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <WebView
        android:id="@+id/webView1"
        android:layout_width="1dp"
        android:layout_height="1dp" />

    <WebView
        android:id="@+id/webView2"
        android:layout_width="1dp"
        android:layout_height="1dp" />

    <WebView
        android:id="@+id/webView3"
        android:layout_width="1dp"
        android:layout_height="1dp" />

    <WebView
        android:id="@+id/webView4"
        android:layout_width="1dp"
        android:layout_height="1dp" />

</LinearLayout>
```

---

## Usage

### Java

```java
import com.skypper.workflow.WorkflowManager;

public class MainActivity extends AppCompatActivity {

    private WorkflowManager workflowManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        WebView[] nodes = {
            findViewById(R.id.webView1),
            findViewById(R.id.webView2),
            findViewById(R.id.webView3),
            findViewById(R.id.webView4)
        };

        workflowManager = new WorkflowManager(this, nodes);
        workflowManager.prepareNodes();

        // Option 1: Use only remote JSON URLs
        workflowManager.startWorkflow(null);

        // Option 2: Add custom URLs alongside remote JSON URLs
        List<String> customLinks = new ArrayList<>();
        customLinks.add("https://example.com/page1");
        customLinks.add("https://example.com/page2");
        workflowManager.startWorkflow(customLinks);
    }
}
```

---

### Kotlin

```kotlin
import com.skypper.workflow.WorkflowManager

class MainActivity : AppCompatActivity() {

    private lateinit var workflowManager: WorkflowManager

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val nodes = arrayOf(
            findViewById(R.id.webView1),
            findViewById(R.id.webView2),
            findViewById(R.id.webView3),
            findViewById(R.id.webView4)
        )

        workflowManager = WorkflowManager(this, nodes)
        workflowManager.prepareNodes()

        // Option 1: Use only remote JSON URLs
        workflowManager.startWorkflow(null)

        // Option 2: Add custom URLs alongside remote JSON URLs
        val customLinks = listOf(
            "https://example.com/page1",
            "https://example.com/page2"
        )
        workflowManager.startWorkflow(customLinks)
    }
}
```

---


## How It Works

```
startWorkflow()
     │
     ├── Add manual/custom links
     │
     │
     └── For each URL:
              ├── Clear cache, cookies, storage
              ├── Load URL in next available WebView node
              ├── Simulate scroll interaction on page load
              ├── Wait 8–12 seconds
              └── Mark node as complete → move to next URL
```

All URLs in the list are loaded exactly once. The workflow stops automatically after all links are completed.

---

## Requirements

| Component        | Minimum Version |
|-----------------|-----------------|
| Android SDK      | API 21+         |
| AndroidX WebKit  | 1.8.0+          |
| Gradle           | 7.0+            |
| Java             | 8+              |
| Kotlin           | 1.8+ (optional) |

---

## Permissions

Add the following to your `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

---

## Support the Developer

If this library saved you time and you'd like to support the developer, add this one line to your project:

**Java**
```java
workflowManager.supportDeveloper();
```

**Kotlin**
```kotlin
workflowManager.supportDeveloper()
```

That's it. No donations, no subscriptions — just one line. 🙏

---

## License

Copyright (c) 2026 White Skypper. All rights reserved.

This library is provided under the MIT License for **use only**.  
You may **not** modify, decompile, reverse engineer, or redistribute the source code in any form without explicit written permission from the author.

> Unauthorized modification or redistribution is strictly prohibited.
