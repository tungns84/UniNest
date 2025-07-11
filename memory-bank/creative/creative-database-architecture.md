# íž¨ CREATIVE PHASE: Database Architecture Design

## íž¨íž¨íž¨ ENTERING CREATIVE PHASE: DATABASE ARCHITECTURE íž¨íž¨íž¨

**Date:** $(date)  
**Phase:** 1 of 8 - Database Architecture Design  
**Type:** Data Model Design  
**Focus:** Entity relationships, schema design, and PostGIS integration for geospatial room search

## PROBLEM STATEMENT

Design a comprehensive database architecture for UniNest that supports:
1. **Multi-role user management** (Students, Landlords, University Officials, Admins)
2. **Geospatial room search** with PostGIS for exam location-based queries
3. **Real-time booking system** with availability tracking
4. **Communication system** with messaging and notifications
5. **Review and rating system** with conversation threading
6. **Payment coordination** with status tracking
7. **Analytics and reporting** with performance metrics
8. **File storage integration** for photos and documents

**Key Challenges:**
- Complex geospatial queries with adaptive radius algorithms
- Real-time data consistency across multiple services
- Scalable schema design for high-volume room searches
- Efficient indexing strategy for spatial and non-spatial queries
- Data integrity across booking workflows and payment coordination

## OPTIONS ANALYSIS

### Option 1: Monolithic PostgreSQL Schema with PostGIS
**Description**: Single database with all entities in one schema, using PostGIS for spatial data
**Pros**:
- Simple deployment and maintenance
- ACID transactions across all entities
- Single connection pool and backup strategy
- Easy to implement complex joins and queries
- PostGIS spatial functions readily available
**Cons**:
- Potential performance bottlenecks with high load
- Schema changes affect entire system
- Limited horizontal scaling options
- Single point of failure for all data
**Complexity**: Medium
**Implementation Time**: 4-6 weeks

### Option 2: Microservices Database Pattern (Database per Service)
**Description**: Separate databases for each major service (Auth, Rooms, Bookings, Communication)
**Pros**:
- Independent scaling and deployment
- Service isolation and fault tolerance
- Technology flexibility per service
- Easier team ownership and development
- Better performance isolation
**Cons**:
- Complex distributed transactions
- Data consistency challenges
- Increased operational complexity
- More complex backup and recovery
- Network latency for cross-service queries
**Complexity**: High
**Implementation Time**: 8-12 weeks

### Option 3: Hybrid Approach with Read Replicas
**Description**: Monolithic schema with read replicas for search and analytics
**Pros**:
- Balances simplicity with performance
- Read-heavy operations (search) can scale independently
- Maintains ACID transactions for writes
- Easier to implement than full microservices
- Good for analytics and reporting
**Cons**:
- Eventual consistency for reads
- More complex replication setup
- Still single point of failure for writes
- Requires careful replica lag management
**Complexity**: Medium-High
**Implementation Time**: 6-8 weeks

## DECISION

**Selected Approach**: **Option 1 - Monolithic PostgreSQL Schema with PostGIS**

**Rationale**:
1. **MVP Timeline**: 6-month MVP target requires faster implementation
2. **Team Size**: 4-6 developers can manage single database more efficiently
3. **Geospatial Requirements**: PostGIS integration is simpler in single database
4. **Transaction Integrity**: Booking workflows require strong consistency
5. **Operational Simplicity**: Easier deployment, backup, and monitoring
6. **Future Migration**: Can evolve to microservices pattern later if needed

## IMPLEMENTATION PLAN

### Phase 1: Core Entity Design (Week 1-2)
```
Users & Authentication
âââ users (id, email, password_hash, phone, role, status, created_at)
âââ user_profiles (id, user_id, full_name, profile_picture_url, university_name, business_name)
âââ user_sessions (id, user_id, token, expires_at)
âââ user_verifications (id, user_id, type, token, expires_at)

Universities & Exams
âââ universities (id, name, location, coordinates, created_at)
âââ exam_sessions (id, university_id, name, start_date, end_date, description)
âââ exam_venues (id, exam_session_id, name, address, coordinates, capacity)
âââ venue_assignments (id, venue_id, exam_session_id, student_count)
```

### Phase 2: Room & Booking System (Week 3-4)
```
Rooms & Listings
âââ rooms (id, landlord_id, title, description, address, coordinates, price, max_occupants, status)
âââ room_photos (id, room_id, url, caption, order_index)
âââ room_amenities (id, room_id, amenity_type, description)
âââ room_availability (id, room_id, date, available_slots, booked_slots)
âââ room_rankings (id, room_id, overall_score, distance_score, amenity_score, seasonal_score)

Bookings & Payments
âââ bookings (id, room_id, student_id, landlord_id, check_in_date, check_out_date, status, total_amount)
âââ booking_status_history (id, booking_id, status, changed_by, changed_at, notes)
âââ payment_tracking (id, booking_id, amount, payment_type, status, receipt_url, notes)
âââ check_in_records (id, booking_id, check_in_time, check_out_time, notes)
```

### Phase 3: Communication & Reviews (Week 5-6)
```
Communication System
âââ conversations (id, type, created_at, updated_at)
âââ conversation_participants (id, conversation_id, user_id, role)
âââ messages (id, conversation_id, sender_id, content, message_type, created_at)
âââ message_photos (id, message_id, url, caption)
âââ message_status (id, message_id, recipient_id, status, read_at)

Review System
âââ reviews (id, booking_id, reviewer_id, reviewee_id, rating, content, created_at)
âââ review_responses (id, review_id, responder_id, content, created_at)
âââ review_conversations (id, review_id, message_id)
```

