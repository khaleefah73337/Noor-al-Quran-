# Phase 2F: Firebase Admin System

## 👑 Objective
Implement a comprehensive admin panel with Firebase authentication and management controls for reciters, audio files, and app content.

## 📋 Tasks

### 1. Admin Service

Create `AdminService`:
```dart
class AdminService {
  // Authentication
  Future<void> authenticateAdmin(String pin) { }
  Future<bool> isAdminAuthenticated() { }
  Future<void> logoutAdmin() { }
  Future<void> changeAdminPin(String oldPin, String newPin) { }
  
  // Reciter Management
  Future<Reciter> addReciter(Reciter reciter) { }
  Future<void> updateReciter(String reciterId, Reciter reciter) { }
  Future<void> deleteReciter(String reciterId) { }
  Future<void> toggleReciterStatus(String reciterId, bool isActive) { }
  Future<List<Reciter>> getAllReciters() { }
  
  // Audio Management
  Future<void> uploadAudio(String reciterId, int surahNumber, File audioFile) { }
  Future<void> replaceAudio(String reciterId, int surahNumber, File audioFile) { }
  Future<void> deleteAudio(String reciterId, int surahNumber) { }
  Future<String> getAudioUrl(String reciterId, int surahNumber) { }
  Future<List<AudioFile>> getReciterAudioFiles(String reciterId) { }
  
  // Content Management
  Future<void> updateSurahMetadata(int surahNumber, Surah surah) { }
  Future<void> updateTranslation(String languageCode, Map<String, dynamic> data) { }
  
  // Statistics & Monitoring
  Future<AdminStats> getStatistics() { }
  Future<List<UserActivity>> getUserActivity() { }
  Future<Map<String, dynamic>> getSystemHealth() { }
  
  // Settings
  Future<void> updateAppSettings(AppSettings settings) { }
  Future<AppSettings> getAppSettings() { }
  
  // Notifications
  Future<void> sendNotificationToUsers(String title, String message) { }
  
  // Backup & Maintenance
  Future<void> triggerDatabaseBackup() { }
  Future<void> cleanupOrphanedFiles() { }
}
```

### 2. Admin Authentication

**Admin PIN System:**
- PIN: `khaleefah37`
- Store hashed PIN in Firebase
- Session management (timeout after 30 mins)
- Biometric login option (Android/iOS)
- Login attempt limiting (max 5 attempts, 15 min lockout)
- Activity logging

**PIN Entry Screen:**
```
┌─────────────────────┐
│   🔐 Admin Access   │
├─────────────────────┤
│ Enter PIN:          │
│ ● ● ● ● ● ●        │
│ [1] [2] [3]         │
│ [4] [5] [6]         │
│ [7] [8] [9]         │
│ [*] [0] [#]         │
│ [Backspace] [Clear] │
│ [Login]             │
└─────────────────────┘
```

### 3. Admin Panel Dashboard

**Main Screen:**
```
┌──────────────────────────┐
│ 👑 Admin Dashboard       │
├──────────────────────────┤
│ Welcome, Administrator   │
│                          │
│ Quick Stats:             │
│ 📱 Active Users: 1,245   │
│ 🎙️  Reciters: 7         │
│ 📥 Downloads: 3,456      │
│ 📊 Storage: 145 GB       │
│                          │
│ Management:              │
│ [🎤 Manage Reciters]    │
│ [🎵 Manage Audio]       │
│ [⚙️  Settings]          │
│ [📊 Analytics]          │
│ [🔄 Backup]             │
│ [🚪 Logout]             │
└──────────────────────────┘
```

### 4. Reciter Management

**Reciter List Screen:**
- Display all 7 reciters
- Show active/inactive status
- Edit button for each reciter
- Delete with confirmation
- Add new reciter button
- Search/filter functionality

**Reciter Card:**
```
┌────────────────────────┐
│ 🎤 Mishary Alafasy    │
├────────────────────────┤
│ Country: Kuwait        │
│ Audio Files: 114/114   │
│ Status: ✅ Active     │
│ Quality: 128kbps       │
│                        │
│ [Edit] [Audio] [Disable] [Delete]
└────────────────────────┘
```

