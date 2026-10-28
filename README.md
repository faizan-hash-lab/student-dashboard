# ScreenFlow — Native Android Application (Assignment 2)

**Course:** Advanced Mobile Application Development (Java + XML)  
**Project Name:** ScreenFlow  
**Package:** `com.example.assignment2`  
**Platform:** Native Android (Java 17, XML Views, SDK 24–34)  
**Location:** `C:\Users\STRIKER\Desktop\app practice for class\Assignment2_Android_Java_XML`  

---

## 1. Project Overview & Purpose

**ScreenFlow** is a native Android application built using **pure Java and Android XML Views**. Designed to demonstrate comprehensive multi-screen navigation, activity lifecycles, and interface layouts, ScreenFlow functions as an academic student portal. It incorporates input validation, password masking, session state management, dialog interactions, and dynamic light/dark mode theming.

The application strictly satisfies all requirements for **Assignment 2: Android Application Development (Java + XML)**:
- **Zero Third-Party Hybrid Frameworks:** 100% Java and standard Android Views (no Kotlin, Compose, Flutter, or React Native).
- **Completely Local & Offline:** Local simulated authentication using secure SharedPreferences session management (zero cloud dependencies or unnecessary network permissions).
- **Complete Isolation:** Housed in its own independent directory (`Assignment2_Android_Java_XML`), keeping Assignment 1 (CardVault) untouched.

---

## 2. Implemented Screens (7 Screens)

| # | Screen Name | Activity Class | XML Layout | Layout Paradigm | Functional Flow & Key Features |
|---|-------------|----------------|------------|-----------------|--------------------------------|
| **1** | **Splash Screen** | `SplashActivity.java` | `activity_splash.xml` | `ConstraintLayout` | Native Android `SplashScreen` API integration (`Theme.ScreenFlow.Starting`), app branding logo, auto-routes to Login (or Dashboard if already authenticated). |
| **2** | **Login Screen** | `LoginActivity.java` | `activity_login.xml` | `LinearLayout` (in ScrollView) | Email and password inputs, error states, password visibility toggle, demo credentials notice, routing to Register and Forgot Password. |
| **3** | **Registration Screen** | `RegisterActivity.java` | `activity_register.xml` | `LinearLayout` (in ScrollView) | Full Name, Email, Password, and Confirm Password validation. Updates local account registry and routes back to Login with pre-filled email. |
| **4** | **Forgot Password Screen** | `ForgotPasswordActivity.java` | `activity_forgot_password.xml` | `ConstraintLayout` | Validates email format, presents academic simulated recovery notice (explaining no remote backend is connected), routes to Reset Password. |
| **5** | **Reset Password Screen** | `ResetPasswordActivity.java` | `activity_reset_password.xml` | `ConstraintLayout` | Validates minimum length (6+ chars) and matching passwords, updates local account credentials, routes back to Login with success confirmation. |
| **6** | **Dashboard / Home Screen** | `DashboardActivity.java` | `activity_dashboard.xml` | `GridLayout` (2-Column Grid) | Personalized greeting ("Welcome, {Name}"), interactive module cards (Profile, Courses, Assignments, Schedule, Settings), quick header and bottom Logout. |
| **7** | **Profile Screen** | `ProfileActivity.java` | `activity_profile.xml` | `LinearLayout` | Student avatar, display name, email, matriculation metadata (ID, Department, Status), Back to Dashboard, and Logout modal with session wipe. |

---

## 3. Explicit Intent Navigation Paths

Every navigation step between screens uses Android's **Explicit Intent** architecture with Intent Extras:

```
[Splash Screen]
       │
       ▼ (Explicit Intent)
  [Login Screen] ◄─────────────────────────────────────────────┐
       │                                                       │
       ├────► [Register Screen] ─────(Success Intent)──────────┤
       │                                                       │
       ├────► [Forgot Password] ───► [Reset Password] ─────────┘
       │
       ▼ (Explicit Intent + Extras: USER_NAME, USER_EMAIL)
[Dashboard Screen]
       │
       ├────► [Profile Screen] ───► (Logout Intent: Clear Task Stack) ──► [Login]
       │
       └────► (Logout Intent: Clear Task Stack) ───────────────────────► [Login]
```

