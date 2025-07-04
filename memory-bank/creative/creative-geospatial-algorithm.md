# íž¨ CREATIVE PHASE: Geospatial Search Algorithm Design

## íž¨íž¨íž¨ ENTERING CREATIVE PHASE: GEOSPATIAL ALGORITHM íž¨íž¨íž¨

**Date:** $(date)  
**Phase:** 5 of 8 - Geospatial Search Algorithm Design  
**Type:** Algorithm Design  
**Focus:** PostGIS query optimization, adaptive radius algorithms, and spatial indexing for room search

## PROBLEM STATEMENT

Design a comprehensive geospatial search algorithm for UniNest that supports:
1. **Exam venue-based room search** with distance-based ranking
2. **Adaptive radius algorithms** that expand search area when insufficient results
3. **Multi-criteria ranking** combining distance, price, amenities, and ratings
4. **Performance optimization** for high-volume spatial queries
5. **Seasonal ranking adjustments** based on exam schedules
6. **Real-time availability filtering** during search
7. **Caching strategies** for frequently searched areas
8. **Scalability** for multiple universities and venues

**Key Challenges:**
- Complex spatial queries with multiple filtering criteria
- Performance optimization for large datasets with spatial indexes
- Adaptive radius calculation based on result availability
- Seasonal weight adjustments for ranking algorithms
- Real-time availability integration with spatial search
- Caching strategies for geospatial query results
- Handling edge cases in distance calculations

## OPTIONS ANALYSIS

### Option 1: Simple Radius Search with PostGIS ST_DWithin
**Description**: Basic circular radius search using PostGIS ST_DWithin function with fixed radius
**Pros**:
- Simple implementation and easy to understand
- Fast execution with spatial indexes
- Well-documented PostGIS function
- Predictable performance characteristics
- Easy to debug and maintain
**Cons**:
- Fixed radius may not find sufficient results
- No adaptive behavior for different areas
- Limited ranking sophistication
- May miss relevant rooms just outside radius
- Poor user experience in low-density areas
**Complexity**: Low
**Implementation Time**: 2-3 weeks

### Option 2: Adaptive Radius with Multi-Criteria Ranking
**Description**: Dynamic radius expansion with sophisticated ranking algorithm combining multiple factors
**Pros**:
- Adapts to result availability automatically
- Comprehensive ranking considering multiple factors
- Better user experience with consistent result counts
- Handles varying room densities effectively
- More sophisticated and competitive advantage
**Cons**:
- More complex implementation and testing
- Higher computational cost for ranking calculations
- More difficult to optimize and debug
- Requires careful tuning of ranking weights
- Potential performance issues with large datasets
**Complexity**: High
**Implementation Time**: 6-8 weeks

### Option 3: Hybrid Approach with Caching and Optimization
**Description**: Adaptive radius with intelligent caching, query optimization, and performance tuning
**Pros**:
- Best performance with caching strategies
- Adaptive behavior with optimized queries
- Scalable for high-volume usage
- Good balance of features and performance
- Future-proof with optimization headroom
**Cons**:
- Most complex implementation
- Requires careful cache invalidation strategies
- Higher initial development cost
- More infrastructure requirements
- Complex monitoring and debugging
**Complexity**: High
**Implementation Time**: 8-10 weeks

## DECISION

**Selected Approach**: **Option 3 - Hybrid Approach with Caching and Optimization**

**Rationale**:
1. **MVP Requirements**: Adaptive search is core value proposition
2. **Performance**: Caching essential for high-volume search
3. **Scalability**: Must handle multiple universities and venues
4. **User Experience**: Consistent result quality across different areas
5. **Competitive Advantage**: Sophisticated ranking differentiates platform
6. **Future Growth**: Optimized foundation for scaling

## IMPLEMENTATION PLAN

### Phase 1: Core Spatial Query Design (Week 1-2)
```
Basic Spatial Queries
âââ ST_DWithin for radius search
âââ ST_Distance for exact distance calculation
âââ Spatial indexing with GIST
âââ Bounding box pre-filtering
âââ Query optimization with EXPLAIN ANALYZE

Database Schema
âââ rooms.coordinates (GEOMETRY(POINT, 4326))
âââ exam_venues.coordinates (GEOMETRY(POINT, 4326))
âââ universities.coordinates (GEOMETRY(POINT, 4326))
âââ Spatial indexes on all coordinate columns
```

