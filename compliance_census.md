# compliance_census — Origami Joy (05g)

- **appCode**: 05g · **appName (listing)**: Origami Joy · **displayName (name on phone)**: Origami Joy
- **packageName**: `com.paper.folding.origamijoy` (internal java package `com.paper.folding.origamilab` — không đổi, chỉ applicationId là `...origamijoy`)
- **store**: BUILD DATA SOLUTIONS SAS [a020] · org `BUILD-DATA-SOLUTIONS-SAS` · brand `BUILD DATA SOLUTIONS SAS` · contact `chandlerpiper499410@gmail.com`
- **variant ASO**: MEADOW (AppDefaults.VARIANT=2, cố định compile-time — app con fanout chỉ mang đúng biến thể này; AppearanceScreen chỉ đổi Light/Dark/System, KHÔNG đổi palette). Mọi ảnh ASO/landing dùng Meadow (xanh lá #4CAE00/#58CC02 + cam #F5A800/#FFC800).
- **targetSdk**: 36 (`app/build.gradle.kts:76`) · minSdk 24 · versionName 1.0.0

Nguồn bằng chứng: merged manifest `app/build/intermediates/merged_manifests/prodDebug/.../AndroidManifest.xml`, `app/src/main/AndroidManifest.xml`, `app/build.gradle.kts`, `gradle/libs.versions.toml`, source `app/src/main/java/...`.

## Ma trận quyết định

| Trục | Câu hỏi | Bằng chứng | Quyết định |
|---|---|---|---|
| **IAP/billing** | Có BILLING + màn/nút MUA thật? | `com.android.vending.BILLING` + `CHECK_LICENSE` trong merged manifest. `AppPurchase` chỉ ở `GlobalApp.kt:124 initBilling(...)` + `ads/AdsLoader.kt:57 isPurchased()` gate ads. `grep launchBillingFlow\|BillingClient app/src/main = 0`; `grep .purchase(\|.subscribe( = 0`. `listIAPId=[lifetimeId]`, `listSubsId=[weekly,monthly]` chỉ khai cho module-ads. | **KHÔNG paywall/nút mua** ⇒ hạ tầng BILLING chỉ phục vụ SKU ad-free của module-ads. **KHÔNG refund page, KHÔNG khai IAP, KHÔNG gỡ lib ads** (FRICTIONLESS_ACCESS v1). |
| **Nội-dung-user** | App-là-công-cụ? Nội dung rời máy? | App hướng dẫn gấp giấy; nội dung mẫu = **BBL CDN manifest** (159 mẫu, `blueprint/survey/api/manifest_api.md`). Ảnh thành phẩm user tự chụp lưu **local** (`CameraScreen.kt`: `cacheDir/capture_*.jpg`, Creations gallery). Không endpoint upload/host. | **Local-only** ⇒ disclosure "content tải từ server" trong description + mục Copyright/IP trong Terms. **KHÔNG trang DMCA**. |
| **Data & privacy** | Permission nhạy cảm? SDK thu thập? Account/login? | SDK: Firebase analytics/crashlytics/config/messaging (`libs.versions.toml:71-74`), module-ads (AdMob GMA Next-Gen + mediation), UMP 4.0.0. FCM `OrigamiMessagingService.kt`. **`grep signIn\|login\|FirebaseAuth\|GoogleSignIn\|OAuth app/src/main = 0`** ⇒ KHÔNG account. | Privacy đúng hành vi (analytics/crash/ads). **KHÔNG account ⇒ Data Safety không user-id, KHÔNG delete-account.** Advertising ID: Shared. |
| **API nhạy cảm** | Accessibility/overlay/UsageStats/VPN/SMS/call-log/bg-location/FGS-type? | Merged manifest: **KHÔNG** có bất kỳ cái nào. `foregroundServiceType` trống. Perm nhạy cảm duy nhất: **CAMERA**. | Chỉ CAMERA cần cân nhắc (dưới). |
| **CAMERA** | Khai CAMERA — dùng thật? | `AndroidManifest.xml:20 CAMERA` + `:21 uses-feature camera required=false`. **DÙNG THẬT**: `features/camera/CameraScreen.kt` CameraX Preview+ImageCapture → crop vuông → cacheDir, hoặc gallery picker. Navmap: Congrats → Take Photo → Camera → Photo preview → Save to Creations. | **Hợp lệ, claim được.** `required=false` ⇒ máy không camera vẫn cài. KHÔNG cần permission-declaration video Console (CAMERA không thuộc nhóm phải khai). |
| **Data Safety** | Thu/chia sẻ data gì? | Firebase Analytics+Crashlytics (app activity, diag/crash), AdMob+mediation (Advertising ID → **Shared**), FCM (device token). Không account, ảnh user local. | CSV: thu ở mức analytics/diag/ad-id. App activity + Crash + Diagnostics (Collected), Advertising ID (Collected + **Shared**). KHÔNG user-id/contacts/location/media. |
| **Ads** | Có ads SDK? | module-ads (`libs.versions.toml:78`), 82 ref ads source, UMP consent. | **"Contains ads" = CÓ.** Không dùng "free" gây nhầm. |
| **Đối tượng** | Trẻ em? Social/UGC? | Hobby gấp giấy, general audience; không UGC rời máy, không social. | **Everyone / general audience.** KHÔNG child-safety. |
| **Appearance variants** | >1 layout/style? Default? | `AppDefaults.VARIANT=2` (Meadow) cố định compile-time; app con fanout mang đúng 1 biến thể. AppearanceScreen chỉ Light/Dark/System. | **CHỐT MEADOW cho toàn bộ ảnh ASO** — không mơ hồ, không dừng hỏi. |
| **Call Log/SMS gate** | Có perm call-log/SMS? | Merged manifest: KHÔNG. | **N/A.** |
| **Minimize targetSdk** | targetSdk? contacts/location/media? | targetSdk=36 (≤36). KHÔNG READ_CONTACTS/location/READ_MEDIA_*. | **Không áp.** |

## Permission đầy đủ (merged prodDebug)
INTERNET · ACCESS_NETWORK_STATE · ACCESS_WIFI_STATE · CAMERA · POST_NOTIFICATIONS · RECEIVE_BOOT_COMPLETED · VIBRATE · WAKE_LOCK · FOREGROUND_SERVICE · READ_BASIC_PHONE_STATE · ACCESS_ADSERVICES_AD_ID/ATTRIBUTION/TOPICS/CUSTOM_AUDIENCE · gms AD_ID · vending.BILLING · CHECK_LICENSE · c2dm RECEIVE (FCM) · BIND_GET_INSTALL_REFERRER_SERVICE · DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION.
→ Tất cả hạ tầng chuẩn (ads/attribution/FCM/install-referrer/billing-adfree) + CAMERA (feature thật) + POST_NOTIFICATIONS (daily reminder) + RECEIVE_BOOT_COMPLETED (reschedule reminder). KHÔNG quyền thừa.

## KẾT LUẬN — app này cần gì
- **Trang legal cần dựng**: `privacy` + `terms` (mục Copyright/IP cho nội dung CDN). **KHÔNG** cần refund/dmca/delete-account/child-safety.
- **Disclosure trong description**: (1) có quảng cáo; (2) cần Internet tải mẫu; (3) CAMERA tùy chọn chụp thành phẩm (không camera vẫn dùng gallery).
- **Mục audit**: IAP↔refund N/A; user-content↔DMCA N/A; Privacy Policy PASS bằng URL live; Data Safety CSV khớp (analytics/crash/diag + Ad-ID Shared, no user-id); CAMERA↔listing khớp; no misleading free.


## BỔ SUNG aso-expert 2026-10-29 (bằng chứng grep — sửa/làm rõ census)
- **Video clip chỉ có 60/145 mẫu**: `python3` đếm `app/src/main/assets/manifest_seed.json` → origami 53 + origami-3d 7 có `video`; origami 46 + kirigami 39 không có; 14 entry catalog `paper` (theme Mini Fold). ⇒ listing/ảnh nói "many models/video models", KHÔNG "mọi mẫu".
- **Mẫu hiển thị 145** (99 origami + 7 3D + 39 kirigami) ⇒ listing "more than 140 models".
- **Approximate location (IP-derived)**: Privacy §2.1 liệt kê; census gốc bỏ sót ⇒ Data Safety khai Approx location Collected+Shared (SDK ads/Firebase), app vẫn KHÔNG có quyền vị trí.
- **SDK attribution**: Adjust 5.7.0, Meta (Facebook) 18.3.0, TikTok Business 1.5.0 (xem legal/CONTENT_READY.md) ⇒ Device IDs "Shared" (advertising/analytics).
- **Icon thật**: launcher adaptive foreground = `drawable-nodpi/m2_icon.png` (cáo origami xanh Meadow). `blueprint/design_previews/app_icon.svg` & `screens/*` thuộc master 05d Origami Lab ⇒ KHÔNG dùng cho ASO.
- **Misbranding UI**: 37 file strings còn "Origami Lab" (rate_prompt_title, a_share_text, camera_rationale_body…).
