# Visibility report

- Thư mục nhãn: `dataset\labels\train`
- 20 ảnh, 29 skeleton, trung bình 15.03 khớp có v > 0 mỗi người
- Tổng: v=2 369 | v=1 67 | v=0 57

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 5 | 2 | 17% |
| 1 | left_eye | 20 | 4 | 5 | 14% |
| 2 | right_eye | 23 | 4 | 2 | 14% |
| 3 | left_ear | 17 | 6 | 6 | 21% |
| 4 | right_ear | 20 | 4 | 5 | 14% |
| 5 | left_shoulder | 27 | 2 | 0 | 7% |
| 6 | right_shoulder | 28 | 1 | 0 | 3% |
| 7 | left_elbow | 24 | 3 | 2 | 10% |
| 8 | right_elbow | 26 | 2 | 1 | 7% |
| 9 | left_wrist | 21 | 5 | 3 | 17% |
| 10 | right_wrist | 22 | 5 | 2 | 17% |
| 11 | left_hip | 22 | 6 | 1 | 21% |
| 12 | right_hip | 23 | 5 | 1 | 17% |
| 13 | left_knee | 19 | 6 | 4 | 21% |
| 14 | right_knee | 23 | 2 | 4 | 7% |
| 15 | left_ankle | 16 | 4 | 9 | 14% |
| 16 | right_ankle | 16 | 3 | 10 | 10% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
