# LostLK - Lost & Found Hub for Sri Lanka 🇱🇰

<div align="center">
  <img src="app/src/main/res/mipmap-hdpi/ic_launcher.png" alt="LostLK Logo" width="120" height="120">
  
  <h3>Find Your Lost Items Across Sri Lanka</h3>
  
  [![Android](https://img.shields.io/badge/Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com/)
  [![Kotlin](https://img.shields.io/badge/Kotlin-0095D5?style=for-the-badge&logo=kotlin&logoColor=white)](https://kotlinlang.org/)
  [![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/)
  [![Jetpack Compose](https://img.shields.io/badge/Jetpack%20Compose-4285F4?style=for-the-badge&logo=jetpackcompose&logoColor=white)](https://developer.android.com/jetpack/compose)
</div>

## 📱 About LostLK

LostLK is a comprehensive lost and found application designed specifically for Sri Lanka. It helps users report lost items, find them through a crowdsourced network, and provides real-time location-based services across the island.

### 🌟 Key Features

- **🗺️ Interactive Map**: View lost items across Sri Lanka with Google Maps integration
- **👤 User Authentication**: Secure login with Firebase Authentication
- **🔐 Admin Panel**: Dedicated admin interface for managing reports
- **📱 Modern UI**: Beautiful, responsive design with Jetpack Compose
- **🌐 Real-time Updates**: Live data synchronization with Firebase
- **📍 Location Services**: Precise location tracking for lost items
- **🎨 Material Design 3**: Modern, accessible user interface

## 🚀 Getting Started

### Prerequisites

- Android Studio Arctic Fox or later
- Android SDK 24 or higher
- Kotlin 1.8.0 or later
- Firebase project setup

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/LostLK.git
   cd LostLK
   ```

2. **Open in Android Studio**
   - Launch Android Studio
   - Select "Open an existing project"
   - Navigate to the cloned repository folder

3. **Firebase Setup**
   - Create a new Firebase project at [Firebase Console](https://console.firebase.google.com/)
   - Add your Android app to the Firebase project
   - Download `google-services.json` and place it in `app/` directory
   - Enable Firebase Authentication and Realtime Database

4. **Google Maps Setup**
   - Get a Google Maps API key from [Google Cloud Console](https://console.cloud.google.com/)
   - Add the API key to your `local.properties` file:
     ```properties
     MAPS_API_KEY=your_google_maps_api_key_here
     ```

5. **Build and Run**
   ```bash
   ./gradlew assembleDebug
   ```

## 🏗️ Project Structure

```
app/
├── src/main/java/fm/mrc/lostlk/
│   ├── models/
│   │   └── LostItemModel.kt          # Data models
│   ├── utils/
│   │   └── AdminManager.kt           # Admin utilities
│   ├── ui/theme/                     # UI theme and styling
│   ├── MainActivity.kt               # Main dashboard
│   ├── LoginActivity.kt              # User authentication
│   ├── RegisterActivity.kt           # User registration
│   ├── UserProfileActivity.kt        # User profile management
│   ├── ReportLostItemActivity.kt     # Lost item reporting
│   ├── MapViewActivity.kt            # Interactive map view
│   ├── AdminPanelActivity.kt         # Admin dashboard
│   └── ViewLostItemsActivity.kt      # Browse lost items
├── src/main/res/                     # Resources (layouts, drawables, etc.)
└── build.gradle.kts                  # App-level build configuration
```

## 🔧 Configuration

### Firebase Configuration

1. **Authentication Methods**
   - Email/Password authentication
   - Google Sign-In (optional)
   - Phone number authentication (optional)

2. **Realtime Database Rules**
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

### Admin Accounts

The app includes two pre-configured admin accounts:
- `admin1@lostlk.com` / `123123`
- `admin2@lostlk.com` / `123123`

Admin accounts have special privileges and red-themed UI elements.

## 📱 Screenshots

<div align="center">
  <img src="screenshots/login.png" alt="Login Screen" width="200">
  <img src="screenshots/home.png" alt="Home Screen" width="200">
  <img src="screenshots/map.png" alt="Map View" width="200">
  <img src="screenshots/profile.png" alt="User Profile" width="200">
</div>

## 🛠️ Technologies Used

- **Frontend**: Jetpack Compose, Material Design 3
- **Backend**: Firebase (Authentication, Realtime Database)
- **Maps**: Google Maps SDK for Android
- **Language**: Kotlin
- **Architecture**: MVVM with Compose
- **Build System**: Gradle with Kotlin DSL

## 🚧 Roadmap

### Current Features ✅
- [x] User authentication and registration
- [x] Lost item reporting system
- [x] Interactive map with markers
- [x] Admin panel for management
- [x] User profile management
- [x] Real-time data synchronization

### Upcoming Features 🔄
- [ ] Barcode scanning for item identification
- [ ] Push notifications for found items
- [ ] AI-powered image matching
- [ ] Premium features and rewards
- [ ] Offline support with Room database
- [ ] Multi-language support (Sinhala, Tamil)

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow Kotlin coding conventions
- Use meaningful commit messages
- Add comments for complex logic
- Test on multiple device sizes
- Ensure accessibility compliance

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👥 Team

- **Developer**: [Your Name]
- **Design**: [Designer Name]
- **Testing**: [Tester Name]

## 📞 Support

If you encounter any issues or have questions:

- 📧 Email: support@lostlk.com
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/LostLK/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/yourusername/LostLK/discussions)

## 🙏 Acknowledgments

- Firebase team for excellent backend services
- Google Maps team for mapping capabilities
- Android team for Jetpack Compose
- Open source community for inspiration

---

<div align="center">
  <p>Made with ❤️ for Sri Lanka</p>
  <p>🇱🇰 LostLK - Connecting People, Finding Items</p>
</div>