**Edit Reciter:**
- Name (Arabic + English)
- Country/Origin
- Biography
- Image upload
- Quality options
- Toggle active/inactive

### 5. Audio Management

**Audio Upload Interface:**
- Reciter selector
- Surah selector (1-114)
- Audio file picker
- Progress indicator
- Quality validation
- Replace existing audio option

**Audio File Management:**
```
┌─────────────────────────────┐
│ 📁 Audio Files              │
├─────────────────────────────┤
│ Reciter: Mishary Alafasy   │
│                             │
│ Surah | Status | Size | Action
│ ─────────────────────────────
│ 001   | ✅     | 5.2MB | [Replace] [Delete]
│ 002   | ✅     | 5.8MB | [Replace] [Delete]
│ 003   | ❌     | -     | [Upload]
│ ...   | ...    | ...   | ...
│                             │
│ [Upload Missing] [Refresh]  │
└─────────────────────────────┘
```

**Audio Upload Process:**
1. Select reciter
2. Select surah (or batch upload)
3. Choose audio file
4. Validate format (MP3/M4A)
5. Validate file size
6. Compress if needed
7. Upload to Firebase Storage
8. Update database
9. Verify upload
10. Show confirmation

### 6. Firebase Integration

**Firestore Collections:**
```
reciters/
├── reciter_1/
│   ├── name
│   ├── country
│   ├── bio
│   ├── image_url
│   ├── is_active
│   └── audio_files (subcollection)
│       ├── 001/
│       │   ├── url
│       │   ├── file_size
│       │   ├── duration
│       │   └── hash
│       └── 002/

audio_logs/
├── upload_log
├── delete_log
└── replace_log

admin_settings/
├── app_config
├── feature_flags
└── maintenance

user_analytics/
├── activity_logs
├── download_stats
└── usage_stats
```

**Firebase Storage Structure:**
```
gs://bucket/
├── audio/
│   ├── mishary_alafasy/
│   │   ├── 001.mp3
│   │   ├── 002.mp3
│   │   └── ... (001-114)
│   ├── abdul_basit/
│   └── ... (other reciters)
├── backups/
│   ├── database_backup_2026-08-25.json
│   └── ... (daily backups)
└── temp/
    └── (temporary files)
```

### 7. Statistics Dashboard

**Metrics:**
```
┌──────────────────────────┐
│ 📊 Admin Analytics       │
├──────────────────────────┤
│ User Statistics:         │
│ • Total Users: 5,432     │
│ • Active Today: 1,245    │
│ • Downloads Today: 342   │
│                          │
│ Popular Reciters:        │
│ 1. Mishary Alafasy - 45% │
│ 2. Abdul Basit - 32%     │
│ 3. Al-Sudais - 23%       │
│                          │
│ Most Bookmarked Surahs:  │
│ 1. Al-Fatiha (112)       │
│ 2. Ar-Rahman (89)        │
│ 3. Al-Mulk (76)          │
│                          │
│ Storage Usage:           │
│ ▓▓▓▓▓▓░░░░ 145/200 GB   │
│                          │
│ [Export Report] [Refresh]│
└──────────────────────────┘
```

### 8. Database Schema (Admin Collections)

```sql
CREATE TABLE admin_sessions (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  pin_hash TEXT NOT NULL,
  session_token TEXT UNIQUE,
  login_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_activity TIMESTAMP,
  expires_at TIMESTAMP,
  ip_address TEXT,
  device_info TEXT
);

CREATE TABLE admin_logs (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  action TEXT NOT NULL, -- upload, delete, update, etc.
  resource_type TEXT, -- reciter, audio, setting
  resource_id TEXT,
  details TEXT,
  admin_id TEXT,
  timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE audio_files (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  reciter_id INTEGER NOT NULL,
  surah_number INTEGER NOT NULL,
  file_url TEXT NOT NULL,
  file_size INTEGER,
  duration INTEGER, -- seconds
  bitrate TEXT,
  hash_value TEXT,
  upload_date TIMESTAMP,
  last_modified TIMESTAMP,
  is_active BOOLEAN DEFAULT 1,
  UNIQUE(reciter_id, surah_number),
  FOREIGN KEY(reciter_id) REFERENCES reciters(id)
);

CREATE TABLE app_settings (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  setting_key TEXT UNIQUE NOT NULL,
  setting_value TEXT,
  setting_type TEXT, -- string, boolean, integer
  last_modified TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_admin_logs_action ON admin_logs(action);
CREATE INDEX idx_admin_logs_timestamp ON admin_logs(timestamp);
CREATE INDEX idx_audio_files_reciter ON audio_files(reciter_id);
```

