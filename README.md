# Intelligent Healthcare Companion for Elderly Users

> A system design study proposing an AI-powered personal health companion that works the way elderly users already trust — through their doctor, their family, and their own patterns.

---

## The Problem with "Just Use the App"

Elderly users are consistently underserved by health technology — not because the technology doesn't exist, but because it wasn't designed with them in mind. The average health monitoring app assumes the user is comfortable navigating menus, interpreting raw sensor data, and making decisions about their own care based on a notification they received at 2am.

Most elderly users aren't. And more importantly, most elderly users have spent decades building trust relationships with the people and systems around them — their family doctor, their children, their caregiver. They don't need another app telling them what to do. They need a system that works *through* those relationships, not around them.

This project starts from that observation. An elderly user who would never trust an app to manage their medication will trust their family doctor. An elderly user who finds smartphone interfaces overwhelming will respond to a system their daughter can monitor and explain. The technology isn't the product — the trust network is.

The system proposed here is designed around that insight: a personal health companion that integrates into the elderly user's existing trust relationships rather than asking them to form a new one with a piece of software.

---

## What This System Does

![System Overview](./system_overview.png)

A paired smartphone and smartwatch that continuously monitors the user's vitals, learns what is normal *for this specific person*, and takes graduated action when something changes — always through the appropriate human in their life first.

```
┌─────────────────────────────────────────────┐
│                   USER                       │
│         (Elderly, low-tech comfort)          │
└──────────────────┬──────────────────────────┘
                   │ wears
┌──────────────────▼──────────────────────────┐
│              SMARTWATCH                      │
│    Heart rate · SpO2 · Activity · Fall       │
└──────────────────┬──────────────────────────┘
                   │ streams to
┌──────────────────▼──────────────────────────┐
│           SMARTPHONE + AI LAYER              │
│   Personal baseline · Anomaly detection      │
│   Medication · Reports · Emotional support   │
└──────┬───────────────────────┬──────────────┘
       │ escalates through      │ alerts
┌──────▼──────┐         ┌──────▼──────────────┐
│   DOCTOR    │         │  FAMILY / CAREGIVER  │
│  (Tier 1)   │         │     (Tier 2)         │
└─────────────┘         └─────────────────────┘
```

Six core capabilities:

- **Health monitoring** — continuous vitals tracking against the user's personal baseline, not population averages
- **Safety and emergency response** — automatic fall detection, SOS, and emergency services contact with GPS location
- **Medication and appointment management** — reminders, compliance tracking, and caregiver-visible reporting
- **Communication and social connection** — simplified calling and family check-ins built into the same interface
- **Cognitive engagement** — memory exercises, orientation support, and long-term cognitive tracking
- **Emotional support** — an AI companion that knows the user's history and checks in during periods of isolation

---

## The Trust Architecture

The system's most important design decision is not technical — it's about who gets to act, and when.

When the AI detects something worth acting on, it doesn't act unilaterally. It escalates through the user's own trust network, in order:

1. **Family doctor** (if connected) — approves non-emergency interventions
2. **Family member** (if doctor unavailable) — receives notifications and confirms actions
3. **Baseline-only autonomy** (if neither available) — system acts conservatively, defaults to alerting over intervening

Only in a genuine emergency — a detected fall, a critical vitals anomaly — does the system bypass this hierarchy and contact emergency services directly. In every other situation, a human the user already trusts is in the loop before anything happens.

This isn't just a safety mechanism. It's the reason an elderly person who distrusts technology might actually use this system: because the system never asks them to trust it directly. It asks them to trust their doctor and their family — which they already do.

---

## Repository Structure

| File | Contents |
|---|---|
| [`SYSTEM_DESIGN.md`](./SYSTEM_DESIGN.md) | Full feature specification and UI design principles |
| [`AI_LAYER.md`](./AI_LAYER.md) | Personal baseline learning — how the system learns what is normal for this user |
| [`AUTONOMY.md`](./AUTONOMY.md) | The tiered autonomy model and the healthcare access question |
| [`USE_CASES.md`](./USE_CASES.md) | Five detailed use case scenarios with flow diagrams |
| [`LIMITATIONS.md`](./LIMITATIONS.md) | Honest scope boundaries and future directions |

---

## Background

This project began as a group course project at Islamic Azad University, Central Tehran Branch (Software Design Principles, Winter 2024), and was subsequently extended and restructured as an independent design study — translating the original system concept into a complete English-language design document with an expanded AI layer, tiered autonomy model, and detailed use case analysis.
