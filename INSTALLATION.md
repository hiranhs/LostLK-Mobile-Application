# Installation Guide for LostLK

This guide will help you set up LostLK for development and deployment.

## Prerequisites

### System Requirements
- **Operating System**: Windows 10/11, macOS 10.15+, or Linux Ubuntu 18.04+
- **RAM**: Minimum 8GB, Recommended 16GB
- **Storage**: At least 10GB free space
- **Internet**: Stable internet connection

### Software Requirements
- **Android Studio**: Arctic Fox (2020.3.1) or later
- **Java Development Kit (JDK)**: Version 17 or later
- **Android SDK**: API level 24 (Android 7.0) or higher
- **Kotlin**: Version 1.8.0 or later
- **Gradle**: Version 8.4 or later

### Accounts Required
- **Google Account**: For Firebase and Google Maps
- **GitHub Account**: For version control (optional)
- **Firebase Account**: For backend services

## Step 1: Install Android Studio

### Download Android Studio
1. Go to [Android Studio Download](https://developer.android.com/studio)
2. Download the latest version for your operating system
3. Run the installer and follow the setup wizard

### Configure Android Studio
1. **Install SDK Components**:
   - Android SDK Platform 24 (Android 7.0)
   - Android SDK Platform 34 (Android 14)
   - Android SDK Build-Tools
   - Android SDK Platform-Tools
   - Android SDK Tools

2. **Set up Emulator**:
   - Create a virtual device (AVD)
   - Choose a device definition (Pixel 4 or similar)
   - Download a system image (API 30 or higher)

## Step 2: Clone the Repository

### Using Git (Recommended)
```bash
# Clone the repository
git clone https://github.com/yourusername/LostLK.git

# Navigate to the project directory
cd LostLK

# Check out the latest release
git checkout main
```

### Download ZIP (Alternative)
1. Go to the [GitHub repository](https://github.com/yourusername/LostLK)
2. Click "Code" > "Download ZIP"
3. Extract the ZIP file to your desired location

## Step 3: Set Up Firebase

### Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project"
3. Enter project name: "LostLK"
4. Enable Google Analytics (optional)
5. Create the project

### Configure Authentication
1. In Firebase Console, go to "Authentication"
2. Click "Get started"
3. Go to "Sign-in method" tab
4. Enable "Email/Password" authentication
5. Save the configuration

### Set Up Realtime Database
1. Go to "Realtime Database"
2. Click "Create Database"
3. Choose "Start in test mode" (for development)
4. Select a location (choose closest to your users)
5. Create the database

### Configure Database Rules
Replace the default rules with:
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

### Add Android App to Firebase
1. In Firebase Console, click "Add app" > "Android"
2. Enter package name: `fm.mrc.lostlk`
3. Enter app nickname: "LostLK"
4. Download `google-services.json`
5. Place the file in `app/` directory

## Step 4: Set Up Google Maps

### Get Google Maps API Key
1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Enable "Maps SDK for Android" API
4. Go to "Credentials" > "Create Credentials" > "API Key"
5. Copy the API key

### Configure API Key
1. Create `local.properties` file in the project root
2. Add your API key:
```properties
MAPS_API_KEY=your_google_maps_api_key_here
```

### Restrict API Key (Recommended)
1. In Google Cloud Console, go to "Credentials"
2. Click on your API key
3. Under "Application restrictions", select "Android apps"
4. Add your package name: `fm.mrc.lostlk`
5. Add your SHA-1 fingerprint (get from Android Studio)

## Step 5: Build and Run

### Open Project in Android Studio
1. Launch Android Studio
2. Click "Open an existing project"
3. Navigate to the LostLK directory
4. Click "OK"

### Sync Project
1. Android Studio will automatically sync the project
2. Wait for Gradle sync to complete
3. Resolve any dependency issues if prompted

### Build the Project
```bash
# Clean the project
./gradlew clean

# Build debug version
./gradlew assembleDebug

# Build release version
./gradlew assembleRelease
```

### Run on Device/Emulator
1. Connect your Android device or start an emulator
2. Click "Run" button in Android Studio
3. Select your device/emulator
4. Wait for the app to install and launch

## Step 6: Verify Installation

### Test Basic Functionality
1. **Create Account**: Register a new user account
2. **Login**: Test login functionality
3. **Report Item**: Create a lost item report
4. **View Map**: Check if map loads correctly
5. **Admin Panel**: Test admin functionality (if applicable)

### Check Logs
1. Open "Logcat" in Android Studio
2. Look for any error messages
3. Verify Firebase connection
4. Check Google Maps integration

## Troubleshooting

### Common Issues

#### Gradle Sync Failed
```bash
# Clean and rebuild
./gradlew clean
./gradlew build

# Invalidate caches in Android Studio
File > Invalidate Caches and Restart
```

#### Firebase Connection Issues
- Verify `google-services.json` is in the correct location
- Check Firebase project configuration
- Ensure internet connection is stable
- Verify package name matches Firebase configuration

#### Google Maps Not Loading
- Check API key in `local.properties`
- Verify Maps SDK is enabled in Google Cloud Console
- Check API key restrictions
- Ensure device has internet connection

#### Build Errors
- Update Android Studio to latest version
- Update SDK components
- Check Kotlin version compatibility
- Verify all dependencies are resolved

### Performance Issues
- Increase heap size in `gradle.properties`:
```properties
org.gradle.jvmargs=-Xmx4g -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
```

### Memory Issues
- Close unnecessary applications
- Increase available RAM
- Use 64-bit Android Studio
- Enable hardware acceleration

## Development Setup

### Recommended Plugins
- **Kotlin**: Built-in
- **Android Material Design**: Built-in
- **Firebase**: Built-in
- **Git Integration**: Built-in

### Code Style
- Follow Kotlin coding conventions
- Use meaningful variable names
- Add comments for complex logic
- Follow Material Design guidelines

### Testing
- Run unit tests: `./gradlew test`
- Run instrumented tests: `./gradlew connectedAndroidTest`
- Check code coverage: `./gradlew jacocoTestReport`

## Production Deployment

### Build Release APK
```bash
# Generate signed APK
./gradlew assembleRelease

# APK will be in app/build/outputs/apk/release/
```

### Build AAB (Android App Bundle)
```bash
# Generate AAB for Play Store
./gradlew bundleRelease

# AAB will be in app/build/outputs/bundle/release/
```

### Signing Configuration
1. Generate keystore:
```bash
keytool -genkey -v -keystore lostlk-release-key.keystore -alias lostlk -keyalg RSA -keysize 2048 -validity 10000
```

2. Configure signing in `app/build.gradle.kts`:
```kotlin
android {
    signingConfigs {
        create("release") {
            storeFile = file("lostlk-release-key.keystore")
            storePassword = "your_store_password"
            keyAlias = "lostlk"
            keyPassword = "your_key_password"
        }
    }
    
    buildTypes {
        release {
            signingConfig = signingConfigs.getByName("release")
            isMinifyEnabled = true
            proguardFiles(getDefaultProguardFile("proguard-android-optimize.txt"), "proguard-rules.pro")
        }
    }
}
```

## Environment Variables

### Required Environment Variables
```bash
# Firebase
FIREBASE_PROJECT_ID=your_project_id
FIREBASE_API_KEY=your_api_key

# Google Maps
GOOGLE_MAPS_API_KEY=your_maps_api_key

# Database
DATABASE_URL=your_database_url
```

### Optional Environment Variables
```bash
# Analytics
ANALYTICS_ENABLED=true
CRASHLYTICS_ENABLED=true

# Features
BARCODE_SCANNING_ENABLED=false
PUSH_NOTIFICATIONS_ENABLED=false
```

## Security Considerations

### API Key Security
- Never commit API keys to version control
- Use `local.properties` for local development
- Use environment variables for CI/CD
- Restrict API keys in Google Cloud Console

### Firebase Security
- Configure proper security rules
- Use authentication for all database access
- Validate data on both client and server
- Regular security audits

### Code Obfuscation
- Enable ProGuard for release builds
- Obfuscate sensitive code
- Remove debug information
- Use code signing

## Support and Resources

### Documentation
- [Android Developer Guide](https://developer.android.com/guide)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Google Maps Documentation](https://developers.google.com/maps/documentation)

### Community
- [GitHub Issues](https://github.com/yourusername/LostLK/issues)
- [GitHub Discussions](https://github.com/yourusername/LostLK/discussions)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/android)

### Contact
- **Email**: support@lostlk.com
- **GitHub**: [@yourusername](https://github.com/yourusername)

---

**Need help?** Contact us at support@lostlk.com or create an issue on GitHub.

**Last Updated**: September 2024
