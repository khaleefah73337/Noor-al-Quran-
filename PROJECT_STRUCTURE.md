# Noor al-Quran - Complete Project Structure

## 📂 Directory Layout

```
Noor-al-Quran-/
├── lib/
│   ├── main.dart                          # App entry point
│   ├── models/
│   │   ├── surah.dart                     # Surah data model
│   │   ├── ayah.dart                      # Ayah data model
│   │   ├── reciter.dart                   # Reciter model
│   │   ├── translation.dart               # Translation model
│   │   ├── bookmark.dart                  # Bookmark model
│   │   ├── note.dart                      # User note model
│   │   └── tasbih.dart                    # Tasbih counter model
│   │
│   ├── services/
│   │   ├── quran_service.dart             # Qur'an data service
│   │   ├── audio_service.dart             # Audio playback service
│   │   ├── download_service.dart          # Download management
│   │   ├── translation_service.dart       # Translation management
│   │   ├── bookmark_service.dart          # Bookmark storage
│   │   ├── firebase_service.dart          # Firebase integration
│   │   ├── storage_service.dart           # Local storage
│   │   └── admin_service.dart             # Admin operations
│   │
│   ├── screens/
│   │   ├── home_screen.dart               # Home/Dashboard
│   │   ├── surah_list_screen.dart         # All surahs list
│   │   ├── surah_detail_screen.dart       # Surah ayahs display
│   │   ├── ayah_player_screen.dart        # Audio player
│   │   ├── search_screen.dart             # Search functionality
│   │   ├── tasbih_screen.dart             # Tasbih counter
│   │   ├── bookmarks_screen.dart          # Saved bookmarks
│   │   ├── downloads_screen.dart          # Offline downloads
│   │   ├── settings_screen.dart           # App settings
│   │   ├── translations_screen.dart       # Translation selection
│   │   ├── admin_screen.dart              # Admin panel
│   │   └── juz_navigation_screen.dart     # Juz/Hizb selector
│   │
│   ├── widgets/
│   │   ├── quran_player.dart              # Audio player widget
│   │   ├── surah_card.dart                # Surah list item
│   │   ├── ayah_view.dart                 # Single ayah display
│   │   ├── translation_toggle.dart        # Translation switcher
│   │   ├── bookmark_button.dart           # Bookmark action button
│   │   ├── search_bar.dart                # Search input
│   │   ├── juz_selector.dart              # Juz/Hizb dropdown
│   │   ├── reciter_selector.dart          # Reciter picker
│   │   ├── playback_speed_slider.dart     # Speed control
│   │   ├── progress_indicator.dart        # Playback progress
│   │   └── admin_card.dart                # Admin panel cards
│   │
│   ├── providers/                         # Riverpod providers
│   │   ├── quran_provider.dart
│   │   ├── audio_provider.dart
│   │   ├── bookmark_provider.dart
│   │   ├── translation_provider.dart
│   │   ├── download_provider.dart
│   │   └── admin_provider.dart
│   │
│   ├── utils/
│   │   ├── constants.dart                 # App constants
│   │   ├── colors.dart                    # Color palette
│   │   ├── strings.dart                   # UI strings
│   │   ├── extensions.dart                # String/Date extensions
│   │   ├── validators.dart                # Form validators
│   │   └── helpers.dart                   # Helper functions
│   │
│   └── config/
│       ├── routes.dart                    # Navigation routes
│       ├── theme.dart                     # App theme
│       └── firebase_config.dart           # Firebase setup
│
├── assets/
│   ├── quran/
│   │   ├── uthmani_text.json              # ✅ All 6,236 ayahs (Uthmani)
│   │   ├── translations_en.json           # English translations
│   │   ├── translations_ur.json           # Urdu translations
│   │   ├── translations_fr.json           # French translations
│   │   ├── translations_ha.json           # Hausa translations
│   │   ├── translations_id.json           # Indonesian translations
│   │   └── translations_tr.json           # Turkish translations
│   │
│   ├── audio/
│   │   ├── mishary_alafasy/
│   │   ├── abdul_basit/
│   │   ├── abdul_rahman_al_sudais/
│   │   ├── mahmoud_al_husary/
│   │   ├── saad_al_ghamdi/
│   │   ├── abu_bakr_al_shatri/
│   │   └── maher_al_muaiqly/
│   │
│   ├── images/
│   │   ├── app_logo.png
│   │   ├── splash_screen.png
│   │   └── reciter_avatars/
│   │
│   ├── icons/
│   │   ├── play.svg
│   │   ├── pause.svg
│   │   ├── bookmark.svg
│   │   └── more_icons/
│   │
│   ├── fonts/
│   │   ├── ArabicFont.ttf                 # For Arabic text
│   │   └── EnglishFont.ttf
│   │
│   └── translations/
│       ├── en.json
│       ├── ar.json
│       └── more_languages/
│
├── test/
│   ├── models/
│   ├── services/
│   ├── widgets/
│   └── integration_test/
│
├── firebase/
│   ├── firestore.rules
│   ├── storage.rules
│   └── functions/
│       ├── manage_reciters.js
│       ├── manage_audio.js
│       └── admin_operations.js
│
├── .github/
│   └── workflows/
│       ├── flutter_test.yml
│       ├── flutter_build.yml
│       └── firebase_deploy.yml
│
├── android/
├── ios/
├── windows/
├── macos/
├── web/
│
├── README.md                              # ✅ Main project README
├── PHASE_2A_REQUIREMENTS.md               # ✅ Phase 2A detailed requirements
├── PHASE_2B_REQUIREMENTS.md               # Phase 2B (Reciters)
├── PHASE_2C_REQUIREMENTS.md               # Phase 2C (Downloads)
├── PHASE_2D_REQUIREMENTS.md               # Phase 2D (Translations)
├── PHASE_2E_REQUIREMENTS.md               # Phase 2E (Bookmarks/Notes)
├── PHASE_2F_REQUIREMENTS.md               # Phase 2F (Admin System)
├── PHASE_2G_REQUIREMENTS.md               # Phase 2G (Final Polish)
│
├── pubspec.yaml                           # ✅ Dependencies
├── pubspec.lock
├── .gitignore
├── .env.example
└── CONTRIBUTING.md

```

