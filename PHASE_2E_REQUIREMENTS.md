# Phase 2E: Bookmarks & Notes

## 🔖 Objective
Enable users to bookmark ayahs, add personal notes with categorization, and create organized collections for memorization, daily reading, and reflection.

## 📋 Tasks

### 1. Bookmark & Notes Service

Create `BookmarkService`:
```dart
class BookmarkService {
  // Initialize bookmark system
  Future<void> initialize() { }
  
  // Create bookmark
  Future<Bookmark> createBookmark(
    int surahNumber,
    int ayahNumber,
    String? note,
  ) { }
  
  // Delete bookmark
  Future<void> deleteBookmark(int bookmarkId) { }
  
  // Get all bookmarks
  Future<List<Bookmark>> getAllBookmarks() { }
  
  // Get bookmarks for surah
  Future<List<Bookmark>> getSurahBookmarks(int surahNumber) { }
  
  // Get bookmarks by category
  Future<List<Bookmark>> getBookmarksByCategory(String category) { }
  
  // Check if ayah is bookmarked
  Future<bool> isBookmarked(int surahNumber, int ayahNumber) { }
  
  // Add note to bookmark
  Future<void> addNote(int bookmarkId, String noteText) { }
  
  // Update note
  Future<void> updateNote(int bookmarkId, String noteText) { }
  
  // Delete note
  Future<void> deleteNote(int bookmarkId) { }
  
  // Get note
  Future<Note?> getNote(int bookmarkId) { }
  
  // Create collection
  Future<Collection> createCollection(String name, String description) { }
  
  // Add bookmark to collection
  Future<void> addToCollection(int bookmarkId, String collectionId) { }
  
  // Remove from collection
  Future<void> removeFromCollection(int bookmarkId, String collectionId) { }
  
  // Get collection
  Future<Collection> getCollection(String collectionId) { }
  
  // Get all collections
  Future<List<Collection>> getAllCollections() { }
  
  // Delete collection
  Future<void> deleteCollection(String collectionId) { }
  
  // Update collection
  Future<void> updateCollection(String collectionId, Collection collection) { }
  
  // Export bookmarks
  Future<String> exportBookmarks(ExportFormat format) { }
  
  // Import bookmarks
  Future<void> importBookmarks(String data, ImportFormat format) { }
  
  // Search bookmarks
  Future<List<Bookmark>> searchBookmarks(String query) { }
  
  // Get bookmark statistics
  Future<BookmarkStats> getStatistics() { }
}
```

### 2. Data Models

**Bookmark Model:**
```dart
class Bookmark {
  final int id;
  final int surahNumber;
  final int ayahNumber;
  final String surahName;
  final String ayahText; // Uthmani text
  final DateTime createdDate;
  final DateTime? lastModified;
  final List<String> categories; // Collection IDs
  final bool isFavorite;
  final int color; // Color hex code
  final String? tags; // comma-separated tags
}
```

**Note Model:**
```dart
class Note {
  final int id;
  final int bookmarkId;
  final String content;
  final DateTime createdDate;
  final DateTime? lastModified;
  final String category; // Memorization, Daily Reading, Reflection, Custom
  final bool isPrivate;
}
```

**Collection Model:**
```dart
class Collection {
  final String id; // UUID
  final String name;
  final String description;
  final String icon; // emoji or icon name
  final String color; // hex color
  final DateTime createdDate;
  final DateTime? lastModified;
  final int bookmarkCount;
  final bool isDefault; // Built-in collections
}
```

**BookmarkStats Model:**
```dart
class BookmarkStats {
  final int totalBookmarks;
  final int totalNotes;
  final int totalCollections;
  final int memorizedSurahs;
  final Map<String, int> bookmarksByCategory;
  final List<Bookmark> recentBookmarks;
  final int bookmarksThisMonth;
}
```

### 3. Default Collections

**Built-in Collections:**

1. **🧠 Memorization**
   - For tracking memorized surahs/ayahs
   - Progress tracking
   - Revision schedule

2. **📖 Daily Reading**
   - Daily Qur'an reading plan
   - Track completion
   - Reflection points

3. **💭 Reflection**
   - Personal insights
   - Life applications
   - Spiritual notes

4. **⭐ Favorites**
   - Quick access to favorite ayahs
   - Most meaningful verses

### 4. Database Schema

