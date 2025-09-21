# Development Guide for LostLK

This guide provides comprehensive information for developers working on LostLK.

## 🏗️ Architecture Overview

### Project Structure
```
app/
├── src/main/java/fm/mrc/lostlk/
│   ├── models/                 # Data models and entities
│   ├── utils/                  # Utility classes and helpers
│   ├── ui/theme/              # UI theme and styling
│   ├── MainActivity.kt        # Main dashboard
│   ├── LoginActivity.kt       # Authentication
│   ├── RegisterActivity.kt    # User registration
│   ├── UserProfileActivity.kt # User profile management
│   ├── ReportLostItemActivity.kt # Lost item reporting
│   ├── MapViewActivity.kt     # Interactive map
│   ├── AdminPanelActivity.kt  # Admin dashboard
│   └── ViewLostItemsActivity.kt # Browse lost items
├── src/main/res/              # Resources (layouts, drawables, etc.)
└── build.gradle.kts           # App-level build configuration
```

### Technology Stack
- **Frontend**: Jetpack Compose, Material Design 3
- **Backend**: Firebase (Authentication, Realtime Database)
- **Maps**: Google Maps SDK for Android
- **Language**: Kotlin
- **Architecture**: MVVM with Compose
- **Build System**: Gradle with Kotlin DSL

## 🚀 Getting Started

### Prerequisites
- Android Studio Arctic Fox or later
- Android SDK 24 or higher
- Kotlin 1.8.0 or later
- Firebase project setup
- Google Maps API key

### Development Environment Setup
1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/LostLK.git
   cd LostLK
   ```

2. **Set up Firebase**
   - Create Firebase project
   - Add `google-services.json` to `app/` directory
   - Configure Authentication and Realtime Database

3. **Configure Google Maps**
   - Get API key from Google Cloud Console
   - Add to `local.properties` file
   - Enable Maps SDK for Android

4. **Open in Android Studio**
   - Launch Android Studio
   - Open the project
   - Sync with Gradle

## 📱 Core Components

### Authentication System
```kotlin
// Firebase Authentication
class AuthManager {
    fun signIn(email: String, password: String)
    fun signUp(email: String, password: String)
    fun signOut()
    fun getCurrentUser(): FirebaseUser?
}
```

### Data Models
```kotlin
// Lost Item Model
data class LostItem(
    val id: String = "",
    val itemName: String = "",
    val description: String = "",
    val category: String = "",
    val location: String = "",
    val userId: String = "",
    val userEmail: String = "",
    val timestamp: Long = 0L,
    val status: String = "Active"
)
```

### Database Operations
```kotlin
// Firebase Realtime Database
class DatabaseManager {
    fun saveLostItem(item: LostItem)
    fun getLostItems(): Flow<List<LostItem>>
    fun updateItemStatus(itemId: String, status: String)
    fun deleteItem(itemId: String)
}
```

## 🎨 UI Development

### Jetpack Compose
LostLK uses Jetpack Compose for modern, declarative UI development.

#### Basic Composable
```kotlin
@Composable
fun LostItemCard(
    item: LostItem,
    onItemClick: (LostItem) -> Unit
) {
    Card(
        modifier = Modifier
            .fillMaxWidth()
            .padding(8.dp)
            .clickable { onItemClick(item) },
        elevation = CardDefaults.cardElevation(defaultElevation = 4.dp)
    ) {
        Column(
            modifier = Modifier.padding(16.dp)
        ) {
            Text(
                text = item.itemName,
                style = MaterialTheme.typography.headlineSmall
            )
            Text(
                text = item.description,
                style = MaterialTheme.typography.bodyMedium
            )
            Text(
                text = item.location,
                style = MaterialTheme.typography.bodySmall
            )
        }
    }
}
```

#### Theme System
```kotlin
// Custom theme with admin support
@Composable
fun LostLKTheme(
    isAdmin: Boolean = false,
    content: @Composable () -> Unit
) {
    val colors = if (isAdmin) {
        // Red theme for admin users
        darkColorScheme(
            primary = Color(0xFFD32F2F),
            secondary = Color(0xFFB71C1C)
        )
    } else {
        // Default blue theme
        lightColorScheme(
            primary = Color(0xFF667eea),
            secondary = Color(0xFF764ba2)
        )
    }
    
    MaterialTheme(
        colorScheme = colors,
        typography = Typography,
        content = content
    )
}
```

### Custom Components
```kotlin
// Reusable UI components
@Composable
fun LostLKLogo(modifier: Modifier = Modifier) {
    // Custom logo implementation
}

