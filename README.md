# HighLevel CRM Automation

A hands-on GoHighLevel CRM automation demo for a home-services lead lifecycle, built and tested end to end in a live HighLevel workspace.

## What this demonstrates

This project shows practical CRM implementation rather than a mock configuration. A synthetic home-services lead was moved through a five-stage opportunity pipeline while HighLevel workflows handled intake, follow-up, task creation, booking confirmation, and won-customer follow-up.

**Pipeline**

`New Lead -> Qualified -> Call Booked -> Won -> Lost`

**Verified path**

`New Lead -> Qualified -> Call Booked -> Won`

The final test opportunity used synthetic business/contact data and a $5,000 opportunity value.

## Workflow design

### 1. Home Services Automation

**Trigger:** contact tag `home-services-lead`

**Actions:**
- create or update an opportunity in `Home Services Lead Pipeline`
- place the opportunity in `New Lead`
- set status to open
- set source to `Website`
- send an immediate lead follow-up email

The email execution was verified in HighLevel as **Success**, **Accepted**, and **Delivered**.

### 2. Qualified Lead Follow-Up

**Trigger:** pipeline stage changed

**Filters:**
- Pipeline: `Home Services Lead Pipeline`
- Stage: `Qualified`

**Action:** create a follow-up task for the qualified lead.

The workflow executed successfully and the task appeared on the test contact.

### 3. Call Booked Confirmation

**Trigger:** pipeline stage changed

**Filters:**
- Pipeline: `Home Services Lead Pipeline`
- Stage: `Call Booked`

**Action:** send a booking-confirmation email.

The workflow executed successfully after the test opportunity was moved from `Qualified` to `Call Booked`.

### 4. Won Customer Follow-Up

**Trigger:** pipeline stage changed

**Filters:**
- Pipeline: `Home Services Lead Pipeline`
- Stage: `Won`

**Action:** send a post-conversion thank-you email.

The workflow executed successfully after the test opportunity was moved from `Call Booked` to `Won`.

## Final CRM state

The final test opportunity reached **Won** with:

- Business: `Smith Heating & Air`
- Source: `Website`
- Opportunity value: `$5,000`
- Contact/opportunity data: synthetic demo data

![Home Services Lead Pipeline](docs/home-services-lead-pipeline.png)

## Architecture

```text
Contact tagged: home-services-lead
        |
        v
Create / update opportunity
        |
        v
New Lead ----> immediate follow-up email
        |
        v
Qualified ---> create follow-up task
        |
        v
Call Booked -> booking confirmation email
        |
        v
Won ---------> won-customer follow-up email
```

## What I practiced

- HighLevel contacts and opportunities
- pipeline and stage design
- workflow triggers and filters
- tags as automation entry points
- create/update opportunity actions
- task creation
- email automation
- stage-driven workflow execution
- execution-log debugging and delivery verification
- separating CRM-native automation from more custom orchestration that would belong in tools such as n8n

## Verification

See [`docs/verification.md`](docs/verification.md) for the test evidence and boundaries.

## Scope

This is a portfolio demonstration built with synthetic test data. It is not presented as a production client deployment, and it does not claim production deliverability, scaled campaign performance, or external API/voice integration.

## Related automation work

For an LLM-driven n8n example using structured classification, conditional routing, Google Sheets, and Gmail, see the companion [`ai-lead-qualification-n8n`](https://github.com/Neverlost-AI/ai-lead-qualification-n8n) repository.

## Author

Jeff Summerhays — Neverlost Systems
