# Smart Ride Scheduler Agent - Specification Template

<p align="center">
  <img src="https://img.shields.io/badge/Agentic_AI-Spec_Driven-0077B5?style=for-the-badge&logo=ai&logoColor=white" alt="Spec Driven Development">
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License">
  <img src="https://img.shields.io/badge/Version-1.0-brightgreen?style=for-the-badge" alt="Version">
</p>

> **A comprehensive specification template for building production-ready Agentic AI applications using a spec-driven development approach.**

This repository contains a complete product specification for the **Smart Ride Scheduler Agent** - an aggregator agentic app that compares multiple ride providers (Uber, Ola) in India to find the best ride option based on real-time rate, availability, and ETA.

## Why Spec-Driven Development for Agentic Apps?

Agentic AI applications require rigorous specification because they operate with:
- **Autonomous decision-making** - Agents make choices without human intervention
- **Multi-step workflows** - Complex state management across stages
- **External API integrations** - Multiple third-party dependencies
- **Safety-critical operations** - Financial transactions, data privacy

Without a spec, you'll get unpredictable behavior, security gaps, and undebuggable failures.

---

## Quick Start

### Using This Template

1. **Clone this template** for your own agentic app
2. **Customize the sections** for your use case
3. **Follow the spec** during implementation

```bash
gh repo fork nakulcopilot/smart-ride-scheduler-spec
```

### Template Sections

