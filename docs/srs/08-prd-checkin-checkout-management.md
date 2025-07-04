# Product Requirements Document (PRD)
## Check-in/Check-out Management System

---

## 1. Introduction/Overview

The Check-in/Check-out Management System is a landlord-focused feature that enables property owners to efficiently manage tenant arrivals and departures during exam periods. This system addresses the critical need for organized tenant tracking, room occupancy management, and streamlined administrative processes when handling multiple short-term tenants during peak exam seasons.

**Goal:** Provide landlords with a comprehensive digital tool to manage tenant check-ins and check-outs, track room occupancy in real-time, and maintain accurate records of all tenant activities throughout exam periods.

---

## 2. Goals

1. **Streamline check-in process**: Reduce tenant check-in time from 15 minutes to under 5 minutes
2. **Improve occupancy tracking**: Provide real-time visibility of room availability and current occupants
3. **Enhance data accuracy**: Eliminate manual record-keeping errors through digital data capture
4. **Support compliance**: Maintain proper records for legal and safety requirements
5. **Increase landlord efficiency**: Allow management of multiple properties from a single dashboard

---

## 3. User Stories

**As a landlord**, I want to quickly check in new tenants by capturing their basic information and room assignment so that I can efficiently process arrivals during busy exam periods.

**As a landlord**, I want to see real-time occupancy status of all my rooms so that I can quickly identify available spaces for new tenants.

**As a landlord**, I want to receive warnings when room capacity is being exceeded so that I can make informed decisions about accommodation arrangements.

**As a landlord**, I want to process tenant check-outs and update room availability so that I can immediately offer those rooms to new candidates.

**As a landlord**, I want to view tenant information in multiple formats (list, room grid, calendar) so that I can manage my properties according to my preferred workflow.

**As a landlord**, I want to maintain historical records of all tenants so that I can reference past stays and build relationships with returning exam candidates.

**As a property manager**, I want to access the same check-in/check-out tools as the property owner so that I can assist with daily operations.

---

## 4. Functional Requirements

### 4.1 User Access Control
1. The system must restrict check-in/check-out functionality to verified landlords and property owners only
2. The system must require landlord authentication before accessing tenant management features
3. The system must associate all check-in/check-out activities with the authenticated landlord's properties
4. The system must maintain audit logs of all user actions for security and accountability

### 4.2 Check-in Process
5. The system must provide a quick check-in form capturing: Full name, ID card/passport number, phone number, assigned room, check-in date/time, planned duration of stay
6. The system must validate that the assigned room exists and belongs to the authenticated landlord
7. The system must automatically timestamp all check-in activities
8. The system must allow editing of check-in information within 24 hours of initial entry
9. The system must support bulk check-in for group bookings (same exam session)
10. The system must send confirmation notifications to the tenant's provided phone number

### 4.3 Room Capacity Management
11. The system must display current occupancy count vs. maximum capacity for each room
12. The system must show visual warnings (orange indicator) when room capacity is approached (80% full)
13. The system must show critical warnings (red indicator) when room capacity is exceeded
14. The system must allow check-in to proceed even when capacity warnings are displayed
15. The system must track capacity history for analytics and planning purposes

### 4.4 Check-out Process
16. The system must provide a check-out interface listing all current tenants for the landlord's properties
17. The system must capture check-out date/time, room condition notes, and general feedback
18. The system must immediately update room availability upon check-out completion
19. The system must calculate actual stay duration vs. planned duration
20. The system must allow partial check-outs for shared rooms
21. The system must notify the tenant of successful check-out via SMS/app notification

### 4.5 Tenant Management Dashboard
22. The system must provide a multi-view dashboard with the following interfaces:
    - List view: All tenants sorted by check-in date, room, or name
    - Room grid view: Visual representation of all rooms with current occupancy
    - Calendar view: Timeline showing check-ins, check-outs, and availability periods
    - Analytics view: Occupancy statistics, revenue tracking, and trends
23. The system must allow filtering by property, room type, check-in date range, and tenant status
24. The system must provide search functionality by tenant name, phone number, or ID number
25. The system must support export of tenant data to CSV/Excel formats

