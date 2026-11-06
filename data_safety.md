# Data Safety — Origami Joy (`com.paper.folding.origamijoy`)

Nguồn bằng chứng: `compliance_census.md` (merged manifest `prodDebug`, `libs.versions.toml`, grep source) + trang Privacy live
`https://build-data-solutions-sas.github.io/origami-joy/privacy/` (§2.1). Import: Play Console → App content → Data safety → Import CSV `data_safety.csv`.

## Khai báo tổng
| Câu hỏi | Trả lời | Bằng chứng |
|---|---|---|
| Thu thập/chia sẻ dữ liệu bắt buộc? | **Yes** (`COLLECTS_PERSONAL_DATA=true`) | Firebase Analytics/Crashlytics/Config/FCM + module-ads (AdMob + mediation) |
| Mã hoá khi truyền | **Yes** | mọi SDK dùng TLS; CDN HTTPS |
| Tạo tài khoản | **None** (`PSL_ACM_NONE`) | `grep signIn\|FirebaseAuth\|GoogleSignIn app/src/main` = 0 |
| Người dùng yêu cầu xoá dữ liệu | **Yes** + URL = Privacy §9 `…/privacy/` | không có account; dữ liệu app ở máy; SDK → email store |
| Families / child-directed | Không điền (general audience) | order/census |

## Loại dữ liệu đã khai (cột "Response value" của CSV)
| Data type (Play) | Collected | Shared | Bắt buộc? | Mục đích | Bằng chứng SDK |
|---|---|---|---|---|---|
| Device or other IDs (Advertising ID, Firebase installation ID, FCM token) | ✓ | ✓ (AdMob/mediation/attribution) | Required | Advertising · Analytics · App functionality · Fraud prevention (collect); Advertising · Analytics (share) | `com.google.android.gms.permission.AD_ID`, `ACCESS_ADSERVICES_AD_ID`, module-ads, Firebase Analytics + Messaging (`OrigamiMessagingService.kt`) |
| App activity → App interactions | ✓ | ✓ (ad SDK đo impression/click) | Required | Analytics · Advertising | Firebase Analytics (`core/Telemetry.kt`), module-ads |
| Location → Approximate location (suy ra từ IP bởi SDK; app KHÔNG xin quyền vị trí) | ✓ | ✓ | Required | Advertising · Analytics | Privacy §2.1 "IP address and approximate location derived from it"; AdMob/Firebase |
| App info & performance → Crash logs | ✓ | ✗ (service provider) | Required | Analytics · App functionality | Firebase Crashlytics |
| App info & performance → Diagnostics | ✓ | ✓ (ad SDK) | Required | Analytics · App functionality (collect); Analytics (share) | Crashlytics / ad SDK performance |

**Không khai** (grep = 0 / on-device): name, email, user-id/account, phone, address, precise location, contacts, messages, **photos/videos**
(ảnh thành phẩm chụp/chọn từ gallery chỉ lưu `filesDir/creations` trên máy, không upload — Privacy §2.4), audio, files, health, financial/purchases
(`com.android.vending.BILLING` chỉ là hạ tầng thư viện ads cho cờ ad-free; không có màn mua — Privacy §3 nói rõ).
Dữ liệu chỉ ở máy (favourites, progress, coins, settings) = không "collected" theo định nghĩa Play.

## Đối chiếu Data Safety ↔ Privacy Policy ↔ Listing (Play cross-check)
- Mọi type trong CSV đều có dòng trong Privacy §2.1 (Advertising ID/device IDs; IP+approx location; app interactions; crash logs & diagnostics; Firebase installation ID & push token). ✓
- Privacy §2.4 + §2.2: ảnh không upload ↔ CSV không khai Photos ✓. Listing nói "photos live in Creations on your device" ✓.
- Listing nêu "contains ads" + "analytics and crash reports" ✓.

## Lệch so với brief (ghi rõ để người submit quyết)
Brief đề xuất "KHÔNG location". Privacy policy (do legal sinh) liệt kê **IP-derived approximate location** cho ad SDK/Firebase ⇒ để Data Safety khớp Privacy Policy
tôi khai **Approximate location (Collected+Shared)**. Nếu đội muốn bỏ dòng này thì phải sửa Privacy §2.1 cùng lúc; khai thiếu so với Privacy = rủi ro cross-check.
Trước khi Submit, người dùng nên đối chiếu lại với trang Data-safety chính thức của từng SDK phiên bản đang dùng (Adjust 5.7, Meta 18.3, TikTok 1.5 — theo CONTENT_READY).