- **Passing Display Information:** `DashboardActivity` and `ProfileActivity` receive `EXTRA_USER_NAME` and `EXTRA_USER_EMAIL` via `intent.putExtra()`.
- **Session Reset:** Logout utilizes `FLAG_ACTIVITY_NEW_TASK | FLAG_ACTIVITY_CLEAR_TASK` to purge the back stack so pressing back cannot re-enter private screens.

---

## 4. Mandatory Technical Requirements Verified

### A. Real Screen Demonstrations of All 3 Required Layout Types
1. **`LinearLayout`:**
   - [`activity_login.xml`](file:///app/src/main/res/layout/activity_login.xml) & [`activity_register.xml`](file:///app/src/main/res/layout/activity_register.xml): Organizes form inputs vertically with clear spacing and consistent button sizing.
   - [`activity_profile.xml`](file:///app/src/main/res/layout/activity_profile.xml): Sequential profile information cards.
2. **`GridLayout`:**
   - [`activity_dashboard.xml`](file:///app/src/main/res/layout/activity_dashboard.xml): Employs a 2-column `GridLayout` with column weights and card margins for responsive academic module tiles.
3. **`ConstraintLayout`:**
   - [`activity_splash.xml`](file:///app/src/main/res/layout/activity_splash.xml), [`activity_forgot_password.xml`](file:///app/src/main/res/layout/activity_forgot_password.xml), and [`activity_reset_password.xml`](file:///app/src/main/res/layout/activity_reset_password.xml): Flexible proportional positioning and responsive anchoring.

### B. Default & Alternative Night-Mode Resources
- **Default Theme Colors:** [`res/values/colors.xml`](file:///app/src/main/res/values/colors.xml) defines accessible light-mode tokens (Primary `#2458D3`, Surface `#FFFFFF`, Background `#F8FAFC`).
- **Night Mode Alternative Colors:** [`res/values-night/colors.xml`](file:///app/src/main/res/values-night/colors.xml) provides high-contrast dark tokens (Primary `#60A5FA`, Surface `#1E293B`, Background `#0F172A`).
- **Centralized Text Strings:** [`res/values/strings.xml`](file:///app/src/main/res/values/strings.xml) stores all button labels, titles, error dialogs, and validation prompts without hardcoded strings.

### C. Input Controls & Error Validation
- `textEmailAddress` for email fields with regex validation (`Patterns.EMAIL_ADDRESS`).
- `textPassword` for password fields with masking, visibility toggling, and 6-character minimum threshold.
- `textPersonName` with capitalization for student names.
- Inline errors on Material `TextInputLayout` components prevent crashes and guide the user.

---

## 5. Local Simulated Authentication & Test Credentials

Because the project is entirely offline and local, accounts are managed in memory and cached via `AuthManager.java` (`SharedPreferences`):

### Default Seed Demo Account
- **Email:** `student@screenflow.edu`
- **Password:** `password123`
- **Full Name:** `Alex Smith`

*Note:* You can also register any new account on the Registration screen or update passwords via Forgot/Reset Password. All changes persist across the local session.

---

## 6. How to Open and Run in Android Studio

1. Launch **Android Studio**.
2. Select **Open** and choose the project directory:
   `C:\Users\STRIKER\Desktop\app practice for class\Assignment2_Android_Java_XML`
3. Allow Gradle to sync using **JDK 17** (configured in `gradle.properties`).
4. Select an Android Emulator or connected physical device (API 24 or newer).
5. Click **Run** (`Shift + F10`) or the green **Play** button.

---

## 7. Command-Line Build & Verification

To compile, test, and assemble the debug APK from the terminal:

```bash
# Run Unit Tests
gradlew.bat testDebugUnitTest

# Build Debug APK
gradlew.bat assembleDebug
```

- **Unit Test Results:** Passed all test cases in `AuthValidationTest.java`.
- **Output APK:** `app/build/outputs/apk/debug/app-debug.apk`
