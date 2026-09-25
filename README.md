# Trail - Real-Time Pedestrian Safety Companion

An app that makes walking journeys feel safer by keeping walkers connected with their companions. Built for iOS and watchOS using Swift, SwiftUI, and The Composable Architecture (TCA).

![Swift](https://img.shields.io/badge/Swift-5.9+-F05138?style=flat-square) ![SwiftUI](https://img.shields.io/badge/SwiftUI-007AFF?style=flat-square) ![TCA](https://img.shields.io/badge/Architecture-TCA-purple?style=flat-square) ![ActivityKit](https://img.shields.io/badge/ActivityKit-Live_Activities-black?style=flat-square) ![MapKit](https://img.shields.io/badge/MapKit-Navigation-green?style=flat-square) ![CoreLocation](https://img.shields.io/badge/CoreLocation-GPS-blue?style=flat-square) ![CloudKit](https://img.shields.io/badge/CloudKit-Sync-blueviolet?style=flat-square) ![WatchConnectivity](https://img.shields.io/badge/WatchConnectivity-watchOS-darkred?style=flat-square) ![AppIntents](https://img.shields.io/badge/AppIntents-Siri-yellowgreen?style=flat-square) ![WidgetKit](https://img.shields.io/badge/WidgetKit-Widgets-informational?style=flat-square)

---

## Video Demo and Walkthrough

| Activity | Description | Video Demo |
| --- | --- | --- |
| **Walker Initiates Journey** | Walker sets their destination and starts a new journey session. | https://github.com/user-attachments/assets/3656f57a-8bed-4859-9885-11214672c280 |
| **Companion Accepts Invitation** | Companion receives a live notification and accepts the journey monitoring request. | https://github.com/user-attachments/assets/bef9d385-81cb-4fb4-8dfd-4cd454cc2f75 |
| **Active Pedestrian Tracking** | Walker begins their route while location telemetry and breadcrumbs update continuously. | https://github.com/user-attachments/assets/9ff074c6-1026-4c40-95ca-009028c7e292 |
| **Live Companion Monitoring** | Companion views real-time movement, traversed routes, and ETA progress on the interactive map and live activity. | https://github.com/user-attachments/assets/197ad9ff-c97f-4f76-8b4d-255798c95aa7 |
| **Apple Watch Status Check** | Walker monitors active journey companion directly on watchOS for hands-free status updates. | https://github.com/user-attachments/assets/c77eb6b6-8c9b-4899-b089-e02d2e65ce6c |
| **Connection & Telemetry Check** | Companion verifies connection stability and detailed walker updates during the journey. | https://github.com/user-attachments/assets/3c1696bb-92d7-4019-931f-9c61839e14d3 |
| **Automated Arrival Detection** | App automatically identifies destination arrival, concludes the session, and triggers alerts. | https://github.com/user-attachments/assets/c26891d7-3718-49b1-9df8-244b95d79c20 |
| **Journey Log Review** | Walker and companion access the finalized route history and session summary log. | https://github.com/user-attachments/assets/6fe92320-5419-4bf5-996b-4eee74dfde37 |
---

## Project Context

An app that makes walking journeys feel safer by keeping walkers connected with their companions.

### Who it is for?
* People walking alone, especially at night, who want someone they trust to know about their journey.
* Companions who want lightweight visibility, without constantly contacting the walker.

### What it does?
* Shares live locations, route history, and automated arrival notifications.
* Provides passive, continuous tracking so the walker does not need to hold their phone or send constant manual updates.

### How to use?
Sign in with Apple -> Set your Default Destination -> Add your Trusted Person -> Start a Journey -> Walk -> Your Companion Watches -> End the Journey

---

## Core Personas and Features

### 1. The Walker (Pedestrian)
* Start a Walk with Zero Friction: Search for an address, select from saved places (such as "Home" or "Office"), or trigger the walk hands-free using Siri ("Hey Siri, start walking home").
* Pedestrian Route Guidance: Computes pedestrian routes via MapKit with real-time waypoint progression, remaining distance, and ETA calculations.
* Glanceable Updates: Uses ActivityKit (Dynamic Island and Lock Screen Live Activities) and an Apple Watch companion app (via WatchConnectivity) so the walker stays aware of their surroundings without staring at their screen.
* Telemetry Streaming: Runs background GPS updates through CoreLocation, broadcasting live coordinates, battery level, and route progress to CloudKit.

### 2. The Companion (Guardian)
* Instant Invite and Access: Receives a push notification when the walker starts a journey.
* Live Route Monitoring: Opens a map interface showing the walker's current location, the path already walked (breadcrumb trail), and the remaining destination route.
* Automated Arrival Confirmation: Gets automatically notified when the walker reaches their destination radius, closing the active session cleanly.

---

## System Flow

1. Authentication and Setup: User signs in with Apple (AuthenticationServices). Default destination (such as Home) is saved.
2. Route Computation: When starting a walk, MapKit calculates the walking polyline, step-by-step waypoints, and estimated arrival time.
3. Multi-Device Tracking Launch: The app starts an ActivityKit Live Activity on the Lock Screen and Dynamic Island, and syncs session state to the Apple Watch via WatchConnectivity.
4. Background Location Streaming: CoreLocation streams continuous GPS updates via an asynchronous stream. Coordinates and status deltas are synced to CloudKit's shared database.
5. Companion Observation: The companion opens the app or web tracking link to follow the walker's live marker and breadcrumb trail on the map.
6. Arrival and Session End: When the walker enters the destination geofence radius, CloudKit flags the journey as completed, notifies the companion, and terminates the Live Activity.

---

## Frameworks and Apple Technologies

### 1. User Interface and Experience
* [SwiftUI](https://developer.apple.com/xcode/swiftui/):
  - Primary UI framework across iOS and watchOS screens (map overlays, bottom sheets, settings, and profile).
  - Uses state management patterns with @Binding, @ObservableState, and StoreOf<R>.
* [UIKit](https://developer.apple.com/documentation/uikit):
  - Bridges low-level UI lifecycle events and specific view controller delegates through AppDelegate.
* [ActivityKit](https://developer.apple.com/documentation/activitykit):
  - Powers Lock Screen Live Activities and Dynamic Island expansions.
  - Presents compact, minimal, and expanded views showing live ETA, distance remaining, and walker safety status.
* [WidgetKit](https://developer.apple.com/documentation/widgetkit):
  - Supports Home Screen widgets and quick-launch shortcuts for common destinations (such as "Always Home").

### 2. Location, Mapping, and Hardware Sync
* [CoreLocation](https://developer.apple.com/documentation/corelocation):
  - High-precision continuous GPS location updates and heading tracking.
  - Encapsulated inside a dedicated asynchronous TrackingClient providing background streaming.
* [MapKit](https://developer.apple.com/documentation/mapkit):
  - Custom polyline rendering for pedestrian walking routes, destination annotations, search completions, and breadcrumb trails.
* [WatchConnectivity](https://developer.apple.com/documentation/watchconnectivity):
  - Synchronizes tracking sessions and alerts between iPhone and Apple Watch companion app (WCSession).
  - Allows the Walker to check progress and safety status directly from their wrist.

### 3. Networking, Cloud Sync, and Storage
* [CloudKit](https://developer.apple.com/documentation/cloudkit):
  - Serves as the cloud database and sync layer.
  - Syncs walk sessions, active telemetry, guardian relationships, and user profile data securely through iCloud Private and Shared Databases.
* [AuthenticationServices](https://developer.apple.com/documentation/authenticationservices):
  - Frictionless, privacy-preserving authentication using Sign in with Apple.

### 4. System Integrations and Accessibility
* [AppIntents](https://developer.apple.com/documentation/appintents):
  - Siri Shortcuts integration enabling hands-free commands like "Hey Siri, start walking home on Trail".
* [Contacts](https://developer.apple.com/documentation/contacts):
  - Native contact picker integration allowing users to select trusted guardians directly from their address book.

### 5. Architecture, Concurrency, and Testing
* [The Composable Architecture (TCA)](https://github.com/pointfreeco/swift-composable-architecture) (Point-Free):
  - Enforces unidirectional data flow (State -> Action -> Reducer -> Effect).
  - Scoped modular architecture (MainFeature, MainMapFeature, GuardianFeature, ProfileFeature).
  - Centralized dependency injection (@Dependency) for mocking GPS, CloudKit, and system clients.
* [Combine](https://developer.apple.com/documentation/combine) and Swift Concurrency (async/await, AsyncStream):
  - Handles continuous asynchronous event streams, background publisher-subscriber models, and network cancellations.
* [Swift Testing](https://developer.apple.com/documentation/testing) and [XCTest](https://developer.apple.com/documentation/xctest):
  - Unit testing of state reducers, actions, and side-effects without hardware dependencies.

---

## Project Structure

```
Astar/
|-- App/
|   |-- AstarApp.swift              # Main app entry point
|   |-- RootFeature.swift           # Root TCA reducer coordinating auth and main view
|   |-- ContentView.swift           # Root view wrapper
|   `-- AppDelegate/                # Lifecycle delegates and push notifications
|-- Features/
|   |-- Login/                      # Sign in with Apple authentication flow
|   |-- Onboarding/                 # First-time destination and permission setup
|   |-- Main/                       # Main navigation container and tab coordinator
|   |-- Map/                        # Interactive MapKit view, search sheets, and location logic
|   |-- Navigation/                 # Active journey routing, ETA calculations, and waypoint tracking
|   |-- Guardian/                   # Companion view for monitoring live walker sessions
|   |-- Profile/                    # User settings, saved places, and emergency contacts
|   `-- Intents/                    # Siri AppIntents and Shortcuts handlers
`-- Shared/                         # Modular Dependency Clients (@Dependency)
    |-- ActivityKit/                # Live Activity attributes and Dynamic Island updates
    |-- CloudKit/                   # CloudKit session sync and database manager
    |-- Contacts/                   # Native address book picker client
    `-- WatchConnectivity/         # watchOS cross-device session communication
```

---

## Team

* Awan - [LinkedIn](https://linkedin.com/in/dimas-prihady-setyawan-47a66821a/) | [GitHub](https://github.com/yaboidimsum)
* Chusen - [LinkedIn](https://linkedin.com/in/chsnkamal/) | [GitHub](https://github.com/chusenkamal)
* Nadia - [LinkedIn](https://linkedin.com/in/lubisnadia/) | [GitHub](https://github.com/chanandlerz)
* Royyan - [LinkedIn](https://linkedin.com/in/mproyyan/) | [GitHub](https://github.com/mproyyan)
* Safa - [LinkedIn](https://linkedin.com/in/safaauliya/) | [GitHub](https://github.com/chusenkamal)
