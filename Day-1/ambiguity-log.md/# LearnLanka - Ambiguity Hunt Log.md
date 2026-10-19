# LearnLanka — Ambiguity Hunt Log

## Brief reference

The LearnLanka challenge describes a platform connecting O/L and A/L
students with vetted tutors for one-to-one online tutoring. The platform
must support tutor search, one-hour bookings, payments, tutor availability,
cancellations, ratings, commission, weekly payouts, video sessions,
privacy requirements, and three-language support.

The brief does not fully define every workflow, exception, or business rule.
The following findings identify questions that should be clarified before
implementation begins.

## Findings

| # | Quote | Why ambiguous | Clarification question | Priority |
|---|---|---|---|---|
| 1 | "vetted tutors" | The vetting process and approval criteria are not defined. | What documents, qualifications, and checks must a tutor pass before being listed? | H |
| 2 | "O/L and A/L students" | The registration and age-verification rules are not specified. | What information and consent are required when a student creates an account, including for minors? | H |
| 3 | "price band" | The available price bands and currency are not defined. | What price ranges should students be able to select, and should prices be displayed in LKR? | M |
| 4 | "book a 1-hour session" | The booking confirmation and payment sequence is unclear. | Is a booking confirmed before payment, after payment, or after tutor acceptance and payment? | H |
| 5 | "pay via card or eZ Cash" | Failed payments, duplicate payments, and refunds are not covered. | What should happen if payment fails, is submitted twice, or a confirmed session is cancelled? | H |
| 6 | "accept or decline bookings" | The tutor's response deadline and automatic expiry rules are not stated. | How long does a tutor have to respond, and what happens if they do not respond? | M |
| 7 | "cancel with at least 12 hours notice" | The consequence of a late cancellation is unclear. | Can a tutor cancel within 12 hours, and if so, what penalty or exception applies? | H |
| 8 | "every completed session" | The definition and verification of completion are missing. | How is a session marked completed, and what happens if one participant disputes completion? | H |
| 9 | "15% commission" | The amount on which commission is calculated is unclear. | Is the commission calculated before or after discounts, refunds, or other charges? | H |
| 10 | "pay tutors weekly" | The payout schedule and eligibility rules are not defined. | Which day are payouts processed, and how are failed transfers or bank holidays handled? | H |
| 11 | "rate each other (1-5 stars)" | Review editing, visibility, and moderation rules are not specified. | Can users edit reviews, report comments, or see a review before submitting their own? | M |
| 12 | "one-line comment" | The exact character limit and content restrictions are unknown. | What is the maximum comment length, and what moderation rules apply? | L |
| 13 | "99.5% monthly uptime" | The measurement window, exclusions, and monitoring rules are not defined. | Is uptime measured continuously across the calendar month, and which outages are excluded? | M |
| 14 | "200 simultaneous video sessions" | The expected session duration and traffic pattern are not specified. | Must the platform support 200 active video sessions at the same time, and how will this be load-tested? | H |
| 15 | "deletion request flow" | The deletion process and treatment of records that must be retained are unclear. | What is the deletion deadline, and which records must be retained for legal or financial reasons? | H |
| 16 | "Sends OTP via SMS" | The challenge's diagram example mentions an SMS gateway, but the business requirements do not clearly require OTP. | Is SMS OTP required at launch, and for which actions? | M |
| 17 | "All UI strings must support Sinhala, Tamil, and English" | The default language and translation review process are unspecified. | Which language is selected by default, and who approves the translations? | M |
| 18 | "preferred cloud is Azure" | The preferred Azure services are listed, but deployment boundaries and data locations are not defined. | Which Azure services and deployment regions are approved for the first release? | M |
| 19 | "Operations Admin" | The brief requires an admin persona for the deliverable but does not define admin functionality. | What actions may an Operations Admin perform, and which actions require additional approval? | H |
| 20 | "one-to-one online sessions" | Rescheduling, no-shows, and session interruptions are not covered. | What should happen when a student or tutor is absent, loses connectivity, or requests a reschedule? | H |

## Results Summary

| Metric | Target | Achieved |
|---|---|---|
| Items found | 10+ | 20 |
| High-priority items | 3+ | 12 |
| Items convertible to test cases | 5+ | At least 10 |

Note: The priority counts above reflect the classifications in this draft.
Review them and adjust them if your assessment differs.

## Top 3 questions to ask the founders

1. What is the exact booking and payment sequence, including tutor
   acceptance, payment failure, cancellation, and refunds?

2. How is a session officially marked as completed, and how does this
   affect the 15% commission and weekly tutor payouts?

3. What are the cancellation, rescheduling, and no-show rules for both
   students and tutors?

## Reflection

### What kind of ambiguity tripped you up most?

The most difficult ambiguities were those involving multiple connected
workflows. For example, booking, payment, cancellation, completion,
commission, and payout depend on one another. A decision in one workflow
can affect the others.

### Which question is most likely to change the architecture if answered?

The booking and payment sequence may affect the architecture because it
determines how booking status, payment status, tutor acceptance, cancellations,
and payment-gateway responses interact. The system may need clear status
transitions and reliable handling of failed or repeated payment notifications.

## Why ambiguity is expensive

If developers make different assumptions about the same business rule,
they may build incompatible features. Discovering the disagreement after
implementation can require code changes, additional testing, and rework.
Clarifying important rules early helps the team agree on expected behaviour
before writing code.