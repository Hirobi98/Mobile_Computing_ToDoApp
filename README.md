# Mobile Computing ToDo App

A simple to-do list app for iOS built with **SwiftUI** and **Firebase**. Users can sign up, log in, and manage their own tasks, which sync in real time through Cloud Firestore.

## Features

- Email and password sign up and log in (Firebase Authentication)
- Create, edit, complete and delete tasks
- Real-time sync across launches and devices (Cloud Firestore snapshot listener)
- Each user sees only their own tasks
- Progress card showing how many tasks are completed
- All / Active / Done filter
- Swipe left on a task to delete it
- Light and dark mode support

## Tech Stack

| Layer | Technology |
| --- | --- |
| UI | SwiftUI |
| Architecture | MVVM (`ObservableObject` view models) |
| Authentication | Firebase Auth |
| Database | Cloud Firestore |
| Minimum iOS | 16.1 |

## Project Structure

```
assin91/
├── assin91App.swift       App entry point, configures Firebase
├── RootView.swift         Shows LoginView or TaskListView based on auth state
├── AuthViewModel.swift    Sign in, sign up, sign out, auth state listener
├── TaskViewModel.swift    Firestore create, read, update, delete for tasks
├── TodoTask.swift         Task model and Firestore mapping
├── LoginView.swift        Login / sign up screen and shared UI theme
├── TaskListView.swift     Task list, progress card, filter, add button
├── TaskEditView.swift     Screen for creating and editing a task
└── GoogleService-Info.plist   Firebase configuration
```

## Data Model

Tasks are stored per user in Firestore:

```
users/{uid}/tasks/{taskId}
```

| Field | Type |
| --- | --- |
| `title` | String |
| `description` | String |
| `isCompleted` | Bool |
| `createdAt` | Timestamp |

## Getting Started

### Requirements

- A Mac with Xcode
- An iOS 16.1+ simulator or device

### Run the app

1. Clone the repository:
   ```bash
   git clone https://github.com/Hirobi98/Mobile_Computing_ToDoApp.git
   ```
2. Open `assin91.xcodeproj` in Xcode.
3. Wait for Swift Package Manager to resolve the Firebase packages.
4. Select a simulator or a connected device.
5. Press **Cmd + R** to build and run.

### Use your own Firebase project (optional)

1. Create a project in the [Firebase Console](https://console.firebase.google.com/).
2. Add an iOS app with the same bundle identifier as in Xcode.
3. Enable **Authentication → Email/Password**.
4. Create a **Cloud Firestore** database.
5. Download your `GoogleService-Info.plist` and replace the one in the `assin91` folder.
6. Set Firestore rules so each user can only access their own tasks:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{uid}/tasks/{taskId} {
         allow read, write: if request.auth != null && request.auth.uid == uid;
       }
     }
   }
   ```

## Notes

- The UI uses a shared theme (colors, cards, button style) defined in `LoginView.swift`.
- Authentication and database logic live only in the view models, so the UI can be changed without touching Firebase code.
