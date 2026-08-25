# Phase 2A: Complete Qur'an (114 Surahs + Arabic Text)

## Objective
Implement the complete Qur'an with all 114 Surahs featuring verified Uthmani Arabic text, metadata, and search/navigation capabilities.

## 📋 Tasks

### 1. Data Structure Setup
- [ ] Create `Surah` model with:
  - Surah number (1-114)
  - Arabic name
  - English name
  - Translation name
  - Revelation type (Meccan/Medinan)
  - Number of ayahs
  - Juz numbers
  - Hizb numbers
  
- [ ] Create `Ayah` model with:
  - Surah ID
  - Ayah number
  - Uthmani Arabic text
  - English translation (placeholder)
  - Audio URLs (prepared for Phase 2B)

### 2. Qur'an Data Acquisition
- [ ] Source verified Uthmani text (Quran.com API or local JSON)
- [ ] Extract surah metadata (revelation type, ayah count)
- [ ] Compile Juz/Hizb boundary data
- [ ] Create `assets/quran/uthmani_text.json` with all 6,236 ayahs

### 3. Database Implementation
- [ ] Setup SQLite with:
  - `surahs` table
  - `ayahs` table
  - `search_index` for full-text search
  
- [ ] Seed database with all Qur'an data on first launch

### 4. Search Feature
- [ ] Implement search by:
  - Surah name (Arabic/English)
  - Surah number
  - Ayah keyword (Arabic text)
  - Juz number
  - Hizb number
  
- [ ] Create `SearchService` class
- [ ] Build search UI with results display

### 5. Navigation Features
- [ ] **Surah List Screen**
  - Display all 114 surahs
  - Show: number, name, revelation type, ayah count
  - Tap to view ayahs
  
- [ ] **Ayah View**
  - Display selected surah's ayahs
  - Show Arabic text with diacritics
  - Display ayah number
  - Show surah name/number at top
  
- [ ] **Juz/Hizb Navigation**
  - Create selector for 30 Juzs
  - Create selector for 60 Hizbhs
  - Jump to start of selected Juz/Hizb

### 6. Data Files to Create

#### `assets/quran/uthmani_text.json`
```json
{
  "surahs": [
    {
      "number": 1,
      "name_arabic": "الفاتحة",
      "name_english": "Al-Fatiha",
      "revelation_type": "Meccan",
      "ayah_count": 7,
      "juzs": [1],
      "hizbhs": [1],
      "ayahs": [
        {
          "number": 1,
          "text_uthmani": "بِسْمِ اللَّهِ الرَّحْمَٰنِ الرَّحِيمِ"
        },
        ...
      ]
    },
    ...
  ]
}
```

### 7. Flutter Implementation

#### Models (`lib/models/`)
```dart
// surah.dart
class Surah {
  final int number;
  final String nameArabic;
  final String nameEnglish;
  final String revelationType; // Meccan/Medinan
  final int ayahCount;
  final List<int> juzs;
  final List<int> hizbhs;
}

// ayah.dart
class Ayah {
  final int surahNumber;
  final int number;
  final String textUthmani;
  final String? audioUrl;
}
```

#### Services (`lib/services/`)
```dart
// quran_service.dart
class QuranService {
  Future<void> initializeDatabase() { }
  Future<List<Surah>> getAllSurahs() { }
  Future<Surah> getSurah(int number) { }
  Future<List<Ayah>> getAyahs(int surahNumber) { }
  Future<List<SearchResult>> search(String query) { }
  Future<List<Ayah>> getJuz(int juzNumber) { }
  Future<List<Ayah>> getHizb(int hizbNumber) { }
}
```

#### Screens (`lib/screens/`)
- `surah_list_screen.dart` - Display all 114 surahs
- `surah_detail_screen.dart` - Display ayahs for selected surah
- `search_screen.dart` - Search functionality
- `juz_navigation_screen.dart` - Juz/Hizb selector

### 8. Testing Checklist
- [ ] All 114 surahs load correctly
- [ ] Uthmani text displays with proper diacritics
- [ ] Search returns accurate results
- [ ] Juz/Hizb navigation works correctly
- [ ] Database seeds only on first launch
- [ ] Ayah numbers match official Qur'an
- [ ] Revelation types are accurate
- [ ] Performance: Load 1000+ ayahs smoothly

### 9. Resources
- **Quran.com API**: https://api.quran.com/
- **Verified Uthmani Text**: quran-json GitHub repo
- **Juz/Hizb Boundaries**: Islamic databases
- **Islamic Data Standards**: https://github.com/risan/quran-json

## 📊 Deliverables
- ✅ Complete Qur'an database (6,236 ayahs)
- ✅ Functional search feature
- ✅ Juz/Hizb navigation
- ✅ Surah list with metadata
- ✅ Ayah view with Arabic text
- ✅ Unit & integration tests

## 🎯 Success Criteria
- User can browse all 114 surahs
- User can view any ayah with proper Arabic text
- Search returns results within 500ms
- Navigation is smooth and intuitive
- Zero data corruption issues
- Proper Islamic text formatting (RTL support)

## 📝 Notes
- Use proper RTL (Right-to-Left) text direction for Arabic
- Maintain Islamic authenticity in all data
- Ensure no typos in Qur'an text (use verified sources)
- Prepare audio URL structure for Phase 2B

---
**Phase**: 2A  
**Priority**: Critical  
**Estimated Time**: 2-3 weeks