### Phase 2: Adaptive Radius Algorithm (Week 3-4)
```
Radius Expansion Logic
âââ Initial radius: 2km (configurable)
âââ Expansion factors: 1.5x, 2x, 3x, 5x
âââ Maximum radius: 20km
âââ Minimum result threshold: 10 rooms
âââ Maximum expansion attempts: 4

Adaptive Parameters
âââ Room density analysis
âââ Historical search patterns
âââ University-specific defaults
âââ Time-based adjustments
```

### Phase 3: Multi-Criteria Ranking System (Week 5-6)
```
Ranking Factors
âââ Distance score (40% weight)
âââ Price score (25% weight)
âââ Amenity score (20% weight)
âââ Rating score (10% weight)
âââ Seasonal adjustment (5% weight)

Scoring Algorithms
âââ Distance: Inverse exponential decay
âââ Price: Normalized within search range
âââ Amenities: Weighted sum of available amenities
âââ Rating: Normalized 1-5 star rating
âââ Seasonal: Exam period multiplier
```

### Phase 4: Caching and Performance Optimization (Week 7-8)
```
Caching Strategy
âââ Redis caching for search results
âââ Query result caching (5 minutes TTL)
âââ Popular venue caching (1 hour TTL)
âââ University data caching (24 hours TTL)
âââ Cache invalidation on room updates

Performance Optimization
âââ Query optimization with indexes
âââ Connection pooling
âââ Query result pagination
âââ Background result pre-computation
âââ Monitoring and alerting
```

## VISUALIZATION

### Adaptive Radius Algorithm Flow
```
Start Search
âââ Set initial radius (2km)
âââ Execute spatial query
âââ Check result count
â   âââ If >= 10 results: Proceed to ranking
â   âââ If < 10 results: Expand radius
âââ Radius expansion loop
â   âââ Increase radius by 1.5x
â   âââ Re-execute query
â   âââ Check result count
â   âââ Repeat until threshold or max radius
âââ Apply ranking algorithm
```

### Multi-Criteria Ranking System
```
Room Scoring
âââ Distance Score (40%)
â   âââ Calculate exact distance
â   âââ Apply exponential decay
â   âââ Normalize to 0-100
âââ Price Score (25%)
â   âââ Find min/max in results
â   âââ Normalize price range
â   âââ Invert for lower price preference
âââ Amenity Score (20%)
â   âââ Weighted amenity count
â   âââ Seasonal amenity bonuses
â   âââ Normalize to 0-100
âââ Rating Score (10%)
â   âââ Average rating calculation
â   âââ Review count consideration
â   âââ Normalize to 0-100
âââ Seasonal Adjustment (5%)
    âââ Exam period detection
    âââ Proximity multiplier
    âââ Final score calculation
```

## ALGORITHM IMPLEMENTATION

### Core Spatial Query
```sql
-- Optimized spatial search query
WITH venue_data AS (
    SELECT 
        id,
        coordinates,
        name,
        capacity
    FROM exam_venues 
    WHERE id = $venue_id
),
room_search AS (
    SELECT 
        r.id,
        r.title,
        r.description,
        r.price,
        r.coordinates,
        r.max_occupants,
        r.overall_score,
        ST_Distance(r.coordinates, v.coordinates) as distance_meters,
        CASE 
            WHEN r.price BETWEEN $min_price AND $max_price THEN 1 
            ELSE 0 
        END as price_filter,
        CASE 
            WHEN r.status = 'active' THEN 1 
            ELSE 0 
        END as status_filter
    FROM rooms r
    CROSS JOIN venue_data v
    WHERE ST_DWithin(r.coordinates, v.coordinates, $radius_meters)
      AND r.status = 'active'
      AND r.price BETWEEN $min_price AND $max_price
)
SELECT 
    *,
    -- Distance score (exponential decay)
    EXP(-distance_meters / 1000.0) * 100 as distance_score,
    -- Price score (normalized)
    (1 - (price - $min_price) / ($max_price - $min_price)) * 100 as price_score,
    -- Overall ranking score
    (EXP(-distance_meters / 1000.0) * 0.4 +
     (1 - (price - $min_price) / ($max_price - $min_price)) * 0.25 +
     COALESCE(overall_score, 0) * 0.1) * 100 as ranking_score
FROM room_search
ORDER BY ranking_score DESC
LIMIT $limit OFFSET $offset;
```

