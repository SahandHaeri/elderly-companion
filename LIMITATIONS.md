# Limitations and Future Work

This document is a design study, not a deployed system. The following limitations are stated honestly — not to diminish the proposal, but because understanding where a system breaks is as important as understanding what it does well.

---

## 1. Local AI and On-Device Compute

The system's AI layer — personal baseline learning, anomaly detection, emotional support conversation, plain-language report generation — requires meaningful computational resources. Running this entirely on a consumer smartphone is not currently feasible for the full feature set.

Modern smartphones (iPhone 15 Pro, Samsung Galaxy S24) include dedicated neural processing units (NPUs) capable of running small language models locally. Apple's on-device models and Google's Gemini Nano demonstrate that lightweight AI is possible on-device. However, the gap between "lightweight summarization" and "persistent conversational AI with long-term memory and real-time sensor processing" is significant.

**Current realistic approach:** a hybrid architecture where time-critical functions (fall detection, anomaly alerting, medication reminders) run fully on-device with no internet dependency, while more computationally intensive functions (emotional support conversation, report generation, long-term trend analysis) run on a connected server when internet is available and degrade gracefully to simplified on-device versions when it is not.

**Future direction:** as on-device AI hardware continues to improve — a trend that is moving quickly — the balance will shift. The system architecture should be designed from the outset to migrate processing from server to device as hardware allows, rather than assuming a fixed split.

---

## 2. Connectivity and the Internet Gap

The system assumes reliable internet connectivity for its full feature set. This assumption fails in several real-world contexts:

- Rural and remote communities where broadband access is limited or unreliable
- Elderly users on fixed incomes who may not maintain consistent mobile data plans
- Power outage scenarios where home internet goes down

**What works offline:** all on-device functions — fall detection, medication reminders, local data logging, SOS, and cached guidebook content — continue operating without internet. Emergency services contact via cellular (not internet) also remains available.

**What breaks offline:** family notifications, remote screen sharing, report delivery, server-side AI features, and pharmacy or physician integration all require connectivity. Which if allowed by user, can send SMS, MMS notifications in emergency scenarios.

---

## 3. Data Storage and Privacy

The system collects sensitive personal health data continuously. Where that data lives and who can access it is not a technical question — it is a trust question, and it directly affects whether elderly users and their families will accept the system at all.

Three storage models are proposed, in order of privacy preference:

### Option A: Fully Local Storage
All data stored on-device. Nothing leaves the phone unless the user explicitly initiates a share (report delivery, emergency contact). Maximum privacy, minimum convenience. Appropriate for users with strong privacy concerns or limited internet access.

**Limitation:** if the device is lost, damaged, or stolen, health history is lost. Mitigated by the encrypted passcode export feature (see SYSTEM_DESIGN.md), which allows a trusted family member to hold a recovery key without having access to the health data itself.

### Option B: Self-Hosted Cloud
A technically capable family member runs a private server (a Raspberry Pi or a home NAS is sufficient) that the system backs up to. Data never leaves the family's own infrastructure. Strong privacy, requires one technically capable person in the support network.

**Limitation:** not realistic for all families. Setup and maintenance require ongoing technical involvement. A server failure without proper backup means data loss.

### Option C: Company Cloud with Verifiable Privacy Guarantees
Data stored on the system provider's servers under a strict, auditable privacy policy — no data sold, no advertising use, no third-party sharing, full data deletion on request. Less private than Options A or B by definition, but realistic for the majority of users.

**Limitation:** requires trusting the provider. This trust must be earned through transparent policy, independent auditing, and ideally open-source code that allows independent verification of what the system actually does with user data. A privacy guarantee that cannot be independently verified is not a guarantee.

**Our position:** the system should support all three options and default to Option A for new users, with Options B and C available as explicit user choices. No health data should be monetized under any storage model.

---

## 4. Sensor Accuracy

Consumer-grade smartwatch sensors are not clinical-grade devices. Blood pressure estimation from optical sensors carries meaningful error margins. SpO2 readings are affected by skin tone, motion, and sensor placement. The system treats all sensor data as indicative rather than diagnostic — a position that must be communicated clearly to users and caregivers to prevent over-reliance on specific readings.

This limitation will narrow over time as consumer sensor hardware improves, but it will not disappear entirely at the consumer price point.

---

## 5. The Cold Start Problem

The personal baseline learning system requires approximately 30 days of passive data collection before it can make personalized anomaly assessments. During this period the system falls back to population-average thresholds, which may produce false positives for users whose baseline differs significantly from the population mean.

Mitigated by allowing a physician to input known baseline values manually during setup, and by using conservative thresholds during calibration that prefer over-alerting to under-alerting.

---

## 6. Healthcare Infrastructure Assumptions

The tiered autonomy model works best when the user has a connected physician (Tier 1) or engaged family members (Tier 2). For users who have neither — isolated elderly individuals with no local family and no regular physician — the system degrades to Tier 3, its most conservative and least protective mode.

This is not a failure of the system design so much as a reflection of a real gap in healthcare infrastructure that technology alone cannot fill. The system can flag this gap (identifying users who are operating in Tier 3 and may benefit from community health worker support), but it cannot solve it unilaterally.

---

## 7. Future Directions

- Clinical pilot study with real elderly users to validate baseline learning approach and alert thresholds
- Integration with national health records systems where APIs are available
- On-device AI model optimization as mobile NPU hardware improves
- Expansion to support users with specific conditions: dementia, Parkinson's, post-surgical recovery
- Accessibility evaluation with users of varying technical literacy, physical ability, and cultural background
- Community health worker integration as a Tier 1.5 option for users without a connected physician
- Longitudinal study of the emotional support component's effect on loneliness and cognitive outcomes
