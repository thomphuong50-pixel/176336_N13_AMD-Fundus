# Bài kiểm tra lần 2 - Nhóm 13

**Đề tài:** Phân loại thoái hóa hoàng điểm tuổi già (AMD) qua ảnh đáy mắt
**Nhóm:** 13 (176336_N13_AMD-Fundus)
**Thành viên:** 20233811

## 1. Đặt vấn đề và mục tiêu

Thoái hóa hoàng điểm tuổi già (AMD) là một nguyên nhân hàng đầu gây suy giảm
thị lực trung tâm ở người cao tuổi. Phát hiện sớm giúp can thiệp kịp thời,
đặc biệt với thể AMD ướt (tân mạch). Ảnh đáy mắt là phương tiện chụp không
xâm lấn, chi phí thấp, phù hợp để sàng lọc ở tuyến cơ sở.

Mục tiêu: xây dựng mô hình học sâu phân loại ảnh đáy mắt theo mức độ AMD:
- Bình thường
- AMD sớm / trung gian
- AMD muộn

Đầu vào là một ảnh đáy mắt màu; đầu ra là nhãn lớp kèm xác suất.
Mô hình chỉ hỗ trợ sàng lọc, không thay thế chẩn đoán của bác sĩ nhãn khoa.

## 2. Dữ liệu

- **Nguồn dữ liệu:** AREDS
- **Số lượng:** 30.000-40.0000
- **Chia tập:** train / validation / test = TỈ LỆ
- **Chống rò rỉ dữ liệu:** chia theo bệnh nhân, không chia theo ảnh, để hai mắt
  hoặc nhiều ảnh của cùng một người không nằm ở các tập khác nhau.
- **Mất cân bằng lớp:** xử lý bằng trọng số lớp trong hàm mất mát HOẶC LẤY MẪU LẠI.
- **Đạo đức dữ liệu:** dữ liệu ẩn danh, không đưa ảnh hay thông tin bệnh nhân
  thật lên repo.

## 3. Phương pháp đề xuất

- **Tiền xử lý:** cắt viền đen quanh ảnh đáy mắt, resize về KÍCH THƯỚC,
  chuẩn hóa theo thống kê ImageNet, có thể dùng CLAHE để tăng tương phản.
- **Tăng cường dữ liệu:** lật ngang, xoay nhẹ, thay đổi độ sáng và tương phản.
  Không dùng phép biến đổi làm mất dấu hiệu bệnh (drusen, tổn thương vùng
  hoàng điểm).
- **Mô hình:** TÊN BACKBONE, VÍ DỤ EfficientNet-B0 / ResNet50 pretrained trên
  ImageNet, fine-tune với đầu phân loại lớp.
- **Huấn luyện:** hàm mất mát cross-entropy có trọng số lớp, bộ tối ưu
  Adam, learning rate GIÁ TRỊ, early stopping theo validation.
- **Lý do chọn:** transfer learning phù hợp khi dữ liệu y tế hạn chế; mô hình
  nhẹ dễ triển khai ở nơi ít tài nguyên tính toán.

## 4. Đánh giá

- **Chỉ số:** accuracy, sensitivity (recall), specificity, precision, F1,
  AUC từng lớp, ma trận nhầm lẫn.
- **Ưu tiên sensitivity** cho nhóm AMD, vì bỏ sót bệnh nguy hiểm hơn báo nhầm.
- Chọn mô hình dựa trên validation; **chỉ báo cáo cuối cùng trên tập test**
  chưa từng dùng để chỉnh tham số.
- **Kết quả:**  "Chưa chạy
  thực nghiệm, đây là kế hoạch đánh giá".

## 5. Giải thích mô hình

Dùng Grad-CAM để trực quan hóa vùng ảnh mô hình chú ý. Mô hình đáng tin cần
tập trung vào vùng hoàng điểm, drusen, tổn thương, thay vì viền ảnh hay ký
hiệu của máy chụp. Nếu bản đồ nhiệt lệch khỏi vùng bệnh thì kết quả có thể
do nhiễu hoặc thiên lệch dữ liệu.

## 6. Hạn chế và rủi ro

- Dữ liệu từ ít loại máy chụp và ít quần thể, có thể giảm hiệu năng khi
  áp dụng ở nơi khác (domain shift).
- Nhãn có thể khác nhau giữa các bác sĩ đọc ảnh.
- Số ca AMD muộn ít nên dễ lệch lớp.
- Chất lượng ảnh kém (mờ, thiếu sáng) ảnh hưởng dự đoán.
- Cần kiểm chứng ngoài (external validation) trước khi dùng thực tế.

## 7. Hướng cải thiện

Bổ sung dữ liệu nhiều nguồn, kiểm chứng ngoài, thử mô hình khác (ViT, ensemble),
hiệu chỉnh xác suất (calibration), và để bác sĩ xem xét các ca có độ tin cậy thấp.
