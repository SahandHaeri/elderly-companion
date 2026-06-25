# System Design

## 1. System Architecture

The system operates across two primary devices: a **smartphone** (the central processing hub) and a **paired smartwatch** (the primary sensor interface). All processing that can be done on-device is done on-device, with cloud connectivity used only for external escalation actions — emergency calls, family notifications, pharmacy integration.

```
┌─────────────────────────────────────────────┐
│              USER INTERFACE LAYER            │
│     (Smartphone App + Smartwatch Display)    │
├─────────────────────────────────────────────┤
│           AI DECISION SUPPORT LAYER          │
│   (Anomaly Detection + Action Recommendation)│
├─────────────────────────────────────────────┤
│            DATA COLLECTION LAYER             │
│  (Vitals + Activity + Medication + Location) │
├─────────────────────────────────────────────┤
│           COMMUNICATION LAYER                │
│   (Family + Caregiver + Emergency Services)  │
└─────────────────────────────────────────────┘
```

---

## 2. Core Features

### 2.1 Health Monitoring
The system continuously monitors vital signs available through consumer smartwatch sensors: heart rate, blood pressure, blood oxygen saturation (SpO2), skin temperature, and step count. Raw sensor data is logged locally and processed against the user's personal baseline to distinguish meaningful changes from normal variation.

- Continuous heart rate and SpO2 monitoring
- Blood pressure trend tracking with physician-set target ranges
- Daily activity level monitoring against personal baseline
- Sleep quality tracking and anomaly detection during overnight periods

### 2.2 Safety and Emergency Response
The system's most time-critical functions operate with minimal user involvement by design — a user in distress cannot be expected to navigate menus.

- Fall detection via accelerometer and gyroscope analysis
- Automatic emergency services contact with GPS location when fall detected and user unresponsive
- SOS button accessible from the watch face at all times
- Location sharing with designated family members during anomaly events

### 2.3 Medication and Appointment Management
- Medication schedule management with smartwatch reminders
- Dosage tracking with family-visible compliance reporting
- Appointment scheduling with pre-visit preparation reminders
- Integration with pharmacy systems for refill management where available

### 2.4 Communication and Social Connection
Recognizing that social connection is a health outcome, not merely a convenience feature, the system includes structured support for maintaining the user's relationships.

- Scheduled check-in prompts with easy one-tap response options
- Simplified video and voice calling with large-format contacts interface
- Automated daily status updates to designated family members
- Remote screen sharing with designated family members, allowing real-time visual assistance without requiring the user to describe what they see on screen
- Encrypted on-device passcode storage with secure export to a trusted family member — ensuring account access is never lost if the device is damaged or replaced

### 2.5 Cognitive and Memory Support
Cognitive engagement is a protective factor against age-related decline.

- Memory games and cognitive exercises with adaptive difficulty
- Daily orientation prompts covering date, weather, and scheduled appointments
- Long-term memory journaling with voice input support
- Cognitive performance tracking over time

### 2.6 Emotional Support Mode
A conversational AI component designed specifically for elderly users — patient, unhurried, and consistent. This is not a general-purpose assistant but a purpose-built interaction mode that:

- Maintains persistent memory of the user's life history, preferences, and significant relationships
- Initiates check-ins during periods of detected low activity or isolation
- Escalates to human contact if the user expresses distress
- Never terminates a conversation abruptly or makes the user feel dismissed
- Personal guidebook feature: a curated, on-device library of step-by-step instructions for tasks the user frequently needs help with (how to make a video call, how to use a specific app, how to connect to Wi-Fi). Instructions are stored locally, readable aloud by the AI, and editable by family members remotely.
- "Be My Eyes" style visual assistance mode: family members or trusted contacts can join a live camera session to help the user with physical tasks — reading a label, identifying a button, navigating an unfamiliar environment — without requiring the user to leave the app.

### 2.7 Reporting and Caregiver Visibility
- Daily, weekly, and monthly health summary reports generated automatically
- Anomaly event logs with timestamps and sensor data
- Medication compliance reports
- Trend analysis for key health indicators over time

---

## 3. User Interface Design Principles

The interface is designed specifically for elderly users, many of whom have reduced visual acuity, limited fine motor control, and low familiarity with smartphone conventions. The following principles are non-negotiable across all screens:

- **Large, clearly labeled interactive elements.** All buttons are minimum 48x48dp with descriptive text labels. No icon-only controls.
- **High contrast typography.** Minimum 4.5:1 contrast ratio between text and background. Default font size 18sp, user-adjustable up to 28sp.
- **Consistent navigation structure.** The same five primary functions always accessible from the same locations. No hidden menus, no gesture-only navigation.
- **Touch-optimized layout.** Sufficient spacing between interactive elements to prevent accidental activation. Confirmation dialogs for irreversible actions.
- **Assistive technology support.** Full compatibility with screen readers and voice control. All images have descriptive alt text.
- **Customization options.** Font size, contrast mode, notification frequency, and feature visibility are all user-configurable.
- **Voice interaction.** All primary functions accessible via voice command for users with mobility limitations.
- **Family-assisted setup mode.** Initial configuration and ongoing customization can be completed remotely by a designated family member via shared access, removing the burden of technical setup from the user entirely.

---

## 4. Development Methodology

This project follows the **Waterfall development model**, selected for the following reasons:

- System requirements were clearly defined from the outset and unlikely to change significantly during development
- Sequential phase structure maps cleanly onto a project with well-understood dependencies between components
- Emphasis on comprehensive documentation at each phase supports the long-term maintainability of a safety-critical system
- Predictable timeline and milestone structure supports coordination across hardware and software components

**The five phases:**

1. **Requirements and Documentation** — stakeholder analysis, feature specification, regulatory considerations
2. **System Design** — architecture definition, database schema, API design, UI wireframing
3. **Implementation** — component development, sensor integration, AI model integration
4. **Verification and Testing** — unit testing, integration testing, validation with representative users
5. **Deployment and Maintenance** — phased rollout, monitoring, iterative improvement based on real-world usage
