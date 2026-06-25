# The AI Layer: Personal Baseline Learning

Most health monitoring systems set alert thresholds based on population averages. A resting heart rate above 100 bpm triggers an alert. An SpO2 reading below 95% triggers an alert. These thresholds are clinically meaningful at the population level but poorly calibrated for any specific individual.

A lifelong athlete may have a resting heart rate of 48 bpm — a reading that would look alarming for the average person. An elderly patient with chronic obstructive pulmonary disease (COPD) may have a baseline SpO2 of 92% — below the standard alert threshold but completely normal for them. A system that alerts on population averages will produce excessive false positives for some users and miss real anomalies for others.

The personal baseline approach addresses this directly.

---

## 1. Baseline Construction

During an initial calibration period (recommended: 30 days of passive monitoring), the system constructs a personal health profile for the user. This profile captures:

- Resting heart rate distribution by time of day and activity level
- Normal SpO2 range during sleep, rest, and light activity
- Typical daily step count and activity pattern
- Normal sleep duration and quality metrics
- Individual medication response patterns where observable

The baseline is not static. It updates continuously using a rolling window approach, giving more weight to recent data while retaining long-term trend information. This allows the system to adapt to gradual changes in the user's condition (appropriate) while remaining sensitive to sudden deviations (the anomalies we want to detect).

---

## 2. Anomaly Detection Against Personal Baseline

Rather than comparing the user's readings to a population reference, the system compares them to this person's own established patterns. An alert is triggered when a reading deviates significantly from the user's personal normal — not from the population average.

```
INCOMING READING
       │
       ▼
┌─────────────────────────────┐
│  Compare to personal        │
│  baseline range             │
└──────┬──────────────────────┘
       │
       ├── Within normal range → Log, continue monitoring
       │
       ├── Mild deviation → Increase monitoring frequency, log event
       │
       ├── Moderate deviation → Notify user + escalate to Tier confirmer
       │
       └── Critical deviation → Bypass confirmation, contact emergency services
```

---

## 3. The Gradual Deterioration Problem

A system that continuously adapts to the user's baseline faces one important risk: it may adapt to a gradual deterioration in health rather than flagging it. If a user's resting heart rate slowly increases by 2 bpm per month over six months, a purely adaptive baseline will absorb that change without alerting anyone — even though the cumulative shift is clinically significant.

This is addressed through two parallel monitoring tracks:

**Short-term anomaly detection** — catches sudden deviations from recent baseline. Fast-moving, sensitive to acute events.

**Long-term trend analysis** — runs in parallel, looking for slow shifts in the baseline itself over weeks and months. Slower-moving, designed to catch gradual deterioration that short-term monitoring would miss.

When long-term trend analysis detects a meaningful baseline shift, it flags this for physician review rather than triggering an emergency alert — the appropriate response to a gradual change is a scheduled conversation with a doctor, not a 911 call.

---

## 4. The Cold Start Problem

The system cannot learn a personal baseline without data. During the initial 30-day calibration period, it falls back to conservative population-average thresholds. Users whose baseline differs significantly from the population average may receive inappropriate alerts during this period.

This is an honest limitation. It is mitigated by:

- Allowing the user's physician to input known baseline values manually during setup, bypassing or shortening the calibration period
- Using conservative thresholds during calibration that prefer false positives over false negatives — better to over-alert in the first month than to miss something real
- Clearly communicating to the user and family that the system is still learning during this period

---

## 5. Sensor Accuracy Limitations

Consumer-grade smartwatch sensors, while improving rapidly, do not match clinical-grade measurement devices:

- **Blood pressure estimation** from optical sensors has meaningful error margins and is best treated as a trend indicator rather than a precise measurement
- **SpO2 readings** can be affected by skin tone, motion, and sensor fit — the system applies motion artifact filtering and flags readings taken during activity as lower confidence
- **Heart rate** is the most reliable of the available sensors but still subject to interference during high-motion periods

The system treats all sensor data as **indicative rather than diagnostic**. It is designed to support clinical decision-making, not replace it. Any reading that triggers an escalation action is presented to the confirming human (doctor, family member, or emergency services) with its confidence level and the raw data behind it — never as a definitive clinical finding.