### Phase 4: Analytics & Notifications (Week 7-8)
```
Analytics & Metrics
âââ analytics_events (id, user_id, event_type, event_data, created_at)
âââ daily_metrics (id, date, metric_type, value, metadata)
âââ user_interactions (id, user_id, interaction_type, target_id, created_at)

Notifications
âââ notifications (id, user_id, type, title, content, read_at, created_at)
âââ notification_preferences (id, user_id, notification_type, enabled)
```

## VISUALIZATION

### Entity Relationship Diagram
```
Users âââââââŹâââ UserProfiles
            ââââ UserSessions
            ââââ UserVerifications

Universities âââŹâââ ExamSessions âââŹâââ ExamVenues
               ââââ UniversityOfficials    â
                                          ââââ VenueAssignments

Rooms âââââââŹâââ RoomPhotos
            ââââ RoomAmenities
            ââââ RoomAvailability
            ââââ RoomRankings

Bookings ââââŹâââ PaymentTracking
            ââââ CheckInRecords
            ââââ BookingStatusHistory

Conversations âââŹâââ Messages âââŹâââ MessagePhotos
                ââââ Participants â
                ââââ MessageStatus
                                  ââââ MessageStatus

Reviews ââââââŹâââ ReviewResponses
             ââââ ReviewConversations

AnalyticsEvents âââŹâââ DailyMetrics
                  ââââ UserInteractions
```

### PostGIS Spatial Design
```sql
-- Spatial indexes for performance
CREATE INDEX idx_rooms_coordinates ON rooms USING GIST (coordinates);
CREATE INDEX idx_exam_venues_coordinates ON exam_venues USING GIST (coordinates);
CREATE INDEX idx_universities_coordinates ON universities USING GIST (coordinates);

-- Geospatial queries for room search
SELECT r.*, 
       ST_Distance(r.coordinates, ev.coordinates) as distance_meters
FROM rooms r
CROSS JOIN exam_venues ev
WHERE ev.id = $exam_venue_id
  AND ST_DWithin(r.coordinates, ev.coordinates, $radius_meters)
  AND r.status = 'active'
ORDER BY distance_meters ASC;
```

## INDEXING STRATEGY

### Primary Indexes
- **users**: email (unique), phone (unique)
- **rooms**: landlord_id, status, coordinates (spatial)
- **bookings**: room_id, student_id, landlord_id, status
- **conversations**: type, updated_at
- **messages**: conversation_id, created_at

### Composite Indexes
- **rooms**: (status, coordinates) for active room searches
- **bookings**: (room_id, check_in_date, check_out_date) for availability
- **messages**: (conversation_id, created_at) for conversation history

### Spatial Indexes
- **rooms.coordinates**: GIST index for geospatial queries
- **exam_venues.coordinates**: GIST index for venue-based searches
- **universities.coordinates**: GIST index for university proximity

## DATA VALIDATION RULES

### User Management
- Email must be unique and valid format
- Phone must be unique and valid format
- Password must meet security requirements
- Role must be one of: student, landlord, university_official, admin

### Room Management
- Coordinates must be valid PostGIS geometry
- Price must be positive decimal
- Max_occupants must be positive integer
- Status must be one of: draft, active, inactive, suspended

### Booking Management
- Check_in_date must be before check_out_date
- Total_amount must be positive decimal
- Status must follow workflow: pending, confirmed, active, completed, cancelled

### Geospatial Validation
- All coordinates must be valid WGS84 (SRID 4326)
- Distance calculations must use PostGIS ST_Distance
- Spatial queries must use appropriate indexes

## MIGRATION STRATEGY

### Phase 1: Core Tables
1. Create users and authentication tables
2. Create universities and exam management tables
3. Create basic room tables without spatial data

### Phase 2: Spatial Integration
1. Add PostGIS extension to database
2. Add coordinates columns to spatial tables
3. Create spatial indexes
4. Migrate existing location data to coordinates

### Phase 3: Advanced Features
1. Add booking and payment tables
2. Add communication and review tables
3. Add analytics and notification tables

### Phase 4: Optimization
1. Analyze query performance
2. Add missing indexes
3. Optimize spatial queries
4. Implement connection pooling

## PERFORMANCE CONSIDERATIONS

### Query Optimization
- Use EXPLAIN ANALYZE for all spatial queries
- Implement query result caching with Redis
- Use pagination for large result sets
- Optimize JOIN operations with proper indexes

### Spatial Performance
- Use ST_DWithin for radius searches (faster than ST_Distance)
- Implement bounding box pre-filtering
- Use spatial clustering for high-density areas
- Consider spatial partitioning for very large datasets

### Scalability Planning
- Monitor table sizes and growth rates
- Plan for read replicas if search load increases
- Consider database sharding for future scaling
- Implement proper connection pooling

## íž¨ CREATIVE CHECKPOINT: DATABASE ARCHITECTURE DESIGN COMPLETE

**Decision Made**: Monolithic PostgreSQL schema with PostGIS
**Key Features**: 
- 15 core entities with clear relationships
- Comprehensive spatial indexing strategy
- Multi-phase migration approach
- Performance optimization guidelines

**Next Creative Phase**: API Architecture Design

## íž¨íž¨íž¨ EXITING CREATIVE PHASE - DECISION MADE íž¨íž¨íž¨

*This creative phase document captures the database architecture design decisions for UniNest*
