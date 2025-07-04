# íž¨ CREATIVE PHASE: Mobile App Architecture Design

## íž¨íž¨íž¨ ENTERING CREATIVE PHASE: MOBILE ARCHITECTURE íž¨íž¨íž¨

**Date:** $(date)  
**Phase:** 4 of 8 - Mobile App Architecture Design  
**Type:** Architecture Design  
**Focus:** Kotlin/Jetpack Compose structure, navigation, and mobile-specific features for Android app

## PROBLEM STATEMENT

Design a comprehensive mobile app architecture for UniNest Android application that supports:
1. **Multi-role user interfaces** (Student, Landlord, University Official, Admin)
2. **Geospatial room search** with offline map capabilities and location services
3. **Real-time features** (messaging, notifications, booking updates)
4. **Offline functionality** for core features when network is unavailable
5. **Performance optimization** for smooth scrolling and image handling
6. **Push notifications** for booking updates and messages
7. **Camera integration** for photo uploads and document scanning
8. **Accessibility features** for inclusive mobile experience

**Key Challenges:**
- Complex state management across multiple screens and user roles
- Real-time data synchronization with WebSocket connections
- Offline data persistence and conflict resolution
- Geospatial map integration with Google Maps SDK
- Performance optimization for image-heavy room listings
- Push notification management and user engagement
- Camera and file upload integration

## OPTIONS ANALYSIS

### Option 1: Traditional Android Architecture with Activities/Fragments
**Description**: Classic Android architecture using Activities, Fragments, and ViewModels with XML layouts
**Pros**:
- Well-established patterns and extensive documentation
- Large community support and resources
- Mature ecosystem with proven libraries
- Easier to find experienced developers
- Stable and predictable behavior
**Cons**:
- More boilerplate code and complexity
- Fragmented lifecycle management
- Less modern compared to Compose
- Harder to implement complex UI interactions
- More difficult to test and maintain
**Complexity**: Medium
**Implementation Time**: 8-10 weeks

### Option 2: Modern Jetpack Compose with MVVM
**Description**: Kotlin-first UI toolkit with Compose, ViewModels, and modern Android architecture components
**Pros**:
- Declarative UI with less boilerplate code
- Better state management and UI consistency
- Easier to implement complex animations and interactions
- More testable and maintainable code
- Future-proof technology aligned with Google's direction
- Better performance with recomposition optimization
**Cons**:
- Steeper learning curve for team
- Newer technology with evolving best practices
- Smaller community compared to traditional approach
- Potential performance issues with complex UIs
- Limited third-party library support
**Complexity**: Medium-High
**Implementation Time**: 7-9 weeks

### Option 3: Hybrid Approach with Compose + Traditional
**Description**: Mix of Compose for new features and traditional Views for complex existing components
**Pros**:
- Gradual migration path from traditional to Compose
- Leverage existing team expertise where needed
- Use best tool for specific use cases
- Reduced risk compared to full Compose adoption
- Maintain compatibility with existing libraries
**Cons**:
- Increased complexity with two UI systems
- Potential inconsistency in user experience
- More difficult to maintain and test
- Team needs expertise in both approaches
- Migration overhead and technical debt
**Complexity**: High
**Implementation Time**: 9-11 weeks

## DECISION

**Selected Approach**: **Option 2 - Modern Jetpack Compose with MVVM**

**Rationale**:
1. **MVP Timeline**: Compose provides faster development for modern UI patterns
2. **Team Future**: Aligns with Google's direction and modern Android development
3. **User Experience**: Better animations and interactions for room search and booking
4. **Maintainability**: Less boilerplate and better state management
5. **Performance**: Optimized recomposition for smooth scrolling
6. **Consistency**: Unified UI approach across all features

## IMPLEMENTATION PLAN

