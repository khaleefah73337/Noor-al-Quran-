# Phase 2D: Translations

## 🌍 Objective
Add support for 6 languages with easy toggle functionality, allowing users to read Qur'an in their preferred language.

## 📋 Tasks

### 1. Translation Service

Create `TranslationService`:
```dart
class TranslationService {
  // Initialize translations
  Future<void> initialize() { }
  
  // Get available translations
  Future<List<Translation>> getAvailableTranslations() { }
  
  // Load specific translation
  Future<void> loadTranslation(String languageCode) { }
  
  // Get translation for ayah
  Future<String> getTranslation(int surahNumber, int ayahNumber, String languageCode) { }
  
  // Get all translations for surah
  Future<Map<String, List<String>>> getSurahTranslations(
    int surahNumber,
    List<String> languageCodes,
  ) { }
  
  // Set default translation
  Future<void> setDefaultTranslation(String languageCode) { }
  
  // Get default translation
  Future<String> getDefaultTranslation() { }
  
  // Toggle translation visibility
  Future<void> toggleTranslation(String languageCode) { }
  
  // Search in translations
  Future<List<SearchResult>> searchTranslation(String query, String languageCode) { }
  
  // Offline translation availability check
  Future<bool> isTranslationAvailable(String languageCode) { }
  
  // Download translation for offline
  Future<void> downloadTranslation(String languageCode) { }
}
```

### 2. Translation Data Models

**Translation Model:**
```dart
class Translation {
  final String languageCode; // en, ur, fr, ha, id, tr
  final String languageName; // English, Urdu, etc.
  final String nativeLanguageName; // اردو, Français, etc.
  final String translatorName;
  final String translatorBio;
  final String sourceUrl;
  final DateTime dateAdded;
  final bool isOfflineAvailable;
  final double fileSize; // MB
}
```

**Ayah Translation Model:**
```dart
class AyahTranslation {
  final int surahNumber;
  final int ayahNumber;
  final String languageCode;
  final String translationText;
  final String? footnote;
  final String? wordByWordTranslation;
}
```

### 3. Supported Languages

| # | Language | Code | Translator | File Size |
|---|----------|------|-----------|-----------|
| 1 | English | en | Sahih International | ~8 MB |
| 2 | Urdu | ur | Abul Ala Maududi | ~7 MB |
| 3 | French | fr | Muhammad Hamidullah | ~10 MB |
| 4 | Hausa | ha | Abubakar Gumi | ~6 MB |
| 5 | Indonesian | id | Abdulmalik Abdulkarim Amrullah | ~9 MB |
| 6 | Turkish | tr | Mehmet Öztürk | ~8 MB |

### 4. Translation JSON Structure

**File: `assets/quran/translations_{language_code}.json`**

```json
{
  "metadata": {
    "language_code": "en",
    "language_name": "English",
    "translator": "Sahih International",
    "version": "1.0",
    "total_ayahs": 6236
  },
  "translations": [
    {
      "surah": 1,
      "ayah": 1,
      "text": "In the name of Allah, the Entirely Merciful, the Especially Merciful."
    },
    {
      "surah": 1,
      "ayah": 2,
      "text": "[All] praise is [due] to Allah, Lord of the worlds -"
    }
  ]
}
```

### 5. Database Schema

```sql
CREATE TABLE translations (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  language_code TEXT UNIQUE NOT NULL,
  language_name TEXT NOT NULL,
  native_language_name TEXT,
  translator_name TEXT,
  translator_bio TEXT,
  source_url TEXT,
  file_size INTEGER,
  is_offline_available BOOLEAN DEFAULT 0,
  date_added TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  is_active BOOLEAN DEFAULT 1
);

CREATE TABLE ayah_translations (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  surah_number INTEGER NOT NULL,
  ayah_number INTEGER NOT NULL,
  language_code TEXT NOT NULL,
  translation_text TEXT NOT NULL,
  footnote TEXT,
  word_by_word TEXT,
  UNIQUE(surah_number, ayah_number, language_code),
  FOREIGN KEY(language_code) REFERENCES translations(language_code)
);

CREATE TABLE user_translation_preferences (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  language_code TEXT UNIQUE NOT NULL,
  is_enabled BOOLEAN DEFAULT 1,
  display_order INTEGER,
  last_used TIMESTAMP,
  FOREIGN KEY(language_code) REFERENCES translations(language_code)
);

CREATE TABLE translation_downloads (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  language_code TEXT UNIQUE NOT NULL,
  is_downloaded BOOLEAN DEFAULT 0,
  downloaded_date TIMESTAMP,
  file_size INTEGER,
  local_path TEXT,
  FOREIGN KEY(language_code) REFERENCES translations(language_code)
);

CREATE INDEX idx_translations_lang ON translations(language_code);
CREATE INDEX idx_ayah_trans_surah ON ayah_translations(surah_number);
CREATE INDEX idx_ayah_trans_lang ON ayah_translations(language_code);
```

### 6. Translation Toggle UI

**Translation Selector Widget:**
- Checkboxes for each language
- Show language name + native name
- Show translator info
- Show file size for offline
- Enable/disable per language
- Save preferences to database

**Example:**
```
✓ English (Sahih International) - 8MB
✓ Urdu (اردو) - 7MB
☐ French (Français) - 10MB
☐ Hausa (Hausa) - 6MB
```

