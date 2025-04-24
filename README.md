# MatrixRSSILogger

This Android application, built using Kotlin, Jetpack Compose, and native C++ via JNI (with Eigen), fulfills the requirements of **Assignment 3 – Sensing and Native Code** for Mobile Computing (Winter 2025). It implements both parts:

1. **Matrix Calculator** (40 marks)
2. **Wi‑Fi RSSI Logger** (40 marks)

---

## Part 1: Matrix Calculator

**Objective:**
- Add, subtract, multiply, and divide matrices of arbitrary dimensions.
- Use a C++ vector/matrix library (Eigen) internally, exposed via JNI.

**Key Deliverables & Mapping:**
1. **UI Activity** (10 marks):
   - `MatrixCalculatorActivity.kt` using Jetpack Compose.
   - Dynamically prompts for two matrices’ dimensions, generates input fields, and displays results.

2. **Input Interface** (10 marks):
   - Text fields for rows/columns of A and B.
   - On "Generate Inputs", renders an `OutlinedTextField` grid for each matrix.
   - Validation and Toast messages for invalid operations.

3. **C++ Library Usage** (5 marks):
   - `CMakeLists.txt` includes Eigen headers and emits both DT_HASH/DT_GNU_HASH for Android 12+ compatibility.
   - All core operations (`+`, `-`, `*`, `inverse`) performed via Eigen.

4. **Native Code & JNI** (15 marks):
   - `matrix_operations.cpp`: templated helpers for square/rectangular operations.
   - `NativeLib.kt` exposes `external fun addMatrices`, `subMatrices`, `mulMatricesRect`, `divMatrices`.
   - Proper `System.loadLibrary("native-lib")` and interop.

**Usage:**
1. Launch **Matrix Calculator**.
2. Enter dimensions for A and B (e.g., 2×3 and 3×2 are supported).
3. Tap **Generate Inputs**, fill each cell.
4. Choose an operation. Results appear in a grid; errors shown via Toasts.

---

## Part 2: Wi‑Fi RSSI Logger

**Objective:**
- Log received signal strength (RSSI) of all accessible Wi‑Fi APs in three distinct locations.
- Collect 100 samples per location, visualize min/max and full 10×10 grid.

**Key Deliverables & Mapping:**
1. **App Interface** (10 marks):
   - `RSSILoggerActivity.kt` + `WifiRssiLoggerScreen()` Compose UI.
   - Three location buttons, a **Start Logging** button, and a 10×10 results grid.

2. **Data Logging** (15 marks):
   - Runtime request of `ACCESS_FINE_LOCATION` permission via Accompanist.
   - `BroadcastReceiver` listens to `WifiManager.SCAN_RESULTS_AVAILABLE_ACTION`.
   - On each scan, computes the **average RSSI** (`level`), repeats 100 times.
   - Persists samples in `SharedPreferences` so they survive app restarts and tab switches.

3. **Demo for Three Locations** (15 marks):
   - State is maintained per location (`loc_0`, `loc_1`, `loc_2` keys).
   - Switching tabs shows previously logged data, not clearing unless a new logging starts.
   - Visualizes min/max and full matrix of strength readings.

**Usage:**
1. Launch **Wi‑Fi RSSI Logger**.
2. Grant location permission when prompted.
3. Select a location tab, tap **Start Logging**; button updates `Logging… (x/100)`.
4. After 100 samples, view the 10×10 grid with RSSI.
5. Switch tabs to compare ranges across locations; data is persisted.

---

## Build & Run

1. **Clone** this repo:
   ```bash
   git clone https://github.com/kanishkgoell/MC_Assignment3.git
   ```
2. **Open** in Android Studio.
3. **Sync** Gradle, ensure NDK r27 & CMake ≥ 3.18 installed.
4. **Run** on an emulator or device (minSdk 21, targetSdk 35).

---

## Dependencies

- **AndroidX**: Compose, Lifecycle, Activity, Core‑ktx
- **Accompanist Permissions**: `com.google.accompanist:accompanist-permissions:0.31.5-beta`
- **Eigen**: bundled under `app/src/main/cpp/include`
- **NDK**: `c++_shared`

---

## File Structure

```text
MatrixRSSILogger/
├── app/
│   ├── src/main/
│   │   ├── cpp/
│   │   │   ├── CMakeLists.txt
│   │   │   └── matrix_operations.cpp
│   │   ├── java/com/example/matrixrssilogger/
│   │   │   ├── MainActivity.kt
│   │   │   ├── MatrixCalculatorActivity.kt
│   │   │   ├── RSSILoggerActivity.kt
│   │   │   └── NativeLib.kt
│   │   └── AndroidManifest.xml
│   └── build.gradle.kts
├── .gitignore
└── README.md
```

---
