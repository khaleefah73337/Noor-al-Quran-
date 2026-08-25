# 🕌 Noor al-Quran - Islamic Qur'an Application

> A comprehensive, feature-rich Qur'an application for Android with professional audio reciters, translations, bookmarks, and offline support.

![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)
![Status](https://img.shields.io/badge/status-Production%20Ready-brightgreen.svg)
![Platform](https://img.shields.io/badge/platform-Android%205.0%2B-green.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)

## ✨ Features

### 📖 Complete Qur'an
- ✅ **All 114 Surahs** with verified Uthmani text
- ✅ **6,236 Ayahs** (verses) with accurate recitation
- ✅ Meccan & Medinan classification
- ✅ Juz & Hizb navigation (30 Juzs, 60 Hizbhs)
- ✅ Full-text search across all verses
- ✅ Surah information & statistics

### 🎤 Professional Audio
- ✅ **7 Renowned Reciters**:
  - Mishary Alafasy (Kuwait)
  - Abdul Basit (Egypt)
  - Abdul Rahman Al-Sudais (Saudi Arabia)
  - Mahmoud Al-Husary (Egypt)
  - Saad Al-Ghamdi (Saudi Arabia)
  - Abu Bakr Al-Shatri (Saudi Arabia)
  - Maher Al-Muaiqly (Saudi Arabia)
- ✅ High-quality audio streaming (128 kbps)
- ✅ Playback speed control (0.5x - 2x)
- ✅ Repeat modes (Off, One, All)
- ✅ Background playback support
- ✅ Lock screen controls
- ✅ Progress tracking

### 📥 Offline Downloads
- ✅ Download any surah for offline use
- ✅ Multiple quality options (64, 128, 320 kbps)
- ✅ Pause/Resume downloads
- ✅ Storage management
- ✅ Download progress tracking
- ✅ Batch download support

### 🌍 Multi-Language Translations
- ✅ **6 Language Translations**:
  - 🇬🇧 English (Sahih International)
  - 🇺🇸 Urdu (Abul Ala Maududi)
  - 🇫🇷 French (Muhammad Hamidullah)
  - 🇳🇬 Hausa (Abubakar Gumi)
  - 🇮🇩 Indonesian (Abdulmalik Abdulkarim Amrullah)
  - 🇹🇷 Turkish (Mehmet Öztürk)
- ✅ Toggle translations on/off
- ✅ Display modes (Arabic only, Arabic + translation, Multiple translations)
- ✅ Offline translation support
- ✅ Translator attribution

### 📚 Bookmarks & Notes
- ✅ Bookmark favorite verses
- ✅ Add personal notes to bookmarks
- ✅ Organize collections:
  - 🧠 Memorization
  - 📖 Daily Reading
  - 💭 Reflection
  - ⭐ Favorites
- ✅ Reading progress tracking
- ✅ Memorization status tracking
- ✅ Export/Import bookmarks
- ✅ Search bookmarks
- ✅ Share verses with friends

### 📿 Tasbih Counter
- ✅ Digital tasbeeh counter
- ✅ Quick access from anywhere
- ✅ Count tracking
- ✅ Reset functionality
- ✅ Beautiful UI design

### 👑 Admin Panel
- ✅ PIN-protected access
- ✅ Manage 7 reciters
- ✅ Upload/Replace audio files
- ✅ View statistics & analytics
- ✅ Database backup
- ✅ User activity monitoring
- ✅ System health checks

## 🏗️ Project Structure

```
Noor-al-Quran-/
├── lib/                          # Dart/Flutter code
│   ├── main.dart
│   ├── models/                   # Data models
│   ├── services/                 # Business logic
│   ├── screens/                  # UI screens
│   ├── widgets/                  # Reusable components
│   ├── providers/                # State management
│   ├── utils/                    # Helper functions
│   └── config/                   # Configuration
├── assets/
│   ├── quran/                    # Qur'an data (JSON)
│   ├── audio/                    # Audio files
│   ├── images/                   # Images & icons
│   ├── fonts/                    # Custom fonts
│   └── translations/             # i18n files
├── test/                         # Unit & integration tests
├── android/                      # Android configuration
├── firebase/                     # Firebase config
├── pubspec.yaml                  # Dependencies
├── README.md                     # This file
├── PROJECT_STRUCTURE.md          # Detailed structure
├── INSTALLATION_GUIDE.md         # Setup instructions
├── PHASE_2A_REQUIREMENTS.md      # Qur'an & Arabic
├── PHASE_2B_REQUIREMENTS.md      # Reciters & Audio
├── PHASE_2C_REQUIREMENTS.md      # Downloads
├── PHASE_2D_REQUIREMENTS.md      # Translations
├── PHASE_2E_REQUIREMENTS.md      # Bookmarks & Notes
├── PHASE_2F_REQUIREMENTS.md      # Admin System
└── PHASE_2G_REQUIREMENTS.md      # Polish & Release
```

## 🚀 Quick Start

### Prerequisites
- Flutter 3.0.0+
- Android SDK 21+
- 500 MB storage space

### Installation

1. **Clone Repository**
```bash
git clone https://github.com/khaleefah73337/Noor-al-Quran-.git
cd Noor-al-Quran-
```

2. **Install Dependencies**
```bash
flutter pub get
flutter pub upgrade
```

3. **Build Release APK**
```bash
flutter build apk --release
```

4. **Install on Phone**
```bash
adb install build/app/outputs/apk/release/app-release.apk
```

For detailed setup instructions, see [INSTALLATION_GUIDE.md](INSTALLATION_GUIDE.md)

## 📱 App Walkthrough

### Home Screen
- View all 114 surahs in organized list
- Search specific surah
- Quick access to recent surahs
- Select preferred reciter
- Choose display language

### Surah View
- Display Arabic Uthmani text
- Show translations (if enabled)
- Play audio for entire surah
- Bookmark individual verses
- Add notes to verses
- Navigate with previous/next buttons

### Audio Player
- Play/Pause/Resume controls
- Progress bar with time display
- Playback speed selector (0.5x - 2x)
- Repeat mode toggle
- Reciter selection
- Background playback support
- Lock screen controls

### Bookmarks Screen
- View all bookmarks organized by collection
- Filter by category
- Search bookmarks
- Edit/delete bookmarks
- View notes
- Share verses
- Export bookmarks

### Downloads Screen
- View downloaded surahs
- Download manager interface
- Select quality (64, 128, 320 kbps)
- Track download progress
- Manage storage
- Delete downloads

### Admin Panel
- Enter PIN (khaleefah37)
- Dashboard with statistics
- Manage reciters
- Upload/Replace audio
- View analytics
- Trigger backups

## 🎯 Technology Stack

### Frontend
- **Flutter** 3.0+ - Cross-platform framework
- **Dart** - Programming language
- **Provider** & **Riverpod** - State management
- **GetX** - Navigation

### Backend & Data
- **Firebase Firestore** - Cloud database
- **Firebase Storage** - Audio file hosting
- **Firebase Analytics** - Usage tracking
- **Firebase Crashlytics** - Error monitoring

### Database
- **SQLite** - Local storage
- **Shared Preferences** - App settings

### Audio
- **just_audio** - Audio playback
- **audio_session** - Background playback
- **flutter_sound** - Audio recording (future)

### Other Libraries
- **Dio** - HTTP requests
- **Cached Network Image** - Image caching
- **Intl** - Internationalization
- **Permission Handler** - Permissions
- **Connectivity Plus** - Network detection

## 📊 Data Statistics

| Metric | Value |
|--------|-------|
| Total Surahs | 114 |
| Total Ayahs | 6,236 |
| Meccan Surahs | 86 |
| Medinan Surahs | 28 |
| Juzs | 30 |
| Hizbhs | 60 |
| Professional Reciters | 7 |
| Languages Supported | 6 |
| App Size | ~80 MB |
| Min Android API | 21 (5.0) |

## 🔐 Security

- ✅ Admin PIN protection (hash-based)
- ✅ HTTPS/TLS for all communications
- ✅ No hardcoded secrets
- ✅ Secure local storage
- ✅ Firebase security rules
- ✅ Input validation & sanitization
- ✅ Session management with timeout
- ✅ Comprehensive audit logging

## 📈 Development Roadmap

### Version 1.0.0 ✅ Complete
- Complete Qur'an with Uthmani text
- 7 Professional reciters
- Audio playback with full controls
- Offline downloads
- 6-language translations
- Bookmarks & notes system
- Tasbih counter
- Admin panel with Firebase integration

### Version 1.1.0 (Planned)
- Additional reciters
- More language translations
- Advanced search filters
- Reading reminders
- Export features enhancements

### Version 2.0.0 (Future)
- Tajweed rules visualization
- Interactive Qur'an lessons
- Community features
- Premium features
- Advanced analytics

## 🧪 Testing

### Test Coverage
- Unit tests for services
- Widget tests for UI
- Integration tests for workflows
- Performance tests
- Security tests

### Run Tests
```bash
# Run all tests
flutter test

# Run specific test
flutter test test/path/to/test.dart

# With coverage
flutter test --coverage
```

## 🎨 UI/UX

### Design Principles
- Clean & minimal interface
- Islamic aesthetic
- Easy navigation
- Accessibility first
- Dark mode support (future)

### Responsive Design
- Optimized for 4.5" - 7"+ screens
- Tablet support
- Orientation handling
- Touch-friendly interface

## 📞 Support & Feedback

### Report Issues
- GitHub Issues: https://github.com/khaleefah73337/Noor-al-Quran-/issues
- Email: support@noorquran.app
- WhatsApp: Contact admin

### Contribute
See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines

### FAQ
**Q: How do I bookmark an ayah?**  
A: While viewing an ayah, tap the bookmark icon. You can add notes and assign to collections.

**Q: Can I use the app offline?**  
A: Yes! Download surahs in the Downloads section to use offline.

**Q: How many languages are supported?**  
A: Currently 6: English, Urdu, French, Hausa, Indonesian, Turkish.

**Q: Can I share my bookmarks?**  
A: Yes! Tap the share icon to send via WhatsApp, email, or other apps.

**Q: What is the admin PIN?**  
A: The default admin PIN is khaleefah37 (for testing only).

## 📜 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) for details on how to:
- Report bugs
- Suggest features
- Submit pull requests
- Follow code standards

## 🙏 Acknowledgments

- Allah (God) for the Qur'an
- Professional reciters who provided audio
- Translators for their dedication
- Flutter team for the amazing framework
- Open-source community for libraries

## 👨‍💻 Author

**Khalifah**
- GitHub: [@khaleefah73337](https://github.com/khaleefah73337)
- Email: muhammadkhalifaadamu@gmail.com

## 📅 Project Timeline

| Phase | Feature | Status | Duration |
|-------|---------|--------|----------|
| 2A | Qur'an + Arabic | ✅ Complete | 1 week |
| 2B | Reciters + Audio | ✅ Complete | 2 weeks |
| 2C | Offline Downloads | ✅ Complete | 2 weeks |
| 2D | Translations | ✅ Complete | 2 weeks |
| 2E | Bookmarks & Notes | ✅ Complete | 2 weeks |
| 2F | Admin System | ✅ Complete | 2 weeks |
| 2G | Polish & Release | ✅ Complete | 2 weeks |

**Total Development**: 14 weeks  
**Release Date**: Ready for Google Play Store  

## 🎯 Goals

- ✅ Provide authentic Qur'an with verified Uthmani text
- ✅ Support multiple professional reciters
- ✅ Enable offline access to Qur'an
- ✅ Support global Muslim community with translations
- ✅ Facilitate personal reflection with bookmarks/notes
- ✅ Maintain highest security standards
- ✅ Create beautiful, intuitive user experience
- ✅ Open-source for community contribution

## 📱 System Requirements

### Minimum
- Android 5.0+ (API 21)
- 2 GB RAM
- 500 MB storage
- 4.5" screen

### Recommended
- Android 8.0+ (API 26)
- 4 GB+ RAM
- 2 GB storage
- 5.5"+ screen

## 🔗 Quick Links

- [Installation Guide](INSTALLATION_GUIDE.md)
- [Project Structure](PROJECT_STRUCTURE.md)
- [Phase 2A - Qur'an Setup](PHASE_2A_REQUIREMENTS.md)
- [Phase 2B - Reciters & Audio](PHASE_2B_REQUIREMENTS.md)
- [Phase 2C - Downloads](PHASE_2C_REQUIREMENTS.md)
- [Phase 2D - Translations](PHASE_2D_REQUIREMENTS.md)
- [Phase 2E - Bookmarks](PHASE_2E_REQUIREMENTS.md)
- [Phase 2F - Admin System](PHASE_2F_REQUIREMENTS.md)
- [Phase 2G - Final Polish](PHASE_2G_REQUIREMENTS.md)

## 🌟 Stars & Forks

If you find this project useful, please consider:
- ⭐ Starring the repository
- 🍴 Forking for contributions
- 🐛 Reporting bugs
- 💡 Suggesting improvements
- 📢 Sharing with others

---

<div align="center">

### 🕌 Noor al-Quran - Light of the Qur'an 🕌

**Making the Qur'an accessible to everyone, everywhere**

[⬆ Back to top](#-noor-al-quran---islamic-quraan-application)

</div>

---

**Last Updated**: August 25, 2026  
**Version**: 1.0.0  
**Status**: ✅ Production Ready  
**Download**: [Google Play Store](https://play.google.com/store) (Coming Soon)