### 7. Display Options

**Translation Display Modes:**
1. **Arabic Only** - Just Uthmani text
2. **Arabic + 1 Translation** - Side by side
3. **Arabic + Multiple Translations** - Stacked view
4. **Translation Only** - Without Arabic

**Layout Options:**
- Horizontal split (Arabic | Translation)
- Vertical stacked (Arabic above, Translation below)
- Inline (Ayah with translation inline)
- Tabs (Switch between translations)

### 8. Translation UI Components

**Ayah View with Translation:**
```
┌─────────────────────────────┐
│ 📖 Al-Fatiha - Ayah 2       │
├─────────────────────────────┤
│ ┌─────────────────────────┐ │
│ │ الْحَمْدُ لِلَّهِ رَبِّ  │ │ (Arabic RTL)
│ └─────────────────────────┘ │
│                             │
│ "All praise is [due] to    │
│  Allah, Lord of the worlds" │ (Translation)
│                             │
│ 🇬🇧 English                │
│    Sahih International      │
└─────────────────────────────┘
```

### 9. Search Across Translations

**Multi-Language Search:**
```dart
Future<List<SearchResult>> searchInAllTranslations(String query) async {
  List<SearchResult> results = [];
  
  for (String langCode in enabledTranslations) {
    final langResults = await searchTranslation(query, langCode);
    results.addAll(langResults);
  }
  
  return results;
}
```

**Display search results:**
- Show in original Arabic
- Show matching translation(s)
- Show surah name and ayah number
- Easy navigation to ayah

### 10. Translation Settings Screen

**Settings Options:**
- Primary translation (for default view)
- Secondary translations (additional display)
- Translation display mode (single/multiple)
- Font size for translations
- Language preference for UI
- Download translations for offline
- Manage downloaded translations

### 11. Offline Translation Support

**Download Manager:**
- Download individual translations
- Download all at once
- Show download progress
- Resume interrupted downloads
- Delete downloaded translations
- Track storage used

**Fallback Logic:**
- Use cached translation if available
- Fallback to online if not downloaded
- Show indicators for online/offline

### 12. Implementation Details

**Loading Translations:**
```dart
Future<void> initializeTranslations() async {
  // Load available translations from JSON
  final translations = await loadTranslationsMetadata();
  
  // Store in database
  for (var translation in translations) {
    await db.insert('translations', translation.toMap());
  }
  
  // Load user preferences
  final preferences = await getUserTranslationPreferences();
  
  // Cache enabled translations
  for (var langCode in preferences.enabledLanguages) {
    await loadTranslation(langCode);
  }
}
```

### 13. Performance Optimization

**Caching Strategy:**
- Cache current surah translations
- Pre-load next surah translations
- LRU cache for frequently accessed translations
- Compress translation data for storage

**Memory Management:**
- Load only active translations into memory
- Stream translations for large queries
- Implement translation paging

### 14. Accessibility Features

- Translation language name in app language
- Clear translation attribution
- Adjustable font sizes
- Color contrast for readability
- Keyboard navigation
- Screen reader support

### 15. Testing Checklist

- [ ] All 6 translations load correctly
- [ ] Toggle translations on/off works
- [ ] Display modes work properly
- [ ] Search across translations works
- [ ] Translations display correctly (no encoding issues)
- [ ] RTL/LTR handling correct for each language
- [ ] Offline translations work
- [ ] Translation download/delete works
- [ ] User preferences save and restore
- [ ] Performance acceptable with all translations
- [ ] No data corruption in translation data
- [ ] Font rendering correct for all languages

### 16. User Experience Flow

1. User opens settings → Translation preferences
2. Selects languages to enable (default: English)
3. Chooses display mode (Arabic + English)
4. Browses surahs → Sees Arabic + selected translations
5. Can toggle translations on/off quickly
6. Can download translations for offline
7. Translations update instantly
8. Preferences persist across sessions

### 17. Admin Controls (Phase 2F integration)

Prepare for admin panel:
- Add new translations
- Update translation content
- Enable/disable languages
- Monitor translation usage
- Set translation quality
- Manage translator information

### 18. Translator Attribution

**Display:**
- Show translator name in UI
- Show translator biography
- Provide source citation
- Link to source document
- Show translation version/date

## 📊 Deliverables

- ✅ TranslationService with full API
- ✅ 6 language translation files (JSON)
- ✅ Translation toggle UI
- ✅ Database schema for translations
- ✅ Display mode options
- ✅ Search across translations
- ✅ Offline translation support
- ✅ User preference management
- ✅ Unit & integration tests

## 🎯 Success Criteria

- All 6 languages display correctly
- Toggle translations smoothly
- Search works in all languages
- RTL/LTR handling perfect
- Offline translations work
- No encoding issues
- Fast loading (<500ms per surah)
- User preferences saved
- Accessibility standards met
- Translator attribution clear

## 📝 Notes

- Use proper text encoding (UTF-8)
- Handle RTL text correctly (Arabic, Urdu)
- Test with native speakers
- Ensure translation accuracy
- Maintain consistent formatting
- Support for future language additions
- Consider regional dialects if applicable

---
**Phase**: 2D  
**Depends On**: Phase 2A ✅, Phase 2B ✅, Phase 2C ✅  
**Priority**: High  
**Estimated Time**: 2-3 weeks