@Composable
fun ActionCard(
    title: String,
    description: String,
    icon: ImageVector,
    color: Color,
    onClick: () -> Unit
) {
    // Action card implementation
}

@Composable
fun EnhancedTextField(
    value: String,
    onValueChange: (String) -> Unit,
    label: String,
    placeholder: String = "",
    isError: Boolean = false,
    errorMessage: String = ""
) {
    // Enhanced text field implementation
}
```

## 🗺️ Map Integration

### Google Maps Setup
```kotlin
class MapViewActivity : ComponentActivity(), OnMapReadyCallback {
    private lateinit var googleMap: GoogleMap
    
    override fun onMapReady(map: GoogleMap) {
        googleMap = map
        setupMap()
        loadLostItems()
    }
    
    private fun setupMap() {
        // Configure map settings
        googleMap.uiSettings.isZoomControlsEnabled = true
        googleMap.uiSettings.isMyLocationButtonEnabled = true
        
        // Set initial camera position
        val sriLankaCenter = LatLng(7.8731, 80.7718)
        googleMap.moveCamera(CameraUpdateFactory.newLatLngZoom(sriLankaCenter, 7f))
    }
    
    private fun loadLostItems() {
        // Load and display lost items as markers
        databaseManager.getLostItems().collect { items ->
            items.forEach { item ->
                addMarker(item)
            }
        }
    }
}
```

### Marker Management
```kotlin
private fun addMarker(item: LostItem) {
    val location = parseLocation(item.location)
    if (location != null) {
        val marker = googleMap.addMarker(
            MarkerOptions()
                .position(location)
                .title(item.itemName)
                .snippet(item.description)
        )
        marker?.tag = item
    }
}

private fun parseLocation(locationString: String): LatLng? {
    // Parse location string to LatLng
    // Implementation depends on location format
    return null
}
```

## 🔐 Security Implementation

### Firebase Security Rules
```json
{
  "rules": {
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid",
        ".write": "$uid === auth.uid"
      }
    },
    "lost_items": {
      ".read": "auth != null",
      ".write": "auth != null",
      ".validate": "newData.hasChildren(['userId', 'userEmail', 'itemName', 'category', 'timestamp'])"
    }
  }
}
```

### Input Validation
```kotlin
class InputValidator {
    fun validateEmail(email: String): Boolean {
        return android.util.Patterns.EMAIL_ADDRESS.matcher(email).matches()
    }
    
    fun validatePassword(password: String): Boolean {
        return password.length >= 6
    }
    
    fun validateItemName(name: String): Boolean {
        return name.trim().isNotEmpty() && name.length <= 100
    }
    
    fun sanitizeInput(input: String): String {
        return input.trim().replace(Regex("[<>\"'&]"), "")
    }
}
```

## 🧪 Testing

### Unit Tests
```kotlin
class InputValidatorTest {
    private val validator = InputValidator()
    
    @Test
    fun `validateEmail returns true for valid email`() {
        assertTrue(validator.validateEmail("test@example.com"))
    }
    
