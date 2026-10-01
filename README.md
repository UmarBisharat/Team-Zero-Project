# Smart Campus Wi-Fi Monitoring & Network Health Dashboard

A **Flutter-based campus network monitoring application** designed to help students, staff, IT teams, managers, and administrators measure Wi-Fi performance, monitor network health, report connectivity problems, and analyze network quality across campus locations.

## Overview

The application follows a simple monitoring workflow:

**Select Campus Location → Run Speed Test → Measure Network Metrics → Calculate Network Health → Store Results → Update Dashboards**

It combines a Flutter front end with Firebase/Cloud Firestore and dedicated services for authentication, location handling, network testing, permissions, and network-health analysis.

## Key Features

- User authentication and account management
- Campus location selection and management
- Wi-Fi/network speed testing
- Download and upload performance measurement
- Ping/latency monitoring
- Network health scoring and status
- Test history and detailed results
- Complaint and outage reporting
- Complaint management for IT/admin users
- Firestore-powered analytics
- Administrative health-threshold configuration
- User and role management
- Role-based permissions
- Manager and IT monitoring views
- System reporting
- User-facing chatbot/troubleshooting screen
- Profile image selection using camera/gallery

## Tech Stack

| Technology | Purpose |
| --- | --- |
| **Flutter / Dart** | Cross-platform application and user interface |
| **Firebase** | Backend integration and configuration |
| **Cloud Firestore** | Operational data storage and real-time streams |
| **GetX** | Application/controller integration used by account functionality |
| **SharedPreferences** | Local device state, including profile-image path |
| **image_picker** | Camera/gallery profile-image selection |

## System Architecture

```text
┌─────────────────────┐
│   Flutter Frontend  │
│ Screens / UI / UX   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Services / Logic    │
│ Auth • Location     │
│ Speed Test • Roles  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Network Measurement │
│ Download • Upload   │
│ Ping • Packet Loss* │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│   Network Health    │
│ Score / Status      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Firebase / Firestore│
│ Tests • Complaints  │
│ Locations • Users   │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│ Dashboards / Admin  │
│ Analytics • Reports │
└─────────────────────┘
```

\* Packet-loss measurement is part of the described project workflow but may be optional depending on the implementation.

## Network Test Flow

1. The user selects a campus location.
2. The user starts a speed test.
3. The application measures network performance.
4. `network_health.dart` converts the measured metrics into a health result.
5. The result is stored in Firestore.
6. History, analytics, dashboard, and management views consume the stored data.

Network-health inputs described by the project include **download speed, upload speed, ping/latency, and packet loss**, with statuses such as **Excellent, Good, Fair, Poor, and Critical**.

> The exact health-score formula and weighting are not documented in the supplied architecture material, so they are intentionally not specified here.

## Project Structure

```text
lib/
├── main.dart
├── firebase_options.dart
│
├── auth_controller.dart
├── permission_service.dart
├── location_service.dart
├── speed_test_service.dart
├── network_health.dart
│
├── login_screen.dart
├── signup_screen.dart
├── home_screen.dart
├── dashboard_screen.dart
├── speed_test_screen.dart
├── network_health_screen.dart
├── history_screen.dart
├── test_results_screen.dart
├── complaint_screen.dart
├── manage_complaints_screen.dart
├── analytics_screen.dart
├── health_thresholds_screen.dart
├── manage_locations_screen.dart
├── manage_users_screen.dart
├── role_permissions_screen.dart
├── manager_screen.dart
├── account_screen.dart
├── chatbot_screen.dart
├── settings_screen.dart
├── system_report_screen.dart
│
├── fade_slide_in.dart
└── exit_app_helper.dart
```

### Important Components