## 🎯 Phase Breakdown

### ✅ Phase 2A: Complete Qur'an + Arabic Text
- All 114 Surahs data
- Verified Uthmani text (6,236 ayahs)
- Surah metadata (Meccan/Medinan, ayah count)
- Search functionality
- Juz/Hizb navigation

### Phase 2B: Multiple Reciters + Audio
- 7 professional reciters
- Audio streaming/playback
- Reciter selection per surah
- Audio file management

### Phase 2C: Offline Downloads
- Download management system
- Local storage tracking
- Offline playback

### Phase 2D: Translations
- 6 languages supported
- Toggle translations on/off
- Translation switching

### Phase 2E: Bookmarks & Notes
- Bookmark ayahs
- Add/edit/delete notes
- Categorized collections
- Export features

### Phase 2F: Admin System
- Firebase admin panel
- Audio management
- Reciter management
- Audio replacement system

### Phase 2G: Final Polish
- Performance optimization
- Bug fixes
- UI refinements
- Final APK build

---

## 🔧 Key Implementation Details

### Database Schema (SQLite)

**Tables:**
- `surahs` - Surah metadata
- `ayahs` - Individual ayah texts
- `reciters` - Reciter information
- `bookmarks` - User bookmarks
- `notes` - User notes
- `downloads` - Downloaded files metadata
- `search_index` - Full-text search index

### API Integrations

- **Quran.com API** - Fallback data source
- **Firebase Firestore** - Admin data
- **Firebase Storage** - Audio files
- **Firebase Auth** - Admin authentication

### State Management

- **Riverpod** for data providers
- **Provider** for UI state
- **Local SQLite** for offline data
- **SharedPreferences** for app settings

### Audio Architecture

- **just_audio** for playback
- **audio_session** for background playback
- **Local caching** for downloaded content
- **Stream support** for remote audio

---

## 📊 Data Statistics

| Metric | Value |
|--------|-------|
| Total Surahs | 114 |
| Total Ayahs | 6,236 |
| Meccan Surahs | 86 |
| Medinan Surahs | 28 |
| Juzs | 30 |
| Hizbhs | 60 |
| Reciters | 7 |
| Languages | 6 |

---

**Last Updated**: August 25, 2026  
**Status**: Phase 2A in progress