### Phase 1: Core Architecture Setup (Week 1-2)
```
Project Structure
âââ app/
â   âââ src/main/
â   â   âââ java/com/uninest/
â   â   â   âââ data/
â   â   â   â   âââ api/           # API services
â   â   â   â   âââ database/      # Room database
â   â   â   â   âââ repository/    # Data repositories
â   â   â   â   âââ models/        # Data models
â   â   â   âââ domain/
â   â   â   â   âââ usecases/      # Business logic
â   â   â   â   âââ models/        # Domain models
â   â   â   â   âââ repository/    # Repository interfaces
â   â   â   âââ presentation/
â   â   â   â   âââ screens/       # Compose screens
â   â   â   â   âââ components/    # Reusable components
â   â   â   â   âââ viewmodels/    # ViewModels
â   â   â   â   âââ navigation/    # Navigation components
â   â   â   âââ di/                # Dependency injection
â   â   â   âââ utils/             # Utility functions
â   â   â   âââ MainActivity.kt
â   â   âââ res/
â   â       âââ values/            # Themes, strings, colors
â   â       âââ drawable/          # Icons and images
```

### Phase 2: Navigation & Core Screens (Week 3-4)
```
Navigation Structure
âââ Authentication Flow
â   âââ LoginScreen
â   âââ RegisterScreen
â   âââ ForgotPasswordScreen
âââ Student Flow
â   âââ HomeScreen
â   âââ SearchScreen
â   âââ RoomDetailScreen
â   âââ BookingScreen
â   âââ MyBookingsScreen
â   âââ ProfileScreen
âââ Landlord Flow
â   âââ DashboardScreen
â   âââ MyRoomsScreen
â   âââ AddRoomScreen
â   âââ BookingsScreen
â   âââ AnalyticsScreen
âââ Common Features
â   âââ MessagingScreen
â   âââ NotificationsScreen
â   âââ SettingsScreen
```

### Phase 3: Data Layer & State Management (Week 5-6)
```
Data Architecture
âââ Repository Pattern
â   âââ RoomRepository
â   âââ BookingRepository
â   âââ UserRepository
â   âââ MessageRepository
â   âââ NotificationRepository
âââ Local Database (Room)
â   âââ RoomDatabase
â   âââ Entities
â   âââ DAOs
âââ Remote API (Retrofit)
â   âââ ApiService
â   âââ WebSocketService
â   âââ NetworkBoundResource
âââ State Management
â   âââ ViewModels
â   âââ StateFlow
â   âââ SharedFlow
```

### Phase 4: Advanced Features & Optimization (Week 7-8)
```
Advanced Features
âââ Offline Support
â   âââ OfflineFirstRepository
â   âââ SyncManager
â   âââ ConflictResolution
âââ Push Notifications
â   âââ NotificationService
â   âââ FCM Integration
â   âââ NotificationManager
âââ Camera & File Upload
â   âââ CameraManager
â   âââ ImageProcessor
â   âââ UploadService
âââ Maps Integration
â   âââ GoogleMapsManager
â   âââ LocationService
â   âââ Geofencing
```

## VISUALIZATION

### Mobile App Architecture Diagram
```
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â                    PRESENTATION LAYER                           â
â  âââââââââââââââââââ  âââââââââââââââââââ  âââââââââââââââââââ  â
â  â   Compose       â  â   Navigation    â  â   ViewModels    â  â
â  â   Screens       â  â   Components    â  â   & State       â  â
â  âââââââââââââââââââ  âââââââââââââââââââ  âââââââââââââââââââ  â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
                                â
                      âââââââââââźââââââââââ
                      â   DOMAIN LAYER    â
                      âââââââââââŹââââââââââ
                                â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
â                      DATA LAYER                                â
â âââââââââââââââ âââââââââââââââ âââââââââââââââ âââââââââââââââ â
â â   Remote    â â   Local     â â Repository  â â   Use Cases â â
â â    API      â â  Database   â â  Pattern    â â             â â
â âââââââââââââââ âââââââââââââââ âââââââââââââââ âââââââââââââââ â
âââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââââ
```

### Navigation Flow Diagram
```
Authentication
âââ Login â Home
âââ Register â Email Verification â Home
âââ Forgot Password â Reset â Login

Student Flow
âââ Home â Search â Room Detail â Booking â Confirmation
âââ Home â My Bookings â Booking Detail
âââ Home â Messages â Conversation
âââ Home â Profile â Settings

Landlord Flow
âââ Dashboard â My Rooms â Room Detail â Edit
âââ Dashboard â Add Room â Photo Upload â Publish
âââ Dashboard â Bookings â Booking Detail â Manage
âââ Dashboard â Analytics â Reports
```

## COMPONENT DESIGN PATTERNS

