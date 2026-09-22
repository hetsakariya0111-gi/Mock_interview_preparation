ReactNative Interview Questions to prepare mock interview 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. React Native vs. hybrid frameworks (Cordova/Ionic)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

React Native renders actual native UI components (real native views), not a webview
Cordova/Ionic wrap a webview (HTML/CSS/JS) inside a native shell — essentially a website running inside an app
Result: React Native apps feel more native in performance and look, while hybrid apps can suffer from webview performance/UX limitations

2. React JS vs. React Native
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

React JS — renders to the DOM, used for web apps, uses HTML elements (div, span)
React Native — renders to native platform components (iOS/Android views), used for mobile apps, uses components like View, Text instead of HTML
Both share the same core React concepts (components, state, props, hooks), but the rendering targets and some APIs differ

3. Core architecture: JS thread, Native thread, Shadow thread
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

JavaScript thread — runs your app's JS code, business logic, React reconciliation
Native/UI thread (Main thread) — handles actual native rendering, gestures, animations at the OS level
Shadow thread — computes layout using the Yoga layout engine (Flexbox), determining component positions/sizes before native rendering
These run in parallel/separately, communicating via the bridge (legacy) or JSI (new architecture)

4. React Native Bridge
~~~~~~~~~~~~~~~~~~~~~~

The (legacy) mechanism that enables communication between the JavaScript thread and the Native thread
Since JS and Native run on separate threads with different memory, they can't call each other directly — the bridge serializes data to JSON and passes messages asynchronously in batches between the two
Limitation: async-only, serialization overhead — this was a key motivation for the New Architecture

5. New Architecture: JSI, TurboModules, Fabric
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

JSI (JavaScript Interface) — a lightweight C++ layer allowing JS to hold direct references to native objects/functions, enabling synchronous calls without JSON serialization, replacing the old bridge
TurboModules — the new system for native modules, built on JSI, allows lazy loading (modules load only when needed) and direct, faster native calls
Fabric — the new rendering system, also built on JSI, unifies and speeds up the UI rendering pipeline, enabling synchronous layout and better concurrent rendering support

6. Expo vs. React Native CLI
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Expo — a managed framework/toolchain on top of React Native, provides pre-built native modules, simpler setup, faster iteration, OTA updates — great for most apps and rapid development
React Native CLI — bare workflow, full control over native code, required when you need custom native modules not supported by Expo, or deep native customization
Choose Expo for speed and simplicity (and it now supports "prebuild" to eject when needed); choose CLI when you need full native control from the start

7. Core components & native mapping
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

<View> — maps to UIView (iOS) / android.view.View (Android) — like a div
<Text> — maps to native text rendering components
<Image> — maps to UIImageView (iOS) / ImageView (Android)
<ScrollView> — maps to native scroll view containers
React Native translates these JSX components into actual native platform widgets, not HTML elements

8. ScrollView vs. FlatList
~~~~~~~~~~~~~~~~~~~~~~~~~~

ScrollView — renders all children at once, regardless of whether they're visible — fine for small, fixed content
FlatList — uses virtualization, only renders items currently visible on screen (plus a buffer), recycling views as you scroll
FlatList is preferred for large datasets because it avoids memory bloat and keeps performance smooth, unlike ScrollView which would render everything upfront

9. Styling in React Native
~~~~~~~~~~~~~~~~~~~~~~~~~~

Uses JavaScript objects (via StyleSheet.create()) with camelCase property names, conceptually similar to CSS but not literal CSS
Supports Flexbox layout by default (main layout system)
Does not support standard CSS features like CSS Grid, media queries, or cascading selectors — styling is component-scoped, not global/cascading like web CSS

10. Responsive layouts across screen sizes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use Flexbox for adaptive layouts instead of fixed pixel dimensions
Use Dimensions API or useWindowDimensions() hook to get screen width/height dynamically
Use percentage-based sizing, flex: 1, and libraries like react-native-responsive-screen for scaling
Handle orientation changes by listening to dimension change events and adjusting layout accordingly

11. Navigation: Stack, Tab, Drawer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

React Navigation is the most common library for handling navigation
Stack Navigator — pushes/pops screens like a stack, standard "go forward/back" navigation (e.g. list → detail screen)
Tab Navigator — bottom or top tabs for switching between top-level sections of the app
Drawer Navigator — a side menu that slides in, typically for app-wide navigation options
These can be nested/combined (e.g. tabs containing stacks) for complex navigation flows

12. State vs. Props (React Native)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Same concept as React JS: State — internal, mutable data managed by a component; Props — data passed down from parent to child, read-only
No difference in behavior between React Native and React JS here — same core React principles apply

