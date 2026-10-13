# Skoolly

Skoolly is a school administration and records management system built as a
Flutter monorepo with Firebase and an offline-first Windows desktop database.

## Applications

- `apps/desktop` — operational Windows application
- `apps/teacher_mobile` — teacher Android application
- `apps/admin_web` — Super Admin web application

## Shared packages

- `packages/core`
- `packages/models`
- `packages/authentication`
- `packages/database`
- `packages/sync_engine`
- `packages/repositories`
- `packages/firebase`
- `packages/permissions`
- `packages/reporting`
- `packages/shared_ui`

## Firebase

The Firebase project is `skoolly-3149f`.

Production security configuration lives in `firebase/`:

- `firestore.rules`
- `storage.rules`
- `firestore.indexes.json`

Cloud Functions live in `functions/`.

Cloud Functions deployment currently requires the Firebase Blaze plan. The
project remains usable for client-side Authentication and Firestore development
on Spark while server-side deployment is deferred.

## Development commands

Desktop:

```powershell
cd apps/desktop
flutter pub get
flutter analyze
flutter run -d windows
```

Admin web:

```powershell
cd apps/admin_web
flutter pub get
flutter analyze
flutter run -d chrome
```

Teacher mobile:

```powershell
cd apps/teacher_mobile
flutter pub get
flutter analyze
flutter run
```

Functions:

```powershell
cd functions
npm install
npm run build
```

Security tests:

```powershell
cd test/security
npm install
npm test
```

Package tests should be executed inside each package using the normal Flutter/Dart
test runner.

## Authentication

Firebase Authentication establishes identity. Small custom claims provide
role, school, and application access. School-scoped profiles belong under:

`schools/{schoolId}/users/{uid}`

The system-level `SUPER_ADMIN` role has no `schoolId` claim.

The one-time first-admin bootstrap has already been completed for the initial
development administrator and is not part of the runtime application.

## Source of truth

Windows desktop reads and writes SQLite first. The Firebase/Firestore backend
is the cloud source of truth. Synchronization is deliberately separated from
normal desktop UI data access.
