# Tài Liệu Định Hướng Sản Phẩm (Product Strategy & Scope)
**Dự án:** Lowca Ecosystem

**Đối tượng đọc:** Team Business Analyst (BA), Team Developer

**Mục đích:** Clear Scope & Business Rules trước khi viết PRD cho 3 Use Cases mới.

---

## Use Case 1: Hệ thống Gamification (Nhiệm vụ lấy Voucher)
**[💡 Core Model: Reward System / Quest Engine]**

### 1. Product Strategy (Chiến lược sản phẩm):
- **Mục tiêu:** Tăng DAU (Daily Active Users), tạo thói quen (Habit-forming), rút ngắn hành trình On-site Verification và điều hướng luồng tiền của khách hàng.
- **Cơ chế cốt lõi (Core Engine):** Hệ thống là một Rule-Engine. System tạo Quest -> User nhận Quest -> User thực hiện Key Action -> System Validate -> Dispatch Reward (Voucher/Lowca Coin).
- **Tính linh hoạt (Scalability):** Chìa khóa của tính năng này là Marketing/Admin phải tự tạo được quest linh hoạt (vd: "Mua 2 ly trà đá", "Check-in tại 1 xe đẩy trong top 10") thông qua CMS, *tuyệt đối không để Dev phải hardcode* mỗi khi có campaign mới.

### 2. Khung Requirement dành cho BA (Scope):
- **CMS/Admin Side:** Tool tạo Quest (Action metrics, Condition trigger, Reward value, Budget limit, Timeline).
- **App Side:** UI Quest Hub (Voucher level, Daily Quest, Special event), Progress Bar (thanh tiến độ thời gian thực).
- **Backend/Logic:** Hệ thống Event-listener (Lắng nghe các API call từ App để update tiến độ quest của user).

### 3. Socratic Gate (Các Business Rules BA cần làm rõ với PM):
1. **Validation Trigger:** Sự kiện (Event) nào được coi là "Hoàn thành"? (Ví dụ: Nhiệm vụ "Đặt 1 đơn", thì quest hoàn thành lúc user *vừa thanh toán xong*, hay lúc *nhận hàng thành công*, hay lúc *hoàn tất review*?).
2. **Các loại quest có thể xây dựng:** Cách tạo, cách hoạt động, luồng tiếp cận các quest của user (theo dịp đặc biệt thì thường sẽ có quest như thế nào, có thể có nhiều quest cùng lúc không, hay có apply các voucher cứng user nào cũng có thể thực hiện không?)
3. **Các loại voucher user có thể dùng:** Các loại voucher người dùng thích nhất là gì, các hoạt động họ hay làm để gây nghiên "tham gia" -> "có voucher" -> lan toả Lowca

---

## Use Case 2: Tính năng Current Pick (Đặt món nhóm)
**[💡 Core Model: Real-time Synchronized Cart & Conflict Handling]**

### 1. Product Strategy (Chiến lược sản phẩm):
- **Mục tiêu:** Tăng giá trị trung bình trên một đơn hàng (AOV), giải quyết nỗi đau "gom đơn, chia tiền, đòi tiền" của dân văn phòng hoặc nhóm bạn.
- **Cơ chế cốt lõi:** Host mở Session (Phòng) -> Bắn Link/QR -> Guest Join -> Sync Giỏ hàng Real-time -> Chia tiền -> Checkout.

### 2. Khung Requirement dành cho BA (Scope):
- **Session Management:** Vòng đời của một phòng (Bắt đầu, Timeout, Khóa phòng, Hủy phòng).
- **Cơ sở hạ tầng (Backend):** Sử dụng Web-socket hoặc Polling để giỏ hàng nhảy số trực tiếp trên máy của tất cả thành viên khi có ai đó add món.
- **UX/UI Side:** Trạng thái "Ready" (Sẵn sàng chốt), UI chia tiền Bill split (Ai ăn gì trả nấy, hoặc chia đều).

### 3. Socratic Gate (Các Business Rules BA cần làm rõ với PM):
1. **Quyền thanh toán (Payment Flow):** Có mấy luồng thanh toán? *Luồng 1:* Chỉ Host được thanh toán giùm cả nhóm. *Luồng 2:* Từng người tự nạp thanh toán phần của mình vào hệ thống, xong hết thì app mới báo cho Vendor làm món?

---

## Use Case 3: Thuật toán đấu giá Keyword (Vendor Search Ads)
**[💡 Core Model: Quality Score x Max CPC]**

### 1. Product Strategy (Chiến lược sản phẩm):
- **Mục tiêu:** Tạo nguồn doanh thu trực tiếp (Ads Monetization) cho Lowca, giúp các Vendor đường phố có cơ hội "leo top" tìm kiếm.
- **Cơ chế cốt lõi:** `Ad Rank = Bid Price (Giá tiền đấu thầu) × Quality Score (Điểm chất lượng)`.
  - *Điểm chất lượng* tính bằng: Rate sao, Khoảng cách (On-site verification), Tỉ lệ hủy đơn rác.
- **Phân phối Ads:** Ads chỉ hiển thị trên khung tỷ lệ nhất định (ví dụ: Slot thứ 2 và thứ 5 trong trang kết quả tìm kiếm).

### 2. Khung Requirement dành cho BA (Scope):
- **Vendor App Side:** Dashboard nạp tiền (Top-up balance), Chọn Keyword đấu giá, Setup Daily Budget (Ngân sách ngày), Báo cáo hiệu năng (Lượt xem, Click, Chuyển đổi).
- **End-user App:** Bắt badge "Tài trợ/Ad" để tuân thủ pháp luật về quảng cáo & UX.
- **Backend Search Engine:** API tìm kiếm bóc tách: 80% Organic Search + 20% Sponsored Search, sort theo `Ad Rank`.

### 3. Socratic Gate (Các Business Rules BA cần làm rõ với PM):
1. **Cost Model (Mô hình tốn tiền):** Vendor bị trừ tiền dựa trên Cost-Per-Click (CPC) hay Cost-Per-Mille (CPM)?
2. **Auction Logic (Cơ chế đấu giá):** Áp dụng *First-price auction* (Trừ đúng số tiền đặt cược) HAY *Second-price auction* (Trừ số tiền bằng người đứng sau + 1đ)?
3. **Keywords Exact/Broad match:** Thuật toán search quảng cáo sẽ đánh theo từ khóa chính xác (Exact match) hay mở rộng (Broad match)?
