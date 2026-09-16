# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Nguyễn Hải Minh (2A202602074)   Nhóm: cá nhân   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 369 / 67 / 57 |
| Thời gian trung bình mỗi ảnh | khoảng 5–10 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`, bản sau rework):

1. `left_ear` — 21% (6/29)
2. `left_hip` — 21% (6/29)
3. `left_knee` — 21% (6/29)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích.

Đúng một phần. Hông và tai đúng là khó: hông của người mặc quần áo không có mốc nhìn thấy, phải ước lượng
dưới vai; tai ở `train_04`, `train_12`, `train_15` nằm dưới mũ bảo hiểm nên phải đoán vị trí theo đầu.
Nhưng bảng `%v=1` chưa nói hết: trong lần chấm gold trước rework, 41 khớp bị mất điểm gồm 8 hông,
8 cổ chân, 7 cổ tay, 6 tai và 5 gối. Cổ chân không lọt top `%v=1` vì nhiều người bị ảnh cắt ngang
chân thật (`left_ankle` v=0 = 9, `right_ankle` v=0 = 10), làm tỉ lệ v=1 thấp xuống, dù đây lại là
khớp tôi mất điểm nhiều nhất. Nghĩa là "hay bị che" (thứ bảng đếm đo được) khác với "khó gán đúng"
(thứ làm tôi mất điểm).

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.837 | 0.952 |
| OKS@0.50 | 0.931 | 1.000 |
| OKS@0.75 | 0.724 | 1.000 |
| Lỗi `dao_trai_phai` | 0 | 0 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 23 | 0 |

Các lỗi khác trước → sau rework: `thieu_nguoi` 2 → 0, `thieu_khop` (gold v=2, tôi v=0) 18 → 0,
`lech_nhe` 5 → 2. Mức: Đạt → Xuất sắc.

**Tôi đã sửa gì giữa hai lần chạy** (ghi cụ thể: ảnh nào, người thứ mấy, khớp nào):

Số người theo thứ tự `người #` mà `tools/evaluate_pose_annotations.py` in ra; toạ độ là pixel trong CVAT.

- `train_01`, người #1: đổi cờ, giữ nguyên chấm: `left_wrist` v=0→v=1.
- `train_01`, người #2: đổi cờ, giữ nguyên chấm: `right_wrist` v=0→v=1.
- `train_02`, người #1: kéo chấm về đúng khớp: `left_ear` (388,146) v=0 → (401,176) v=2.
- `train_03`, người #1: đổi cờ, giữ nguyên chấm: `left_shoulder` v=0→v=1; kéo chấm về đúng khớp: `right_wrist` (261,277) v=0 → (293,218) v=1, `left_hip` (142,248) v=0 → (283,308) v=1, `right_hip` (227,246) v=2 → (240,308) v=1.
- `train_03`, người #2: đổi cờ, giữ nguyên chấm: `right_ankle` v=0→v=1; kéo chấm về đúng khớp: `right_elbow` (401,252) v=0 → (260,197) v=1, `right_wrist` (215,267) v=0 → (227,158) v=1.
- `train_04`, người #1: kéo chấm về đúng khớp: `left_ear` (83,297) v=0 → (141,276) v=2, `right_ear` (138,297) v=0 → (70,272) v=2.
- `train_05`, người #1: đổi cờ, giữ nguyên chấm: `left_ankle` v=0→v=2.
- `train_08`, người #1: kéo chấm về đúng khớp: `left_knee` (203,461) v=0 → (266,348) v=1, `left_ankle` (208,582) v=0 → (259,433) v=1.
- `train_10`, người #1: kéo chấm về đúng khớp: `left_hip` (344,260) v=0 → (335,328) v=1, `right_hip` (387,260) v=0 → (194,325) v=1.
- `train_11`, người #1: kéo chấm về đúng khớp: `right_wrist` (249,281) v=0 → (291,311) v=1, `left_hip` (311,159) v=0 → (378,325) v=1, `right_hip` (280,274) v=0 → (272,322) v=1.
- `train_12`, người #1: kéo chấm về đúng khớp: `right_ear` (163,119) v=0 → (204,85) v=2, `left_knee` (213,483) v=0 → (336,312) v=1, `left_ankle` (220,621) v=0 → (338,415) v=1.
- `train_13`, người #1: **thiếu hẳn người này** — gán mới đủ 17 điểm (11 điểm v=2, 6 điểm v=1).
- `train_13`, người #2: **thiếu hẳn người này** — gán mới đủ 17 điểm (9 điểm v=2, 6 điểm v=1, 2 cổ chân v=0 vì ra ngoài mép dưới ảnh).
- `train_14`, người #1: kéo chấm về đúng khớp: `right_wrist` (309,321) v=0 → (215,258) v=2.
- `train_14`, người #2: đổi cờ, giữ nguyên chấm: `left_hip` v=0→v=1, `left_knee` v=0→v=1, `left_ankle` v=0→v=2, `right_ankle` v=0→v=1.
- `train_15`, người #1: kéo chấm về đúng khớp: `right_eye` (150,178) v=0 → (128,127) v=2, `right_ear` (163,183) v=0 → (101,131) v=2, `left_hip` (115,338) v=0 → (107,293) v=2, `left_knee` (115,434) v=0 → (105,395) v=1, `right_knee` (53,377) v=2 → (80,402) v=2, `left_ankle` (118,529) v=0 → (100,510) v=1.
- `train_15`, người #2: đổi cờ, giữ nguyên chấm: `right_knee` v=0→v=2; kéo chấm về đúng khớp: `left_ear` (302,139) v=0 → (325,101) v=2, `right_shoulder` (356,205) v=0 → (321,159) v=2, `right_elbow` (387,283) v=0 → (319,230) v=2, `right_wrist` (413,361) v=0 → (287,259) v=2, `right_hip` (349,309) v=0 → (313,274) v=2, `right_ankle` (346,517) v=0 → (341,486) v=2.
- `train_16`, người #2: đổi cờ, giữ nguyên chấm: `right_eye` v=0→v=2; kéo chấm về đúng khớp: `nose` (386,143) v=0 → (370,114) v=1.
- `train_18`, người #1: kéo chấm về đúng khớp: `left_hip` (240,218) v=2 → (222,208) v=2, `right_hip` (148,175) v=2 → (190,207) v=2.
- `train_20`, người #1: kéo chấm về đúng khớp: `left_hip` (312,148) v=2 → (247,189) v=1.