    @Test
    fun `validateEmail returns false for invalid email`() {
        assertFalse(validator.validateEmail("invalid-email"))
    }
}
```

### UI Tests
```kotlin
@Test
fun testLoginScreen() {
    composeTestRule.setContent {
        LostLKTheme {
            LoginScreen()
        }
    }
    
    composeTestRule.onNodeWithText("Email").performTextInput("test@example.com")
    composeTestRule.onNodeWithText("Password").performTextInput("password123")
    composeTestRule.onNodeWithText("Login").performClick()
    
    // Verify navigation or success state
}
```

### Integration Tests
```kotlin
@Test
fun testLostItemFlow() {
    // Test complete flow from login to item reporting
    // This would involve Firebase test environment
}
```

## 🚀 Performance Optimization

### Image Loading
```kotlin
@Composable
fun AsyncImage(
    imageUrl: String,
    contentDescription: String?,
    modifier: Modifier = Modifier
) {
    val imageLoader = ImageLoader.Builder(context)
        .crossfade(true)
        .build()
    
    AsyncImage(
        model = ImageRequest.Builder(context)
            .data(imageUrl)
            .crossfade(true)
            .build(),
        contentDescription = contentDescription,
        modifier = modifier,
        imageLoader = imageLoader
    )
}
```

### Database Optimization
```kotlin
class DatabaseManager {
    fun getLostItemsPaginated(
        limit: Int = 20,
        startAfter: String? = null
    ): Flow<List<LostItem>> {
        return flow {
            val query = if (startAfter != null) {
                database.child("lost_items")
                    .orderByKey()
                    .startAfter(startAfter)
                    .limitToFirst(limit)
            } else {
                database.child("lost_items")
                    .orderByKey()
                    .limitToFirst(limit)
            }
            
            // Execute query and emit results
        }
    }
}
```

### Memory Management
```kotlin
class LostItemRepository {
    private val _items = MutableStateFlow<List<LostItem>>(emptyList())
    val items: StateFlow<List<LostItem>> = _items.asStateFlow()
    
    fun clearCache() {
        _items.value = emptyList()
    }
    
    fun updateItem(item: LostItem) {
        _items.value = _items.value.map { 
            if (it.id == item.id) item else it 
        }
    }
}
```

## 🔧 Build Configuration

### Gradle Configuration
```kotlin
// app/build.gradle.kts
android {
    compileSdk = 34
    
    defaultConfig {
        applicationId = "fm.mrc.lostlk"
        minSdk = 24
        targetSdk = 34
        versionCode = 1
        versionName = "1.0.0"
    }
    
    buildTypes {
        release {
            isMinifyEnabled = true
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
    
    compileOptions {
        sourceCompatibility = JavaVersion.VERSION_17
        targetCompatibility = JavaVersion.VERSION_17
    }
    
    kotlinOptions {
        jvmTarget = "17"
    }
    
    buildFeatures {
        compose = true
    }
    
    composeOptions {
        kotlinCompilerExtensionVersion = "1.5.4"
    }
}
```

### Dependencies
```kotlin
dependencies {
    // Core Android
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.7.0")
    implementation("androidx.activity:activity-compose:1.8.2")
    
    // Compose BOM
    implementation(platform("androidx.compose:compose-bom:2024.02.00"))
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.ui:ui-graphics")
    implementation("androidx.compose.ui:ui-tooling-preview")
    implementation("androidx.compose.material3:material3")
    
    // Firebase
    implementation(platform("com.google.firebase:firebase-bom:32.7.0"))
    implementation("com.google.firebase:firebase-auth-ktx")
    implementation("com.google.firebase:firebase-database-ktx")
    
    // Google Maps
    implementation("com.google.android.gms:play-services-maps:18.2.0")
    implementation("com.google.android.gms:play-services-location:21.0.1")
    
    // Testing
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.test.ext:junit:1.1.5")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.5.1")
    androidTestImplementation(platform("androidx.compose:compose-bom:2024.02.00"))
    androidTestImplementation("androidx.compose.ui:ui-test-junit4")
    debugImplementation("androidx.compose.ui:ui-tooling")
    debugImplementation("androidx.compose.ui:ui-test-manifest")
}
```

## 📊 Analytics and Monitoring

### Firebase Analytics
```kotlin
class AnalyticsManager {
    fun logEvent(eventName: String, parameters: Bundle? = null) {
        FirebaseAnalytics.getInstance(context).logEvent(eventName, parameters)
    }
    
    fun logItemReported(itemCategory: String) {
        val bundle = Bundle().apply {
            putString("item_category", itemCategory)
        }
        logEvent("item_reported", bundle)
    }
    
    fun logUserLogin(method: String) {
        val bundle = Bundle().apply {
            putString("login_method", method)
        }
        logEvent("user_login", bundle)
    }
}
```

### Crash Reporting
```kotlin
class CrashReporter {
    fun logException(exception: Throwable) {
        FirebaseCrashlytics.getInstance().recordException(exception)
    }
    
    fun setUserId(userId: String) {
        FirebaseCrashlytics.getInstance().setUserId(userId)
    }
    
    fun setCustomKey(key: String, value: String) {
        FirebaseCrashlytics.getInstance().setCustomKey(key, value)
    }
}
```

## 🔄 CI/CD Pipeline

### GitHub Actions
```yaml
name: Android CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main, develop ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        java-version: '17'
        distribution: 'temurin'
        
    - name: Cache Gradle packages
      uses: actions/cache@v3
      with:
        path: |
          ~/.gradle/caches
          ~/.gradle/wrapper
        key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}
        
    - name: Run tests
      run: ./gradlew test
      
    - name: Run lint
      run: ./gradlew lint
      
    - name: Build debug APK
      run: ./gradlew assembleDebug
