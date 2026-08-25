# Phase 2B: Multiple Reciters + Audio

## 🎧 Objective
Integrate 7 professional Qur'an reciters with high-quality audio files and reciter selection functionality.

## 📋 Tasks

### 1. Reciter Data Setup

Create `Reciter` model:
```dart
class Reciter {
  final int id;
  final String name;
  final String nameArabic;
  final String countryOfOrigin;
  final String biography;
  final String imageUrl;
  final String audioQuality; // 64kbps, 128kbps, 320kbps
  final List<String> surahs; // URLs for each surah
  final bool isActive;
  final DateTime dateAdded;
}
```

### 2. Reciters to Integrate

| # | Reciter Name | Country | Quality |
|---|---|---|---|
| 1 | Mishary Alafasy | Kuwait | 128kbps |
| 2 | Abdul Basit | Egypt | 64kbps, 128kbps |
| 3 | Abdul Rahman Al-Sudais | Saudi Arabia | 128kbps |
| 4 | Mahmoud Al-Husary | Egypt | 128kbps |
| 5 | Saad Al-Ghamdi | Saudi Arabia | 128kbps |
| 6 | Abu Bakr Al-Shatri | Saudi Arabia | 128kbps |
| 7 | Maher Al-Muaiqly | Saudi Arabia | 128kbps |

### 3. Audio Storage Structure

**Firebase Storage Architecture:**
```
audio/
├── mishary_alafasy/
│   ├── 001.mp3 (Surah Al-Fatiha)
│   ├── 002.mp3 (Surah Al-Baqarah)
│   └── ... (001-114)
├── abdul_basit/
│   └── ... (same structure)
├── abdul_rahman_al_sudais/
│   └── ... (same structure)
└── ... (other reciters)
```

**Naming Convention:**
- Format: `{surah_number}.mp3`
- Padded: `001.mp3`, `002.mp3`, ... `114.mp3`
- Full URL: `https://firebasestorage.googleapis.com/...`

### 4. Audio Service Implementation

```dart
// audio_service.dart
class AudioService {
  // Initialize audio session
  Future<void> initializeAudio() { }
  
  // Load audio for specific reciter and surah
  Future<void> loadAudio(String reciterId, int surahNumber) { }
  
  // Playback controls
  Future<void> play() { }
  Future<void> pause() { }
  Future<void> resume() { }
  Future<void> stop() { }
  
  // Seeking
  Future<void> seek(Duration position) { }
  
  // Playback speed (0.5x, 0.75x, 1x, 1.25x, 1.5x, 2x)
  Future<void> setPlaybackSpeed(double speed) { }
  
  // Repeat functionality
  void setRepeatMode(RepeatMode mode); // OFF, ONE, ALL
  
  // Progress tracking
  Stream<Duration> get positionStream;
  Stream<Duration> get durationStream;
  
  // Background playback
  Future<void> enableBackgroundPlayback() { }
  
  // Lock screen controls
  Future<void> updateLockScreenMetadata(
    String title,
    String surah,
    String artist,
  ) { }
  
  // Volume control
  Future<void> setVolume(double volume) { }
  
  // Cleanup
  Future<void> dispose() { }
}
```

### 5. Reciter Selection UI

**Reciter Picker Widget:**
- Display grid/list of 7 reciters
- Show reciter photo
- Show reciter name (Arabic + English)
- Show country of origin
- Show current selection indicator
- Save preference to SharedPreferences

### 6. Audio Player Widget Enhancement

**Current Player + New Features:**
- ⏯️ Play/Pause button
- ⏮️ Previous surah
- ⏭️ Next surah
- 🔄 Repeat (OFF → ONE → ALL)
- 🔊 Volume slider
- ⚡ Playback speed selector
- 📊 Progress bar with time display
- 🎵 Reciter info display (name, surah, ayah)
- 🔒 Lock-screen controls
- 📱 Background playback support

### 7. Database Schema Update

Add to SQLite:
```sql
CREATE TABLE reciters (
  id INTEGER PRIMARY KEY,
  name TEXT NOT NULL,
  name_arabic TEXT,
  country TEXT,
  biography TEXT,
  image_url TEXT,
  audio_quality TEXT,
  is_active BOOLEAN DEFAULT 1,
  date_added TIMESTAMP
);

CREATE TABLE reciter_surahs (
  id INTEGER PRIMARY KEY,
  reciter_id INTEGER,
  surah_number INTEGER,
  audio_url TEXT,
  file_size INTEGER,
  FOREIGN KEY(reciter_id) REFERENCES reciters(id)
);

CREATE TABLE playback_preferences (
  id INTEGER PRIMARY KEY,
  reciter_id INTEGER,
  playback_speed REAL DEFAULT 1.0,
  volume REAL DEFAULT 1.0,
  repeat_mode TEXT DEFAULT 'OFF',
  FOREIGN KEY(reciter_id) REFERENCES reciters(id)
);

CREATE TABLE playback_history (
  id INTEGER PRIMARY KEY,
  reciter_id INTEGER,
  surah_number INTEGER,
  last_position_ms INTEGER,
  timestamp TIMESTAMP,
  FOREIGN KEY(reciter_id) REFERENCES reciters(id)
);
```

