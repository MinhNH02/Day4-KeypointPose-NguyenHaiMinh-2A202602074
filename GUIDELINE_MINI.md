# Mini guideline - nhóm: cá nhân  |  người gán: Nguyễn Hải Minh  |  ngày: 16/09/2026

> Điền file này **trong lúc** gán nhãn, không phải sau khi xong. Mỗi lần bạn dừng lại
> hơn 10 giây để phân vân, đó là một dòng phải ghi vào đây.

## 1. Luật bắt buộc (đã thống nhất cả lớp - không sửa)

- Bộ 17 điểm COCO, đúng tên, đúng thứ tự. Lấy từ file `.SVG` chung.
- Mọi người trong ảnh đều có **đủ 17 điểm**. Điểm không dùng được thì gắn cờ, không xoá.
- Trái/phải tính theo **cơ thể người**, không theo bức ảnh.
- Bị che, còn trong khung -> `v = 1`, **vẫn đặt chấm** ở vị trí ước lượng.
- Ra ngoài mép ảnh -> `v = 0`, **không** đặt chấm.
- Không dùng `Hidden` (`h`) - nó không được lưu vào file.

## 2. Luật của tôi (làm cá nhân)

Tỉ lệ cơ thể dùng để ước lượng bên dưới được đo từ chính các nhãn `v=2` trong 20 ảnh train và 10 ảnh
test (lấy trung vị; trong ngoặc là khoảng chứa 50% số người ở giữa):

- Vai → hông (chiều dọc) ≈ **1.42 × bề rộng vai** (1.31–1.71), đo trên 24 người.
- Bề rộng hai hông ≈ **0.9 × bề rộng vai** (0.72–1.07), đo trên 24 người.
- Cẳng chân ≈ **0.9 × đùi** (0.79–1.15), đo trên 39 chân.
- Cẳng tay ≈ **0.8 × cánh tay trên** (0.58–0.99), đo trên 59 tay.

| Tình huống | Luật tôi chọn | Vì sao |
| --- | --- | --- |
| Hông của người mặc quần áo dài | Vẫn thấy được vùng thắt lưng–đùi qua quần áo → `v=2`, đặt ở nếp gập giữa thân và đùi. Hông bị **vật khác** che hẳn (bàn, xe, người khác, áo khoác dài trùm qua đùi) → `v=1`, ước lượng từ trung điểm hai vai: đi thẳng xuống 1.42 × bề rộng vai, hai hông cách nhau 0.9 × bề rộng vai. `v=0` chỉ khi điểm ước lượng rơi ra ngoài ảnh. | Quần áo che da nhưng không che vị trí: dáng thân và nếp gập đùi vẫn xác định được hông. Nhãn test phát sẵn cũng để hông `v=2` ở 9/13 người, `v=1` ở 4/13. Nếu mọi hông mặc quần đều là `v=1` thì cờ `v=1` mất nghĩa "bị vật che". |
| Tai bị tóc hoặc mũ bảo hiểm che một phần | Đầu còn trong khung thì tai **không bao giờ** là `v=0`. Thấy được vành tai hoặc dái tai → `v=2`. Tóc hoặc mũ che hết → `v=1`, đặt ngang tầm mắt ở chỗ đầu/mũ phình rộng nhất. Tai phía khuất (đầu quay đi) → `v=1`, đặt đối xứng với tai phía gần qua trục mũi–gáy. | Đúng luật lớp: bị che mà còn trong khung là `v=1`. Mũ bảo hiểm cho biết chắc vị trí tai nhưng không cho thấy tai, nên không được là `v=2`; nếu không, model học rằng "mặt bên của mũ = tai nhìn thấy". |
| Người bị cắt ở mép ảnh (chỉ thấy từ hông trở lên) | Quyết bằng **toạ độ ước lượng**, không bằng cảm giác "có thấy không": kéo dài xương đang thấy theo tỉ lệ (cẳng chân ≈ 0.9 đùi, cẳng tay ≈ 0.8 cánh tay trên, hông cách vai 1.42 bề rộng vai). Điểm rơi **ngoài ảnh** → `v=0`, tick Outside, không đặt chấm. Điểm rơi **trong ảnh** nhưng bị che → `v=1`. | `v=0` loại khớp khỏi OKS. Để `v=0` cho khớp còn trong khung chính là lỗi "xoá khớp bị che" (bài tôi có 23 lỗi này trước rework). Quyết bằng toạ độ thì hai người gán cho ra cùng kết quả. |
| Cổ tay nằm sau tay lái / sau thân mình | Cổ tay khuất sau tay lái, sau vật đang cầm (khay, túi) hoặc sau thân → `v=1`, đặt trên đường kéo dài của cẳng tay từ khuỷu, dài ≈ 0.8 cánh tay trên, dừng ở chỗ bàn tay đang cầm. Thấy được cổ tay hoặc cổ găng tay trên tay lái → `v=2`. | Cổ tay là khớp tôi mất điểm nhiều thứ ba trước rework (7 khớp). Hướng cẳng tay và vật đang cầm cho biết khá chắc vị trí, nên luôn đặt được chấm. |
| Hai người chồng lên nhau | Gán **xong trọn người đứng trước** rồi mới sang người đứng sau. Khớp của người sau bị người trước che → `v=1`, chấm nằm trên đường xương **của chính người sau**, được phép rơi lên pixel của người trước. Gán xong bật đường nối: không có xương nào nối sang khớp của người kia; tay bắt chéo thì lần từ vai ra, không lần từ bàn tay vào. | Tránh lỗi nhầm người. Ở `train_03` (hai người đứng sát, tay đưa chéo), model báo `nham_nguoi` đúng ở `right_elbow`/`right_wrist` người #1 (người mặc áo khoác da, không đội mũ), cho thấy chỗ này rất dễ nối nhầm. |
| Người nhỏ đến mức nào thì không gán nữa | Gán mọi người phân biệt được đầu và thân, **kể cả mờ, ngoài vùng nét ở hậu cảnh**. Ngưỡng dừng: người cao dưới khoảng 50 px trong ảnh gốc thì ghi thành một ca mơ hồ ở mục 3, không bỏ im lặng. Trước khi sang ảnh khác, quét lại ba lớp tiền cảnh / trung cảnh / hậu cảnh. | Sót một người là OKS 0 cho người đó và dạy model rằng người ở hậu cảnh "không phải người". `train_13` tôi sót hai người ở phía sau (cao ≈ 150 px và ≈ 265 px): họ không nhỏ mà chỉ bị mờ. |