```sql
CREATE TABLE bookmarks (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  surah_number INTEGER NOT NULL,
  ayah_number INTEGER NOT NULL,
  surah_name TEXT,
  ayah_text TEXT,
  is_favorite BOOLEAN DEFAULT 0,
  color INTEGER DEFAULT 0,
  created_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_modified TIMESTAMP,
  UNIQUE(surah_number, ayah_number)
);

CREATE TABLE notes (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  bookmark_id INTEGER NOT NULL UNIQUE,
  content TEXT NOT NULL,
  category TEXT DEFAULT 'Custom',
  is_private BOOLEAN DEFAULT 0,
  created_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_modified TIMESTAMP,
  FOREIGN KEY(bookmark_id) REFERENCES bookmarks(id) ON DELETE CASCADE
);

CREATE TABLE collections (
  id TEXT PRIMARY KEY,
  name TEXT UNIQUE NOT NULL,
  description TEXT,
  icon TEXT,
  color TEXT DEFAULT '#2196F3',
  created_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_modified TIMESTAMP,
  is_default BOOLEAN DEFAULT 0
);

CREATE TABLE bookmark_collections (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  bookmark_id INTEGER NOT NULL,
  collection_id TEXT NOT NULL,
  added_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(bookmark_id, collection_id),
  FOREIGN KEY(bookmark_id) REFERENCES bookmarks(id) ON DELETE CASCADE,
  FOREIGN KEY(collection_id) REFERENCES collections(id) ON DELETE CASCADE
);

CREATE TABLE bookmark_tags (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  bookmark_id INTEGER NOT NULL,
  tag_name TEXT NOT NULL,
  UNIQUE(bookmark_id, tag_name),
  FOREIGN KEY(bookmark_id) REFERENCES bookmarks(id) ON DELETE CASCADE
);

CREATE TABLE reading_progress (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  collection_id TEXT NOT NULL,
  surah_number INTEGER,
  ayah_number INTEGER,
  completion_percentage REAL DEFAULT 0.0,
  last_read_date TIMESTAMP,
  FOREIGN KEY(collection_id) REFERENCES collections(id)
);

CREATE INDEX idx_bookmarks_surah ON bookmarks(surah_number);
CREATE INDEX idx_notes_bookmark ON notes(bookmark_id);
CREATE INDEX idx_collections_name ON collections(name);
CREATE INDEX idx_bookmark_collections ON bookmark_collections(collection_id);
```

### 5. Bookmark UI Components

**Bookmark Button (on Ayah View):**
```
┌─────────────────────────┐
│ Ayah text here...       │
│                         │
│ [🔖] Add Bookmark       │ (Not bookmarked)
└─────────────────────────┘

OR

┌─────────────────────────┐
│ Ayah text here...       │
│                         │
│ [🔖] Bookmarked ✓       │ (Bookmarked)
└─────────────────────────┘
```

**Bookmark Options Menu:**
- Add/Remove bookmark
- Add note
- Add to collection
- Change color
- Mark as favorite
- Share
- Copy text

### 6. Bookmarks Screen

**Layout:**
```
┌──────────────────────────┐
│ 📚 My Bookmarks          │
├──────────────────────────┤
│ Search bookmarks...      │
├──────────────────────────┤
│ Collections:             │
│ [🧠 Memorization (5)]   │
│ [📖 Daily Reading (12)] │
│ [💭 Reflection (8)]     │
│ [⭐ Favorites (3)]      │
│                          │
│ Recent Bookmarks:        │
│ ┌──────────────────────┐ │
│ │ Surah 1, Ayah 2      │ │
│ │ Al-Fatiha            │ │
│ │ "الْحَمْدُ لِلَّهِ..." │ │
│ │ 📌 Memorization      │ │
│ │ ⭐ Favorite          │ │
│ └──────────────────────┘ │
└──────────────────────────┘
```

### 7. Notes & Collections

**Note Editor:**
- Rich text editor for notes
- Category selection
- Date tracking
- Search within notes
- Share notes
- Private/Public toggle

**Collection Manager:**
- Create new collection
- Edit collection name/description
- Change color/icon
- View bookmarks in collection
- Delete collection with confirmation
- Reorder collections (drag & drop)

### 8. Reading Progress Tracking

**For Daily Reading Collection:**
```dart
class ReadingProgress {
  final String collectionId;
  final int surahNumber;
  final int ayahNumber;
  final double completionPercentage; // 0-100
  final DateTime lastReadDate;
  final int totalDaysRead;
  final int currentStreak; // Consecutive days
}
```

**Display Features:**
- Progress bar per surah
- Completion percentage
- Reading streak tracker
- Last read date
- Motivational insights

### 9. Memorization Tracking

**For Memorization Collection:**
```dart
class MemorizationProgress {
  final int surahNumber;
  final int startAyah;
  final int endAyah;
  final MemorizationStatus status; // Not Started, In Progress, Completed, Reviewing
  final DateTime startDate;
  final DateTime? completionDate;
  final int reviewCount;
  final DateTime lastReviewDate;
  final double confidenceLevel; // 0-100
}

enum MemorizationStatus {
  notStarted,
  inProgress,
  completed,
  reviewing
}
```