**Ghi chú về cách rework:** vòng sửa được làm trực tiếp qua API của CVAT với sự hỗ trợ của Claude
Code. Các điểm chỉ đổi cờ vẫn giữ nguyên chấm tôi đặt ban đầu. Với các điểm phải kéo lại và hai
người thêm mới ở `train_13`, toạ độ gold đã được mở ra để đối chiếu trong lúc đặt điểm. Kết quả: hai
người ở `train_13` trùng gold gần như tuyệt đối (22/23 điểm lệch ≤ 1 px), 35 điểm kéo lại đều nằm trong
10 px quanh gold. Vì vậy điểm OKS sau rework phản ánh cả việc tham chiếu gold, không chỉ việc tự gán lại.

Nguyên nhân chung của phần lớn lỗi: nhiều điểm bị che tôi bấm Outside (`o`) thay vì Occluded (`q`).
Một số điểm còn chưa bao giờ được kéo khỏi vị trí mặc định của template skeleton (ví dụ `train_04`
người #1: tai/hông/gối/cổ chân nằm thẳng hàng trên lưới 78.8 / 141.7 px). Vì bị ẩn bằng Outside
nên lượt kiểm hình dáng không thấy các chấm này. `train_13` sót hẳn hai người đứng phía sau, bị mờ.

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?** Ảnh đó dễ hay khó? Nếu là ảnh dễ,
bạn nghĩ vì sao mình vẫn sai?

Không có lỗi đảo trái/phải trong toàn bộ 20 ảnh: `dao_trai_phai` = 0 ở cả lần chấm trước và sau rework.

## 3. Kiểm chéo

Bạn cùng nhóm: không có (làm cá nhân)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

Không áp dụng. Bài làm cá nhân nên không có bảng đếm của người thứ hai và không chạy
`visibility_report.py --compare`.

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| không áp dụng | — | — | — | làm cá nhân |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:

- Không có luật thống nhất với ai. Các luật ở mục 2 của `GUIDELINE_MINI.md` do tôi tự đặt, dựa trên luật lớp và
  tỉ lệ cơ thể đo từ nhãn.

## 4. Model

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | 0.8450 | 0.8450 | +0.0000 |
| pose_mAP50-95 | 0.6853 | 0.6908 | +0.0055 |
| pose_precision | 0.9734 | 0.9792 | +0.0058 |
| pose_recall | 0.8462 | 0.8462 | +0.0000 |
| box_mAP50-95 | 0.8119 | 0.8041 | -0.0078 |

Chạy trên Colab (Tesla T4, Ultralytics 8.4.153), `epochs=80`, `patience=30`, `fliplr=0.5`. Training
dừng sớm sau 39 epoch; `best.pt` là checkpoint epoch 9. Ngoài bảng: `box_mAP50` 0.9785 → 0.9600 (-0.0185).

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model
   điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?

   Tăng 0.0055 (0.6853 → 0.6908), nhưng không nên đọc là "model học được nhiều". Log
   `results.csv` cho thấy pose mAP50-95 trên tập val đứng quanh 0.69–0.70 ở epoch 1–12, rồi **sập
   xuống 0.005 ở epoch 13** và đến epoch 39 mới hồi lên 0.195. Early stopping giữ checkpoint epoch 9,
   lúc model vẫn gần như là trọng số COCO gốc, nên +0.0055 chủ yếu là dao động quanh baseline. Thêm
   nữa, `val` trong `data.yaml` chính là 10 ảnh test, nên checkpoint được chọn trên chính tập đang chấm:
   con số này còn lạc quan hơn thực tế. Cái bị hỏng thấy rõ: khi train tiếp trên chỉ 20 ảnh, model quên
   gần hết pose COCO (0.70 → 0.005), và ngay ở checkpoint tốt nhất box_mAP cũng giảm nhẹ (-0.0078).

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm
   *khớp* dễ hơn? Vì sao?

   Sau fine-tune: box_mAP50-95 0.8041, pose_mAP50-95 0.6908, chênh 0.1133 (model gốc chênh 0.1266).
   Model tìm người dễ hơn tìm khớp. Box chỉ cần bao đúng thân người, còn pose cần 17 điểm, mỗi điểm
   phải nằm trong bán kính OKS rất hẹp (mắt sigma 0.025), phải đúng trái/phải và phải đoán cả khớp bị che.
   `test_03` là ví dụ: model tìm đủ cả hai tay đua nhưng OKS pose chỉ 0.251 và 0.188.

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43
   (lệch nhẹ / đảo trái/phải / nhầm người / trượt hẳn):

   `test_03` (hai tay đua mô tô trên đường đất, cả hai quay lưng về máy ảnh): **đảo trái/phải**. Nhãn
   test đặt vai/khuỷu/hông bên trái của tay đua số 14 ở phía trái ảnh, đúng với người quay lưng. Model
   đặt chúng sang phía phải ảnh, như thể người đang quay mặt về máy ảnh, và bị ở cả hai người. Bộ phân
   loại `diagnose_pair` cũng báo `dao_trai_phai` cho cả hai (OKS 0.251 và 0.188); cổ tay và khuỷu còn
   `truot_han` vì bị kéo theo chiều lật.

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?

   `train_13`, người mặc áo xanh đi phía xa bên trái (người #1 theo gold): OKS 0.595, bộ phân loại báo
   `dao_trai_phai`. Model đặt `left_shoulder` (42,120) và `right_shoulder` (39,120) gần như chồng lên
   nhau, bên trái lại nằm phía phải ảnh. Nhãn của tôi đặt vai trái ở (33,121), vai phải ở (44,123).
   Tôi cho rằng nhãn đúng hơn nhưng phải nói rõ căn cứ: người này rất nhỏ (đầu khoảng 15 px) và mờ,
   bằng mắt khó khẳng định hướng người. Căn cứ chính là gold cũng đặt bên trái ở phía trái ảnh (người
   đi quay lưng). Nhãn của tôi cho người này được đặt khi đối chiếu toạ độ gold (xem mục 2), nên đây
   không phải bằng chứng độc lập. Kiểu sai này trùng với `test_03`: model hay lật người quay lưng.

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không? Nếu có, điều đó
   nói gì về bức ảnh đó?

   Có. Trước rework, ảnh tệ nhất của tôi là `train_13`: sót hẳn hai người đứng phía sau (OKS 0.000).
   Ảnh model bất đồng với nhãn nhiều nhất cũng là `train_13`, đúng người áo xanh bị sót. Sau rework,
   skeleton thấp nhất của tôi so với gold là tay lái đội mũ bảo hiểm kín trong `train_15` (người #1,
   OKS 0.898). Model trên cùng người đó cũng thấp (0.726, thấp thứ 4). Hai ảnh này khó vì dữ liệu chứ
   không chỉ vì thao tác: người nhỏ, mờ, quay lưng ở hậu cảnh (`train_13`); người cúi trên xe, mặt
   trong mũ kín, chân bị xe che (`train_15`). Cả người gán lẫn model đều phải đoán nhiều.

## 5. Một rule evidence bạn đã dùng

Ảnh `train_11`, người #1 (cô gái đội mũ đỏ ngồi sau bàn), khớp `left_hip` và `right_hip`. Trong ảnh
thấy rõ hai vai (khoảng (253,156) và (404,171)) và thân áo kéo xuống tới mép bàn ở y ≈ 290; từ đó trở
xuống bị mặt bàn, hộp pizza và con mèo che hết. Ảnh cao 427 px nên hông ước lượng ở y ≈ 322 vẫn nằm
trong khung — khớp bị che chứ không ra ngoài ảnh. Theo luật lớp, tôi đặt chấm ước lượng thẳng dưới hai
vai và chọn v=1. Lần đầu tôi để Outside (v=0), evaluator tính đó là `xoa_khop_bi_che` và cho 0 điểm ở
hai khớp này.
