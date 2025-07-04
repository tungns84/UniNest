# Product Requirements Document (PRD)
## Room Search by Exam Location Feature

---

## 1. Introduction/Overview

The Room Search by Exam Location feature is the core functionality of the room rental application that connects exam candidates with nearby accommodation during university entrance exam periods. This feature solves multiple critical problems: difficulty finding accommodation near exam venues, time pressure during peak exam season, and lack of location awareness when searching for suitable rooms.

**Goal:** Enable exam candidates to efficiently find and evaluate accommodation options within optimal distance from their assigned exam locations, with intelligent search capabilities that adapt to area density and user preferences.

---

## 2. Goals

1. **Reduce search time**: Allow users to find suitable rooms within 60 seconds of starting their search
2. **Improve location relevance**: 90% of search results should be within practical commuting distance to exam venues
3. **Enhance user satisfaction**: Achieve 4.5+ star rating for search functionality
4. **Increase booking conversion**: 25% of users who perform searches should proceed to contact landlords
5. **Support peak demand**: Handle concurrent searches during exam registration periods without performance degradation

---

## 3. User Stories

**As an exam candidate**, I want to select my university and exam session so that I can see relevant exam locations for my test.

**As an exam candidate**, I want to choose my specific exam location so that the system shows me rooms within commuting distance.

**As an exam candidate**, I want to filter search results by distance, price, amenities, room type, and availability dates so that I can find accommodation that meets my specific needs and budget.

**As an exam candidate**, I want to see search results in a list format with an option to view on a map so that I can quickly scan options and understand their geographic distribution.

**As an exam candidate**, I want the system to automatically expand the search radius if no rooms are found so that I always have accommodation options.

**As a landlord**, I want my rooms to appear in relevant searches based on proximity to exam locations so that I can attract exam candidates.

---

## 4. Functional Requirements

### 4.1 Search Flow
1. The system must provide a hierarchical search interface: University → Exam Session → Exam Location → Room Results
2. The system must display available universities in a searchable dropdown list
3. The system must show exam sessions relevant to the selected university with dates clearly displayed
4. The system must present exam locations for the selected session with addresses and venue names
5. The system must initiate room search immediately upon exam location selection

### 4.2 Smart Search Radius
6. The system must implement adaptive search radius based on area density:
   - Urban areas: Start with 1-2km radius
   - Suburban areas: Start with 2-3km radius  
   - Rural areas: Start with 5-10km radius
7. The system must allow manual radius adjustment by users (1km, 2km, 5km, 10km, 15km options)
8. The system must automatically expand search radius by 50% increments if fewer than 5 results are found

### 4.3 Filtering Capabilities
9. The system must provide distance-based filtering with visual indicators (walking time, driving time)
10. The system must enable price range filtering with min/max price inputs
11. The system must offer amenity-based filtering (WiFi, AC, private bathroom, parking, kitchen access)
12. The system must include room type filtering (single room, shared room, dormitory, apartment)
13. The system must filter by availability dates specific to exam periods
14. The system must show landlord verification status as a filterable option

### 4.4 Results Display
15. The system must display results in list view by default, sorted by relevance score
16. The system must provide a map toggle button to switch between list and map views
17. The system must show essential information in list view: room image, price, distance, key amenities, rating
18. The system must display walking/driving time estimates to exam location
19. The system must implement infinite scroll or pagination for large result sets

### 4.5 Search Performance
20. The system must return search results within 3 seconds under normal load
21. The system must cache frequently searched exam locations for faster subsequent searches
22. The system must handle minimum 100 concurrent search requests during peak periods

---

## 5. Non-Goals (Out of Scope)

- **Booking/Reservation functionality**: Users contact landlords directly; no in-app booking system
- **Payment processing**: No payment gateway integration in this feature
- **Review/Rating system**: Ratings display only; review submission is separate feature
- **Real-time availability updates**: Room availability updated manually by landlords
- **Advanced AI recommendations**: Basic relevance scoring only; no machine learning recommendations
- **Multi-city search**: Search limited to single exam location at a time
- **Saved searches**: No search history or saved search functionality

---

## 6. Design Considerations

### 6.1 User Interface
- **Mobile-first design**: Primary focus on Android app interface with responsive web support
- **Progressive disclosure**: Show basic filters first, advanced filters in expandable section
- **Visual hierarchy**: Clear distinction between search inputs, filters, and results
- **Loading states**: Skeleton screens during search, progress indicators for map loading

### 6.2 Map Integration
- **Google Maps SDK**: For accurate location data and familiar user experience
- **Cluster markers**: Group nearby rooms when zoomed out to avoid marker overlap
- **Custom markers**: Different icons for different room types and price ranges
- **Direction integration**: Tap marker to get directions to exam location

---

## 7. Technical Considerations

### 7.1 Database Requirements
- **Geospatial queries**: Implement PostGIS for efficient radius-based searches
- **Indexing strategy**: Geographic indexes on room coordinates and exam location coordinates
- **Query optimization**: Pre-calculate distances for frequently searched locations

### 7.2 API Design
- **Search endpoint**: `GET /api/rooms/search?examLocationId=123&radius=2000&filters={}`
- **Pagination**: Implement cursor-based pagination for consistent results
- **Caching layer**: Redis cache for exam location data and popular search results

### 7.3 Integration Points
- **University Management module**: Fetch exam locations and schedules
- **Room Listing module**: Access room data, availability, and landlord information
- **Auto-ranking module**: Integrate room quality scores into search results

---

## 8. Success Metrics

### 8.1 User Engagement
- **Search completion rate**: >85% of users who start a search view results
- **Filter usage**: >40% of users apply at least one filter
- **Map view adoption**: >30% of users switch to map view during search

### 8.2 Performance Metrics
- **Search response time**: <3 seconds for 95% of searches
- **Search accuracy**: >90% of results within specified radius
- **Error rate**: <1% of searches fail or timeout

### 8.3 Business Impact
- **Contact generation**: 25% of search sessions lead to landlord contact
- **User retention**: Users who successfully find rooms return for future exam periods
- **Peak load handling**: System maintains performance during exam registration weeks

---

## 9. Open Questions

1. **Offline capability**: Should the app cache recent search results for offline viewing?
2. **Search analytics**: What level of search behavior tracking is needed for optimization?
3. **Multi-language support**: Will search interface need Vietnamese and English versions?
4. **Accessibility**: What specific accessibility features are required for the search interface?
5. **Search suggestions**: Should we implement auto-complete for university/exam location names?
6. **Results ranking**: How should we weight distance vs. price vs. amenities in default sorting?
7. **Integration timeline**: When will University Management and Room Listing modules be available for integration?

---

**Document Version:** 1.0  
**Created:** 2024-12-19  
**Target Audience:** Junior Developer  
**Feature Priority:** High (MVP Core Feature) 