### Compose Screen Structure
```kotlin
// Screen composable with ViewModel
@Composable
fun SearchScreen(
    viewModel: SearchViewModel = hiltViewModel(),
    onRoomClick: (String) -> Unit
) {
    val uiState by viewModel.uiState.collectAsState()
    
    SearchScreenContent(
        uiState = uiState,
        onSearch = viewModel::searchRooms,
        onFilterChange = viewModel::updateFilters,
        onRoomClick = onRoomClick
    )
}

// Screen content composable
@Composable
fun SearchScreenContent(
    uiState: SearchUiState,
    onSearch: (SearchParams) -> Unit,
    onFilterChange: (SearchFilters) -> Unit,
    onRoomClick: (String) -> Unit
) {
    Column(
        modifier = Modifier.fillMaxSize()
    ) {
        SearchHeader(
            filters = uiState.filters,
            onFilterChange = onFilterChange
        )
        
        when (uiState) {
            is SearchUiState.Loading -> LoadingIndicator()
            is SearchUiState.Success -> RoomList(
                rooms = uiState.rooms,
                onRoomClick = onRoomClick
            )
            is SearchUiState.Error -> ErrorMessage(
                message = uiState.message,
                onRetry = { onSearch(uiState.lastParams) }
            )
        }
    }
}
```

### ViewModel with State Management
```kotlin
@HiltViewModel
class SearchViewModel @Inject constructor(
    private val roomRepository: RoomRepository,
    private val locationManager: LocationManager
) : ViewModel() {
    
    private val _uiState = MutableStateFlow<SearchUiState>(SearchUiState.Initial)
    val uiState: StateFlow<SearchUiState> = _uiState.asStateFlow()
    
    private val _filters = MutableStateFlow(SearchFilters())
    val filters: StateFlow<SearchFilters> = _filters.asStateFlow()
    
    fun searchRooms(params: SearchParams) {
        viewModelScope.launch {
            _uiState.value = SearchUiState.Loading
            
            try {
                val rooms = roomRepository.searchRooms(params)
                _uiState.value = SearchUiState.Success(rooms, params)
            } catch (e: Exception) {
                _uiState.value = SearchUiState.Error(e.message ?: "Search failed", params)
            }
        }
    }
    
    fun updateFilters(newFilters: SearchFilters) {
        _filters.value = newFilters
    }
}

sealed class SearchUiState {
    object Initial : SearchUiState()
    object Loading : SearchUiState()
    data class Success(val rooms: List<Room>, val params: SearchParams) : SearchUiState()
    data class Error(val message: String, val lastParams: SearchParams) : SearchUiState()
}
```

### Repository Pattern Implementation
```kotlin
class RoomRepositoryImpl @Inject constructor(
    private val apiService: ApiService,
    private val roomDao: RoomDao,
    private val networkBoundResource: NetworkBoundResource
) : RoomRepository {
    
    override fun searchRooms(params: SearchParams): Flow<Resource<List<Room>>> {
        return networkBoundResource.query(
            query = {
                roomDao.searchRooms(params.toDatabaseQuery())
            },
            fetch = {
                apiService.searchRooms(params.toApiQuery())
            },
            saveFetchResult = { rooms ->
                roomDao.insertRooms(rooms)
            },
            shouldFetch = { cachedRooms ->
                cachedRooms.isEmpty() || isStale(cachedRooms.first().lastUpdated)
            }
        )
    }
    
    override fun getRoom(id: String): Flow<Resource<Room>> {
        return networkBoundResource.query(
            query = {
                roomDao.getRoom(id)
            },
            fetch = {
                apiService.getRoom(id)
            },
            saveFetchResult = { room ->
                roomDao.insertRoom(room)
            },
            shouldFetch = { cachedRoom ->
                cachedRoom == null || isStale(cachedRoom.lastUpdated)
            }
        )
    }
}
```

## OFFLINE FUNCTIONALITY