Với mỗi luật, chèn **một ảnh mẫu** (screenshot từ CVAT) thay vì chỉ viết một câu.
Slide 12 nói rõ: khớp không có bề mặt nhìn thấy được thì phải có ảnh mẫu, không phải
một câu văn chung chung.

### Ảnh mẫu

Ảnh chụp từ canvas CVAT (task Keypose, project Day4_keypose), đúng như CVAT hiển thị nhãn đã lưu: mỗi khớp
một màu theo label, điểm Occluded (`v=1`) có viền nét đứt, điểm Outside (`v=0`) không được vẽ.

**Hông:** `train_08`, hông `v=2` trên quần jeans (vẫn thấy nếp gập đùi).

![Hông v=2 - train_08](reports/screenshot/train_08.jpg)

**Hông:** `train_11`, hông `v=1` vì bị bàn che hết.

![Hông v=1 - train_11](reports/screenshot/train_11.jpg)

**Tai dưới mũ bảo hiểm:** `train_20`, hai tai `v=1` đặt ở hai bên mũ, ngang tầm mắt.

![Tai - train_20](reports/screenshot/train_20.jpg)

**Người bị cắt ở mép ảnh:** `train_01`, mép dưới cắt ngang gối cả hai người, nên cổ chân `v=0`, không vẽ.

![Mép ảnh - train_01](reports/screenshot/train_01.jpg)

**Cổ tay khuất:** cũng `train_01`, hai cổ tay khuất dưới khay pizza, `v=1` (hai điểm viền nét đứt trên khay).

![Cổ tay - train_01](reports/screenshot/train_01.jpg)

**Hai người chồng nhau:** `train_13`, nửa trái của người áo polo khuất sau người mặc vest: `v=1`,
chấm nằm trên đường xương của người áo polo.

![Chồng người - train_13](reports/screenshot/train_13.jpg)

**Người mờ ở hậu cảnh:** cũng `train_13`, người áo xanh bên trái cao ≈ 150 px, bị mờ nhưng vẫn gán đủ 17 điểm.