### Adaptive Radius Implementation
```typescript
interface SearchParams {
  venueId: string;
  initialRadius: number; // meters
  minResults: number;
  maxExpansions: number;
  expansionFactor: number;
  maxRadius: number;
}

class AdaptiveRadiusSearch {
  async searchRooms(params: SearchParams): Promise<SearchResult> {
    let currentRadius = params.initialRadius;
    let attempts = 0;
    let results: Room[] = [];
    
    while (attempts < params.maxExpansions && results.length < params.minResults) {
      const searchResult = await this.executeSpatialQuery({
        ...params,
        radius: currentRadius
      });
      
      results = searchResult.rooms;
      
      if (results.length >= params.minResults) {
        break;
      }
      
      // Expand radius for next attempt
      currentRadius = Math.min(
        currentRadius * params.expansionFactor,
        params.maxRadius
      );
      attempts++;
    }
    
    // Apply ranking to final results
    const rankedResults = this.applyRanking(results, params);
    
    return {
      rooms: rankedResults,
      searchRadius: currentRadius,
      expansionAttempts: attempts,
      totalResults: results.length
    };
  }
  
  private async executeSpatialQuery(params: SpatialQueryParams): Promise<QueryResult> {
    const query = `
      SELECT r.*, 
             ST_Distance(r.coordinates, ev.coordinates) as distance_meters
      FROM rooms r
      CROSS JOIN exam_venues ev
      WHERE ev.id = $1
        AND ST_DWithin(r.coordinates, ev.coordinates, $2)
        AND r.status = 'active'
        AND r.price BETWEEN $3 AND $4
      ORDER BY distance_meters ASC
      LIMIT $5
    `;
    
    return await this.database.query(query, [
      params.venueId,
      params.radius,
      params.minPrice,
      params.maxPrice,
      params.limit
    ]);
  }
}
```

### Multi-Criteria Ranking Algorithm
```typescript
interface RankingWeights {
  distance: number;    // 0.4
  price: number;       // 0.25
  amenities: number;   // 0.2
  rating: number;      // 0.1
  seasonal: number;    // 0.05
}

class RoomRankingAlgorithm {
  calculateRankingScore(room: Room, venue: ExamVenue, weights: RankingWeights): number {
    const distanceScore = this.calculateDistanceScore(room, venue);
    const priceScore = this.calculatePriceScore(room);
    const amenityScore = this.calculateAmenityScore(room);
    const ratingScore = this.calculateRatingScore(room);
    const seasonalScore = this.calculateSeasonalScore(room, venue);
    
    return (
      distanceScore * weights.distance +
      priceScore * weights.price +
      amenityScore * weights.amenities +
      ratingScore * weights.rating +
      seasonalScore * weights.seasonal
    );
  }
  
  private calculateDistanceScore(room: Room, venue: ExamVenue): number {
    const distanceKm = this.calculateDistance(room.coordinates, venue.coordinates);
    // Exponential decay: closer rooms get higher scores
    return Math.exp(-distanceKm / 2.0) * 100;
  }
  
  private calculatePriceScore(room: Room): number {
    // Normalize price within search range
    const minPrice = this.searchContext.minPrice;
    const maxPrice = this.searchContext.maxPrice;
    const priceRange = maxPrice - minPrice;
    
    if (priceRange === 0) return 50; // Neutral score for single price
    
    const normalizedPrice = (room.price - minPrice) / priceRange;
    // Invert so lower prices get higher scores
    return (1 - normalizedPrice) * 100;
  }
  
  private calculateAmenityScore(room: Room): number {
    const amenityWeights = {
      wifi: 20,
      kitchen: 15,
      bathroom: 25,
      parking: 10,
      air_conditioning: 15,
      heating: 15
    };
    
    let totalScore = 0;
    let maxPossibleScore = Object.values(amenityWeights).reduce((a, b) => a + b, 0);
    
    room.amenities.forEach(amenity => {
      totalScore += amenityWeights[amenity.type] || 0;
    });
    
    return (totalScore / maxPossibleScore) * 100;
  }
  
  private calculateRatingScore(room: Room): number {
    if (!room.rating || room.ratingCount === 0) {
      return 50; // Neutral score for no ratings
    }
    
    // Consider both rating and number of reviews
    const ratingScore = (room.rating / 5.0) * 80; // 80% weight for rating
    const reviewCountScore = Math.min(room.ratingCount / 10, 1) * 20; // 20% weight for review count
    
    return ratingScore + reviewCountScore;
  }
  
  private calculateSeasonalScore(room: Room, venue: ExamVenue): number {
    const examPeriods = this.getExamPeriods(venue.universityId);
    const currentDate = new Date();
    
    // Check if current date is within exam period
    const isExamPeriod = examPeriods.some(period => 
      currentDate >= period.startDate && currentDate <= period.endDate
    );
    
    if (isExamPeriod) {
      // Boost score for rooms during exam periods
      return 20;
    }
    
    return 0; // No seasonal adjustment
  }
}
```