### Offline-First Architecture
```kotlin
// NetworkBoundResource for offline-first approach
class NetworkBoundResource<ResultType, RequestType> @Inject constructor() {
    
    fun query(
        query: () -> Flow<ResultType>,
        fetch: suspend () -> RequestType,
        saveFetchResult: suspend (RequestType) -> Unit,
        shouldFetch: (ResultType) -> Boolean,
        onFetchFailed: (Throwable) -> Unit = { }
    ): Flow<Resource<ResultType>> = flow {
        emit(Resource.Loading())
        
        val data = query().first()
        
        val flow = if (shouldFetch(data)) {
            emit(Resource.Loading(data))
            
            try {
                val fetchedResult = fetch()
                saveFetchResult(fetchedResult)
                query().map { Resource.Success(it) }
            } catch (t: Throwable) {
                onFetchFailed(t)
                query().map { Resource.Error(t.toString(), it) }
            }
        } else {
            query().map { Resource.Success(it) }
        }
        
        emitAll(flow)
    }
}

// Offline data synchronization
class SyncManager @Inject constructor(
    private val workManager: WorkManager
) {
    
    fun scheduleSync() {
        val syncRequest = PeriodicWorkRequestBuilder<SyncWorker>(
            repeatInterval = 15,
            repeatIntervalTimeUnit = TimeUnit.MINUTES
        ).setConstraints(
            Constraints.Builder()
                .setRequiredNetworkType(NetworkType.CONNECTED)
                .build()
        ).build()
        
        workManager.enqueueUniquePeriodicWork(
            "sync_work",
            ExistingPeriodicWorkPolicy.KEEP,
            syncRequest
        )
    }
}
```

## PUSH NOTIFICATIONS

### FCM Integration
```kotlin
class UniNestFirebaseMessagingService : FirebaseMessagingService() {
    
    @Inject
    lateinit var notificationManager: NotificationManager
    
    override fun onMessageReceived(remoteMessage: RemoteMessage) {
        super.onMessageReceived(remoteMessage)
        
        val notification = remoteMessage.notification
        val data = remoteMessage.data
        
        if (notification != null) {
            showNotification(
                title = notification.title ?: "",
                body = notification.body ?: "",
                data = data
            )
        }
    }
    
    private fun showNotification(title: String, body: String, data: Map<String, String>) {
        val notification = NotificationCompat.Builder(this, CHANNEL_ID)
            .setContentTitle(title)
            .setContentText(body)
            .setSmallIcon(R.drawable.ic_notification)
            .setAutoCancel(true)
            .setContentIntent(createPendingIntent(data))
            .build()
        
        notificationManager.notify(System.currentTimeMillis().toInt(), notification)
    }
    
    override fun onNewToken(token: String) {
        super.onNewToken(token)
        // Send token to server
        sendTokenToServer(token)
    }
}
```

## CAMERA & FILE UPLOAD

### Camera Integration
```kotlin
class CameraManager @Inject constructor(
    private val context: Context
) {
    
    fun takePhoto(
        activity: ComponentActivity,
        onPhotoTaken: (Uri) -> Unit
    ) {
        val photoFile = createImageFile()
        val photoUri = FileProvider.getUriForFile(
            context,
            "${context.packageName}.fileprovider",
            photoFile
        )
        
        val takePictureIntent = Intent(MediaStore.ACTION_IMAGE_CAPTURE).apply {
            putExtra(MediaStore.EXTRA_OUTPUT, photoUri)
        }
        
        activity.registerForActivityResult(
            ActivityResultContracts.TakePicture()
        ) { success ->
            if (success) {
                onPhotoTaken(photoUri)
            }
        }.launch(photoUri)
    }
    
    fun selectImage(
        activity: ComponentActivity,
        onImageSelected: (Uri) -> Unit
    ) {
        activity.registerForActivityResult(
            ActivityResultContracts.GetContent()
        ) { uri ->
            uri?.let { onImageSelected(it) }
        }.launch("image/*")
    }
}

// Image upload service
class ImageUploadService @Inject constructor(
    private val apiService: ApiService
) {
    
    suspend fun uploadImage(imageUri: Uri): Result<String> {
        return try {
            val file = createFileFromUri(imageUri)
            val response = apiService.uploadImage(file)
            Result.success(response.url)
        } catch (e: Exception) {
            Result.failure(e)
        }
    }
}
```

## MAPS INTEGRATION

