# Permission declarations — Origami Joy

Nguồn: merged manifest `prodDebug` (grep) + `compliance_census.md`. **Kết luận: KHÔNG có permission/foreground-service-type thuộc nhóm Play bắt khai form riêng + video demo ⇒ KHÔNG cần video demo cho Console, và KHÔNG quay video giả.**

| Permission (manifest merged) | Nguồn | Feature thấy được | Nhóm Play | Cần form/video? | Disclosure trong listing |
|---|---|---|---|---|---|
| `CAMERA` (+ `uses-feature camera required=false`) | app | Chụp ảnh thành phẩm (CameraScreen, CameraX) → Creations | runtime thường (KHÔNG thuộc restricted-declaration) | Không | "The app asks for Camera access only if you choose to take a photo of your finished model" |
| `POST_NOTIFICATIONS` | app | Daily reminder (tùy chọn, Profile) | runtime thường | Không | "notification permission only if you want the daily reminder" |
| `RECEIVE_BOOT_COMPLETED` | app | dựng lại lịch reminder sau khi khởi động | normal | Không | (hạ tầng của reminder) |
| `VIBRATE`, `WAKE_LOCK` | app/SDK | rung nhẹ khi gấp | normal | Không | — |
| `INTERNET`, `ACCESS_NETWORK_STATE`, `ACCESS_WIFI_STATE` | app/SDK | tải mẫu/clip từ CDN, ads | normal | Không | "loaded from our servers … Internet connection" |
| `com.google.android.gms.permission.AD_ID`, `ACCESS_ADSERVICES_AD_ID/ATTRIBUTION/TOPICS/CUSTOM_AUDIENCE` | module-ads/AdMob | quảng cáo | Advertising ID | **Khai ở App content → Advertising ID = Yes** (không có video) | "contains ads" |
| `com.android.vending.BILLING`, `CHECK_LICENSE` | lib ads | không có màn mua | — | Không (không khai IAP) | — |
| `FOREGROUND_SERVICE` (không kèm `foregroundServiceType`) | thư viện (Firebase/WorkManager/ads) | không feature foreground-service riêng của app | — | Không có service type nào để khai; nếu Console vẫn hỏi form Foreground service ⇒ chọn "app không dùng foreground service tác vụ nào cho người dùng" và ghi dòng này làm căn cứ | — |
| `READ_BASIC_PHONE_STATE`, `c2dm.RECEIVE`, `BIND_GET_INSTALL_REFERRER_SERVICE` | SDK | attribution/FCM | normal | Không | Privacy §3 |

**KHÔNG có**: Accessibility, overlay, UsageStats, NotificationListener, VPN, DeviceAdmin, QUERY_ALL_PACKAGES, SMS/Call-log, location (foreground/background), MANAGE_EXTERNAL_STORAGE, READ_MEDIA_*, READ_CONTACTS, mic, exact alarm. targetSdk 36 (≤36) ⇒ gate "Minimize" (policy 16909972, hiệu lực 27/01/2027) chưa áp, và app không dùng các permission đó.

## Việc người dùng cần làm ở Console (không có video)
1. App content → **Advertising ID**: Yes; mục đích Advertising/Analytics (khớp Data Safety).
2. App content → Data safety: import `data_safety.csv`.
3. Nếu Console đòi **Foreground service** declaration: xem dòng `FOREGROUND_SERVICE` ở trên (nguồn lib, không có type) — không cần video.