| Section | Purpose |
|---------|---------|
| [Overview](#overview) | Product vision and agent persona |
| [Capability Inventory](#capability-inventory) | Tools, APIs, and permissions |
| [Decision Logic Framework](#decision-logic-framework) | CLI demo with step-by-step flow |
| [State & Memory Management](#state--memory-management) | Short-term, long-term, retention |
| [Interaction Protocol](#interaction-protocol) | Input/output specifications |
| [Error Handling](#error-handling) | Failure recovery strategies |
| [Safety & Guardrails](#safety--guardrails) | Security and compliance boundaries |
| [Acceptance Criteria](#acceptance-criteria) | Testable success conditions |
| [Testing Strategy](#testing-strategy) | Unit, behavior, security tests |
| [Deployment Metrics](#deployment-metrics) | KPIs for production monitoring |

---

## Overview

### Product Vision

The Smart Ride Scheduler is an **aggregator agentic app** that compares multiple ride providers in India (Uber, Ola) and recommends the best option based on:
- **Real-time pricing**
- **Availability**
- **Estimated Time of Arrival (ETA)**

This ensures optimal convenience, cost-effectiveness, and reliability for airport transportation.

### Agent Persona

| Role | Goal |
|------|------|
| Personal Transportation Coordinator | Schedule reliable rides to selected airport |

### Success Metrics

- **≥ 95%** on-time pickups
- **≤ 5%** booking-failure rate

---

## Capability Inventory

| Capability | Tool / API | Permission Scope | Purpose (What & Why) |
|------------|-----------|-----------------|-------------------|
| Book Uber ride | Uber Booking API (v1.2) | `uber:book` | Direct ride scheduling for reliable airport transport |
| Book Ola ride | Ola Booking API | `ola:book` | Direct ride scheduling for reliable airport transport |
| Estimate traffic | Google Maps Traffic API | `location:read` | Real-time traffic data for accurate ETAs |
| Add calendar event | Google Calendar API | `calendar:write` | Insert ride details with reminders |
| Persist ride history | Internal KV store | `memory:write` | Store ride data across sessions |
| Retrieve ride history | Internal KV store | `memory:read` | Access past rides for personalization |

> **Note:** A KV store (Key-Value store) is a simple database that stores data as key-value pairs (e.g., `ride_12345` → `{driver: "Ravi", price: 850}`).

---

## Decision Logic Framework

### CLI Demo - Step by Step Flow

```
────────────────────────────────────────
# Trigger
User: "Schedule a ride to Hyderabad Airport at 6:30 AM"
→ Parsed intent: Schedule ride
→ Destination: HYD (Rajiv Gandhi International Airport)
→ Requested time: 06:30 AM

────────────────────────────────────────
# Validation
✓ Time check: 90 minutes before flight departure ✅
✓ Airport code verified: HYD ✅
✓ Location detected: Jubilee Hills, Hyderabad ✅

────────────────────────────────────────
# Availability Check
→ Querying Uber Booking API...
✓ Uber Premier available, pickup ETA: 5 mins, Price: ₹850
→ Querying Ola Booking API...
✓ Ola Prime available, pickup ETA: 7 mins, Price: ₹780

────────────────────────────────────────
# Comparison
→ Evaluating providers...
✓ Uber Premier: Faster pickup, slightly higher price
✓ Ola Prime: Lower price, slightly longer wait
→ Best option selected: **Uber Premier** (optimized for ETA reliability)

────────────────────────────────────────
# Travel-Time Estimation
→ Using Google Maps Traffic API...
✓ Distance: 32 km
✓ ETA: 45 mins
✓ Recommended departure: 5:45 AM

────────────────────────────────────────
# Booking
→ Calling POST /v1/rides (Uber)
✓ Ride booked successfully!
✓ Ride ID: UBER-HYD-20260419-001
✓ Driver: Ravi Kumar (Maruti Suzuki Dzire)
✓ Price estimate: ₹850

────────────────────────────────────────
# Fallback
(No fallback triggered – at least one provider available)

────────────────────────────────────────
# Confirmation
📩 Summary:
Ride ID: UBER-HYD-20260419-001
Pickup ETA: 5 mins
Driver: Ravi Kumar
Vehicle: Maruti Suzuki Dzire
Price: ₹850
Calendar event added: "Airport Ride – HYD"
SMS sent to user with ride details ✅

────────────────────────────────────────
✅ Process Complete – Ride Scheduled Successfully
```

---

## State & Memory Management

### State Types

| Type | Description | Purpose |
|------|-------------|---------|
| **Short-term state** | Temporary data during single interaction (location, time, destination) | Immediate decisions without persistent storage |
| **Long-term memory** | Persistent data (ride history, user preferences, frequent airports) | Personalization and continuity |
| **Retention policy** | Auto-purge rides older than 30 days | Privacy + data minimization |
| **State transitions** | Progressive updates (Trigger → Validation → Booking → Confirmation) | Deterministic, traceable workflow |

### Example Flow

```
User: "Schedule a ride to Hyderabad Airport at 6:30 AM."
→ Agent stores short-term state → location=Jubilee Hills, time=6:30 AM, airport=HYD
→ Agent checks long-term memory → Finds last 3 rides to HYD, prefers Uber Premier
→ Uses both → Suggests Uber Premier, estimates ETA, confirms booking
→ After completion → Saves ride to KV store for 30 days
```

### Why It Matters

- **Performance**: Separates transient vs. persistent data for speed
- **Privacy**: Auto-purge ensures compliance
- **Personalization**: Long-term memory enables smarter suggestions
- **Reliability**: Clear state transitions prevent logic errors

---

## Interaction Protocol

### Inputs

| Input Type | Example | Purpose |
|------------|---------|---------|
| Natural language | "Schedule a ride to Hyderabad Airport at 6:30 AM." | Human-friendly trigger |
| JSON API | `{ "destination": "HYD", "time": "6:30" }` | Developer integration |
| Slack slash command | `/schedule_ride` | Workplace quick actions |

### Outputs

| Output Type | Example | Purpose |
|-------------|---------|---------|
| Text confirmation | "Ride booked: Driver Ravi Kumar, ETA 5 mins, Price ₹850." | Immediate user feedback |
| Calendar invite | Adds "Airport Ride – HYD" to Google Calendar | Schedule visibility + reminders |
| SMS/WhatsApp notification | "Your Uber to HYD is confirmed. Driver arriving in 5 mins." | Mobile updates |

---

## Error Handling

| Error Type | Example | Purpose |
|------------|---------|---------|
| API failure recovery | If Uber API fails 3 times → "Uber booking unavailable. Try Ola?" | Prevents dead ends |
| Fallback options | After 3 Uber failures → Switch to Ola | Ensures completion |

---

## Safety & Guardrails

| Guardrail | Example | Purpose |
|------------|---------|---------|
| No payment data access | Agent never stores/processes credit card numbers | Protects financial data |
| Manual override for cancellations > $100 | Premium ride (₹8,500) requires human approval | Prevents fraud |
| Fallback after repeated API failures | 3 Uber failures → Offer Ola | Keeps user informed |

---

## Acceptance Criteria

| Criteria | Example | Validation |
|----------|---------|------------|
| Ride booking success | User requests ride → Books successfully via Uber API | Core function works |
| Price estimate within 1 sec | Returns "₹850" in under 1 second | Responsiveness |
| Try later fallback | If Uber unavailable → Offer Ola | Alternative handling |
| Ride history queryable | GET /rides returns last 5 rides | Memory persistence |
| No unintended logging | Logs show no sensitive data | Privacy compliance |

---

## Testing Strategy

| Test Type | Example | Purpose |
|-----------|---------|---------|
| **Unit** | Validate invalid airport codes are rejected | Component correctness |
| **Behavior** | "Schedule a ride tomorrow morning" → Handle ambiguous time | Edge case handling |
| **Security** | Unauthorized payment access → Blocked | Guardrail enforcement |

---

## Deployment Metrics

| Metric | Target | Purpose |
|--------|--------|---------|
| Success rate | 95% | Reliability measurement |
| ETA error | ≤ 5 minutes | Prediction accuracy |
| Fallback frequency | ≤ 3% | API stability monitoring |

---

## Using This Template

### For Your Own Agentic App

1. Fork this repository
2. Replace "Smart Ride Scheduler" with your agent name
3. Customize each section with your spec
4. Share as a template for your team

### Contributing

This spec template is open for improvements:
- Open an issue for suggestions
- Submit PRs to enhance sections
- Share your adapted versions

---

## Connect

- **GitHub**: [nakulcopilot](https://github.com/nakulcopilot)
- **LinkedIn**: [Nakul Raut](https://linkedin.com/in/nakulraut)
- **Email**: nakulcopilot@gmail.com

---

## License

MIT License - Feel free to use this as a template for your own agentic app.

---

<p align="center">
  <sub>Built with 🔥 using Spec-Driven Development</sub>
</p>