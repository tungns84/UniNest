# UniNest System Patterns

## Development Approach
**Strategy:** Phased delivery based on business value vs development effort matrix  
**MVP Target:** 6-month timeline with core booking functionality  
**Team Structure:** 4-6 developers (2 backend, 2 frontend, 1 mobile, 1 DevOps)

## Architecture Patterns

### Data Management
- **Pattern:** Repository pattern with TypeORM
- **Geographic Data:** PostGIS for spatial queries with GIST indexing
- **Caching Strategy:** Redis for session management and search results
- **File Storage:** MinIO S3-compatible object storage

### API Design
- **Pattern:** RESTful APIs with real-time capabilities
- **Authentication:** JWT-based with role-based access control (RBAC)
- **Real-time Features:** Socket.IO for messaging and notifications
- **Rate Limiting:** API gateway level protection

### Frontend Patterns
- **Web:** React with Redux Toolkit for state management
- **Mobile:** Kotlin with Jetpack Compose and local state
- **Shared:** TypeScript interfaces for type safety
- **UI Consistency:** Design system with shared components

## Database Design Patterns

### Core Entities
```
Users ──────┬─── UserProfiles
            └─── UserSessions

Universities ──┬─── ExamSessions ──┬─── ExamVenues
               └─── UniversityOfficials    │
                                          └─── VenueAssignments

Rooms ──────┬─── RoomPhotos
            ├─── RoomAmenities  
            ├─── RoomAvailability
            └─── RoomRankings

Bookings ───┬─── PaymentTracking
            ├─── CheckInRecords
            └─── BookingStatusHistory
```

### Geographic Optimization
- **Spatial Indexing:** GIST indexes on room coordinates
- **Query Optimization:** Adaptive radius algorithm for search
- **Performance:** Geographic bounds checking before distance calculations

## Security Patterns
- **Authentication:** JWT tokens with refresh mechanism
- **Authorization:** Role-based permissions (Student, Landlord, University Official, Admin)
- **Data Protection:** Encrypted sensitive fields, GDPR compliance
- **API Security:** Rate limiting, input validation, SQL injection prevention

## Deployment Patterns
- **Containerization:** Docker multi-stage builds
- **Orchestration:** Kubernetes for production scaling
- **CI/CD:** GitHub Actions for automated testing and deployment
- **Monitoring:** Prometheus metrics with Grafana dashboards

*Derived from Technical Architecture Overview*
