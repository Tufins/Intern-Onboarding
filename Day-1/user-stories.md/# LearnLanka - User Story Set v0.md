# LearnLanka — User Story Set v0.1

## Story 1: Search for Tutors

**As a** Student  
**I want** to filter tutors by subject, grade, language, and price band  
**So that** I can find a tutor who meets my learning needs.

### Acceptance Criteria

- Given a student is on the tutor search page,
  when they select a subject, grade, language, and price band,
  then the results should match the selected filters.
- Given matching tutors exist,
  when the student searches,
  then the platform should display matching tutor results.
- Given a search request is made from a Sri Lankan ISP,
  when the search API response time is measured,
  then the 95th-percentile response time should be under 800 ms.

### INVEST self-check

- [x] Independent
- [x] Negotiable
- [x] Valuable
- [x] Estimable
- [x] Small
- [x] Testable

Note: Search performance depends on the agreed measurement conditions.

---

## Story 2: Book a Tutoring Session

**As a** Student  
**I want** to request a one-hour session from an available tutor  
**So that** I can receive individual online tutoring.

### Acceptance Criteria

- Given a tutor has an available time slot,
  when a student requests a one-hour session,
  then the platform should record the booking request.
- Given a requested slot is no longer available,
  when the student attempts to book it,
  then the platform should not confirm a conflicting booking.
- Given a booking request has been submitted,
  when its status changes,
  then the student should be able to see the updated status.

### INVEST self-check

- [x] Independent
- [x] Negotiable
- [x] Valuable
- [x] Estimable
- [x] Small
- [x] Testable

Note: The exact booking-confirmation and payment sequence needs clarification.

---

## Story 3: Manage Availability and Bookings

**As a** Tutor  
**I want** to publish available time slots and accept or decline booking requests  
**So that** I can control when I provide tutoring.

### Acceptance Criteria

- Given a tutor is signed in,
  when they publish an available time slot,
  then the platform should make that slot available for booking.
- Given a tutor receives a booking request,
  when they accept it,
  then the booking status should reflect the acceptance.
- Given a tutor receives a booking request,
  when they decline it,
  then the booking status should reflect the decline.
- Given a tutor wants to cancel a booking,
  when the cancellation request is submitted,
  then the platform should apply the agreed cancellation policy.

### INVEST self-check

- [ ] Independent
- [x] Negotiable
- [x] Valuable
- [ ] Estimable
- [x] Small
- [x] Testable

Note: Cancellation rules and the relationship between availability,
acceptance, and payment need further clarification.

---

## Story 4: Pay for a Session

**As a** Student  
**I want** to pay using a card or eZ Cash  
**So that** I can pay for my tutoring session using an available method.

### Acceptance Criteria

- Given a student is ready to pay,
  when they choose card or eZ Cash,
  then the platform should offer the supported payment flow through PayHere.
- Given the payment gateway reports a successful payment,
  when the platform processes the result,
  then the payment status should be recorded.
- Given the payment gateway reports a failed payment,
  when the result is processed,
  then the platform should not record the payment as successful.
- Given a payment is processed,
  when storage is checked,
  then LearnLanka servers should not store payment card data.

### INVEST self-check

- [ ] Independent
- [x] Negotiable
- [x] Valuable
- [ ] Estimable
- [x] Small
- [x] Testable

Note: The booking and payment sequence and the handling of refunds
or duplicate payment notifications need clarification.

---

## Story 5: Rate a Completed Session

**As a** Student or Tutor  
**I want** to give the other participant a star rating and a short comment  
**So that** users can share feedback after a completed session.

### Acceptance Criteria

- Given a session is completed,
  when a participant submits a rating,
  then the rating must be between 1 and 5 stars.
- Given a participant submits a comment,
  when the comment is saved,
  then it should be associated with the relevant session and participant.
- Given a session has not been completed,
  when a participant attempts to submit a review,
  then the platform should not accept the review.

### INVEST self-check

- [x] Independent
- [x] Negotiable
- [x] Valuable
- [x] Estimable
- [x] Small
- [x] Testable

Note: Comment length limits and moderation rules need clarification.

---

## Story 6: Manage Tutor Payouts

**As a** Tutor  
**I want** to receive weekly payouts for completed sessions  
**So that** I can receive the earnings I am owed.

### Acceptance Criteria

- Given a session is marked as completed,
  when the commission is calculated,
  then LearnLanka's commission should be 15% of the session amount.
- Given completed sessions are eligible for payout,
  when the weekly payout process runs,
  then the eligible tutor earnings should be calculated.
- Given a payout is processed through the agreed banking integration,
  when its result is received,
  then the payout status should be recorded.

### INVEST self-check

- [ ] Independent
- [x] Negotiable
- [x] Valuable
- [ ] Estimable
- [x] Small
- [x] Testable

Note: Tax, refunds, payout timing, and failed transfers are not
defined in the brief and may affect this story's scope.

---

## Story 7: Support Platform Operations

**As an** Operations Admin  
**I want** to investigate booking and payment issues  
**So that** I can help resolve problems reported by students and tutors.

### Acceptance Criteria

- Given an authorised Operations Admin needs to investigate an issue,
  when they access the relevant booking record,
  then they should see the information permitted by their role.
- Given a user reports a booking or payment problem,
  when the issue is recorded,
  then it should be available for follow-up.
- Given an admin attempts to access information outside their permissions,
  when the access is checked,
  then the platform should deny unauthorised access.

### INVEST self-check

- [ ] Independent
- [x] Negotiable
- [x] Valuable
- [ ] Estimable
- [x] Small
- [x] Testable

Note: This story is provisional because the brief does not define
admin permissions or an issue-management workflow.