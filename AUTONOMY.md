# Tiered Autonomy Model

The system's ability to take autonomous action — ordering medication, contacting emergency services, alerting family — raises a fundamental design question: when should the system act without waiting for human confirmation?

The answer depends on two variables: **the severity of the situation** and **what support infrastructure the user has access to.**

---

## 1. The Escalation Hierarchy

The system never acts unilaterally in non-emergency situations. Every action is routed through the most appropriate human in the user's existing trust network — the people they already rely on, not a new relationship with a piece of software.

```
ANOMALY DETECTED
       │
       ▼
┌─────────────────────┐
│  CRITICAL?          │──── YES ──→ Emergency services contacted immediately
└──────┬──────────────┘             No confirmation required
       │ NO
       ▼
┌─────────────────────┐
│  TIER 1             │
│  Family Doctor      │──── Connected? ──→ Sends recommendation for approval
│  (highest trust)    │                    Doctor approves, modifies, or declines
└──────┬──────────────┘
       │ Not connected or unavailable
       ▼
┌─────────────────────┐
│  TIER 2             │
│  Family Member      │──── Available? ──→ Sends notification + confirmation request
│  (second tier)      │                   Family member approves or declines
└──────┬──────────────┘
       │ Not available
       ▼
┌─────────────────────┐
│  TIER 3             │
│  Baseline-only      │──── Acts conservatively based on learned baseline only
│  autonomy           │     Defaults to alerting over intervening
└─────────────────────┘
```

---

## 2. Tier Definitions

### Tier 1 — Verified Family Doctor
The system sends actionable recommendations to the physician's connected interface. Medication adjustments, referrals, and non-emergency interventions require physician confirmation before execution.

The physician receives: the specific recommendation, the sensor data that triggered it, the user's recent baseline trends, and a simple approve / modify / decline interface. Response time expectations are set appropriately — this is not an emergency channel.

Emergency actions bypass this confirmation entirely. A detected fall or critical vitals reading does not wait for physician approval.

### Tier 2 — Designated Family Member
In the absence of a connected physician, a designated family member receives notifications and confirmation requests for non-emergency autonomous actions. The family member can approve, modify, or decline the system's recommendation through a simple mobile interface — designed to be usable by someone with no medical background.

The system translates clinical data into plain language before presenting it to the family member. A family member should never be asked to interpret a raw SpO2 reading — they should be told "Mum's blood oxygen has been slightly lower than usual for the past two hours. The system suggests scheduling a call with her doctor. Do you want to do this?"

### Tier 3 — Baseline-Only Autonomy
Where neither a physician nor a family member is available, the system operates with the most conservative autonomy profile. It takes only actions it can justify purely from the user's learned baseline, with a strong preference for notification over intervention.

In practice, Tier 3 means: send alerts, log events, increase monitoring frequency, initiate check-ins. It does not mean: order medication, contact services, or make clinical decisions without a human in the loop.

Emergency contact remains automatic regardless of tier.

---

## 3. Emergency Override

In all tiers, the following conditions trigger immediate autonomous action with no confirmation required:

- Fall detected + user unresponsive for 30 seconds
- Heart rate outside critical range (below 40 or above 150 bpm) sustained for 60 seconds
- SpO2 below 88% sustained for 60 seconds
- User activates SOS manually

In these cases the system contacts emergency services directly, shares GPS location and recent vitals data, and simultaneously notifies all designated contacts. The threshold values above are defaults and can be adjusted by a physician during setup.

---

## 4. The Healthcare Access Question

The tiered autonomy model implicitly assumes a level of healthcare infrastructure that is not equally available to all elderly users:

- **Tier 1** assumes access to a family physician who is willing and able to engage with a connected health system — something that varies significantly between healthcare systems and between urban and rural settings
- **Tier 2** assumes the user has family members with smartphones who are available and willing to participate
- **Tier 3** is the fallback for users with neither — and it is a meaningfully weaker form of protection

In healthcare systems with universal access, where elderly patients are expected to have a family physician as a matter of policy, Tier 1 is the realistic default. In contexts where this is not guaranteed, the system degrades gracefully to Tier 2 or Tier 3 rather than failing entirely.

This is an honest limitation of the current design. A future direction worth pursuing: integration with community health worker networks, where a trained community health worker could serve a Tier 1-equivalent role for users without a connected physician — extending the trust hierarchy to serve users in underserved healthcare contexts.