### Google Maps Integration
```kotlin
@Composable
fun RoomMap(
    rooms: List<Room>,
    onRoomClick: (Room) -> Unit,
    modifier: Modifier = Modifier
) {
    val cameraPositionState = rememberCameraPositionState {
        position = CameraPosition.fromLatLngZoom(
            LatLng(0.0, 0.0), 10f
        )
    }
    
    GoogleMap(
        modifier = modifier,
        cameraPositionState = cameraPositionState,
        properties = MapProperties(
            isMyLocationEnabled = true,
            mapType = MapType.NORMAL
        )
    ) {
        rooms.forEach { room ->
            Marker(
                state = MarkerState(position = room.coordinates.toLatLng()),
                title = room.title,
                snippet = "$${room.price}/night",
                onClick = {
                    onRoomClick(room)
                    true
                }
            )
        }
    }
}

// Location services
class LocationManager @Inject constructor(
    private val context: Context
) {
    
    private val fusedLocationClient = LocationServices.getFusedLocationProviderClient(context)
    
    fun getCurrentLocation(): Flow<Location> = callbackFlow {
        if (checkLocationPermission()) {
            fusedLocationClient.lastLocation
                .addOnSuccessListener { location ->
                    trySend(location)
                }
                .addOnFailureListener { exception ->
                    close(exception)
                }
        } else {
            close(IllegalStateException("Location permission not granted"))
        }
    }
    
    fun startLocationUpdates(): Flow<Location> = callbackFlow {
        val locationRequest = LocationRequest.create().apply {
            priority = LocationRequest.PRIORITY_HIGH_ACCURACY
            interval = 10000 // 10 seconds
        }
        
        val locationCallback = object : LocationCallback() {
            override fun onLocationResult(locationResult: LocationResult) {
                locationResult.lastLocation?.let { location ->
                    trySend(location)
                }
            }
        }
        
        fusedLocationClient.requestLocationUpdates(
            locationRequest,
            locationCallback,
            Looper.getMainLooper()
        )
        
        awaitClose {
            fusedLocationClient.removeLocationUpdates(locationCallback)
        }
    }
}
```

## PERFORMANCE OPTIMIZATION

### Lazy Loading and Pagination
```kotlin
@Composable
fun RoomList(
    rooms: List<Room>,
    onRoomClick: (String) -> Unit,
    onLoadMore: () -> Unit
) {
    LazyColumn {
        items(
            items = rooms,
            key = { it.id }
        ) { room ->
            RoomCard(
                room = room,
                onClick = { onRoomClick(room.id) }
            )
        }
        
        item {
            if (rooms.isNotEmpty()) {
                LaunchedEffect(Unit) {
                    onLoadMore()
                }
            }
        }
    }
}

// Image loading with Coil
@Composable
fun AsyncImage(
    url: String,
    contentDescription: String?,
    modifier: Modifier = Modifier,
    placeholder: @Composable (() -> Unit)? = null,
    error: @Composable (() -> Unit)? = null
) {
    AsyncImage(
        model = ImageRequest.Builder(LocalContext.current)
            .data(url)
            .crossfade(true)
            .build(),
        contentDescription = contentDescription,
        modifier = modifier,
        placeholder = placeholder,
        error = error
    )
}
```

## ACCESSIBILITY FEATURES

### Accessibility Implementation
```kotlin
@Composable
fun AccessibleButton(
    onClick: () -> Unit,
    modifier: Modifier = Modifier,
    enabled: Boolean = true,
    content: @Composable RowScope.() -> Unit
) {
    Button(
        onClick = onClick,
        modifier = modifier.semantics {
            contentDescription = "Button for action"
            role = Role.Button
        },
        enabled = enabled,
        content = content
    )
}

@Composable
fun AccessibleTextField(
    value: String,
    onValueChange: (String) -> Unit,
    label: String,
    modifier: Modifier = Modifier
) {
    OutlinedTextField(
        value = value,
        onValueChange = onValueChange,
        label = { Text(label) },
        modifier = modifier.semantics {
            contentDescription = "Input field for $label"
        }
    )
}
```

## íž¨ CREATIVE CHECKPOINT: MOBILE APP ARCHITECTURE DESIGN COMPLETE

**Decision Made**: Modern Jetpack Compose with MVVM
**Key Features**: 
- Clean architecture with domain, data, and presentation layers
- Offline-first approach with Room database
- Push notifications with FCM integration
- Camera and file upload capabilities
- Google Maps integration with location services
- Performance optimization with lazy loading and image caching

**Next Creative Phase**: Geospatial Search Algorithm Design

## íž¨íž¨íž¨ EXITING CREATIVE PHASE - DECISION MADE íž¨íž¨íž¨

*This creative phase document captures the mobile app architecture design decisions for UniNest*
