# Technical Architecture Overview
## Room Rental Platform - System Design

---

## 🏗️ **System Architecture Overview**

This document outlines the complete technical architecture for the room rental platform, designed to support all 12 features with scalability, security, and maintainability in mind.

**Architecture Pattern**: Microservices-like modular monolith  
**Deployment Model**: Cloud-native with containerization  
**Database Strategy**: PostgreSQL with PostGIS for geospatial data  
**API Design**: RESTful APIs with real-time capabilities

---

## 🌐 **High-Level System Architecture**

```
┌─────────────────────────────────────────────────────────────────┐
│                        CLIENT LAYER                             │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │   Android App   │  │   Web App       │  │  Admin Portal   │  │
│  │   (Kotlin)      │  │   (React)       │  │   (React)       │  │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                                │
                      ┌─────────▼─────────┐
                      │   Load Balancer   │
                      │    (Nginx)        │
                      └─────────┬─────────┘
                                │
┌─────────────────────────────────────────────────────────────────┐
│                      API GATEWAY LAYER                         │
│  ┌─────────────────────────────────────────────────────────────┐ │
│  │              NestJS API Gateway                            │ │
│  │  • Authentication & Authorization                          │ │
│  │  • Rate Limiting & Request Validation                     │ │
│  │  • API Routing & Load Balancing                          │ │
│  └─────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                                │
┌─────────────────────────────────────────────────────────────────┐
│                    APPLICATION LAYER                           │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │   Auth      │ │   Room      │ │  Booking    │ │ Messaging   │ │
│ │  Service    │ │  Service    │ │  Service    │ │  Service    │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘ │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │ University  │ │  Payment    │ │ Analytics   │ │Notification │ │
│ │  Service    │ │  Service    │ │  Service    │ │  Service    │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                                │
┌─────────────────────────────────────────────────────────────────┐
│                      DATA LAYER                                │
│ ┌─────────────────┐ ┌─────────────────┐ ┌─────────────────────┐ │
│ │   PostgreSQL    │ │     Redis       │ │   File Storage      │ │
│ │   + PostGIS     │ │    (Cache)      │ │     (MinIO)         │ │
│ │   (Primary DB)  │ │                 │ │                     │ │
│ └─────────────────┘ └─────────────────┘ └─────────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                                │
┌─────────────────────────────────────────────────────────────────┐
│                   EXTERNAL SERVICES                            │
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ │
│ │Google Maps  │ │   Email     │ │    SMS      │ │Push Notif.  │ │
│ │    API      │ │  Service    │ │  Service    │ │  (Firebase) │ │
│ └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

---

## 💻 **Technology Stack**

### **Backend Technologies**
```yaml
Framework: NestJS (Node.js + TypeScript)
Database: PostgreSQL 15+ with PostGIS extension
Cache: Redis 7.0+
ORM: TypeORM with repository pattern
Authentication: JWT + bcrypt
File Storage: MinIO (S3-compatible)
Message Queue: Redis + Bull Queue
WebSockets: Socket.IO for real-time features
```

### **Frontend Technologies**
```yaml
Web Application: React 18+ with TypeScript
Mobile App: Kotlin with Jetpack Compose
State Management: Redux Toolkit (Web) / Compose State (Mobile)
UI Framework: TailwindCSS (Web) / Material Design 3 (Mobile)
Build Tools: Vite (Web) / Gradle (Mobile)
Maps: Google Maps SDK
```

### **Infrastructure & DevOps**
```yaml
Containerization: Docker + Docker Compose
Orchestration: Kubernetes (production)
CI/CD: GitHub Actions
Monitoring: Prometheus + Grafana
Logging: ELK Stack (Elasticsearch, Logstash, Kibana)
Load Balancer: Nginx
Cloud Platform: Google Cloud Platform (preferred)
```

---

## 🗄️ **Database Design**

### **Core Entity Relationships**
```sql
-- Core User Management
Users ──────┬─── UserProfiles
            └─── UserSessions

