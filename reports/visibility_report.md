# Visibility report

- Thư mục nhãn: `dataset/labels/train`
- 20 ảnh, 29 skeleton, trung bình 16.38 khớp có v > 0 mỗi người
- Tổng: v=2 361 | v=1 114 | v=0 18

| # | Khớp | v=2 | v=1 | v=0 | %v=1 |
| ---: | --- | ---: | ---: | ---: | ---: |
| 0 | nose | 22 | 7 | 0 | 24% |
| 1 | left_eye | 20 | 9 | 0 | 31% |
| 2 | right_eye | 22 | 7 | 0 | 24% |
| 3 | left_ear | 16 | 13 | 0 | 45% |
| 4 | right_ear | 19 | 10 | 0 | 34% |
| 5 | left_shoulder | 27 | 2 | 0 | 7% |
| 6 | right_shoulder | 27 | 2 | 0 | 7% |
| 7 | left_elbow | 23 | 5 | 1 | 17% |
| 8 | right_elbow | 26 | 3 | 0 | 10% |
| 9 | left_wrist | 21 | 7 | 1 | 24% |
| 10 | right_wrist | 23 | 6 | 0 | 21% |
| 11 | left_hip | 24 | 5 | 0 | 17% |
| 12 | right_hip | 25 | 4 | 0 | 14% |
| 13 | left_knee | 19 | 8 | 2 | 28% |
| 14 | right_knee | 20 | 7 | 2 | 24% |
| 15 | left_ankle | 14 | 9 | 6 | 31% |
| 16 | right_ankle | 13 | 10 | 6 | 34% |

## Đọc bảng này thế nào

1. Khớp nào có **%v=1 cao**: khớp hay bị che. Cổ tay và hông thường là hai vị trí cần xem lại guideline trước khi kết luận.
2. Khớp nào có **v=0 cao bất thường**: mọi người đang dùng Outside ở chỗ đáng lẽ là Occluded. Đó là lỗi số 3 của slide 46, và nó xoá thẳng khớp đó khỏi bảng điểm OKS.
3. Khi so hai người: **lệch lớn = bất đồng về guideline**, không phải về bức ảnh. Sửa guideline trước, sửa nhãn sau.