### Caching Strategy Implementation
```typescript
class GeospatialCacheManager {
  private redis: Redis;
  private readonly CACHE_TTL = {
    SEARCH_RESULTS: 300,    // 5 minutes
    POPULAR_VENUES: 3600,   // 1 hour
    UNIVERSITY_DATA: 86400,  // 24 hours
    ROOM_DETAILS: 1800      // 30 minutes
  };
  
  async getCachedSearchResults(cacheKey: string): Promise<SearchResult | null> {
    try {
      const cached = await this.redis.get(cacheKey);
      return cached ? JSON.parse(cached) : null;
    } catch (error) {
      console.error('Cache retrieval error:', error);
      return null;
    }
  }
  
  async cacheSearchResults(cacheKey: string, results: SearchResult): Promise<void> {
    try {
      await this.redis.setex(
        cacheKey,
        this.CACHE_TTL.SEARCH_RESULTS,
        JSON.stringify(results)
      );
    } catch (error) {
      console.error('Cache storage error:', error);
    }
  }
  
  generateCacheKey(params: SearchParams): string {
    const keyComponents = [
      'search',
      params.venueId,
      params.radius,
      params.minPrice,
      params.maxPrice,
      params.checkIn,
      params.checkOut,
      params.amenities?.sort().join(',') || ''
    ];
    
    return `geospatial:${keyComponents.join(':')}`;
  }
  
  async invalidateRoomCache(roomId: string): Promise<void> {
    try {
      // Invalidate all search results that might include this room
      const pattern = `geospatial:search:*`;
      const keys = await this.redis.keys(pattern);
      
      // Check each cached result and invalidate if it contains the room
      for (const key of keys) {
        const cached = await this.redis.get(key);
        if (cached) {
          const results = JSON.parse(cached);
          const containsRoom = results.rooms.some((room: Room) => room.id === roomId);
          if (containsRoom) {
            await this.redis.del(key);
          }
        }
      }
    } catch (error) {
      console.error('Cache invalidation error:', error);
    }
  }
}
```

## PERFORMANCE OPTIMIZATION

### Spatial Index Optimization
```sql
-- Create optimized spatial indexes
CREATE INDEX CONCURRENTLY idx_rooms_coordinates_spatial 
ON rooms USING GIST (coordinates);

CREATE INDEX CONCURRENTLY idx_rooms_status_coordinates 
ON rooms USING GIST (coordinates) 
WHERE status = 'active';

CREATE INDEX CONCURRENTLY idx_rooms_price_status 
ON rooms (price) 
WHERE status = 'active';

-- Composite index for common query patterns
CREATE INDEX CONCURRENTLY idx_rooms_search_optimized 
ON rooms USING GIST (coordinates) 
WHERE status = 'active' AND price > 0;

-- Analyze table statistics for query planner
ANALYZE rooms;
ANALYZE exam_venues;
ANALYZE universities;
```