-- University System
Universities ──┬─── ExamSessions ──┬─── ExamVenues
               └─── UniversityOfficials    │
                                          └─── VenueAssignments

-- Room System
Rooms ──────┬─── RoomPhotos
            ├─── RoomAmenities
            ├─── RoomAvailability
            └─── RoomRankings

-- Booking System
Bookings ───┬─── PaymentTracking
            ├─── CheckInRecords
            └─── BookingStatusHistory

-- Communication System
Conversations ──┬─── Messages ──┬─── MessagePhotos
                └─── Participants │
                                  └─── MessageStatus

-- Review System
Reviews ─────┬─── ReviewResponses
             └─── ReviewConversations

-- Analytics System
AnalyticsEvents ──┬─── DailyMetrics
                  └─── UserInteractions
```

### **Key Database Tables**

#### **Users & Authentication**
```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    phone VARCHAR(20) UNIQUE NOT NULL,
    role user_role NOT NULL,
    status user_status DEFAULT 'active',
    email_verified BOOLEAN DEFAULT false,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE user_profiles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    full_name VARCHAR(255) NOT NULL,
    profile_picture_url TEXT,
    university_name VARCHAR(255), -- for students/officials
    business_name VARCHAR(255),   -- for landlords
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **Geographic & Room Data**
```sql
CREATE TABLE rooms (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    landlord_id UUID REFERENCES users(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    address TEXT NOT NULL,
    coordinates GEOMETRY(POINT, 4326), -- PostGIS for spatial queries
    price DECIMAL(10,2) NOT NULL,
    max_occupants INTEGER DEFAULT 1,
    status room_status DEFAULT 'draft',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Spatial index for efficient geographic queries
CREATE INDEX idx_rooms_coordinates ON rooms USING GIST (coordinates);

CREATE TABLE room_rankings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    room_id UUID REFERENCES rooms(id) ON DELETE CASCADE,
    overall_score DECIMAL(3,2),
    distance_score DECIMAL(3,2),
    amenity_score DECIMAL(3,2),
    review_score DECIMAL(3,2),
    calculated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### **Booking & Payment System**
```sql
CREATE TABLE bookings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    room_id UUID REFERENCES rooms(id),
    tenant_id UUID REFERENCES users(id),
    check_in_date DATE NOT NULL,
    check_out_date DATE NOT NULL,
    total_amount DECIMAL(10,2) NOT NULL,
    status booking_status DEFAULT 'pending',
    booking_reference VARCHAR(20) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE payment_tracking (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    booking_id UUID REFERENCES bookings(id) ON DELETE CASCADE,
    amount DECIMAL(10,2) NOT NULL,
    payment_method VARCHAR(50) DEFAULT 'cash',
    status payment_status DEFAULT 'pending',
    due_date TIMESTAMP,
    paid_at TIMESTAMP,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 🔌 **API Architecture**

### **RESTful API Design**

#### **Core API Endpoints Structure**
```typescript
// Authentication & User Management
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout
GET    /api/auth/me
PUT    /api/auth/profile

// University & Exam Management
GET    /api/universities
POST   /api/universities
GET    /api/universities/{id}/exam-sessions
POST   /api/exam-sessions
GET    /api/exam-venues

// Room Search & Listing
GET    /api/rooms/search?examLocationId={id}&radius={km}
POST   /api/rooms
PUT    /api/rooms/{id}
GET    /api/rooms/{id}/ranking-score
POST   /api/rooms/{id}/photos

// Booking System
POST   /api/bookings
GET    /api/bookings/{id}
PUT    /api/bookings/{id}/status
GET    /api/user/bookings

// Communication System
GET    /api/conversations
POST   /api/conversations/{id}/messages
GET    /api/conversations/{id}/messages
POST   /api/messages/upload-photo

// Analytics & Reporting
GET    /api/analytics/dashboard
GET    /api/analytics/landlord/{id}
GET    /api/analytics/export/csv
```

#### **Real-time WebSocket Events**
```typescript
// Message Events
'message:sent'         // New message in conversation
'message:delivered'    // Message delivery confirmation
'message:read'         // Message read receipt

// Booking Events
'booking:confirmed'    // Booking status change
'booking:reminder'     // Check-in reminder

// Notification Events
'notification:new'     // New notification
'notification:read'    // Notification marked as read

// System Events
'user:online'         // User came online
'user:offline'        // User went offline
```

### **API Security & Performance**

#### **Authentication Flow**
```typescript
// JWT Token Structure
interface JWTPayload {
  sub: string;        // User ID
  email: string;      // User email
  role: UserRole;     // User role for authorization
  iat: number;        // Issued at
  exp: number;        // Expiration (24 hours)
}

// Authorization Middleware
@UseGuards(JwtAuthGuard, RoleGuard)
@Roles(UserRole.LANDLORD, UserRole.ADMIN)
async createRoom(@Req() req, @Body() roomData) {
  // Only landlords and admins can create rooms
}
```

#### **Rate Limiting & Caching**
```typescript
// Rate Limiting Configuration
@Throttle(100, 60)  // 100 requests per minute
export class RoomController {
  
  @Get('search')
  @CacheKey('room-search')
  @CacheTTL(300)  // Cache for 5 minutes
  async searchRooms(@Query() searchParams) {
    // Cached search results
  }
}
```

---

## 🔒 **Security Architecture**

### **Authentication & Authorization**
```yaml
Authentication Strategy: JWT tokens with refresh token rotation
Session Management: Redis-based session storage
Password Security: bcrypt with salt rounds = 12
Role-Based Access Control (RBAC): Four roles with specific permissions
API Security: Rate limiting, input validation, CORS configuration
```

### **Data Protection**
```yaml
Encryption in Transit: TLS 1.3 for all API communications
Encryption at Rest: Database encryption for sensitive data
Input Validation: Server-side validation with class-validator
SQL Injection Prevention: TypeORM with parameterized queries
XSS Prevention: Content Security Policy headers
CSRF Protection: Anti-CSRF tokens for state-changing operations
```

### **File Upload Security**
```typescript
// File Upload Configuration
const multerConfig = {
  limits: {
    fileSize: 5 * 1024 * 1024, // 5MB limit
    files: 10 // Max 10 files per request
  },
  fileFilter: (req, file, cb) => {
    const allowedTypes = ['image/jpeg', 'image/png'];
    cb(null, allowedTypes.includes(file.mimetype));
  }
};
```

---

## 📈 **Scalability & Performance**

### **Horizontal Scaling Strategy**
```yaml
Application Layer: Stateless services for easy horizontal scaling
Database Layer: Read replicas for analytics and search queries
Cache Layer: Redis cluster for distributed caching
File Storage: MinIO cluster for high-availability file storage
Load Balancing: Nginx with round-robin for even distribution
```

### **Performance Optimizations**
```typescript
// Database Query Optimization
class RoomSearchService {
  async searchNearbyRooms(lat: number, lng: number, radius: number) {
    // Optimized spatial query with PostGIS
    return this.roomRepository
      .createQueryBuilder('room')
      .where(
        'ST_DWithin(room.coordinates, ST_SetSRID(ST_MakePoint(:lng, :lat), 4326), :radius)',
        { lat, lng, radius: radius * 1000 } // Convert km to meters
      )
      .cache('nearby-rooms', 300000) // Cache for 5 minutes
      .getMany();
  }
}

// Redis Caching Strategy
@Injectable()
export class CacheService {
  // Search results cache
  async cacheSearchResults(key: string, results: any[], ttl = 300) {
    await this.redis.setex(`search:${key}`, ttl, JSON.stringify(results));
  }
  
  // User session cache
  async cacheUserSession(userId: string, sessionData: any) {
    await this.redis.setex(`session:${userId}`, 3600, JSON.stringify(sessionData));
  }
}
```

### **Monitoring & Observability**
```yaml
Application Metrics: Custom metrics for business KPIs
System Metrics: CPU, memory, disk, network monitoring
Database Metrics: Query performance, connection pool stats
Error Tracking: Sentry for error monitoring and alerting
Logging: Structured JSON logs with correlation IDs
Health Checks: Kubernetes readiness and liveness probes
```

---

## 🚀 **Deployment Architecture**

### **Container Configuration**
```dockerfile
# Multi-stage build for production
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine AS runtime
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY dist ./dist
EXPOSE 3000
CMD ["node", "dist/main.js"]
```

### **Kubernetes Deployment**
```yaml
# Example deployment configuration
apiVersion: apps/v1
kind: Deployment
metadata:
  name: room-rental-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: room-rental-api
  template:
    metadata:
      labels:
        app: room-rental-api
    spec:
      containers:
      - name: api
        image: room-rental-api:latest
        ports:
        - containerPort: 3000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: url
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
```

---

## 🔄 **Integration Patterns**

### **External Service Integration**
```typescript
// Google Maps Integration
@Injectable()
export class MapsService {
  async geocodeAddress(address: string): Promise<Coordinates> {
    const response = await this.httpService.get(
      `https://maps.googleapis.com/maps/api/geocode/json`,
      {
        params: {
          address,
          key: this.configService.get('GOOGLE_MAPS_API_KEY')
        }
      }
    ).toPromise();
    
    return this.parseCoordinates(response.data);
  }
  
  async calculateDistance(from: Coordinates, to: Coordinates): Promise<number> {
    // Use PostGIS for accurate distance calculation
    return this.databaseService.query(
      'SELECT ST_Distance(ST_SetSRID(ST_MakePoint($1, $2), 4326), ST_SetSRID(ST_MakePoint($3, $4), 4326))',
      [from.lng, from.lat, to.lng, to.lat]
    );
  }
}
```

### **Event-Driven Architecture**
```typescript
// Event publishing for inter-service communication
@Injectable()
export class EventService {
  async publishBookingConfirmed(booking: Booking) {
    await this.eventBus.publish(new BookingConfirmedEvent(booking));
  }
}

