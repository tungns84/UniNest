# Product Requirements Document (PRD)
## Cash Payment Tracking System

---

## 1. Introduction/Overview

The Cash Payment Tracking System enables landlords and tenants to coordinate and track cash-based payments for room rentals during exam periods. This system focuses on payment coordination, documentation, and tracking rather than digital payment processing, providing a simple foundation for managing financial agreements while maintaining the flexibility of cash transactions.

**Goal:** Provide a simple payment coordination and tracking system that helps landlords and tenants manage cash payments with proper documentation and record-keeping, serving as a foundation for future digital payment integration.

---

## 2. Goals

1. **Coordinate cash payments**: Facilitate clear communication about payment amounts, timing, and methods
2. **Document agreements**: Provide written records of payment terms and agreements
3. **Track payment status**: Enable both parties to track payment completion and history
4. **Maintain simplicity**: Focus on coordination rather than complex payment processing
5. **Prepare for growth**: Design system architecture to support future digital payment integration

---

## 3. User Stories

**As a landlord**, I want to set clear payment terms for my listings so that tenants understand the payment requirements before booking.

**As a tenant**, I want to see payment requirements clearly so that I can prepare the correct amount and understand when payment is due.

**As a landlord**, I want to mark payments as received so that I can track which tenants have paid and which haven't.

**As a tenant**, I want confirmation when my payment is recorded so that I have proof of payment for my records.

**As both parties**, I want to see payment history so that we can reference past transactions if needed.

**As a system administrator**, I want to track payment activities so that I can monitor platform usage and help resolve disputes.

---

## 4. Functional Requirements

### 4.1 Payment Terms Configuration
1. The system must allow landlords to specify payment requirements when creating listings:
   - Total amount due
   - Deposit amount (if applicable)
   - Payment due date (at booking, at check-in, etc.)
   - Accepted payment methods (cash, bank transfer, etc.)
   - Late payment policies
2. The system must display payment terms prominently on room listings
3. The system must require tenant acknowledgment of payment terms before booking confirmation
4. The system must generate payment agreements with terms clearly documented
5. The system must support Vietnamese Dong (VND) as the primary currency

### 4.2 Full Payment at Booking Coordination
6. The system must coordinate full payment collection at the time of booking confirmation
7. The system must provide clear instructions to tenants about payment amount and timing
8. The system must generate payment coordination messages with landlord contact information
9. The system must create payment tracking records when bookings are confirmed
10. The system must provide payment confirmation workflow for landlords

### 4.3 Payment Status Tracking
11. The system must track payment status through simple states:
    - Payment Due: Booking confirmed, payment not yet received
    - Payment Received: Landlord confirms cash payment received
    - Payment Overdue: Payment not received by due date
    - Payment Disputed: Issues with payment amount or timing
12. The system must allow landlords to mark payments as received with timestamp
13. The system must send automatic reminders for overdue payments
14. The system must provide payment status visibility to both landlords and tenants
15. The system must maintain complete payment history for all transactions

### 4.4 Manual Admin-Only Refund Tracking
16. The system must provide admin interface for recording refund requests
17. The system must allow administrators to document refund reasons and amounts
18. The system must track refund status (requested, approved, completed, denied)
19. The system must maintain refund history linked to original booking records
20. The system must notify both parties when refund status changes

### 4.5 Single Currency Support (VND)
21. The system must display all amounts in Vietnamese Dong (VND)
22. The system must format currency displays according to Vietnamese standards
23. The system must validate payment amounts for reasonable ranges
24. The system must support common VND denominations in payment calculations
25. The system must provide clear currency notation in all payment communications

### 4.6 Payment Documentation and Records
26. The system must generate payment receipts when payments are marked as received
27. The system must provide downloadable payment records for both parties
28. The system must maintain audit trail of all payment-related activities
29. The system must support payment record search and filtering
30. The system must archive payment records for compliance and reference

---

## 5. Non-Goals (Out of Scope)

- **Digital payment processing**: No credit cards, online banking, or payment gateways
- **Automated fee calculation**: No processing fees or platform commission tracking
- **Advanced security features**: No encryption, tokenization, or fraud detection
- **Multi-currency support**: VND only for MVP
- **Automated refunds**: No automatic refund processing
- **Payment splitting**: No shared payments or group billing
- **Integration with banks**: No direct banking system connections

---

## 6. Design Considerations

### 6.1 User Interface Design
- **Clear payment information**: Prominent display of payment terms and amounts
- **Simple status indicators**: Easy-to-understand payment status displays
- **Mobile-friendly**: Optimized for Android app payment coordination
- **Documentation focus**: Easy access to payment records and receipts

### 6.2 Future Integration Readiness
- **Modular design**: Architecture that can accommodate future payment processors
- **Data structure**: Payment records designed to support digital payment data
- **API design**: Endpoints that can be extended for online payment processing
- **Scalable foundation**: Framework that supports growth to digital payments

---

## 7. Technical Considerations

### 7.1 Database Design
- **Payment terms table**: Store payment requirements per listing
- **Payment tracking table**: Track payment status and history
- **Payment agreements table**: Document agreed payment terms per booking
- **Refund records table**: Track refund requests and processing

### 7.2 API Design
- **Payment terms**: `GET/PUT /api/listings/{id}/payment-terms`
- **Payment status**: `GET/PUT /api/bookings/{id}/payment-status`
- **Payment history**: `GET /api/user/payment-history`
- **Payment confirmation**: `POST /api/payments/confirm`
- **Refund management**: `POST/GET /api/admin/refunds`

### 7.3 Integration Requirements
- **Booking System**: Link payment tracking to booking confirmations
- **User Authentication**: Verify landlord and tenant identities
- **Notification System**: Send payment reminders and confirmations
- **Document Generation**: Create payment agreements and receipts

### 7.4 Future Enhancement Preparation
- **Payment processor integration points**: Designed endpoints for future digital payments
- **Security framework**: Foundation for adding encryption and security features
- **Multi-currency foundation**: Database structure to support additional currencies
- **Automated processing**: Framework for adding automated payment workflows

---

## 8. Success Metrics

### 8.1 Payment Coordination
- **Payment clarity**: >90% of tenants understand payment requirements before booking
- **Payment completion**: >85% of bookings result in completed payment coordination
- **Payment disputes**: <5% of payments result in disputes or issues

### 8.2 Record Keeping
- **Documentation completeness**: >95% of payments have complete records
- **Receipt generation**: >80% of completed payments generate proper receipts
- **History accessibility**: Users can access payment history within 3 seconds

### 8.3 System Reliability
- **Status accuracy**: Payment status tracking matches actual payment completion >98% of time
- **Data integrity**: No payment record corruption or data loss
- **User satisfaction**: >4.0 rating for payment coordination experience

---

## 9. Open Questions

1. **Payment verification**: What documentation should landlords provide when marking payments as received?
2. **Dispute resolution**: What process should handle payment disputes between landlords and tenants?
3. **Late payment handling**: Should there be automatic consequences for overdue payments?
4. **Receipt format**: What information should be included in generated payment receipts?
5. **Integration timeline**: When should digital payment integration be planned for future versions?
6. **Audit requirements**: What level of payment auditing and reporting is needed?
7. **Backup verification**: Should there be secondary verification methods for large payment amounts?

---

**Document Version:** 1.0  
**Created:** 2024-12-19  
**Target Audience:** Junior Developer  
**Feature Priority:** Medium (Financial Foundation)  
**Dependencies:** Booking System, User Authentication, Notification System, Document Generation**Note:** This system is designed as a foundation for cash payment coordination and can be extended to support digital payment processing in future versions. 