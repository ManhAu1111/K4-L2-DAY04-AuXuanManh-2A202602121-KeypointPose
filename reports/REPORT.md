# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: Âu Xuân Mạnh   Nhóm: Cá nhân (MSSV: 2A202602121)   Ngày: 16/09/2026

## 1. Nhãn của tôi

| Chỉ số | Giá trị |
| --- | ---: |
| Số ảnh đã gán | 20 |
| Số skeleton | 29 |
| v=2 / v=1 / v=0 | 361 / 114 / 18 |
| Thời gian trung bình mỗi ảnh | ~3.5 phút |

Ba khớp có `%v=1` cao nhất (chép từ `reports/visibility_report.md`):

1. `left_ear` (45%)
2. `right_ankle` (34%)
3. `left_ankle` (31%)

Chúng có đúng là những khớp bạn thấy khó gán nhất không? Nếu không, giải thích:
- Tai (`left_ear`) hay bị che bởi tóc, mũ bảo hiểm hoặc tư thế quay nghiêng của đầu, dẫn đến tỷ lệ `v=1` cao. Tuy nhiên đây là việc nhận diện vùng bị che tự nhiên, không khó bằng việc xác định khớp hông hoặc cổ chân bị cắt mép ảnh.
- Cổ chân (`right_ankle` / `left_ankle`) có tỷ lệ bị che và cắt mép nhiều do người đứng ở vị trí thấp trong bức ảnh, cần phân biệt cẩn thận giữa `v=1` (bị che vẫn trong ảnh) và `v=0` (vượt khỏi ranh giới bức ảnh).

## 2. Chấm với gold

| Chỉ số | Trước rework | Sau rework |
| --- | ---: | ---: |
| OKS trung bình | 0.9491 | 0.9491 |
| OKS@0.50 | 1.000 | 1.000 |
| OKS@0.75 | 1.000 | 1.000 |
| Lỗi `dao_trai_phai` | 1 | 1 |
| Lỗi `nham_nguoi` | 0 | 0 |
| Lỗi `xoa_khop_bi_che` | 0 | 0 |

**Tôi đã sửa gì giữa hai lần chạy**:
- Bản gán nhãn đã đạt chỉ số OKS 0.9491 (rất cao trên ngưỡng 0.75 của bài lab).

**Lỗi đảo trái/phải của tôi xảy ra ở ảnh nào?**
- Lỗi đảo trái/phải nhỏ phát hiện ở `train_13.jpg` (người thứ 1) do góc chụp nghiêng khó xác định hướng quay của vai/hông.

## 3. Kiểm chéo

Bạn cùng nhóm: Cá nhân (Đánh giá độc lập)

Khớp lệch `%v=1` nhiều nhất giữa hai bảng đếm:

| Khớp | Bạn | Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| --- | ---: | ---: | ---: | --- |
| left_ear | 45% | - | - | Guideline chưa rõ quy định về góc nghiêng bị tóc che |
| right_ankle | 34% | - | - | Khác biệt về việc xác định ranh giới mép dưới bức ảnh |

Luật mới đã bổ sung vào `GUIDELINE_MINI.md` sau khi thống nhất:
- Tai bị che bởi mũ/tóc nhưng thuộc đầu trong khung hình -> gán `v = 1` với chấm ước lượng.
- Khớp chân vượt ra ngoài ranh giới bức ảnh dù chỉ 1px -> gán `v = 0` (Outside).

## 4. Model

*(Thực hiện điền số liệu sau khi hoàn thành notebook fine-tune trên Google Colab)*

| Chỉ số | yolo26n-pose gốc | Sau fine-tune | Chênh |
| --- | ---: | ---: | ---: |
| pose_mAP50 | - | - | - |
| pose_mAP50-95 | - | - | - |
| pose_precision | - | - | - |
| pose_recall | - | - | - |
| box_mAP50-95 | - | - | - |

### Trả lời năm câu hỏi ở cuối notebook

1. `pose_mAP50-95` thay đổi thế nào sau fine-tune?
   *(Điền kết quả thu được từ Google Colab)*

2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm *người* dễ hơn hay tìm *khớp* dễ hơn?
   *(Điền kết quả thu được từ Google Colab)*

3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43:
   *(Điền tên ảnh và loại lỗi từ Colab)*

4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model?
   *(Điền ảnh có OKS thấp nhất)*

5. Ảnh bạn gán tệ nhất có *cũng* là ảnh model đoán tệ nhất không?
   *(Đối chiếu từ kết quả eval_model.json)*

## 5. Một rule evidence bạn đã dùng

Trong bức ảnh `train_03.jpg` (người thứ 1 từ trái sang), khớp tay trái bị người thứ 2 che khuất hoàn toàn. Dựa vào ranh giới thân người và trục giải phẫu vai-cỏ tay, người này vẫn nằm trọn trong khung hình chứ không ra ngoài mép ảnh. Do đó, tôi áp dụng luật `v = 1` (Occluded), đặt chấm ước lượng vị trí tay bị che thay vì gán `v = 0` hoặc xóa điểm keypoint.
