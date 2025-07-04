# ��� CREATIVE PHASE: API Architecture Design

## ��������� ENTERING CREATIVE PHASE: API ARCHITECTURE ���������

**Date:** $(date)  
**Phase:** 2 of 8 - API Architecture Design  
**Type:** Architecture Design  
**Focus:** RESTful endpoints, WebSocket integration, and API gateway design for multi-platform support

## PROBLEM STATEMENT

Design a comprehensive API architecture for UniNest that supports:
1. **Multi-platform clients** (Web React app, Android Kotlin app, Admin portal)
2. **Real-time features** (messaging, notifications, booking updates)
3. **Geospatial search** with complex filtering and ranking
4. **Authentication and authorization** with role-based access control
5. **File upload/download** for photos and documents
6. **Rate limiting and security** for production deployment
7. **API versioning and documentation** for maintainability
8. **Performance optimization** for high-volume search queries

**Key Challenges:**
- Balancing RESTful design with real-time WebSocket requirements
- Implementing efficient geospatial search APIs with PostGIS
- Managing authentication across multiple client types
- Handling file uploads with progress tracking
- Implementing proper error handling and validation
- Designing scalable API gateway with load balancing

## OPTIONS ANALYSIS

### Option 1: Pure REST API with WebSocket Gateway
**Description**: RESTful API for all CRUD operations, separate WebSocket gateway for real-time features
**Pros**:
- Clear separation of concerns between REST and real-time
- Easy to cache REST endpoints with Redis
- Simple to implement rate limiting and security
- Well-established patterns and documentation
- Easy to version and maintain
**Cons**:
- Two separate connection types for clients
- Potential data inconsistency between REST and WebSocket
- More complex client implementation
- Additional infrastructure for WebSocket gateway
**Complexity**: Medium
**Implementation Time**: 6-8 weeks

### Option 2: GraphQL API with WebSocket Subscriptions
**Description**: GraphQL for data queries and mutations, WebSocket for real-time subscriptions
**Pros**:
- Flexible querying with client-defined data requirements
- Built-in real-time subscriptions support
- Single endpoint for all data operations
- Strong typing and introspection
- Efficient data fetching with minimal over-fetching
**Cons**:
- Steeper learning curve for team
- More complex caching strategies
- Potential N+1 query problems
- Less mature ecosystem for geospatial queries
- More complex error handling
**Complexity**: High
**Implementation Time**: 8-10 weeks

### Option 3: Hybrid REST + WebSocket with API Gateway
**Description**: REST API for CRUD operations, WebSocket for real-time, unified API gateway
**Pros**:
- Best of both worlds - REST simplicity and WebSocket real-time
- Single API gateway for authentication and routing
- Easy to implement caching and rate limiting
- Clear API documentation with OpenAPI/Swagger
- Flexible deployment and scaling options
**Cons**:
- More complex gateway implementation
- Potential for API gateway bottlenecks
- Requires careful session management
- More infrastructure components to maintain
**Complexity**: Medium-High
**Implementation Time**: 7-9 weeks

## DECISION

**Selected Approach**: **Option 3 - Hybrid REST + WebSocket with API Gateway**

**Rationale**:
1. **MVP Timeline**: Balances implementation speed with functionality
2. **Team Expertise**: REST is well-understood, WebSocket adds real-time capability
3. **Client Requirements**: Different client types benefit from different API patterns
4. **Scalability**: API gateway provides future scaling options
5. **Documentation**: OpenAPI/Swagger support for better developer experience
6. **Security**: Centralized authentication and rate limiting

## IMPLEMENTATION PLAN

### Phase 1: Core REST API Design (Week 1-2)
```
Authentication & Users
├── POST /api/v1/auth/register
├── POST /api/v1/auth/login
├── POST /api/v1/auth/refresh
├── POST /api/v1/auth/logout
├── GET /api/v1/users/profile
├── PUT /api/v1/users/profile
└── POST /api/v1/auth/forgot-password

Universities & Exams
├── GET /api/v1/universities
├── GET /api/v1/universities/{id}
├── GET /api/v1/exam-sessions
├── GET /api/v1/exam-sessions/{id}/venues
└── GET /api/v1/venues/{id}
```

### Phase 2: Room Search & Booking APIs (Week 3-4)
```
Room Search & Management
├── GET /api/v1/rooms/search
├── GET /api/v1/rooms/{id}
├── POST /api/v1/rooms
├── PUT /api/v1/rooms/{id}
├── DELETE /api/v1/rooms/{id}
├── POST /api/v1/rooms/{id}/photos
└── GET /api/v1/rooms/{id}/availability

Booking System
├── POST /api/v1/bookings
├── GET /api/v1/bookings
├── GET /api/v1/bookings/{id}
├── PUT /api/v1/bookings/{id}/status
├── POST /api/v1/bookings/{id}/check-in
└── POST /api/v1/bookings/{id}/check-out
```

