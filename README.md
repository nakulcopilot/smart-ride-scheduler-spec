# Smart Ride Scheduler Agent - Specification Template

<p align="center">
  <img src="https://img.shields.io/badge/Agentic_AI-Spec_Driven-0077B5?style=for-the-badge&logo=ai&logoColor=white" alt="Spec Driven Development">
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License">
  <img src="https://img.shields.io/badge/Version-1.0-brightgreen?style=for-the-badge" alt="Version">
</p>

> **A comprehensive specification template for building production-ready Agentic AI applications using a spec-driven development approach.**

---

## About This Template

### Why I Created This Spec

Traditional software has requirements documents, functional specs, and acceptance criteria. But when it came to AI agents, teams would just say "let the agent figure it out" - and then wonder why the agent made unexpected decisions, leaked sensitive data, or failed in production.

This template is my attempt to solve that problem. I wanted to create a **practical, learnable framework** that Product Owners can use to:
1. **Think critically** about what their agent should and shouldn't do
2. **Define clear boundaries** (guardrails) for autonomous behavior
3. **Document decision logic** so engineers know what to build
4. **Set measurable success criteria** so QA can verify the agent works

### Why This Use Case?

I chose the **Smart Ride Scheduler** because it demonstrates real-world complexity:
- **Multiple APIs** (Uber, Ola, Google Maps, Calendar)
- **Autonomous decision-making** (choosing between providers)
- **Stateful workflows** (multi-step booking process)
- **Safety concerns** (financial transactions, privacy)
- **Error handling** (API failures, fallbacks)

This is NOT a toy example. It's a production-grade scenario that forces you to think through all the critical aspects of agentic app development.

### Who Is This For?

This template is designed for:

| Role | How They Use This |
|-------|---------------|
| **Product Owners** | Learn the key sections to include in an agent spec |
| **Technical PMs** | Translate requirements into engineering tasks |
| **Engineering Leads** | Understand agent behavior before coding |
| **QA Engineers** | Build test plans from acceptance criteria |

---

## Quick Start

### Using This Template

1. **Read through the spec** to understand each section's purpose
2. **Fork this repository** as a starting point
3. **Replace the content** with your agent's specification
4. **Use as a living document** throughout development

```bash
gh repo fork nakulcopilot/smart-ride-scheduler-spec
```

### Template Sections Overview

