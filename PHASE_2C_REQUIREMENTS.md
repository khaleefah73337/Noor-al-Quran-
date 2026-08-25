# Phase 2C: Offline Downloads

## 📥 Objective
Enable users to download individual surahs for offline listening without internet connection.

## 📋 Tasks

### 1. Download Manager Service

Create `DownloadService`:
```dart
class DownloadService {
  // Initialize download manager
  Future<void> initialize() { }
  
  // Download single surah
  Future<void> downloadSurah(
    int surahNumber,
    String reciterId,
    String quality, // 64kbps, 128kbps, 320kbps
  ) { }
  
  // Download multiple surahs
  Future<void> downloadSurahs(
    List<int> surahNumbers,
    String reciterId,
    String quality,
  ) { }
  
  // Download entire Qur'an
  Future<void> downloadFullQuran(
    String reciterId,
    String quality,
  ) { }
  
  // Cancel download
  Future<void> cancelDownload(int surahNumber, String reciterId) { }
  
  // Pause download
  Future<void> pauseDownload(int surahNumber, String reciterId) { }
  
  // Resume download
  Future<void> resumeDownload(int surahNumber, String reciterId) { }
  
  // Delete downloaded surah
  Future<void> deleteSurah(int surahNumber, String reciterId) { }
  
  // Delete all downloads
  Future<void> deleteAllDownloads() { }
  
  // Get download status
  Future<DownloadStatus> getDownloadStatus(int surahNumber, String reciterId) { }
  
  // Get all downloads
  Future<List<DownloadedSurah>> getAllDownloads() { }
  
  // Get storage info
  Future<StorageInfo> getStorageInfo() { }
  
  // Stream download progress
  Stream<DownloadProgress> downloadProgressStream(int surahNumber, String reciterId);
}
```

### 2. Data Models

**DownloadedSurah Model:**
```dart
class DownloadedSurah {
  final int surahNumber;
  final String reciterId;
  final String reciterName;
  final String quality; // 64kbps, 128kbps, 320kbps
  final int fileSize; // in MB
  final DateTime downloadedDate;
  final DateTime lastAccessed;
  final bool isComplete;
  final String localPath;
}
```

**DownloadProgress Model:**
```dart
class DownloadProgress {
  final int surahNumber;
  final String reciterId;
  final double progressPercentage; // 0.0 to 100.0
  final int downloadedBytes;
  final int totalBytes;
  final Duration remainingTime;
  final double downloadSpeed; // MB/s
  final DownloadStatus status; // downloading, paused, completed, failed
}
```

**StorageInfo Model:**
```dart
class StorageInfo {
  final int totalStorageBytes;
  final int usedStorageBytes;
  final int availableStorageBytes;
  final int quranStorageBytes; // Used by Qur'an app
  final double percentageUsed;
}
```

### 3. Download Quality Options

| Quality | Bitrate | File Size (Per Surah) | Total 114 Surahs |
|---------|---------|----------------------|-----------------|
| Low | 64 kbps | ~2-3 MB | ~250-340 MB |
| Medium | 128 kbps | ~4-6 MB | ~500-680 MB |
| High | 320 kbps | ~10-15 MB | ~1.2-1.7 GB |

### 4. Database Schema

```sql
CREATE TABLE downloaded_surahs (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  surah_number INTEGER NOT NULL,
  reciter_id INTEGER NOT NULL,
  reciter_name TEXT NOT NULL,
  quality TEXT NOT NULL, -- 64kbps, 128kbps, 320kbps
  file_size INTEGER NOT NULL, -- bytes
  file_path TEXT NOT NULL,
  downloaded_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_accessed TIMESTAMP,
  is_complete BOOLEAN DEFAULT 1,
  hash_value TEXT, -- for integrity check
  UNIQUE(surah_number, reciter_id),
  FOREIGN KEY(reciter_id) REFERENCES reciters(id)
);

CREATE TABLE download_queue (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  surah_number INTEGER NOT NULL,
  reciter_id INTEGER NOT NULL,
  quality TEXT NOT NULL,
  status TEXT DEFAULT 'pending', -- pending, downloading, paused, completed, failed
  progress REAL DEFAULT 0.0,
  downloaded_bytes INTEGER DEFAULT 0,
  total_bytes INTEGER,
  error_message TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE download_history (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  surah_number INTEGER NOT NULL,
  reciter_id INTEGER NOT NULL,
  downloaded_date TIMESTAMP,
  deleted_date TIMESTAMP,
  file_size INTEGER
);

CREATE INDEX idx_downloads_surah ON downloaded_surahs(surah_number);
CREATE INDEX idx_downloads_reciter ON downloaded_surahs(reciter_id);
CREATE INDEX idx_queue_status ON download_queue(status);
```

### 5. Download UI Components

**Downloads Screen:**
- List of downloaded surahs
- Grouped by reciter
- Show file size per surah
- Show total storage used
- Delete individual downloads
- Delete all downloads button

**Download Dialog:**
- Reciter selector
- Quality selector (64/128/320 kbps)
- Download button
- Progress bar
- Cancel button
- Pause/Resume during download