// Event handlers for business logic
@EventsHandler(BookingConfirmedEvent)
export class BookingConfirmedHandler {
  async handle(event: BookingConfirmedEvent) {
    // Send confirmation notifications
    await this.notificationService.sendBookingConfirmation(event.booking);
    
    // Update room availability
    await this.roomService.updateAvailability(event.booking.roomId);
    
    // Create payment tracking record
    await this.paymentService.createPaymentRecord(event.booking);
  }
}
```

---

## 📊 **Data Flow Diagrams**

### **Room Search Flow**
```
User Input → Search API → Database Query → Ranking Algorithm → Map Integration → Results Display
    ↓
Location Filter → PostGIS Spatial Query → Cache Check → Results Formatting → UI Rendering
```

### **Booking Flow**
```
Booking Request → Availability Check → Booking Creation → Payment Tracking → Confirmation
       ↓              ↓                    ↓                ↓               ↓
   WebSocket      Room Locking        Event Publishing   Notification    Email/SMS
   Notification    (Redis)            (Background Job)    Service         Service
```

---

## 🎯 **Performance Targets**

### **Response Time Requirements**
```yaml
API Response Times:
  - Authentication: < 200ms
  - Room Search: < 500ms
  - Booking Creation: < 1000ms
  - Message Delivery: < 100ms
  - File Upload: < 5000ms

Database Query Performance:
  - Spatial Queries: < 100ms
  - User Lookups: < 50ms
  - Analytics Queries: < 2000ms

System Availability:
  - Uptime Target: 99.9%
  - Recovery Time: < 5 minutes
  - Data Backup: Daily with 30-day retention
```

---

**Architecture Last Updated**: 2024-12-19  
**Next Review**: Quarterly during development  
**Document Maintainer**: Technical Lead 