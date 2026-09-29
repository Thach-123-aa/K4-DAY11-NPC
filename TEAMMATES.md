# Thành viên và phân vai — Day11 SVM 360 Fisheye

## 1. Thông tin nhóm

- Khóa/lớp: K4
- Tên nhóm: Thạch – Minh – Quân
- Repo Public: https://github.com/Thach-123-aa/K4-DAY11-NPC
- Máy giữ hồ sơ chính / người quản lý: Cam Vũ Ngọc Thạch (vai C)
- Slice chung lấy từ mode.json: mỗi người 1 slice riêng theo `mode --members thach,minh,quan` — Thạch: `B3-edge`, Minh: `B4-edge`, Quân: `B2-edge`
- Tên định danh vai A dùng cho `--self`: `minh`
- Kênh trao đổi nội bộ: nhóm chat lớp
- Đại diện nộp (vai C): Cam Vũ Ngọc Thạch, 2A202602067
- Commit chốt bài: điền SHA/URL khi push xong

## 2. Ba vai chính

| Vai | Họ và tên | MSSV | Tên định danh trong mode | Trách nhiệm | Bằng chứng đóng góp |
|---|---|---|---|---|---|
| A · Gán nhãn | Minh | 2A202602092 | `minh` | Gán nhãn slice `B4-edge`: box, `ego_body`, attribute, self-QC, lock (`r1-final.zip`, mã `C0E9-DE25`) | `submission/r1_craft/annotations.xml` trong repo của Minh |
| B · QA độc lập | Quân | 2A202602231 | `quan` | Review slice `B2-edge` trước reference, ghi finding QA (mã khoá `B28A-2C10`) | `submission/r2_qa/` trong repo của Quân |
| C · Chẩn đoán & điều phối | Cam Vũ Ngọc Thạch | 2A202602067 | `thach` | Gán nhãn slice `B3-edge` (đã khoá, mã `421B-0850`); QA slice `B4-edge` của Minh; chạy P4 (`reference`, `compare`, `local-quality`, `model`, `iou-sweep`), phân xử 25 finding, viết `zone_table.md`, `10_error_card.md`, `20_guideline_patch.md`, `30_escalation_ticket.md`, `45_review_plan.md`, `50_exit_ticket.md`, `45_sampling_plan.csv`, `46_gold_set_plan.md`; chạy `check` tới exit 0 | Toàn bộ `submission/` trong repo cá nhân này (`K4-DAY11-CamVuNgocThach-2A202602067`) |

Bảng này xác định vai của nhóm. Vòng QA tự sinh trong `team.json` (Minh QA Quân, Quân QA Thạch, Thạch QA Minh) thuộc quy trình nhiều hồ sơ của CLI — mỗi người vẫn giữ đủ `r1_craft`/`r2_qa`/`r3_diag` trong repo cá nhân của mình; bảng vai A/B/C ở đây mô tả thêm phân công điều phối/báo cáo cho repo nhóm.

## 3. Bàn giao theo pha

| Mốc | Người giao → nhận | File / commit / mã khóa | Người nhận đã kiểm gì? | Trạng thái / vướng mắc |
|---|---|---|---|---|
| P0 · Chốt môi trường và vai | C → A, B | `mode.json`, slice riêng từng người, phân vai A/B/C | Mỗi người xác nhận đúng slice của mình qua `python3 lab11.py status` | Xong |
| P2 · Khóa bản đầu | A → B, C | Minh: `r1-final.zip`, mã `C0E9-DE25`; Thạch (vai C, cũng là annotator slice riêng): mã `421B-0850` (đã relock sau khi bổ sung K12) | C đã chạy `qa --slice B4-edge --file ... --code C0E9-DE25` xác nhận mã khớp | Xong |
| P3 · Chốt QA mù | B → C, A | Quân review slice `B2-edge` (mã `B28A-2C10`) gửi Minh; C (Thạch) tự QA slice Minh, ghi `qa_review.md` 3 nhận xét (R01, R07, R02) | Đã đối chiếu bằng luật, chưa mở reference lúc QA | Xong |
| P4 · Quyết định sửa | C → A, B | `findings.csv` 25 dòng round=r3_diag (why/severity/owner/action), `zone_table.md` | Phát hiện 2 ca P0 (Truck h=219px, ThreeWheeler h=310px bị bỏ sót hoàn toàn ở slice Thạch) | Đã báo, xử lý ở P5 |
| P5 · Kiểm bản sửa | A → B → C | `rework/annotations-v2.xml`, `lock2.txt`, `delta.md` | Rút gọn qua `degrade rework` do hết thời gian, không giả vờ đã sửa | Đã ghi rõ ở `40_decision_log.csv` (d2) |
| P6 · Chốt nộp | A, B → C | `10_error_card.md` → `50_exit_ticket.md`, `45_sampling_plan.csv`, `46_gold_set_plan.md`, parking dùng chung theo chỉ đạo Lab Coach | C chạy `python3 lab11.py check` xác nhận exit 0 | Xong — "Hồ sơ hình thức đầy đủ" |

