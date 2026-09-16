# Verification Notes

## Test environment

The workflow was built and exercised in a live GoHighLevel workspace using synthetic demo data for a fictional home-services company.

Test opportunity:
- Contact: `John Smith`
- Business: `Smith Heating & Air`
- Source: `Website`
- Opportunity value: `$5,000`

## Verified behavior

### New lead intake

A contact tag (`home-services-lead`) triggered the intake workflow.

Observed results:
- create/update opportunity action executed
- opportunity entered `Home Services Lead Pipeline`
- stage set to `New Lead`
- status set to open
- source set to `Website`
- follow-up email action executed

The HighLevel email event details showed:
- Event Status: `Success`
- Message: `Email queued successfully.`
- Email Statistics: `Delivered`
- Email Statistics: `Accepted`

### Qualified stage

The opportunity was manually moved from `New Lead` to `Qualified`.

Observed result:
- `Qualified Lead Follow-Up` workflow executed
- follow-up task was created on the contact/opportunity record

### Call Booked stage

The opportunity was manually moved from `Qualified` to `Call Booked`.

Observed result:
- `Call Booked Confirmation` workflow executed successfully
- booking-confirmation email action ran

### Won stage

The opportunity was manually moved from `Call Booked` to `Won`.

Observed result:
- `Won Customer Follow-Up` workflow executed successfully
- final CRM board showed the synthetic opportunity in the `Won` stage at `$5,000`

## Debugging performed

The build included real troubleshooting rather than only successful configuration.

During testing, an email initially failed because the synthetic contact used a non-deliverable example address. The contact email was changed to a real test inbox, the workflow was retriggered, and the execution log then reported `Success`, `Accepted`, and `Delivered`.

This distinction mattered: the workflow itself was functioning, while the original failure was caused by test-recipient data.

## Evidence boundary

These checks establish that the configured HighLevel workflows executed as described in the test workspace.

They do **not** establish:
- production client deployment
- large-scale deliverability or campaign performance
- production monitoring/alerting
- API, webhook, Twilio, Vapi, or Retell integration
- automated appointment-calendar synchronization beyond the stage-driven confirmation demo

Those capabilities are intentionally outside this repository's verified scope.
