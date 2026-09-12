# One-channel pilot specification
Status: proposed; not live.

## Scope
One business, one inbound lead channel, one service category, and a 14-day observation window after setup.
Channel is selected from customer evidence, not assumed to be missed-call SMS.
If volume is insufficient, report insufficient evidence; do not manufacture a success rate.

## Preconditions
- Written business authorization and agreed pilot price/payment terms.
- Approved contact eligibility rules, consent basis, privacy handling, and opt-out behavior.
- Approved service area, opening hours, qualification questions, and escalation owner.
- An authoritative booking source, or staff confirmation before calling a request booked.
- Agreed cost cap, stop conditions, retention policy, and deletion process.
- Baseline lead outcomes and a defined attribution method.
- Customer-approved disclosure of AI and any human review; no hidden handling claims.

## Smallest flow
Receive eligible lead -> deduplicate -> check suppression and policy -> draft response -> operator approval -> send through approved provider -> qualify -> request booking -> confirm authoritative booking -> record outcome.

Production sends are disabled until launch approval.
A draft or appointment request is not a confirmed booking.
For initial testing use synthetic data and provider sandbox/test destinations.

## Safety requirements
- Verify webhook authenticity and reject invalid payloads.
- Replayed events cannot create duplicate sends or bookings.
- Check opt-out and eligibility immediately before every send.
- Enforce per-business limits and a global kill switch.
- Use bounded retries; uncertain send results require reconciliation, not blind resending.
- Never invent prices, availability, guarantees, or technical repair advice.
- Route emergencies, complaints, unknown answers, and human requests to the client's designated staff.
- The AI cannot override policies through customer instructions.
- Keep tenant data isolated and logs minimal; redact secrets and personal data.
- Never collect real customer data in GitHub issues or this public repository.

## Acceptance tests before launch
1. Duplicate webhook produces one proposed action.
2. Suppressed recipient receives no message, including after approval.
3. Missing eligibility evidence blocks sending.
4. Out-of-area inquiry cannot receive a confirmed service booking.
5. Unavailable slot cannot be booked.
6. Provider timeout does not cause duplicate action.
7. Kill switch prevents queued sends.
8. Human request and urgent-risk scenario reach the escalation path.
9. Prompt injection cannot change permissions or reveal another customer's data.
10. Cancellation/refund updates outcome records without erasing audit history.

These are requirements, not tests that have already passed.

## Measurement
Track eligible leads, delivered responses, qualified conversations, requested appointments,
confirmed bookings, completed jobs, collected invoices, refunds, costs, and payments to Oto.
Maintain event times and links to the authoritative evidence in private customer systems.

Attribution: define an inactivity threshold and exclude existing bookings and already-active staff follow-up.
Label assisted outcomes honestly. Where feasible use an agreed holdout or staggered rollout;
without a credible comparison, report associated revenue, not proven incremental revenue.

Collected customer revenue is not pipeline, estimated job value, or Oto revenue.
Oto revenue is the amount a customer actually pays Oto.
Report contribution alongside revenue where direct job costs are known.

## Stop / continue
Stop immediately for unauthorized contact, data leakage, repeated duplicate actions, or unsafe responses.
Pause at the agreed cost cap.
Continue only after outcome review and explicit customer agreement.
One booking proves the flow can work, not scalable profitability or product-market fit.

## Deferred
Multi-agent platform, self-service SaaS, autonomous outbound, AI sales calls, multi-channel CRM,
automated pricing/contract commitments, payments integration, and elaborate dashboards.