### 8. Reciter Metadata (JSON)

**File: `assets/quran/reciters_metadata.json`**
```json
{
  "reciters": [
    {
      "id": 1,
      "name": "Mishary Alafasy",
      "name_arabic": "مشاري العفاسي",
      "country": "Kuwait",
      "bio_en": "Modern clear recitation",
      "bio_ar": "تلاوة واضحة وحديثة",
      "image_url": "assets/images/reciters/mishary_alafasy.png",
      "audio_quality": "128kbps",
      "birth_year": 1976,
      "style": "Modern, clear, precise"
    },
    ...
  ]
}
```

### 9. Audio Streaming Integration

**Streaming Service:**
- Stream from Firebase Storage
- Cache recently played surahs
- Support resume from last position
- Handle network interruptions
- Fallback to local files (Phase 2C)

### 10. Background Playback Implementation

```dart
// Enable background playback
class BackgroundPlaybackService {
  // Setup audio session for background play
  Future<void> setupAudioSession() async {
    await AudioSession.instance.then((session) async {
      await session.configure(AudioSessionConfiguration(
        avAudioSessionCategory: AVAudioSessionCategory.playback,
        avAudioSessionCategoryOptions: AVAudioSessionCategoryOptions.duckOthers,
        avAudioSessionMode: AVAudioSessionMode.default_,
        avAudioSessionRouteSharingPolicy:
            AVAudioSessionRouteSharingPolicy.defaultPolicy,
        androidAudioAttributes: const AndroidAudioAttributes(
          contentType: AndroidAudioContentType.music,
          flags: AndroidAudioFlags.noDuck,
          usage: AndroidAudioUsage.media,
        ),
        androidWillPauseWhenDucked: true,
      ));
    });
  }
  
  // Keep screen-off playback active
  Future<void> enableScreenOffPlayback() async {
    // Use WakelockPlus to prevent sleep during playback
  }
}
```

### 11. Lock Screen Controls

**Platforms:**
- **iOS**: Use AVPlayer with MediaPlayer framework
- **Android**: Use MediaSession with notification
- **Display**: Surah name, reciter name, progress

### 12. Testing Checklist

- [ ] All 7 reciters load correctly
- [ ] Audio plays smoothly (no stuttering)
- [ ] Reciter selection persists across sessions
- [ ] Playback speed changes work properly
- [ ] Repeat modes cycle correctly
- [ ] Previous/Next surah buttons work
- [ ] Progress bar updates in real-time
- [ ] Seeking works accurately
- [ ] Background playback works on both platforms
- [ ] Lock screen shows correct metadata
- [ ] Screen-off playback continues
- [ ] Volume controls work
- [ ] Audio resumes from last position
- [ ] Network interruption handling works
- [ ] Memory usage is optimal

### 13. Performance Optimization

- Lazy load audio files (don't preload all 114)
- Cache current and next surah
- Optimize audio bitrate (128kbps recommended)
- Implement efficient memory management
- Monitor battery consumption
- Test with low bandwidth connections

### 14. Admin Audio Management

Prepare for Phase 2F:
- Store audio URLs in Firestore
- Track audio file status (active/inactive)
- Support audio replacement workflow
- Monitor storage usage

### 15. User Experience

**Flow:**
1. User opens app → Default reciter plays (Mishary Alafasy)
2. User taps reciter selector → See all 7 options
3. User selects reciter → Audio switches instantly
4. User controls playback (play, pause, seek, speed)
5. User taps next → Loads next surah from same reciter
6. App remembers preferences → Loads last reciter next launch

### 16. Resources

- **just_audio**: https://pub.dev/packages/just_audio
- **audio_session**: https://pub.dev/packages/audio_session
- **Audio Quality Standards**: 128kbps MP3 for balance
- **Firebase Storage**: Setup CDN for faster delivery
- **Reciter Audio Sources**: 
  - Quran.com API
  - Everyayah.com
  - Custom encoded files

## 📊 Deliverables

- ✅ 7 Reciter profiles with metadata
- ✅ Audio streaming from Firebase
- ✅ Complete player with all controls
- ✅ Background playback working
- ✅ Lock screen controls
- ✅ Playback preferences saved
- ✅ Playback history tracking
- ✅ Unit & integration tests

## 🎯 Success Criteria

- User can select from 7 reciters
- Audio plays without interruption
- Player UI shows all controls (play, pause, speed, repeat, seek)
- Background playback works (screen off)
- Lock screen shows metadata
- Playback preferences persist
- No audio quality degradation
- Smooth transitions between surahs
- Fast audio loading (<2s)

## 📝 Notes

- Use high-quality audio files (minimize compression artifacts)
- Optimize for mobile data usage
- Test with various network conditions
- Ensure proper audio licensing/rights
- Support both WiFi and cellular streaming

---
**Phase**: 2B  
**Depends On**: Phase 2A ✅  
**Priority**: High  
**Estimated Time**: 3-4 weeks
