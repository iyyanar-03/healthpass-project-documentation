# HealthPass

HealthPass is a patient-centred healthcare-record MVP. It connects a Flutter patient mobile app, a FastAPI backend, and a clinician web portal so patients can selectively share records with doctors.

> **MVP notice:** This is a student/prototype project. It is not a certified clinical system and must not be used for emergency decisions or real clinical deployment without security, privacy, compliance and operational review.

## Problem

Patients need to carry allergies, medicines, health issues, emergency details and history between appointments. HealthPass lets the patient control this information and lets a doctor access only the data a patient chooses to share.

## Solution overview

- **Patient mobile app:** profile, emergency card, health timeline, allergies, health issues and medicines.
- **Doctor web portal:** doctor sign-up/sign-in, QR camera scan or 8-character code entry, authorised patient record view and clinician record additions.
- **Backend API:** authentication, patient data APIs, consent-based sharing, QR/code redemption and audit events.
- **Cloud deployment:** FastAPI service and PostgreSQL database hosted on Railway.

## Architecture

```text
Flutter Patient App ── HTTPS REST API ── FastAPI Backend ── PostgreSQL
        │                         │
        │ generate QR / code      └── Doctor Web Portal
        └────────────────────────────── scan or enter code
```

| Component | Technology | Responsibility |
|---|---|---|
| Patient application | Flutter / Dart | Patient UI, authentication, profile, records, timeline, QR sharing |
| Backend | Python, FastAPI, SQLAlchemy | REST API, validation, access control, QR sessions and audit logs |
| Database | PostgreSQL on Railway | Persistent users, profiles, records, sessions and sharing grants |
| Doctor portal | HTML, CSS, JavaScript | Doctor account, camera scan/manual code and authorised data entry |
| Deployment | Railway + GitHub | Hosted API/database and source version control |

## Patient-to-doctor sharing workflow

1. Patient signs in to the mobile app.
2. Patient selects which information to share: profile, allergies, health issues, medicines and/or timeline.
3. The app requests a short-lived, one-time sharing session.
4. The app displays both a QR code and an 8-character code.
5. Doctor signs in to the web portal and scans the QR, or types the code.
6. Backend validates expiry, single-use status and the patient-selected permissions.
7. The doctor sees only authorised fields and can add records only in permitted categories.
8. Additions are stored in the patient record and appear in the patient app.

## Methodology

1. **Requirements discovery** — define patient, doctor and consent use cases.
2. **UI-first prototype** — build patient screens and test on Flutter web/mobile.
3. **API integration** — replace mock data with FastAPI endpoints.
4. **Role-based workflow** — add a doctor portal and consent sharing.
5. **Cloud deployment** — move from local Wi-Fi testing to Railway/PostgreSQL.
6. **Security hardening** — validate inputs, hash passwords/tokens, limit sensitive requests and add browser security headers.
7. **User testing** — test sign-up, profile editing, records, QR/code access and doctor-created updates.

## Security approach in this MVP

- Passwords use PBKDF2 hashes with per-user salts.
- Login session tokens and sharing codes are stored as hashes.
- Sharing codes are random, 8-character, short-lived and single-use by default.
- The patient chooses the scope of each share.
- Doctor actions are permission-checked and audit logged.
- CORS, HTTP security headers and rate limits protect the API.

## Local development

### Backend

```bash
cd healthpass-backend
.venv\Scripts\activate
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

### Patient app

```bash
cd healthpass-patient-app
flutter pub get
flutter run
```

Cloud Android build:

```bash
flutter build apk --release --dart-define=API_BASE_URL=https://YOUR-RAILWAY-DOMAIN
```

## Future improvements

- Verified clinician onboarding and organisation management
- Patient consent history and revoke dashboard
- Encrypted backups and managed secret rotation
- Automated tests and CI/CD
- Accessibility, multilingual UI and production monitoring
- Formal privacy, data retention and healthcare compliance review

## Repository policy

This repository documents the HealthPass design. It intentionally does **not** publish database credentials, API keys, patient data or private environment files.
