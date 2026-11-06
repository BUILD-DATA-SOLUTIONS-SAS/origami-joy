# review_notes — Origami Joy (Google Play: Testing instructions / App access)

> Mapping từ mẫu App Store Connect sang Play Console. Marketing URL = landing; Support URL = trang legal của store.

## URLs
- **Marketing URL (landing):** https://build-data-solutions-sas.github.io/origami-joy/  *(đang là index legal; landing marketing sẽ được publish từ `landing.json` bằng `tools/landing_publish.sh`)*
- **Support / legal:** Privacy https://build-data-solutions-sas.github.io/origami-joy/privacy/ · Terms https://build-data-solutions-sas.github.io/origami-joy/terms/
- **Developer:** BUILD DATA SOLUTIONS SAS · chandlerpiper499410@gmail.com

## App access (Play Console → App content → App access)
**Không cần tài khoản/đăng nhập/mã.** Reviewer mở app là dùng ngay; mọi mẫu đều mở (không paywall, không IAP). Chọn "All functionality is available without special access".

## Testing instructions (dán vào Console)
1. Mở app, bấm Next qua 4 màn onboarding (có thể có màn chọn ngôn ngữ/consent quảng cáo trước đó).
2. Home → "Start folding" (mẫu Whale, 6 bước) → xem sơ đồ từng bước, bấm "Next step"; ở cuối bấm "Finish & celebrate".
3. Với mẫu có video (vd **Cute Cat**, Discover → chọn mẫu → Start fold → bấm "Watch this step") clip chạy đúng đoạn của bước đó.
4. Màn chúc mừng: "Take photo" (xin quyền CAMERA — tùy chọn) hoặc "From gallery"; ảnh lưu trong tab Creations (trên máy).
5. Tab **Game**: "Play level 1" — gập 4 góc (vuốt/chạm nút) để lộ ảnh; nhận coin (kiếm trong game, không bán bằng tiền thật).
6. Profile: đổi ngôn ngữ (12), Appearance (Light/Dark/System), bật/tắt Daily reminder (xin quyền thông báo — tùy chọn).
App cần Internet để tải mẫu/ảnh/clip từ CDN. App có quảng cáo (Google AdMob + mediation, UMP consent cho EEA/UK). Không cần 2 thiết bị, không cần tài khoản test, không có nội dung do người dùng chia sẻ.

## Định vị (đồng hook với listing)
- **Mục đích:** hướng dẫn gấp giấy origami / 3D origami / kirigami từng bước, có clip video cho đúng bước khó trên nhiều mẫu.
- **Đối tượng cụ thể:** người mới tập gấp giấy và người chơi thủ công giấy muốn một hướng dẫn rõ hơn sơ đồ tĩnh; người lớn và thiếu niên gấp cùng nhau (general audience, KHÔNG nhắm riêng trẻ em).
- **Pain-point → cách giải:** "kẹt ở một bước" vì sơ đồ mờ/ít bước → mỗi bước có sơ đồ lớn + số bước + thanh thumbnail nhảy bước, và (với mẫu có video) nút "Watch this step" phát đúng đoạn clip của bước đó; không phải xem lại cả video.
- **Khác biệt:** clip cắt theo bước (không phải 1 video dài), mọi mẫu mở sẵn, không tài khoản, mini-game Mini Fold (gập để lộ ảnh) thay cho "kiếm tiền trong app", 12 ngôn ngữ.
- **Trung thực:** chỉ khoảng 60/145 mẫu có video (53 origami + 7 3D origami; 39 kirigami và 46 origami chỉ có sơ đồ) — listing nói "many models", không "every".
