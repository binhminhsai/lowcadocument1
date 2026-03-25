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
Ví dụ luồng này hoạt động: Nhân dịp 30/04, Lowca ra campaign "Check-in 3 quán có trang trí dạng lịch sử như Cà phê Đỗ Phủ - Cơm tấm Đại Hàn, Cộng coffee, highland Dinh độc lập, hoặc các quán có trang trí chủ đề Việt Nam" thì sẽ được đổi voucher áo dài hoặc tượng danh lam thắng cảnh Sài Gòn" hoặc đổi ra xu để đổi quà trong cửa hàng.
=> Bước 1: Admin thêm quà tặng vào cửa hàng, option là đổi thường hay dịp đặc biệt
=> Bước 2: Admin Thêm điều kiện để đổi lấy quà
- Rule 1: Nếu là quà theo dạng đổi xu thường thì admin chọn type quà phù hợp [{loai_tiet_kiem}; {loai_trai_nghiem}; {loai_dac_quyen}] với lượng xu phù hợp
- Rule 2: Nếu là quà theo dạng dịp đặc biệt thì admin chọn type {loai_dac_biet} loại này khách hàng chỉ được nhận khi thực hiện nhiệm vụ trong một dịp và quãng thời gian nhất định
=> Bước 3: Nếu là dạng quà đặc biệt thì Admin tạo nhiệm vụ để người chơi thực hiện để đổi quà theo Rule sau:
Để Admin không cần Dev, CMS nên được thiết kế theo dạng IF - THEN:
IF (Condition Set):
Action_Type: [Check-in (người dùng chụp ảnh post lên social, post lên profile Lowca) / Payment / Review / Referral]
Scope: [Specific_Vendor_ID / Category_Tag (Loại quán ăn, Taste, style quán) / Area_ID]
Value_Requirement: [Số lượng người >= n /Số lượng quán ≥ n / 
a mount (số lượng chữ, hình ảnh trong review, lượt tương tác...
giá tiền, khoảng cách)  >= x or < =x (km)]
Time_Constraint: [Start_Date / End_Date / Specific_Hours / Lặp lại theo ngày]

THEN (Reward Set):
Reward_Type: [Special_Item / Coin / Voucher_Code]
=> Total_Stock: Giới hạn tổng số lượng quà (ví dụ: chỉ có 100 áo dài).
=> Winner_Selection: [First come first served (Ai đến trước nhận trước)] hoặc [Lucky Draw (Quay số) hoặc không giới hạn].
Exclusivity: Chỉ những User đạt Level x hoặc có Badge y mới được tham gia Quest này để tăng tính "Đặc quyền".
=> Bước 4: Admin tạo UI/UX cho user của Lowca thấy sự kiện
***Phần A***: Admin chỉnh sửa Visual & Header (Kích thích thị giác) cho phần thông báo hiện ra đầu tiên khi user vào LOWCA app
Business Requirement:
- Chỉnh Hero Image: Ảnh quà tặng đặc biệt (ví dụ: Hình ảnh chiếc Áo dài hoặc Tượng danh lam thắng cảnh).
- Chỉnh vị trí Countdown Timer (Time Constraint): Đồng hồ đếm ngược thời gian còn lại của sự kiện (Ví dụ: "Còn 2 ngày 14 giờ"). => Theo Start và End day
- Exclusivity Badge: Nếu Quest chỉ dành cho Level cao, hiển thị badge: "Dành riêng cho Thành viên Vàng trở lên".
***Phần B***: Progress Bar (Thanh tiến độ - Logic Bước 3)
Business Requirement:
- Nhấn vào banner thì ra trang đánh giá progress, và trang profile có page user xem lại progress
- Thanh tiến độ trực quan: Dựa trên Value_Requirement.
*****Ví dụ: Nếu quest yêu cầu check-in 3 quán, layout sẽ hiện 3 vòng tròn check-list. Vòng tròn nào đã xong sẽ sáng lên hoặc có dấu tích xanh.*****
- Status Label: "Đã hoàn thành 1/3 quán thuộc phong cách #SàiGònXưa".
***Phần C: Instruction & Map Scope (Hướng dẫn & Phạm vi)***
Business Requirement:
- Thuật toán đề xuất quán: Danh sách quán gợi ý: Dựa trên Category_Tag hoặc Area_ID. Hiển thị dưới dạng danh sách cuộn các quán ăn thỏa điều kiện để User nhấn vào chỉ đường ngay.
- Nút Action chính: Tùy vào Action_Type mà nút sẽ thay đổi khi user đang trong page cuả một vendor:
Rule 1: Nếu là Check-in: Hiện lên Nút "Chụp ảnh & Check-in ngay share cho bạn bè cùng sử dụng Lowca hoặc lên social".
Rule 2: Nếu là Review: Hiện lên Nút "Viết cảm nhận".
Rule 3: Nếu là Thanh toán thì hiện lên Nút "Đặt món ngay"
***Phần D: Reward Info (Thông tin phần thưởng - Logic THEN)***
- Chỉnh sửa lable: Số lượng còn lại (Total Stock): Một dòng chữ nhỏ: "Chỉ còn 15/100 quà tặng". Điều này đánh vào tâm lý sợ bỏ lỡ (FOMO).
- Chỉnh sửa lable: Cơ chế nhận quà (Winner Selection): "Quà tặng sẽ trao cho 100 người hoàn thành sớm nhất".
=> Bước 5: Admin preview với góc nhìn người dùng và Admin setup lịch Launch sự kiện và có thể chỉnh sửa bất kỳ lúc nào

Đó là luồng User thực hiện thử thách, sau đó mới đưọc nhận quà, còn Admin thì setup quà
trước trong Dashboard rồi sau đó mới tạo nhiệm vụ

[Link tham khảo]:{https://whimsical.com/minh-s-workspace7334/rule-engine-flexible-YMgf6DVnjXkeoHe8pDkEGn}

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
