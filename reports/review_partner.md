# Review - tự kiểm (làm cá nhân)

> Bài làm cá nhân, không có bài của bạn cùng nhóm để kiểm chéo. File này dùng đúng
> `reports/REVIEWER_CHECKLIST.md` để **tự kiểm chính bài đã nộp** (nhãn trong `dataset/labels/train/`,
> sau rework). Người gán và người kiểm là cùng một người, nên đây không thay thế được kiểm chéo độc lập.

Người gán: Nguyễn Hải Minh   Người kiểm: Nguyễn Hải Minh (tự kiểm)   Ngày: 16/09/2026

Đã chạy trước khi soi bằng mắt:

```bash
python tools/check_pose_labels.py --images dataset/images/train --labels dataset/labels/train
python tools/visualize_pose.py --images dataset/images/train --labels dataset/labels/train --out <thư mục tạm>
python tools/visibility_report.py --labels dataset/labels/train --out outputs/visibility_report.json --markdown reports/visibility_report.md
python tools/evaluate_pose_annotations.py --pred dataset/labels/train --gold gold/labels/train --images dataset/images/train --out outputs/eval_vs_gold.json
```

Không chạy `visibility_report.py --compare` vì không có bài thứ hai để so.

## Reviewer checklist

| | Mục kiểm | Đạt? | Ghi chú / ảnh nào |
| --- | --- | --- | --- |
| 1 | Mọi người trong ảnh đều có đủ 17 điểm, không ai bị thiếu | ☑ | 29/29 skeleton đủ 17 điểm; so với gold thiếu 0 người. Trước rework thiếu 2 người ở `train_13`, đã gán bổ sung. |
| 2 | Bật đường nối: không có xương nào cắt chéo ở vai hoặc hông | ☑ | `dao_trai_phai` = 0 trong `outputs/eval_vs_gold.json`. |
| 3 | Không có xương nào kéo dài sang một cơ thể khác | ☑ | `nham_nguoi` = 0 so với gold. Model fine-tune có báo `nham_nguoi` ở `train_03` người #1 (`right_elbow`, `right_wrist`), nhưng gold xác nhận nhãn đúng. |
| 4 | Khớp bị che dùng `v = 1` **và có chấm**, không phải `v = 0` | ☐ | **Không đạt:** 31 khớp bị che nhưng còn trong khung vẫn để `v=0` (20 khớp mắt/mũi/tai, 11 khớp thân). Chi tiết ở bảng lỗi. |
| 5 | `v = 0` chỉ xuất hiện ở khớp thật sự ra ngoài mép ảnh | ☐ | **Không đạt:** cùng 31 khớp trên. 26 khớp `v=0` còn lại đúng là ra ngoài khung (cổ chân `train_01`, `train_07`, `train_13`; hông, gối, cổ chân `train_04` người #1; gối, cổ chân `train_04` người #2 và `train_10`; cổ chân `train_11`). |
| 6 | Không có dấu hiệu dùng `Hidden` (điểm `v = 2` nằm ở chỗ vô lý) | ☑ | So với gold không còn khớp "trượt hẳn". Trước rework có 2 chấm `v=2` ở chỗ vô lý (`train_18` `right_hip` nằm ngang tầm vai; `train_20` `left_hip` nằm trên yếm xe), đã sửa. Các khớp gold không gán thì không đối chiếu được bằng gold. |
| 7 | Export đúng **COCO Keypoints 1.0**: mảng `keypoints` có 51 số mỗi người | ☑ | `annotations/coco_keypoints/person_keypoints_default.json`: 29 người, mỗi người 51 số, 17 tên khớp. |
| 8 | Bản YOLO Pose: mỗi dòng 56 số, `kpt_shape: [17, 3]` | ☑ | 20 file, mỗi dòng 56 số; `data.yaml` có `kpt_shape: [17, 3]`. Convert lại từ file export ra giống hệt. |
| 9 | Visibility report đã nộp, và hai bảng đã được đặt cạnh nhau | ☐ | Một phần: đã nộp `reports/visibility_report.md`, nhưng không có bảng thứ hai để đặt cạnh (làm cá nhân). |
| 10 | Mọi ca không rõ đều được ghi trong `GUIDELINE_MINI.md` | ☑ | 6 luật có ảnh mẫu và 3 ca mơ hồ. Lưu ý: luật tai/mặt bị che đã ghi nhưng nhãn chưa sửa theo (lỗi nhóm A, C bên dưới). |
| 11 | `check_pose_labels.py` chạy 0 lỗi | ☑ | 0 lỗi, 5 cảnh báo `v=0` ở người nằm gọn trong ảnh (`train_02`, `04`, `10`, `11`, `14`), trùng với lỗi ở mục 4. |

## Lỗi tìm được

Số thứ tự người theo `người #` mà `tools/evaluate_pose_annotations.py` in ra. Các lỗi này **chưa được
sửa** trong bài đã nộp.

**Nhóm A: mắt, mũi, tai bị che nhưng đầu còn trong khung, lại để `v=0` (20 khớp)**

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_02` | 1 | `nose`, `left_eye`, `right_eye`, `right_ear` | Người đạp xe quay lưng, mặt khuất nhưng đầu trong khung → để `v=0` là xoá khớp bị che | Trong CVAT đặt chấm ước lượng: mắt, mũi ở phía trước đầu theo hướng mặt; tai ngang tầm mắt ở mép sau đầu. Tick Occluded (`q`) → `v=1` |
| `train_03` | 2 | `right_ear` | Tai phía khuất sau đầu, để `v=0` | Đặt đối xứng với tai trái qua trục mũi–gáy, tick Occluded → `v=1` |
| `train_04` | 2 | `left_ear`, `right_ear` | Tai trong mũ bảo hiểm kín, để `v=0` | Đặt ở hai bên mũ chỗ phình rộng nhất, ngang tầm mắt, tick Occluded → `v=1` |
| `train_07` | 1 | `left_ear` | Tai phía khuất, để `v=0` | Đặt đối xứng với tai phải, tick Occluded → `v=1` |
| `train_12` | 1 | `left_ear` | Tai phía khuất dưới mũ bảo hiểm, để `v=0` | Đặt đối xứng với tai phải, tick Occluded → `v=1` |
| `train_13` | 3 | `left_ear` | Tai phía khuất của người mặc vest, để `v=0` | Đặt đối xứng với tai phải, tick Occluded → `v=1` |
| `train_14` | 1 | `nose`, `left_eye`, `right_eye` | Người đứng quay lưng, mặt khuất nhưng đầu trong khung, để `v=0` | Đặt ước lượng phía trước đầu, dưới vành ô, tick Occluded → `v=1` |
| `train_14` | 2 | `right_ear` | Tai phía khuất, để `v=0` | Đặt đối xứng với tai trái, tick Occluded → `v=1` |
| `train_15` | 1 | `left_eye`, `left_ear` | Nửa mặt phía khuất trong mũ bảo hiểm kín, để `v=0` | Đặt đối xứng với mắt, tai phải qua trục mũi–gáy, tick Occluded → `v=1` |
| `train_15` | 2 | `right_ear` | Tai phía khuất dưới mũ bảo hiểm, để `v=0` | Đặt đối xứng với tai trái, tick Occluded → `v=1` |
| `train_16` | 1 | `left_eye`, `left_ear` | Nửa mặt khuất khi quay đầu, để `v=0` | Đặt đối xứng với mắt, tai phải, tick Occluded → `v=1` |
| `train_16` | 2 | `left_eye` | Mắt phía khuất khi ngửa đầu, để `v=0` | Đặt đối xứng với mắt phải, tick Occluded → `v=1` |

**Nhóm B: khớp thân bị che nhưng còn trong khung, lại để `v=0` (11 khớp)**

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_03` | 1 | `left_elbow`, `left_wrist` | Tay trái khuất sau người đội mũ bên cạnh, vẫn trong khung | Ước lượng từ vai trái xuống theo tỉ lệ cẳng tay ≈ 0.8 cánh tay trên, tick Occluded → `v=1`. Bật đường nối kiểm không nối sang người bên cạnh |
| `train_06` | 1 | `right_elbow`, `right_wrist` | Tay phải khuất sau thân và xe (người quay lưng), vẫn trong khung | Đặt khuỷu, cổ tay phải về phía tay lái bên phải, tick Occluded → `v=1` |
| `train_09` | 1 | `right_wrist`, `right_ankle` | Cổ tay phải trên tay lái và cổ chân phải ở phía khuất của xe, vẫn trong khung | Đặt cổ tay ở tay lái phải, cổ chân dưới gối phải theo tỉ lệ cẳng chân ≈ 0.9 đùi, tick Occluded → `v=1` |
| `train_11` | 1 | `left_elbow`, `left_wrist` | Tay trái khuất sau con mèo, vẫn trong khung | Ước lượng từ vai trái (≈ 404,171) xuống theo hướng thân, tick Occluded → `v=1` |
| `train_11` | 1 | `left_knee`, `right_knee` | Người ngồi sau bàn, gối dưới gầm bàn vẫn trong khung | Đặt gối phía trước hông, ngang hoặc hơi thấp hơn hông (y ≈ 330–350), tick Occluded → `v=1` |
| `train_14` | 1 | `left_wrist` | Người quay lưng, cổ tay trái khuất trước thân, vẫn trong khung | Đặt theo hướng cẳng tay từ khuỷu trái, tick Occluded → `v=1` |

**Nhóm C: tai dưới mũ bảo hiểm để `v=2`, trái với luật tai trong `GUIDELINE_MINI.md` (5 khớp)**

| Ảnh | Người thứ | Khớp | Lỗi gì | Sửa thế nào |
| --- | ---: | --- | --- | --- |
| `train_04` | 1 | `left_ear`, `right_ear` | Tai trong mũ kín, không nhìn thấy mà gắn `v=2` | Giữ nguyên chấm, tick Occluded → `v=1` (không đổi điểm OKS) |
| `train_12` | 1 | `right_ear` | Tai dưới mũ bảo hiểm gắn `v=2` | Giữ nguyên chấm, tick Occluded → `v=1` |
| `train_15` | 1 | `right_ear` | Tai trong mũ kín gắn `v=2` | Giữ nguyên chấm, tick Occluded → `v=1` |
| `train_15` | 2 | `left_ear` | Tai dưới mũ bảo hiểm gắn `v=2` | Giữ nguyên chấm, tick Occluded → `v=1` |

## Hai câu kết luận

- Lỗi lặp đi lặp lại nhiều nhất của bài này: dùng Outside (`v=0`) cho khớp bị che nhưng vẫn nằm trong
  khung. Bài nộp còn 31 khớp như vậy, 20 khớp trong số đó là mắt/mũi/tai phía khuất; trước rework
  evaluator còn đếm 23 lỗi "xoá khớp bị che" và 18 lỗi "thiếu khớp gold có" cùng gốc.
- Nó là lỗi **thao tác** hay lỗi **guideline chưa rõ**? Cả hai, nhưng gốc là thao tác. Nhiều chấm `v=0` vẫn
  nằm nguyên ở vị trí mặc định của template (chưa từng được kéo), tức đã bấm Outside thay vì ước lượng. Lúc
  gán cũng chưa có luật cho mặt/tai phía khuất và tai dưới mũ bảo hiểm; các luật này nay đã ghi ở mục 2
  `GUIDELINE_MINI.md` nhưng nhãn chưa được sửa theo.