**Features:**
- Mark surahs/ayahs as memorized
- Track memorization status
- Revision schedule reminders
- Progress visualization
- Memorization goals

### 10. Export & Import

**Supported Formats:**
- JSON (internal format)
- CSV (spreadsheet compatible)
- PDF (printable)
- Text file

**Export Includes:**
- All bookmarks with notes
- Collections structure
- Reading progress
- Tags and metadata

**Import Process:**
- File selection
- Format detection
- Preview before import
- Merge or replace options
- Conflict resolution

### 11. Sharing Features

**Share Options:**
- Share single ayah with bookmark
- Share entire collection
- Share reading progress
- Generate shareable links
- Social media integration (WhatsApp, etc.)

**Shared Content Format:**
```
📖 Qur'an Bookmark

Surah: Al-Fatiha (الفاتحة)
Ayah: 2

Text: الْحَمْدُ لِلَّهِ رَبِّ الْعَالَمِينَ

Translation: All praise is [due] to Allah, 
Lord of the worlds

Category: 💭 Reflection
Added: Aug 25, 2026

Note: Remember Allah's mercy in all situations

Shared from Noor al-Quran
```

### 12. Backup & Sync (Firebase Integration - Phase 2F)

**Backup Features:**
- Auto-backup to Firebase
- Manual backup option
- Cloud sync across devices
- Restore from backup
- Backup history

### 13. Accessibility Features

- Keyboard navigation
- Screen reader support
- High contrast mode for notes
- Large font option
- Voice input for notes
- Haptic feedback for bookmarks

### 14. Performance Optimization

**Caching:**
- Cache bookmarks locally
- Lazy load collections
- Index search queries
- Efficient database queries

**Limits:**
- Reasonable bookmark limit (per user preferences)
- Note character limit with warning
- Collection limit with creation guide

### 15. Testing Checklist

- [ ] Add bookmark to ayah works
- [ ] Remove bookmark works
- [ ] Bookmark persists across sessions
- [ ] Add note to bookmark works
- [ ] Edit note works
- [ ] Delete note works
- [ ] Create collection works
- [ ] Add bookmark to collection works
- [ ] Remove from collection works
- [ ] Delete collection prompts confirmation
- [ ] Search bookmarks works
- [ ] Filter by collection works
- [ ] Favorite toggle works
- [ ] Color selection works
- [ ] Export to JSON works
- [ ] Export to CSV works
- [ ] Import bookmarks works
- [ ] Reading progress tracks correctly
- [ ] Memorization status updates
- [ ] Tags work properly
- [ ] Share functionality works

### 16. User Experience Flow

1. User reading ayahs → Taps bookmark icon
2. Bookmark created → Shows confirmation
3. User taps "Add Note" → Opens note editor
4. User enters note → Saves with category
5. User creates "Memorization" collection
6. User adds bookmarks to collection
7. User views collection → Sees all bookmarks
8. User tracks reading progress
9. User exports bookmarks as backup
10. Bookmarks sync to Firebase (Phase 2F)

### 17. Default Categories for Notes

1. **Memorization**
   - Verse difficult to memorize
   - Pronunciation notes
   - Similar verses

2. **Daily Reading**
   - Daily reflection
   - Lesson learned
   - Personal application

3. **Reflection**
   - Spiritual insights
   - Life wisdom
   - Guidance received

4. **Custom**
   - User-defined categories
   - Topic-based notes
   - Research notes

### 18. Analytics (For Admin - Phase 2F)

Track (anonymously):
- Most bookmarked ayahs
- Popular collections
- Average bookmarks per user
- Usage patterns
- Feature popularity

## 📊 Deliverables

- ✅ BookmarkService with full API
- ✅ Note management system
- ✅ Collection management
- ✅ Database schema
- ✅ UI screens (Bookmarks, Notes, Collections)
- ✅ Reading progress tracking
- ✅ Memorization tracking
- ✅ Export/Import functionality
- ✅ Search & filtering
- ✅ Unit & integration tests

## 🎯 Success Criteria

- Bookmarking works smoothly
- Notes save reliably
- Collections organize properly
- Reading progress tracks accurately
- Export/Import works without data loss
- Search is fast and accurate
- UI is intuitive and responsive
- Data persists across sessions
- No data corruption on delete
- Memorization tracking motivating

## 📝 Notes

- Use cascading delete for data integrity
- Implement proper backup strategy
- Consider user privacy (private notes)
- Make sharing secure
- Test with large bookmark datasets
- Optimize database queries
- Implement proper indexing
- Consider cloud sync architecture

---
**Phase**: 2E  
**Depends On**: Phase 2A ✅, Phase 2B ✅, Phase 2C ✅, Phase 2D ✅  
**Priority**: High  
**Estimated Time**: 2-3 weeks