| Component | Responsibility |
| --- | --- |
| `main.dart` | Application entry point |
| `firebase_options.dart` | Firebase project configuration |
| `auth_controller.dart` | Authentication and account operations |
| `speed_test_screen.dart` | Starts tests and presents progress/results |
| `speed_test_service.dart` | Coordinates network measurements |
| `network_health.dart` | Calculates network-health score/status |
| `location_service.dart` | Associates activity with campus locations |
| `analytics_screen.dart` | Firestore-driven network/complaint analytics |
| `complaint_screen.dart` | User issue/complaint submission |
| `manage_complaints_screen.dart` | IT/admin complaint workflow |
| `permission_service.dart` | Centralized role/permission checks |
| `manage_users_screen.dart` | User administration |
| `role_permissions_screen.dart` | Role-based access management |
| `health_thresholds_screen.dart` | Network-health threshold configuration |
| `chatbot_screen.dart` | User assistance/troubleshooting interface |

## Data Model

Although the backend uses **Cloud Firestore** rather than a relational SQL database, the logical relationships can be represented as:

```text
USER 1 ────────< SPEED_TEST >──────── 1 LOCATION
  │                   │                    │
  │                   │ optional           │
  │                   ▼                    │
  └──────────────< COMPLAINT >─────────────┘
```

Conceptually:

- A user can run multiple speed tests.
- A user can submit multiple complaints.
- A campus location can have multiple tests and complaints.
- A complaint may reference a related speed test.
- Firestore represents these relationships using document IDs/references rather than SQL foreign-key constraints.

## Firebase & Real-Time Analytics

The Flutter application is connected to Firebase through `firebase_options.dart`.

The analytics layer listens to Firestore collections such as:

```dart
db.collection('speed_tests').snapshots()
db.collection('complaints').snapshots()
```

Using Firestore streams allows analytics and monitoring views to react when operational data changes.

## Roles & Access Control

The project defines several user categories:

- Student/staff user
- IT support staff
- Network/IT manager
- Administrator

Role and permission checks are centralized through the application's permission-related logic, while dedicated management screens provide administrative functionality.

## Chatbot

The project contains a dedicated `chatbot_screen.dart`, positioning the chatbot as a user-facing assistance or troubleshooting feature.

The supplied architecture documentation does **not** identify the chatbot model/provider, API, prompt logic, retrieval system, or backend implementation. Those details should be documented here once the implementation is available.

## Getting Started

### Prerequisites

Install:

- Flutter SDK
- Dart SDK
- Android Studio and/or VS Code
- A configured Firebase project
- A supported Android/iOS device or emulator

### Installation

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
cd YOUR_REPOSITORY
flutter pub get
```

Configure Firebase for your environment and ensure the generated Firebase configuration is available to the application.

Run the project:

```bash
flutter run
```

## Suggested Firestore Collections

The confirmed analytics implementation uses:

```text
speed_tests/
complaints/
```

The wider project architecture also includes location and user/role data. Exact collection names and the complete Firestore schema should be documented from the source implementation rather than assumed.

## Security

Because the application contains authentication, user roles, complaints, and operational network data, production deployments should use appropriately configured Firebase Authentication, Firestore Security Rules, and role-based authorization.

> The exact Firestore security rules were not included in the supplied architecture material.

## Future Documentation

Useful additions as the implementation evolves include:

- Exact network-health scoring formula
- Health threshold definitions
- Full Firestore schema
- Firebase Security Rules
- Chatbot provider/API architecture
- Screenshots or demo GIF
- Testing strategy
- Deployment instructions
- Supported platforms and minimum versions

## Contributing

Contributions can follow the standard GitHub workflow:

1. Fork the repository.
2. Create a feature branch.
3. Commit your changes with a clear message.
4. Push the branch.
5. Open a pull request describing the change.

## License

Add the license selected for this project here. For example, include a `LICENSE` file and replace this section with the appropriate license name.

---

**Smart Campus Wi-Fi Monitoring & Network Health Dashboard**  
Built with Flutter and Firebase to make campus network performance easier to measure, understand, and manage.