```

## 🐛 Debugging

### Logging
```kotlin
class Logger {
    companion object {
        private const val TAG = "LostLK"
        
        fun d(message: String) {
            Log.d(TAG, message)
        }
        
        fun e(message: String, throwable: Throwable? = null) {
            Log.e(TAG, message, throwable)
        }
        
        fun i(message: String) {
            Log.i(TAG, message)
        }
        
        fun w(message: String) {
            Log.w(TAG, message)
        }
    }
}
```

### Debug Tools
```kotlin
// Debug-only code
if (BuildConfig.DEBUG) {
    // Debug-specific functionality
    Logger.d("Debug mode enabled")
}
```

## 📚 Best Practices

### Code Organization
- **Single Responsibility**: Each class should have one responsibility
- **Dependency Injection**: Use dependency injection for better testability
- **Error Handling**: Implement proper error handling and user feedback
- **Resource Management**: Properly manage resources and memory

### Performance
- **Lazy Loading**: Load data only when needed
- **Caching**: Implement appropriate caching strategies
- **Image Optimization**: Optimize images for different screen densities
- **Database Queries**: Optimize database queries and use pagination

### Security
- **Input Validation**: Validate all user inputs
- **Authentication**: Implement proper authentication and authorization
- **Data Encryption**: Encrypt sensitive data
- **API Security**: Secure API endpoints and data transmission

### Testing
- **Unit Tests**: Write unit tests for business logic
- **Integration Tests**: Test component interactions
- **UI Tests**: Test user interface and user flows
- **Performance Tests**: Test app performance under load

## 🔗 Resources

### Documentation
- [Android Developer Guide](https://developer.android.com/guide)
- [Jetpack Compose](https://developer.android.com/jetpack/compose)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Google Maps Android SDK](https://developers.google.com/maps/documentation/android-sdk)

### Tools
- [Android Studio](https://developer.android.com/studio)
- [Firebase Console](https://console.firebase.google.com/)
- [Google Cloud Console](https://console.cloud.google.com/)
- [GitHub](https://github.com/)

### Community
- [Android Developers](https://developer.android.com/)
- [Kotlin](https://kotlinlang.org/)
- [Firebase Community](https://firebase.google.com/community)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/android)

---

**Happy coding!** 🚀

For questions or support, contact us at dev@lostlk.com or create an issue on GitHub.

**Last Updated**: September 2024
