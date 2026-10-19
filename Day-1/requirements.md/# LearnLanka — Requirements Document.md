# LearnLanka — Requirements Document

## 1. Problem Statement

LearnLanka is a Colombo-based startup that connects O/L and A/L students
with vetted tutors for one-to-one online tutoring sessions. Students need
a way to find tutors based on subject, grade, language, and price, while
tutors need to manage their availability and bookings. The platform must
also support payments, tutor payouts, reviews, and online video sessions.
The goal is to provide a reliable and accessible tutoring platform that
supports Sinhala, Tamil, and English users.

## 2. Personas

### Persona 1: Student

**Role:** A student preparing for O/L or A/L examinations.

**Goals:**
- Find tutors for a particular subject and grade.
- Compare tutors by language, price, and ratings.
- Book a one-hour tutoring session.
- Pay for sessions using an available payment method.
- Rate the tutor after completing a session.

**Frustrations:**
- Difficulty finding a tutor who teaches the required subject.
- Unclear prices or availability.
- Difficulty using a platform in a preferred language.
- Uncertainty about whether a booking or payment succeeded.

### Persona 2: Tutor

**Role:** A vetted tutor who provides online tutoring sessions.

**Goals:**
- Publish available time slots.
- Accept or decline booking requests.
- Manage cancellations according to the platform rules.
- Receive weekly payouts for completed sessions.
- Receive ratings and feedback from students.

**Frustrations:**
- Booking requests that conflict with availability.
- Unclear cancellation rules.
- Uncertainty about earnings and payout status.
- Difficulty managing reviews or session schedules.

### Persona 3: Operations Admin

**Role:** A member of the LearnLanka operations team.

**Goals:**
- Oversee platform activity and booking issues.
- Support students and tutors when problems occur.
- Review reported issues and inappropriate feedback.
- Help maintain reliable platform operations.

**Frustrations:**
- Unclear responsibility for resolving disputes.
- Missing information about bookings and payments.
- No defined process for handling complaints or account issues.

Note: The Operations Admin's detailed permissions and workflows
are assumptions because the brief does not define them.

## 3. Functional Requirements

### Student Requirements

FR-S1: The platform shall allow students to search for tutors by
subject, grade, language, and price band.

FR-S2: The platform shall display relevant tutor search results,
including available tutor information.

FR-S3: The platform shall allow students to request a one-hour
session with a tutor using an available time slot.

FR-S4: The platform shall allow students to pay for a booking
using a card or eZ Cash through the specified payment gateway.

FR-S5: The platform shall allow students to rate tutors from
1 to 5 stars and leave a one-line comment after a session is completed.

### Tutor Requirements

FR-T1: The platform shall allow tutors to publish their available
time slots.

FR-T2: The platform shall allow tutors to accept or decline booking
requests.

FR-T3: The platform shall enforce the stated minimum 12-hour
notice requirement for tutor cancellations, once the cancellation
policy is clarified.

FR-T4: The platform shall calculate a 15% commission on every
completed session.

FR-T5: The platform shall support weekly tutor payouts by bank
transfer through the specified payout service.

FR-T6: The platform shall allow tutors to rate students from
1 to 5 stars and leave a one-line comment after a session is completed.

### Operations Admin Requirements

FR-A1: The platform should provide an Operations Admin with access
to the information needed to investigate booking and payment issues.

FR-A2: The platform should support an administrative process for
handling reported problems and user complaints.

FR-A3: The platform should support requests for personal-data deletion
in accordance with the applicable privacy requirements.

Note: FR-A1 and FR-A2 require founder confirmation because the
brief does not specify the admin's exact permissions or workflows.

### Platform and Integration Requirements

FR-P1: The platform shall use a third-party video service for
online tutoring sessions rather than building video calling in-house.

FR-P2: The platform shall use PayHere as the payment gateway.

FR-P3: The platform shall support weekly tutor payouts through
Sampath Vishwa, subject to confirmation of the integration method.

FR-P4: The platform shall provide Sinhala, Tamil, and English UI
strings from launch.

FR-P5: The platform shall capture user consent and support a
personal-data deletion request flow.

FR-P6: The platform shall not store payment card data on
LearnLanka's servers.

## 4. Non-Functional Requirements

| Category | Metric | Target | How we'll measure it |
|---|---|---|---|
| Performance | Tutor search response time at p95 | Under 800 ms from a Sri Lankan ISP | Measure API response times |
| Availability | Booking endpoint uptime | 99.5% monthly | Monitor successful booking-endpoint responses |
| Concurrency | Simultaneous video sessions | 200 within the first 6 months | Monitor active video sessions through the provider |
| Privacy | Consent and deletion | Consent capture and deletion-request flow supported | Test consent records and deletion-request processing |
| Payment security | Payment card data stored on LearnLanka servers | Zero | Review data flows, storage, and integration behaviour |
| Mobile support | Expected Android usage | Design for mobile-first use; 80% expected Android usage | Test on representative Android devices |
| Internationalisation | UI language support | Sinhala, Tamil, and English at launch | Test UI strings in all three languages |

## 5. Assumptions

A1. Students and tutors will need accounts to access booking-related
features. The brief does not explicitly define registration.

A2. Tutor profiles will contain enough information for students to
compare tutors. The exact profile fields need confirmation.

A3. Tutor vetting will be completed through a process before tutors
are made available for booking. The vetting process is unspecified.

A4. Students will select an available slot before requesting a booking.
The exact booking-confirmation workflow needs confirmation.

A5. The platform will notify users about important booking changes.
The notification channels and message types are unspecified.

A6. Operations Admins will need a way to investigate booking and
payment problems. Their permissions need founder approval.

A7. Ratings and comments will be available only after a session is
completed, as stated in the brief.

A8. The platform will need a way to identify completed sessions before
calculating commission and including them in tutor payouts.

## 6. Out of Scope

The following features are not confirmed as part of the first version:

1. Building a custom video-calling system instead of using a
   third-party provider.
2. A dedicated mobile application; the brief specifies mobile-first
   design but does not require a native app.
3. Group tutoring sessions, since the brief specifies one-to-one sessions.
4. An AI-powered tutor recommendation system.
5. Automated dispute resolution or an advanced complaint-management system.
6. Recording tutoring sessions, which is not specified in the brief.

These exclusions should be confirmed with the founders before
the scope is finalised.