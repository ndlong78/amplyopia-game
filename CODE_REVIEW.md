# Đánh giá mã nguồn `amplyopia-game`

## Tổng quan nhanh
- Dự án hiện là **1 file tĩnh `index.html`** (HTML + CSS + JS inline), chạy được nhanh cho MVP và không cần build step.
- Luồng game rõ ràng: Home → Game → Result, có 4 mode (`catch`, `puzzle`, `trace`, `pop`) và có state tập trung trong object `S`.

## Điểm mạnh
1. **MVP rất gọn**: mở file là chạy, không phụ thuộc framework.
2. **Responsive hợp lý**: có xử lý portrait/landscape và resize để tính lại vùng chơi.
3. **UX cơ bản tốt**: có timer, score, progress, feedback ngay khi người chơi thao tác.
4. **Cấu trúc JS dễ đọc**: chia theo khối chức năng (timer, build UI, game loop, end session).

## Vấn đề quan trọng (ưu tiên cao)
1. **Lỗi ký tự CSS nghiêm trọng**  
   Trong phần CSS đang dùng dấu `–` (en dash) thay vì `--` cho custom properties và dùng quote kiểu “smart quote”. Điều này làm nhiều khai báo CSS có thể bị trình duyệt bỏ qua, ảnh hưởng toàn bộ giao diện.

2. **Inline event handlers**  
   Nhiều phần tử dùng `onclick="..."` trực tiếp. Cách này khó mở rộng, khó test, và kém an toàn/maintain so với đăng ký sự kiện bằng `addEventListener`.

3. **Không có lớp tách domain logic**  
   Logic game, rendering, state update dồn vào một file script. Khi thêm mode mới sẽ dễ phát sinh regression.

4. **Thiếu kiểm thử tự động**  
   Chưa có unit/integration test cho các hàm cốt lõi như tính accuracy/progress, timer lifecycle, và spawn target.

## Rủi ro với use-case y tế (nhược thị)
- Nội dung có khuyến nghị bác sĩ, nhưng chưa có **guardrails** kỹ thuật như:
  - giới hạn thời lượng mặc định theo nhóm tuổi,
  - pause bắt buộc sau mỗi khoảng thời gian,
  - lưu lịch sử luyện tập để bác sĩ/theo dõi phụ huynh đánh giá tiến triển.
- Chưa có phần **disclaimer rõ ràng** về việc đây là công cụ hỗ trợ, không thay thế phác đồ điều trị.

## Đề xuất roadmap cải thiện (theo mức effort)
### 1) Nhanh trong ngày (Quick wins)
- Chuẩn hóa lại toàn bộ CSS token (`--var`) và quote ASCII thường.
- Bỏ inline `onclick`, chuyển sang binding sự kiện trong JS.
- Tách constants (`EMOJIS`, `SHAPES`, session config) vào module/khối riêng.

### 2) 1–2 ngày
- Tách mã thành 3 lớp: `state`, `engine`, `ui`.
- Thêm localStorage để lưu lịch sử buổi chơi (điểm, độ chính xác, thời lượng, mode).
- Thêm test cơ bản cho:
  - `accuracy = hits/attempts`,
  - điều kiện end session,
  - logic spawn target không vượt biên field.

### 3) 3–5 ngày
- Chuyển sang TypeScript + bundler nhẹ (Vite) để có type-safety và test runner chuẩn.
- Thêm telemetry tối thiểu (không chứa dữ liệu nhạy cảm) để đo retention và mức hoàn thành buổi tập.
- Chuẩn hóa accessibility: keyboard support, aria labels, contrast checks.

## Kết luận
- Đây là bản MVP có ý tưởng tốt và gameplay đã thể hiện được mục tiêu “kích hoạt mắt yếu”.
- Tuy nhiên cần xử lý ngay lỗi ký tự CSS và refactor cấu trúc sự kiện/state trước khi mở rộng tính năng hoặc đưa vào sử dụng rộng rãi.