### 9. Admin Features

**Settings Management:**
- Enable/disable features
- Set app version
- Manage feature flags
- Maintenance mode
- Update notices
- Download limits

**Monitoring:**
- Real-time active users
- Download statistics
- Storage usage
- Error tracking
- Performance metrics

**Maintenance:**
- Database backup
- Clean up orphaned files
- Cache clearing
- Log rotation
- Update checks

### 10. Security Measures

**PIN Protection:**
- Hash PIN with bcrypt
- Rate limiting (5 attempts = 15 min lockout)
- Session timeout (30 minutes)
- Biometric as secondary auth (optional)
- Log all admin actions
- IP address tracking

**Firebase Rules:**
```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /reciters/{document=**} {
      allow read: if request.auth != null;
      allow write: if request.auth.token.isAdmin == true;
    }
    match /admin_logs/{document=**} {
      allow write: if request.auth.token.isAdmin == true;
      allow read: if request.auth.token.isAdmin == true;
    }
  }
}
```

### 11. Backup System

**Automatic Backups:**
- Daily backup at 2 AM
- 7-day retention
- Compress backup files
- Store in Firebase Storage
- Email notification on completion

**Backup Contents:**
- Reciter data
- Audio URLs
- App settings
- Admin logs
- User statistics

**Restore Process:**
- Select backup date
- Preview data
- Confirm restore
- Rollback option

### 12. Testing Checklist

- [ ] Admin PIN login works
- [ ] Session timeout works
- [ ] Failed attempts limit works
- [ ] Reciter CRUD operations work
- [ ] Audio upload works
- [ ] Audio replace works
- [ ] Audio delete works
- [ ] Statistics load correctly
- [ ] Backup triggers successfully
- [ ] Restore from backup works
- [ ] Admin logs recorded properly
- [ ] Firebase sync works
- [ ] Settings save/load correctly
- [ ] Feature flags work
- [ ] Biometric login works

### 13. User Experience Flow

1. Admin enters PIN → Authenticates
2. Dashboard loads → Shows statistics
3. Admin clicks "Manage Reciters"
4. Admin sees all 7 reciters
5. Admin clicks "Edit" on reciter
6. Admin updates details and saves
7. Admin clicks "Manage Audio"
8. Admin selects reciter and surah
9. Admin uploads audio file
10. Progress shows upload status
11. Verification completes
12. Admin views statistics
13. Admin triggers backup
14. Admin logs out

### 14. Notifications & Alerts

**In-App Alerts:**
- Upload completion
- Storage threshold warnings
- System errors
- User reports
- Failed backups

**Admin Notifications:**
- Email on backup completion
- Alert on storage > 80%
- Error notifications
- Critical system issues

## 📊 Deliverables

- ✅ AdminService with full API
- ✅ PIN authentication system
- ✅ Admin dashboard UI
- ✅ Reciter management interface
- ✅ Audio upload/management
- ✅ Statistics dashboard
- ✅ Firebase integration
- ✅ Backup system
- ✅ Admin logging
- ✅ Unit & integration tests

## 🎯 Success Criteria

- PIN authentication secure
- All CRUD operations work
- Audio uploads reliably
- Statistics accurate
- Backups complete daily
- No data loss
- Admin panel intuitive
- Logging comprehensive
- Session management secure
- Performance acceptable

## 📝 Notes

- Secure PIN transmission (HTTPS only)
- Implement rate limiting
- Regular security audits
- Encrypt sensitive data
- Monitor for suspicious activity
- Maintain detailed audit logs
- Implement database transactions
- Test recovery procedures

---
**Phase**: 2F  
**Depends On**: Phase 2A ✅, 2B ✅, 2C ✅, 2D ✅, 2E ✅  
**Priority**: Critical  
**Estimated Time**: 2-3 weeks