### Phase 3: Communication & Reviews APIs (Week 5-6)
```
Communication System
├── GET /api/v1/conversations
├── POST /api/v1/conversations
├── GET /api/v1/conversations/{id}/messages
├── POST /api/v1/conversations/{id}/messages
├── PUT /api/v1/messages/{id}/read
└── POST /api/v1/messages/{id}/photos

Review System
├── POST /api/v1/reviews
├── GET /api/v1/rooms/{id}/reviews
├── POST /api/v1/reviews/{id}/responses
└── GET /api/v1/reviews/{id}/conversation
```

### Phase 4: Analytics & Notifications APIs (Week 7-8)
```
Analytics & Metrics
├── GET /api/v1/analytics/dashboard
├── GET /api/v1/analytics/bookings
├── GET /api/v1/analytics/revenue
└── GET /api/v1/analytics/export

Notifications
├── GET /api/v1/notifications
├── PUT /api/v1/notifications/{id}/read
├── GET /api/v1/notification-preferences
└── PUT /api/v1/notification-preferences
```

## VISUALIZATION

### API Gateway Architecture
```
┌─────────────────────────────────────────────────────────────────┐
│                        API GATEWAY                              │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │   Authentication│  │   Rate Limiting │  │   Request       │  │
│  │   & Authorization│  │   & Security    │  │   Routing       │  │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                                │
                      ┌─────────▼─────────┐
                      │   Load Balancer   │
                      └─────────┬─────────┘
                                │
┌─────────────────────────────────────────────────────────────────┐
│                    APPLICATION SERVICES                         │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │   Auth      │ │   Room      │ │  Booking    │ │ Messaging   │ │
│ │  Service    │ │  Service    │ │  Service    │ │  Service    │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘ │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ University  │ │  Payment    │ │ Analytics   │ │Notification │ │
│ │  Service    │ │  Service    │ │  Service    │ │  Service    │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

### WebSocket Event Flow
```
Client Connection
├── Authentication (JWT token)
├── Room join (user-specific room)
└── Event subscription

Real-time Events
├── Message received
├── Booking status changed
├── Notification received
├── Room availability updated
└── Payment status changed

Event Broadcasting
├── User-specific events
├── Room-specific events
├── Global notifications
└── System announcements
```

## API DESIGN PATTERNS

### RESTful Endpoint Design
```typescript
// Standard CRUD operations
GET    /api/v1/resources          // List resources
POST   /api/v1/resources          // Create resource
GET    /api/v1/resources/{id}     // Get resource
PUT    /api/v1/resources/{id}     // Update resource
DELETE /api/v1/resources/{id}     // Delete resource

// Specialized operations
POST   /api/v1/resources/{id}/action  // Custom action
GET    /api/v1/resources/search       // Search with filters
POST   /api/v1/resources/bulk         // Bulk operations
```

### Geospatial Search API
```typescript
// Room search with geospatial parameters
GET /api/v1/rooms/search
Query Parameters:
- venue_id: string (exam venue ID)
- radius: number (search radius in meters)
- price_min: number
- price_max: number
- amenities: string[] (array of amenity types)
- check_in: string (ISO date)
- check_out: string (ISO date)
- sort_by: 'distance' | 'price' | 'rating'
- page: number
- limit: number

Response:
{
  "data": [
    {
      "id": "uuid",
      "title": "string",
      "price": number,
      "distance_meters": number,
      "coordinates": { "lat": number, "lng": number },
      "amenities": string[],
      "rating": number,
      "photos": string[]
    }
  ],
  "pagination": {
    "page": number,
    "limit": number,
    "total": number,
    "total_pages": number
  }
}
```

### WebSocket Event Structure
```typescript
// Event types
enum WebSocketEventType {
  MESSAGE_RECEIVED = 'message_received',
  BOOKING_UPDATED = 'booking_updated',
  NOTIFICATION = 'notification',
  ROOM_AVAILABILITY = 'room_availability',
  PAYMENT_STATUS = 'payment_status'
}

// Event payload structure
interface WebSocketEvent {
  type: WebSocketEventType;
  data: any;
  timestamp: string;
  user_id?: string;
  room_id?: string;
}

// Client subscription
{
  "action": "subscribe",
  "channels": ["user:123", "room:456", "notifications"]
}
```

## AUTHENTICATION & AUTHORIZATION

### JWT Token Structure
```typescript
interface JWTPayload {
  sub: string;           // User ID
  email: string;         // User email
  role: UserRole;        // User role
  permissions: string[]; // User permissions
  iat: number;          // Issued at
  exp: number;          // Expires at
}

enum UserRole {
  STUDENT = 'student',
  LANDLORD = 'landlord',
  UNIVERSITY_OFFICIAL = 'university_official',
  ADMIN = 'admin'
}
```

### Role-Based Access Control
```typescript
// API endpoint permissions
const API_PERMISSIONS = {
  'GET /api/v1/rooms/search': ['student', 'landlord', 'admin'],
  'POST /api/v1/rooms': ['landlord', 'admin'],
  'PUT /api/v1/bookings/{id}/status': ['landlord', 'admin'],
  'GET /api/v1/analytics/dashboard': ['admin'],
  'POST /api/v1/universities': ['university_official', 'admin']
};
```

## ERROR HANDLING & VALIDATION

### Standard Error Response
```typescript
interface APIError {
  error: {
    code: string;
    message: string;
    details?: any;
    timestamp: string;
    request_id: string;
  };
}