## 4. Bất đồng và phối hợp

- **Một ca đã phân xử:** `adasind_019560.jpg` (round `calib`) — 2 box tôi (Thạch) vẽ `Pedestrian` trùng vị trí 2 box `Bike` của reference (lệch <15px). Ý kiến ban đầu: nghi ngờ reference sai. Bằng chứng: vị trí đúng vùng rider ngồi trên xe 2 bánh, không phải người đứng riêng. Quyết định: nhận là `E1_annotator_error` — rider bị tách nhầm khỏi `Bike` theo luật R03, không đổ lỗi reference. Ghi ở `findings.csv` dòng L1/L2/R2 (round=calib).
- **Ca còn mở:** `adasind_019560.jpg` R5+R6 — 2 box `ThreeWheeler` chồng lấn mạnh trong reference, chưa rõ là 2 xe thật hay reference lỗi trùng lặp. Người theo dõi: Thạch. Phép kiểm tiếp theo: cần Lab Coach xác nhận trên ảnh gốc — đã ghi Ticket 1 trong `30_escalation_ticket.md` và decision log `40_decision_log.csv`.
- **Đóng góp của A/B/C vào kế hoạch và exit ticket:** Minh cung cấp dữ liệu gán nhãn gốc slice `B4-edge` làm căn cứ đối chiếu; Quân cung cấp `qa_review.md` với nhận xét rule-based cho slice `B2-edge`; Thạch (C) tổng hợp thành `zone_table.md`, 2 escalation ticket, kế hoạch phân bổ 200 frame 4 camera (`45_sampling_plan.csv`, `46_gold_set_plan.md`) và trả lời `50_exit_ticket.md`.
- **Thay đổi phân công nếu có:** (1) Nhóm ban đầu định để Thạch tự chuyên trách viết báo cáo, Minh chuyên annotator, Quân chuyên QA suốt buổi — sau khi rà lại hướng dẫn, quay về đúng cơ chế mỗi người tự làm đủ 3 vai trên slice riêng (rotation A→B→C→A tự sinh từ `team.json`), chỉ thêm vai trò điều phối/báo cáo tổng hợp ở cấp nhóm cho Thạch. (2) Bài `parking` (P0): theo chỉ đạo trực tiếp của Lab Coach, chỉ 1 thành viên trong nhóm cần làm, các thành viên còn lại dùng chung kết quả — ghi ở `40_decision_log.csv` dòng `d5`. (3) Rework P5 rút gọn do hết thời gian (dòng `d2`).

## 5. Xác nhận trước khi nộp

- [x] A xác nhận nhãn và export đúng phiên bản: Minh — tự xác nhận đã kiểm trong repo của Minh (báo lại qua kênh nhóm)
- [x] B xác nhận đã QA độc lập trước reference và kiểm lại ca sửa: Quân — tự xác nhận đã kiểm trong repo của Quân (báo lại qua kênh nhóm)
- [x] C xác nhận báo cáo đúng bản khóa, các file đầy đủ và check exit 0: Cam Vũ Ngọc Thạch — `python3 lab11.py check` báo "Hồ sơ hình thức đầy đủ" trong repo `K4-DAY11-CamVuNgocThach-2A202602067`
- [ ] manifest.json tại commit chốt có failed_gates rỗng.
- [ ] Repo nhóm Public, ảnh và các bằng chứng mở được.
- [ ] C đã push và gửi link repo nhóm + commit qua kênh lớp công bố.

Ghi chú: tài liệu này được lưu trong repo cá nhân của Thạch theo yêu cầu trực tiếp của Lab Coach, sau đó copy sang repo nhóm này. Xác nhận của A (Minh) và B (Quân) dựa trên báo cáo miệng/qua kênh chat của chính họ, chưa kèm bằng chứng file cụ thể trong tài liệu này — nếu cần đối chiếu, xem trực tiếp `submission/` trong repo riêng của từng người. Link repo cá nhân Public của Minh (`K4-DAY11-NguyenVuQuangMinh-2A202602092`) và Quân (`K4-DAY11-ToVanAnhQuan-2A202602231`) chưa có tài khoản GitHub cụ thể trong tài liệu này — cần bổ sung khi có.
