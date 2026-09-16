# Architecture Summary

## CRM-native boundary

This demo keeps the entire tested flow inside GoHighLevel because the required behavior is CRM-native:

- contact tagging
- opportunity creation/update
- pipeline stage management
- task creation
- transactional follow-up emails
- stage-change triggers

That keeps the implementation simple and makes the CRM itself the source of truth for lead state.

## Flow

```text
Contact
  |
  | tag: home-services-lead
  v
Home Services Automation
  |
  +--> create/update opportunity
  |      pipeline: Home Services Lead Pipeline
  |      stage: New Lead
  |      status: Open
  |      source: Website
  |
  +--> immediate follow-up email

Opportunity stage change
  |
  +--> Qualified
  |      -> create follow-up task
  |
  +--> Call Booked
  |      -> send booking confirmation
  |
  +--> Won
         -> send won-customer follow-up
```

## Why not put this in n8n?

The tested actions are native CRM operations, so an external orchestrator would add unnecessary complexity.

A tool such as n8n becomes useful when the workflow needs capabilities outside the CRM boundary, for example:

- custom LLM classification or extraction
- cross-system synchronization
- unsupported third-party APIs
- complex JSON transformation
- webhook orchestration
- custom retry/error-handling logic
- enrichment from external systems
- voice-agent handoffs

This separation is intentional: keep routine CRM state and follow-up inside HighLevel, and use an integration layer only when the workflow crosses system boundaries or needs custom logic.