![Người nhỏ - train_13](reports/screenshot/train_13.jpg)

## 3. Ba ca mơ hồ đã gặp (bắt buộc, ghi ít nhất 3)

### Ca 1 - ảnh `train_16`, người thứ `2`, khớp `nose`

- Mơ hồ ở chỗ nào: Cầu thủ áo trắng số 15 đang bật nhảy bắt đĩa, đầu ngửa lên và quay ra sau. Trong
  ảnh chỉ thấy tóc và má phải, không còn pixel nào của mũi. "Không thấy gì" rất dễ bị hiểu thành
  Outside.
- Bạn quyết thế nào: `v=1`, đặt chấm phía trước và hơi dưới mắt phải, tại (370, 114).
- Vì sao: Cả đầu nằm trọn trong ảnh. Mũi bị chính đầu người đó che, chứ không ra khỏi khung. Lần đầu
  tôi để `v=0` và evaluator tính đúng ca này là "xoá khớp bị che".
- Nếu người khác quyết ngược lại thì model học sai cái gì: Để `v=0` thì model học rằng người quay đi
  "không có mũi", và sẽ bỏ các khớp đầu khi gặp người quay lưng. Khớp `v=0` bị loại khỏi OKS nên lỗi
  này không hiện ra trên điểm số.

### Ca 2 - ảnh `train_13`, người thứ `2`, khớp `left_shoulder`, `left_elbow`, `left_wrist`

- Mơ hồ ở chỗ nào: Người áo polo vàng đi phía sau người mặc vest, cả nửa trái bị người mặc vest che.
  Có hai điểm khó: (1) không còn pixel nào của tay trái nên dễ chọn Outside; (2) chấm ước lượng sẽ
  nằm lên người mặc vest, dễ thành nhầm người.
- Bạn quyết thế nào: `v=1`. Đặt đối xứng với tay phải đang thấy qua trục giữa thân: vai (163, 77),
  khuỷu (176, 124), cổ tay (178, 172). Sau đó bật đường nối để chắc skeleton không nối vào khớp của
  người mặc vest.
- Vì sao: Tay trái vẫn nằm trong khung, chỉ bị người khác che, nên theo luật là `v=1`. Tay phải đối
  xứng là căn cứ nhìn thấy duy nhất để ước lượng.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Để `v=0` thì model học rằng người đứng sau
  bị mất tay. Nối chấm vào khớp của người mặc vest thì model học gộp hai người thành một skeleton.

### Ca 3 - ảnh `train_04`, người thứ `1`, khớp `left_hip`, `right_hip`

- Mơ hồ ở chỗ nào: Người bên trái mặc hoodie xám, đội mũ kín, ngồi trên xe. Mép dưới ảnh cắt ngang
  ngực và tay lái, thân áo kéo sát tới mép. Không rõ hông còn trong ảnh mà bị xe che (`v=1`), hay đã
  ra ngoài khung (`v=0`).
- Bạn quyết thế nào: `v=0`, không đặt chấm.
- Vì sao: Vai nằm ở y ≈ 315–332 px, bề rộng vai ≈ 171 px. Theo tỉ lệ vai → hông 1.42 thì hông ở
  y ≈ 566. Kể cả lấy tỉ lệ thấp 1.31 thì vẫn là y ≈ 547, đều nằm ngoài ảnh cao 457 px. Tức là hông đã
  ra khỏi khung, không phải bị che. Gold cũng để `v=0` ở hai hông của người này.
- Nếu người khác quyết ngược lại thì model học sai cái gì: Để `v=1` thì chấm hông bị kẹp vào mép
  dưới ảnh, ngang ngực người. Model sẽ học kéo hông lên cao với người ngồi trên xe.

## 4. Sau khi so visibility report với bạn cùng nhóm

Không áp dụng: bài làm cá nhân, không có bạn cùng nhóm nên không có bảng đếm thứ hai để so.

- Khớp lệch `%v=1` nhiều nhất: không áp dụng (làm cá nhân)
- Nguyên nhân là **guideline chưa rõ** hay **một trong hai bên gán sai**: không áp dụng (làm cá nhân)
- Luật mới bổ sung vào mục 2 sau khi thống nhất: không có; toàn bộ luật ở mục 2 do tôi tự đặt, dựa trên luật lớp ở mục 1 và tỉ lệ đo từ nhãn.