// Error codes
enum ErrorCode {
  VALIDATION_ERROR = 'VALIDATION_ERROR',
  AUTHENTICATION_ERROR = 'AUTHENTICATION_ERROR',
  AUTHORIZATION_ERROR = 'AUTHORIZATION_ERROR',
  NOT_FOUND = 'NOT_FOUND',
  CONFLICT = 'CONFLICT',
  RATE_LIMIT_EXCEEDED = 'RATE_LIMIT_EXCEEDED',
  INTERNAL_ERROR = 'INTERNAL_ERROR'
}
```

### Request Validation
```typescript
// Using class-validator decorators
export class CreateRoomDto {
  @IsString()
  @IsNotEmpty()
  title: string;

  @IsString()
  @IsNotEmpty()
  description: string;

  @IsNumber()
  @Min(0)
  price: number;

  @IsNumber()
  @Min(1)
  max_occupants: number;

  @IsObject()
  coordinates: {
    lat: number;
    lng: number;
  };
}
```

## PERFORMANCE OPTIMIZATION

### Caching Strategy
```typescript
// Redis caching layers
const CACHE_STRATEGY = {
  // User sessions (1 hour)
  'user:session:{id}': 3600,
  
  // Room search results (5 minutes)
  'room:search:{hash}': 300,
  
  // University data (1 day)
  'university:{id}': 86400,
  
  // Room details (30 minutes)
  'room:{id}': 1800,
  
  // Analytics data (1 hour)
  'analytics:dashboard': 3600
};
```

### Database Query Optimization
```typescript
// Efficient geospatial search
const searchRooms = async (params: SearchParams) => {
  const query = `
    SELECT r.*, 
           ST_Distance(r.coordinates, ev.coordinates) as distance_meters,
           r.overall_score
    FROM rooms r
    CROSS JOIN exam_venues ev
    WHERE ev.id = $1
      AND ST_DWithin(r.coordinates, ev.coordinates, $2)
      AND r.status = 'active'
      AND r.price BETWEEN $3 AND $4
    ORDER BY 
      CASE WHEN $5 = 'distance' THEN distance_meters END ASC,
      CASE WHEN $5 = 'price' THEN r.price END ASC,
      CASE WHEN $5 = 'rating' THEN r.overall_score END DESC
    LIMIT $6 OFFSET $7
  `;
  
  return await db.query(query, [
    params.venue_id,
    params.radius,
    params.price_min,
    params.price_max,
    params.sort_by,
    params.limit,
    params.offset
  ]);
};
```

## API VERSIONING & DOCUMENTATION

### Versioning Strategy
```typescript
// URL-based versioning
const API_VERSIONS = {
  v1: {
    status: 'stable',
    deprecated: false,
    sunset_date: null
  },
  v2: {
    status: 'beta',
    deprecated: false,
    sunset_date: null
  }
};

// Backward compatibility
app.use('/api/v1', v1Router);
app.use('/api/v2', v2Router);
app.use('/api', v1Router); // Default to v1
```

### OpenAPI Documentation
```yaml
openapi: 3.0.0
info:
  title: UniNest API
  version: 1.0.0
  description: University room rental platform API

servers:
  - url: https://api.uninest.com/v1
    description: Production server
  - url: https://staging-api.uninest.com/v1
    description: Staging server

paths:
  /rooms/search:
    get:
      summary: Search for rooms
      parameters:
        - name: venue_id
          in: query
          required: true
          schema:
            type: string
        - name: radius
          in: query
          schema:
            type: number
            default: 5000
      responses:
        '200':
          description: Successful search results
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RoomSearchResponse'
```

## SECURITY CONSIDERATIONS

### Rate Limiting
```typescript
// Rate limiting configuration
const RATE_LIMITS = {
  // General API limits
  'api': { window: '15m', max: 1000 },
  
  // Authentication endpoints
  'auth': { window: '15m', max: 10 },
  
  // Search endpoints
  'search': { window: '1m', max: 60 },
  
  // File upload endpoints
  'upload': { window: '1h', max: 10 }
};
```

### Security Headers
```typescript
// Security middleware
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      scriptSrc: ["'self'"],
      imgSrc: ["'self'", "data:", "https:"],
    },
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  }
}));
```

## ��� CREATIVE CHECKPOINT: API ARCHITECTURE DESIGN COMPLETE

**Decision Made**: Hybrid REST + WebSocket with API Gateway
**Key Features**: 
- 40+ RESTful endpoints across 8 service areas
- Real-time WebSocket events for live updates
- Comprehensive authentication and authorization
- Geospatial search API with PostGIS integration
- Performance optimization with caching and query optimization

**Next Creative Phase**: Frontend Architecture Design

## ��������� EXITING CREATIVE PHASE - DECISION MADE ���������

*This creative phase document captures the API architecture design decisions for UniNest*