### Query Performance Monitoring
```typescript
class QueryPerformanceMonitor {
  private metrics: Map<string, QueryMetrics> = new Map();
  
  async monitorQueryPerformance(queryName: string, queryFn: () => Promise<any>): Promise<any> {
    const startTime = Date.now();
    const startMemory = process.memoryUsage();
    
    try {
      const result = await queryFn();
      const endTime = Date.now();
      const endMemory = process.memoryUsage();
      
      this.recordMetrics(queryName, {
        executionTime: endTime - startTime,
        memoryUsage: endMemory.heapUsed - startMemory.heapUsed,
        success: true,
        timestamp: new Date()
      });
      
      return result;
    } catch (error) {
      const endTime = Date.now();
      
      this.recordMetrics(queryName, {
        executionTime: endTime - startTime,
        memoryUsage: 0,
        success: false,
        error: error.message,
        timestamp: new Date()
      });
      
      throw error;
    }
  }
  
  private recordMetrics(queryName: string, metrics: QueryMetrics): void {
    if (!this.metrics.has(queryName)) {
      this.metrics.set(queryName, {
        totalExecutions: 0,
        totalExecutionTime: 0,
        averageExecutionTime: 0,
        maxExecutionTime: 0,
        minExecutionTime: Infinity,
        successRate: 0,
        lastExecuted: null
      });
    }
    
    const existing = this.metrics.get(queryName)!;
    existing.totalExecutions++;
    existing.totalExecutionTime += metrics.executionTime;
    existing.averageExecutionTime = existing.totalExecutionTime / existing.totalExecutions;
    existing.maxExecutionTime = Math.max(existing.maxExecutionTime, metrics.executionTime);
    existing.minExecutionTime = Math.min(existing.minExecutionTime, metrics.executionTime);
    existing.successRate = metrics.success ? 
      (existing.successRate * (existing.totalExecutions - 1) + 1) / existing.totalExecutions :
      (existing.successRate * (existing.totalExecutions - 1)) / existing.totalExecutions;
    existing.lastExecuted = metrics.timestamp;
  }
  
  getPerformanceReport(): PerformanceReport {
    const report: PerformanceReport = {
      queries: Array.from(this.metrics.entries()).map(([name, metrics]) => ({
        name,
        ...metrics
      })),
      summary: {
        totalQueries: Array.from(this.metrics.values()).reduce((sum, m) => sum + m.totalExecutions, 0),
        averageExecutionTime: Array.from(this.metrics.values()).reduce((sum, m) => sum + m.averageExecutionTime, 0) / this.metrics.size,
        overallSuccessRate: Array.from(this.metrics.values()).reduce((sum, m) => sum + m.successRate, 0) / this.metrics.size
      }
    };
    
    return report;
  }
}
```

## SEASONAL ADJUSTMENTS

### Exam Period Detection
```typescript
class SeasonalRankingAdjustment {
  async getExamPeriods(universityId: string): Promise<ExamPeriod[]> {
    const query = `
      SELECT 
        id,
        name,
        start_date,
        end_date,
        student_count
      FROM exam_sessions
      WHERE university_id = $1
        AND start_date >= CURRENT_DATE
        AND end_date <= CURRENT_DATE + INTERVAL '6 months'
      ORDER BY start_date ASC
    `;
    
    return await this.database.query(query, [universityId]);
  }
  
  calculateSeasonalMultiplier(room: Room, venue: ExamVenue, currentDate: Date): number {
    const examPeriods = await this.getExamPeriods(venue.universityId);
    
    // Check if current date is within exam period
    const activeExamPeriod = examPeriods.find(period => 
      currentDate >= period.startDate && currentDate <= period.endDate
    );
    
    if (activeExamPeriod) {
      // Calculate distance-based seasonal boost
      const distanceKm = this.calculateDistance(room.coordinates, venue.coordinates);
      
      // Closer rooms get higher seasonal boost during exams
      if (distanceKm <= 1) return 1.5;  // 50% boost for very close rooms
      if (distanceKm <= 2) return 1.3;  // 30% boost for close rooms
      if (distanceKm <= 5) return 1.1;  // 10% boost for nearby rooms
    }
    
    return 1.0; // No seasonal adjustment
  }
}
```

## íž¨ CREATIVE CHECKPOINT: GEOSPATIAL SEARCH ALGORITHM DESIGN COMPLETE

**Decision Made**: Hybrid Approach with Caching and Optimization
**Key Features**: 
- Adaptive radius algorithm with intelligent expansion
- Multi-criteria ranking system with 5 weighted factors
- Comprehensive caching strategy with Redis
- Performance optimization with spatial indexes
- Seasonal adjustments for exam periods
- Real-time monitoring and performance tracking

**Next Creative Phase**: Real-time Communication Design

## íž¨íž¨íž¨ EXITING CREATIVE PHASE - DECISION MADE íž¨íž¨íž¨

*This creative phase document captures the geospatial search algorithm design decisions for UniNest*