**Storage Management Screen:**
- Total app storage usage
- Storage breakdown by reciter
- Available space indicator
- Cleanup options
- Auto-delete old downloads setting

### 6. Implementation Details

**Local Storage Path:**
```
Android:
  /data/data/com.noor_al_quran/files/quran_audio/
  {reciter_id}/
    {surah_number}.mp3

iOS:
  Documents/QuranAudio/
  {reciter_id}/
    {surah_number}.mp3
```

**Download Manager Initialization:**
```dart
Future<void> initializeDownloads() async {
  final downloadDir = await _getDownloadDirectory();
  await _createDirectoryStructure(downloadDir);
  await _cleanupIncompleteDownloads();
  await _verifyDownloadedFiles();
}
```

### 7. Network Handling

**Smart Download Logic:**
```dart
// Only download on WiFi by default
bool shouldAllowCellularDownload = false;

// Pause on network change
// Resume when network restores
// Handle connection timeouts

// Resumable downloads (use HTTP Range headers)
// Partial downloads recovery
```

### 8. File Integrity

**Verification:**
- Calculate MD5/SHA-256 hash after download
- Verify file size matches expected
- Validate audio format
- Re-download if corruption detected

```dart
String calculateFileHash(File file) {
  // Calculate hash for integrity check
}

bool verifyDownloadedFile(File file, String expectedHash) {
  String actualHash = calculateFileHash(file);
  return actualHash == expectedHash;
}
```

### 9. Download States

```dart
enum DownloadStatus {
  notDownloaded,      // Not downloaded yet
  downloading,        // Currently downloading
  paused,            // Download paused
  completed,         // Download complete
  failed,            // Download failed
  corrupted,         // File corrupted
}
```

### 10. Background Download Support

- **iOS**: Use background URLSession
- **Android**: Use WorkManager for background downloads
- Continue downloads even if app closes
- Resume on app restart
- Notification with progress indicator

```dart
class BackgroundDownloadService {
  Future<void> setupBackgroundTasks() async {
    // Configure WorkManager (Android)
    // Configure BGAppRefresh (iOS)
  }
}
```

### 11. Storage Quota Management

**Automatic Cleanup:**
- Delete oldest accessed surahs when storage full
- Show warning at 80% capacity
- Show alert at 95% capacity
- Prevent download if insufficient space

**User Control:**
- Manual deletion options
- Select quality to free space
- Auto-delete option (oldest first)

### 12. Download Optimization

**Strategies:**
- Cache file metadata locally
- Implement intelligent download scheduling
- Batch downloads during off-peak hours
- Compress metadata for sync
- Efficient memory usage during download

### 13. Testing Checklist

- [ ] Single surah downloads successfully
- [ ] Multiple surah downloads work
- [ ] Full Qur'an download available
- [ ] Download progress updates correctly
- [ ] Pause/Resume works properly
- [ ] Cancel stops download
- [ ] Delete removes file
- [ ] Storage calculation accurate
- [ ] File integrity verification works
- [ ] Network change handled correctly
- [ ] WiFi/Cellular toggle works
- [ ] Background download continues
- [ ] App restart resumes downloads
- [ ] Low storage warning shows
- [ ] Quality selection works
- [ ] Download history tracked
- [ ] Offline playback works for downloads

### 14. User Experience Flow

1. User opens "Downloads" screen
2. User taps "Download Surah"
3. Dialog appears: Select Reciter + Quality
4. Download starts with progress indicator
5. Can pause/resume/cancel during download
6. Download completes → Available offline
7. User can play offline without internet
8. Can delete individual downloads
9. Can see storage usage
10. Can manage storage with cleanup options

### 15. Admin Controls (Phase 2F integration)

Prepare for admin panel:
- Monitor download statistics
- Track popular downloads
- Manage storage quotas
- Set default quality
- Enable/disable download feature
- Set bandwidth limits

### 16. Accessibility Features

- Progress announcements (VoiceOver/TalkBack)
- Clear status indicators
- Simple delete confirmations
- Accessible storage display
- Keyboard navigation

## 📊 Deliverables

- ✅ DownloadService with full API
- ✅ Download manager UI
- ✅ Storage management screen
- ✅ Database schema for downloads
- ✅ Background download support
- ✅ File integrity verification
- ✅ Network-aware downloading
- ✅ Unit & integration tests

## 🎯 Success Criteria

- User can download any surah
- Download works on WiFi and cellular
- Pause/Resume functions smoothly
- Storage info is accurate
- Downloads persist after restart
- Playback works offline
- No data corruption
- Download speed >500KB/s on good connection
- Storage usage displayed clearly
- Easy to manage downloads

## 📝 Notes

- Use SQLite to track all downloads
- Implement proper cleanup on app uninstall
- Support resumable downloads (HTTP Range)
- Test with various network speeds
- Monitor battery consumption
- Consider bandwidth for users
- Provide download scheduler

---
**Phase**: 2C  
**Depends On**: Phase 2A ✅, Phase 2B ✅  
**Priority**: High  
**Estimated Time**: 2-3 weeks