| Section | Purpose | Why It Matters |
|---------|---------|--------------|
| [Overview](#product-vision) | Product vision and agent persona | Defines what the agent IS |
| [Capability Inventory](#capability-inventory) | Tools, APIs, and permissions | Knows what the agent CAN do |
| [Decision Logic Framework](#decision-logic-framework) | Step-by-step workflow demo | Shows HOW the agent thinks |
| [State & Memory Management](#state--memory-management) | Short-term vs. long-term data | Manages agent "memory" |
| [Interaction Protocol](#interaction-protocol) | Input/output specifications | Defines communication |
| [Error Handling](#error-handling) | Failure recovery | Prevents dead ends |
| [Safety & Guardrails](#safety--guardrails) | Security boundaries | Stops unsafe actions |
| [Acceptance Criteria](#acceptance-criteria) | Testable success conditions | Defines "done" |
| [Testing Strategy](#testing-strategy) | Test approach | Validates behavior |
| [Deployment Metrics](#deployment-metrics) | KPIs for monitoring | Measures success |

---

## The Problem: Why Agentic Apps Need More

Agentic AI applications are fundamentally different from traditional software:

### Traditional Software

```
User clicks button → System executes code → Return result
```

The system does exactly what it's programmed to do.

### Agentic AI

```
User makes request → Agent decides how to act → Agent uses tools → Agent adapts based on response
```

The agent makes decisions autonomously - and can make wrong ones.

### This Creates New Challenges

1. **Unpredictable Behavior**: Without specs, agents make decisions you didn't anticipate
2. **Multi-Vendor Dependencies**: Agents integrate with multiple APIs that can fail
3. **State Complexity**: Agents maintain state across complex, multi-step workflows
4. **Safety Risks**: Agents can leak data, make unauthorized transactions
5. **Testing Difficulty**: How do you test "autonomy"?

**A good spec solves all of these by making the implicit explicit.**

---

## Key Concepts Explained

### 1. Agent Persona

The agent persona defines WHO the agent is - its role, goals, and constraints.

Think of it like a job description:
- What is the agent's role?
- What should it achieve?
- What should it NEVER do?

### 2. Capability Inventory

This is the agent's "toolbox" - every capability should answer:
- **What**: The capability name
- **Tool/API**: The technical implementation
- **Permission**: Access scope (critical for security)
- **Why**: The purpose (helps reviewers understand intent)

### 3. Decision Logic Framework

This is the heart of the spec. It shows HOW the agent processes a request step-by-step.

The CLI demo format makes it concrete:
- Real input examples
- Every decision point shown
- Clear outputs at each stage
- Fallback behavior documented

### 4. State & Memory Management

Agents have two types of memory:

| Type | What | Why |
|------|------|-----|
| **Short-term** | Current conversation context | Immediate decisions |
| **Long-term** | Past interactions, preferences | Personalization |

Key insight: **Clear retention policies** prevent privacy violations.

### 5. Guardrails

Guardrails are explicit boundaries that STOP the agent from doing unsafe things.

Critical guardrail rule: **No guardrail = no boundary = potential failure.**

---

## How To Use This As A Learning Template

### Step 1: Read the Full Spec
Start by reading each section. Don't just glance - understand WHY each section exists.

### Step 2: Identify Your Agent
Ask: "What autonomous agent do I want to build?"

### Step 3: Fill In The Template
For each section:
- What's the agent's role?
- What tools does it need?
- What decisions does it make?
- What can it NOT do?

### Step 4: Review With Stakeholders
Share the spec. Get feedback on:
- Did we miss any capabilities?
- Are the guardrails enough?
- Are success metrics measurable?

### Step 5: Treat As Living Document
The spec evolves. Update as:
- New requirements emerge
- Engineers find gaps
- Users provide feedback

---

## Example: Why Spec-Driven Matters

### Before Spec-Driven Development

```
Product: "Build an AI agent that books rides"
Engineer: "Okay" (builds whatever they think makes sense)
Result:
- Agent books ANY ride without validation
- No fallback if Uber fails
- No limit on spending
- Mayhem in production
```

### After Spec-Driven Development

```
Product: "Here's the spec with validation, fallbacks, guardrails"
Engineer: "Here's the implementation that matches spec"
Result:
- Agent validates inputs correctly
- Fallback to Ola if Uber fails
- Manual approval for expensive rides
- Predictable, safe behavior
```

---

## The Smart Ride Scheduler Spec

Below is a complete working example. Read it to understand how each section comes together.

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

### Capability Inventory

| Capability | Tool / API | Permission Scope | Purpose |
|------------|-----------|-----------------|---------|
| Book Uber ride | Uber Booking API (v1.2) | `uber:book` | Direct ride scheduling |
| Book Ola ride | Ola Booking API | `ola:book` | Direct ride scheduling |
| Estimate traffic | Google Maps Traffic API | `location:read` | Real-time ETAs |
| Add calendar event | Google Calendar API | `calendar:write` | Ride reminders |
| Persist ride history | Internal KV store | `memory:write` | Session storage |
| Retrieve ride history | Internal KV store | `memory:read` | Personalization |

---

### Decision Logic Framework

```
# Trigger
User: "Schedule a ride to Hyderabad Airport at 6:30 AM"
→ Parsed intent: Schedule ride
→ Destination: HYD (Rajiv Gandhi International Airport)
→ Requested time: 06:30 AM

# Validation
✓ Time check: 90 minutes before flight departure
✓ Airport code verified: HYD
✓ Location detected: Jubilee Hills, Hyderabad

# Availability Check
→ Querying Uber Booking API...
✓ Uber Premier available, pickup ETA: 5 mins, Price: ₹850
→ Querying Ola Booking API...
✓ Ola Prime available, pickup ETA: 7 mins, Price: ₹780

# Comparison
→ Best option selected: **Uber Premier** (optimized for ETA)

# Booking
✓ Ride booked successfully!
✓ Ride ID: UBER-HYD-20260419-001
✓ Driver: Ravi Kumar
✓ Price: ₹850

# Confirmation
📩 SMS & Calendar invite sent
```

---

### State & Memory

| Type | Example | Purpose |
|------|---------|---------|
| **Short-term** | location=Jubilee Hills, time=6:30 AM | Immediate decisions |
| **Long-term** | Last 3 rides to HYD, preferences | Personalization |
| **Retention** | Auto-purge after 30 days | Privacy compliance |

---

### Interaction Protocol

**Inputs:**
- Natural language: "Schedule a ride..."
- JSON API: `{ "destination": "HYD", "time": "6:30" }`
- Slack: `/schedule_ride`

**Outputs:**
- Text confirmation
- Calendar invite
- SMS/WhatsApp notification

---

### Error Handling

| Scenario | Response |
|----------|---------|
| Uber API fails 3 times | Switch to Ola |
| Both fail | "Unable to book. Please try later." |

---

### Safety & Guardrails

| Guardrail | Implementation |
|-----------|---------------|
| No payment data access | Agent never sees/processes CC numbers |
| Manual override > $100 | Human approval for ₹8,500+ cancellations |
| Fallback after failures | 3 failures → Offer alternative |

---

### Acceptance Criteria

| Criteria | Test |
|----------|------|
| Ride books successfully | API returns confirmation |
| Price in < 1 sec | Response time < 1000ms |
| Fallback works | Simulate Uber failure |
| No data leak | Audit logs for PII |

---

### Testing Strategy

| Type | Example |
|------|---------|
| Unit | Invalid airport code rejected |
| Behavior | "Tomorrow morning" handled correctly |
| Security | Unauthorized payment access blocked |

---

### Deployment Metrics

| Metric | Target |
|--------|--------|
| Success rate | 95% |
| ETA error | <= 5 minutes |
| Fallback frequency | ≤ 3% |

---

## Adapting This Template For Your Agent

### 1. Start With The Persona
Replace "Smart Ride Scheduler" with your agent's role.

### 2. Map Your Capabilities
List the tools and APIs your agent will use.

### 3. Document The Workflow
Walk through a real example from trigger to completion.

### 4. Define Your Guardrails
What's the worst thing your agent could do? Prevent it.

### 5. Set Measurable Goals
How will you know the agent is successful?

---

## Contributing

This template is open for improvements:
- **Open an issue** if you find gaps
- **Submit a PR** to enhance sections
- **Share your adapted version** with the community

---

## Connect

- **GitHub**: [nakulcopilot](https://github.com/nakulcopilot)
- **LinkedIn**: [Nakul Raut](https://linkedin.com/in/nakulraut)
- **Email**: nakulcopilot@gmail.com

---

## License

MIT License - Use this as a template for your own agentic apps.

---

<p align="center">
  <sub>Built with 🔥 using Spec-Driven Development</sub>
</p>
