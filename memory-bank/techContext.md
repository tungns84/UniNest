# UniNest Technical Context

## Architecture Pattern
**Primary:** Microservices-like modular monolith  
**Deployment:** Cloud-native with containerization  
**Database Strategy:** PostgreSQL with PostGIS for geospatial data  

## Technology Stack

### Backend
- **Framework:** NestJS (Node.js + TypeScript)
- **Database:** PostgreSQL 15+ with PostGIS extension  
- **Cache:** Redis 7.0+
- **ORM:** TypeORM with repository pattern
- **Authentication:** JWT + bcrypt
- **File Storage:** MinIO (S3-compatible)
- **Message Queue:** Redis + Bull Queue
- **WebSockets:** Socket.IO for real-time features

### Frontend
- **Web Application:** React 18+ with TypeScript
- **Mobile App:** Kotlin with Jetpack Compose
- **State Management:** Redux Toolkit (Web) / Compose State (Mobile)
- **UI Framework:** TailwindCSS (Web) / Material Design 3 (Mobile)
- **Maps:** Google Maps SDK

### Infrastructure
- **Containerization:** Docker + Docker Compose
- **Cloud Platform:** Google Cloud Platform (preferred)
- **Load Balancer:** Nginx
- **Monitoring:** Prometheus + Grafana

## Core Technical Requirements
- Geospatial search with PostGIS
- Real-time messaging with Socket.IO
- Adaptive radius algorithm for room search
- Multi-platform deployment support

*Extracted from SRS Technical Architecture Overview*