### 4.6 Data Management
26. The system must permanently store all tenant check-in and check-out records
27. The system must maintain data integrity with automatic backups
28. The system must provide data access controls ensuring landlords only see their own property data
29. The system must support data import from existing landlord record systems
30. The system must comply with data privacy regulations while maintaining business records

---

## 5. Non-Goals (Out of Scope)

- **Payment processing**: Financial transactions handled separately; this is purely occupancy management
- **Property management**: No utilities, maintenance, or property condition tracking
- **Tenant screening**: No background checks or tenant verification beyond ID capture
- **Communication platform**: No in-app messaging between landlords and tenants
- **Booking/reservation system**: No advance booking functionality; immediate check-in only
- **Multi-property ownership**: Single landlord account per property set only
- **Government reporting**: No automatic submission to authorities; manual export only

---

## 6. Design Considerations

### 6.1 User Interface
- **Mobile-optimized**: Primary interface for Android app with tablet support for dashboard views
- **Quick actions**: One-tap check-in/check-out buttons prominently displayed
- **Visual indicators**: Color-coded occupancy status (green=available, yellow=near capacity, red=over capacity)
- **Offline capability**: Basic check-in/check-out functionality when internet is unavailable

### 6.2 Dashboard Design
- **Responsive layout**: Adapts to different screen sizes while maintaining functionality
- **Widget-based**: Customizable dashboard components based on landlord preferences
- **Real-time updates**: Live occupancy status without manual refresh
- **Quick filters**: Easy access to common filter combinations (today's check-ins, overdue check-outs)

---

## 7. Technical Considerations

### 7.1 Database Design
- **Tenant records table**: Stores all check-in/check-out data with foreign keys to rooms and landlords
- **Occupancy tracking**: Real-time view of current room status and capacity utilization
- **Audit trail**: Complete history of all system interactions for compliance

### 7.2 API Endpoints
- **Check-in**: `POST /api/checkin` - Create new tenant record
- **Check-out**: `PUT /api/checkout/{tenantId}` - Update tenant record with check-out data
- **Dashboard data**: `GET /api/landlord/dashboard` - Retrieve current occupancy and tenant lists
- **Tenant search**: `GET /api/tenants/search?query={term}` - Find specific tenant records

### 7.3 Integration Requirements
- **Room Listing module**: Access to room data, capacity limits, and property information
- **User Authentication**: Verify landlord identity and property ownership
- **Notification system**: SMS/push notifications for check-in confirmations
- **Room Search module**: Update room availability for search results

---

## 8. Success Metrics

### 8.1 Efficiency Metrics
- **Check-in time**: Average check-in process completion under 5 minutes
- **Data accuracy**: <2% error rate in tenant information capture
- **System adoption**: >80% of landlords use digital check-in vs. manual methods

### 8.2 Operational Metrics
- **Occupancy tracking accuracy**: Real-time room status matches physical reality >95% of time
- **Check-out completion**: >90% of tenants properly checked out through the system
- **Dashboard usage**: >60% of landlords access dashboard at least daily during exam periods

### 8.3 User Satisfaction
- **Landlord satisfaction**: >4.0 rating for check-in/check-out feature usability
- **Feature completion**: >85% of check-ins completed without assistance
- **Error resolution**: <24 hours to resolve data discrepancies

---

## 9. Open Questions

1. **Offline synchronization**: How should the system handle check-ins performed offline when internet connectivity is restored?
2. **Tenant privacy**: What level of tenant data should be visible to other tenants in shared rooms?
3. **Emergency procedures**: Should there be special check-in procedures for late-night arrivals or emergency situations?
4. **Integration timeline**: When will the Room Listing and User Authentication modules be ready for integration?
5. **Capacity override policies**: Should there be system-level controls preventing capacity overrides during peak periods?
6. **Photo documentation**: Should the system support photos of tenant IDs or room conditions during check-in/check-out?
7. **Multi-language support**: Does the landlord interface need Vietnamese language support?

---

**Document Version:** 1.0  
**Created:** 2024-12-19  
**Target Audience:** Junior Developer  
**Feature Priority:** High (Core MVP Feature)  
**Dependencies:** Room Listing Module, User Authentication System 