13. Deep Linking
~~~~~~~~~~~~~~~~

Allows the app to open directly to a specific screen via a URL (e.g. myapp://profile/123)
Configured via native setup (URL schemes on iOS Info.plist, intent filters on Android Manifest) plus React Navigation's linking configuration, which maps URL paths to specific screens
Also supports Universal Links (iOS) / App Links (Android) for https:// links that open the app directly

14. Native Modules & Native Components
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Native Modules — expose native (Java/Kotlin/Swift/Objective-C) functionality to JavaScript (e.g. accessing a native SDK, Bluetooth, custom hardware APIs)
Native Components — custom native UI views exposed to be used as React Native components (e.g. wrapping a native video player)
Needed when: a required feature isn't available in JS/React Native core or existing libraries, or when performance-critical code needs to run natively

15. Handling images efficiently
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use appropriately sized/compressed images rather than loading huge originals
Use caching libraries like react-native-fast-image for better memory management and caching
Avoid holding onto large image references unnecessarily; unmount/clear image state when not needed
Use resizeMode properly and lazy-load images in lists (e.g. within FlatList)

16. Local data persistence: AsyncStorage vs. MMKV
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

AsyncStorage — simple, asynchronous key-value storage, built by the community, easy to use but relatively slower, especially at scale
MMKV — a much faster, synchronous key-value storage library (built by WeChat, ported to RN), uses native memory-mapped files — significantly better performance for frequent read/writes
MMKV is generally preferred for performance-sensitive apps; AsyncStorage is fine for simple, low-frequency storage needs

17. Performance bottlenecks & FPS optimization
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Common causes: too many re-renders, heavy computations on the JS thread blocking UI updates, unoptimized lists (not using FlatList/virtualization), large unoptimized images, excessive bridge traffic (legacy architecture)
Optimizations: use React.memo/useMemo/useCallback to prevent unnecessary re-renders, use FlatList with proper keyExtractor and windowing props, move heavy logic off the JS thread (native modules or worklets), use useNativeDriver for animations, minimize bridge calls

18. InteractionManager
~~~~~~~~~~~~~~~~~~~~~~

A React Native API that lets you schedule long-running JS tasks to run after any current interactions/animations have finished
Helps keep animations smooth by deferring non-urgent work (like heavy computations or navigation transitions) until the UI thread is free
js
  InteractionManager.runAfterInteractions(() => { /* heavy task */ });

19. Animations: Animated API vs. Reanimated
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Animated API — React Native's built-in animation library, works reasonably well but animations can still be affected by JS thread congestion unless using useNativeDriver
Reanimated (v2/v3) — a more powerful library that runs animations on the UI thread directly using worklets, enabling smoother, more complex animations without JS thread bottlenecks, and better gesture-driven interaction support
Reanimated is generally preferred for complex, performance-critical, or gesture-based animations

20. useNativeDriver
~~~~~~~~~~~~~~~~~~~

A property in the Animated API that offloads the animation execution to the native UI thread instead of running it on the JS thread
Important because it keeps animations smooth even if the JS thread gets blocked by other work — avoids dropped frames/jank
Limitation: only supports non-layout properties (like opacity, transform), not layout-affecting properties (like width, height)

21. Secure data storage (tokens, passwords)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Never store sensitive data in plain AsyncStorage (not encrypted)
Use secure, encrypted storage: react-native-keychain (iOS Keychain / Android Keystore) or expo-secure-store
These leverage the OS's native secure storage mechanisms designed specifically for sensitive credentials

22. Hermes Engine
~~~~~~~~~~~~~~~~~

An open-source JavaScript engine optimized specifically for React Native, built by Meta
Improves performance by: precompiling JS to bytecode ahead of time (faster startup), smaller app size, reduced memory usage, and improved garbage collection compared to JavaScriptCore (the default engine on iOS)
Now the default engine for both platforms in modern React Native

23. Error boundaries & crash reporting
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Error Boundaries — React components that catch JS errors in their child component tree, preventing the whole app from crashing, showing a fallback UI instead
js
  class ErrorBoundary extends React.Component {
    componentDidCatch(error, info) { /* log error */ }
    render() { return this.state.hasError ? <Fallback /> : this.props.children; }
  }
Crash reporting tools — Sentry, Firebase Crashlytics — automatically capture and report JS errors and native crashes in production, providing stack traces, device info, and breadcrumbs for debugging

24. Generating APK/AAB (Android) and IPA (iOS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Android: build a release-signed APK (direct install) or AAB (Android App Bundle, required for Play Store — allows Google to generate optimized APKs per device) via Gradle build commands or Expo EAS Build
iOS: build an IPA file through Xcode (Archive → Export), requires proper provisioning profiles, certificates, and signing configured through the Apple Developer account
Modern workflow often uses EAS Build (Expo Application Services) to handle both platforms' builds in the cloud without needing local native toolchains

25. Fast Refresh
~~~~~~~~~~~~~~~~

React Native's modern development feature that automatically reflects code changes in the running app while preserving component state where possible
Combines the benefits of both Hot Reloading (state preservation) and Live Reloading (reliability on syntax/render errors) into one unified, more robust system

26. Hot Reloading vs. Live Reloading
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Hot Reloading (legacy concept) — injects updated modules into the running app, attempting to preserve component state
Live Reloading — reloads the entire app on every file save, losing all current state
Fast Refresh has effectively replaced both as the modern standard, combining their strengths

27. CodePush
~~~~~~~~~~~~

A service (originally Microsoft App Center, now community-maintained forks) that allows pushing JS/asset updates directly to users' devices without going through app store review
Works by delivering the updated JS bundle over-the-air; the app downloads and applies it on next launch (or immediately, depending on config)
Limitation: only works for JS/asset changes — any native code changes still require a full app store submission

28. Handling permissions (Camera, Location, Notifications)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Both platforms require permission declarations plus runtime requests:
Android — declared in AndroidManifest.xml, requested at runtime via PermissionsAndroid API or libraries
iOS — declared with usage description strings in Info.plist (e.g. NSCameraUsageDescription), requested at runtime through native prompts
Libraries like react-native-permissions provide a unified cross-platform API to check and request permissions consistently

29. RN Threading vs. Web Workers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

React Native — has distinct threads by architecture (JS thread, Native/UI thread, Shadow thread), with JS logic and native rendering separated by design, communicating via the bridge/JSI
Web Workers — an optional, explicit mechanism in standard JS/web to run scripts in background threads, separate from the main UI thread, mainly used for offloading heavy computation without blocking the DOM
Key difference: RN's threading is a core architectural feature deeply tied to rendering; Web Workers are an opt-in tool for parallelizing specific tasks in a web app

30. Debugging tools
~~~~~~~~~~~~~~~~~~~

Flipper — Meta's official debugging platform for React Native, provides network inspection, layout inspector, logs, and plugin support
React Developer Tools — inspects the React component tree, props, and state (browser extension or standalone)
Chrome DevTools — used for debugging JS logic, breakpoints, console logs when running in Chrome debugging mode
Also: console.log, in-app dev menu, native debugging tools (Xcode/Android Studio) for native-level issues

31. App Registry
~~~~~~~~~~~~~~~~

AppRegistry is the entry point API that registers the root React Native component with the native app runtime
js
  AppRegistry.registerComponent('MyApp', () => App);
Needed because it tells the native side which JS component to render as the app's root, bridging native app launch to the React component tree

32. Push Notifications: Remote vs. Local
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Remote notifications — sent from a server via push notification services (FCM for Android, APNs for iOS), require device token registration and backend integration
Local notifications — scheduled and triggered directly from within the app itself (no server involved), e.g. a reminder set by the user
Common libraries: react-native-push-notification, @notifee/react-native, or Firebase Cloud Messaging (FCM) for remote notifications

33. Memory leaks: causes & detection
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Common causes: unremoved event listeners/subscriptions, uncanceled timers/intervals, large unreleased image caches, retaining references to unmounted components (e.g. state updates after unmount), improper use of closures holding onto large data
Detection: use profiling tools like Flipper's memory plugin, Xcode Instruments (iOS), Android Studio Profiler, and watch for warnings like "state update on unmounted component"
Fix pattern: always clean up in useEffect return functions (remove listeners, clear timers)

34. Global state management: Redux Toolkit vs. Context API vs. Zustand
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Context API — built-in, simple, good for low-frequency global data (theme, auth), but can cause unnecessary re-renders at scale
Redux Toolkit — structured, predictable state management with a centralized store, good for large/complex apps with frequent state changes, includes dev tools and middleware support
Zustand — a lightweight, minimal-boilerplate state management library, simpler API than Redux, good middle ground for apps that need more than Context but don't want Redux's structure/overhead

35. Offline data sync & connectivity handling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use @react-native-community/netinfo to detect network status changes (online/offline) in real time
Store data locally (AsyncStorage/MMKV/SQLite/WatermelonDB) when offline, then sync with the server once connectivity returns
Implement a sync queue/strategy — track pending changes made offline, replay/sync them when back online, handle conflict resolution as needed
Libraries like WatermelonDB or Redux Persist + custom sync logic are common for more robust offline-first architectures