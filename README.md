# NearLink 📡 — Fully Real, No Demo Mode
### Secure Offline Mesh Messaging | Kotlin + Jetpack Compose + Real BLE + Real NFC

---

## ✅ What is REAL in this version

| Feature | Status |
|---------|--------|
| BLE Scanning | ✅ Real — discovers actual nearby phones |
| BLE Advertising | ✅ Real — broadcasts as "NL-YourName" |
| BLE GATT Server | ✅ Real — receives encrypted packets |
| BLE GATT Client | ✅ Real — sends encrypted packets |
| NFC Tap Pairing | ✅ Real — reads NDEF from physical tap |
| AES-256-GCM Encryption | ✅ Real — every message encrypted |
| PBKDF2 Key Derivation | ✅ Real — 10,000 iterations |
| Multi-hop Mesh Routing | ✅ Real — 3 hop max, flood relay |
| Loop Prevention | ✅ Real — seen packet ID tracking |
| Room Database | ✅ Real — all messages stored offline |
| Hilt DI | ✅ Real — full dependency injection |
| MVVM Architecture | ✅ Real — ViewModel + Repository |
| Background BLE | ✅ Real — foreground service |

---

## 🚀 Setup Steps

### 1. Open in Android Studio
- File → Open → select `NearLinkReal` folder
- Wait for Gradle sync (~3–5 min)

### 2. Create Launcher Icons (Required before building)
- Right-click `app/src/main/res` → New → Image Asset
- Choose your icon or use the default Android robot
- Click Next → Finish

### 3. Run on Phone A
- Connect Phone A via USB
- Enable USB Debugging (Settings → Developer Options)
- Press Shift+F10 or click ▶ Run
- Grant ALL permissions when asked (Bluetooth, Location, NFC)

### 4. Run on Phone B  
- Connect Phone B via USB (use a second USB port or USB hub)
- In Android Studio, select Phone B from the device dropdown
- Click ▶ Run again

### 5. Test Real Communication

#### Via NFC:
1. Both phones open NearLink
2. Go to Settings on each phone and set your name
3. Hold phones back-to-back (NFC chips — usually near top/middle of back)
4. Phone B should show "Paired with [Phone A name] via NFC! ✓"
5. Go to Pair tab → tap the paired device → start chatting!

#### Via BLE:
1. On Phone A: Pair tab → tap Scan
2. You will see Phone B appear as "NL-[PhoneBName]"
3. Tap it to pair
4. Start chatting!

---

## 📱 How to Install APK on Both Phones Without USB

### Generate APK:
1. Build → Generate Signed Bundle/APK → APK → Next
2. Create keystore → Next → release → Finish
3. APK saved to `app/release/app-release.apk`

### Share APK:
- Send via WhatsApp / Google Drive to both phones
- On phone: Settings → Install Unknown Apps → allow
- Tap the APK to install

---

## 🎓 Presentation Script (5 Minutes)

### Minute 1 — Show the architecture
> "NearLink uses MVVM Clean Architecture. The data layer has BleManager for real Bluetooth communication, NfcManager for tap pairing, CryptoManager for AES-256-GCM encryption, and Room database for offline storage."

### Minute 2 — NFC Demo
> "Watch — I tap Phone A to Phone B. No internet involved. What just happened: Phone B read an NDEF record containing Phone A's device ID and public key. Our PBKDF2 key derivation then generated a shared AES-256 key locally on each device."
> (Show the green "Paired via NFC" message)

### Minute 3 — Send a Message
> "I type a message on Phone A. Before it leaves the app it's encrypted with AES-256-GCM — that's authenticated encryption with a 12-byte random IV. The ciphertext goes over BLE GATT to Phone B, which decrypts it with the same shared key."
> (Show message delivered with tick)

### Minute 4 — Mesh Routing  
> "The Mesh tab shows live topology. If Phone A can't reach Phone C directly, the packet hops through Phone B. We track packet IDs to prevent relay loops. Max 3 hops — extending range to approximately 30 metres indoors."

### Minute 5 — Airplane Mode Demo
> (Turn on Airplane Mode on both phones — re-enable BT and NFC manually)
> "Still works. No internet. No SIM. No server. Everything is local."

---

## 🏗️ Architecture

```
MainActivity
    ↓
MainViewModel (Hilt)
    ↓
NearLinkRepository
    ├── BleManager      → Real BLE scan + GATT server + advertising
    ├── NfcManager      → Real NFC read/write NDEF
    ├── MeshRouter      → Multi-hop packet forwarding
    ├── CryptoManager   → AES-256-GCM + PBKDF2 + SHA-256
    └── Room Database   → DeviceDao + MessageDao
```

---

## 🔐 Security Details

| Algorithm | Use | Params |
|-----------|-----|--------|
| AES-256-GCM | Message encryption | 256-bit key, 12-byte IV, 128-bit auth tag |
| PBKDF2-SHA256 | Shared key derivation | 10,000 iterations |
| SHA-256 | Public key fingerprinting | Standard |
| NFC NDEF | Key exchange transport | Mime type: application/com.nearlink.app |
| BLE GATT | Message transport | Service UUID: 0000FFF0 |

---

## ⚠️ Important Notes

1. Both phones must have **Bluetooth ON** and **Location permission granted**
2. For NFC: go to phone Settings → Connected Devices → NFC → turn ON
3. First scan may take 5–10 seconds to find the other device
4. BLE range is approximately 10m indoors
5. Mesh relay requires 3+ phones all running NearLink

---

Made with Kotlin + Jetpack Compose + Real BLE + Real NFC
Zero internet. Zero servers. Fully encrypted.
