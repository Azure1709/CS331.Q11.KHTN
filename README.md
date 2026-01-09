# So sánh Softmax và Sigmoid Loss trong Contrastive Learning cho Ảnh Y tế

Đồ án môn học CS331 - Thị giác máy tính nâng cao.
Trường Đại học Công nghệ Thông tin - ĐHQG-HCM.

## 👥 Nhóm thực hiện
* **Hồ Ngọc Luật** - 23520900
* **Bùi Ngọc Thiên Thanh** - 23521436

## 📝 Giới thiệu
Dự án này tập trung vào việc tiền huấn luyện (pretraining) các mô hình học sâu trên dữ liệu y tế khan hiếm nhãn bằng phương pháp **conVIRT** (Contrastive learning of medical Visual Representations from Text).

Mục tiêu chính là **so sánh hiệu quả giữa Softmax Loss và Sigmoid Loss**, đồng thời đề xuất giải pháp khắc phục hiện tượng "loss collapse" của Sigmoid Loss để cải thiện biểu diễn hình ảnh y khoa.

## 📊 Dữ liệu (Datasets)

1.  **Pretraining:** [MIMIC-CXR](https://physionet.org/content/mimic-cxr/)
    * 257,873 cặp ảnh X-quang và báo cáo bệnh lý.
    * Tiền xử lý: Cắt ngẫu nhiên câu trong báo cáo, biến đổi ảnh (Resize, Crop, Flip).

2.  **Downstream Task:** [RSNA Pneumonia Detection](https://www.kaggle.com/c/rsna-pneumonia-detection-challenge)
    * Bài toán: Phân loại nhị phân (Có/Không viêm phổi).
    * Số lượng: 26,684 ảnh (77.47% Negative, 22.53% Positive).

## 📈 Kết quả (Results)

Đánh giá trên tập dữ liệu RSNA với các tỷ lệ dữ liệu huấn luyện khác nhau (1%, 10%, 100%).

| Method | 1% Data (F1/AUC) | 10% Data (F1/AUC) | 100% Data (F1/AUC) |
| :--- | :---: | :---: | :---: |
| Resnet50 ImageNet | 0.44 / 0.66 | 0.54 / 0.74 | 0.72 / 0.85 |
| Resnet50 ConVIRT Softmax | 0.50 / 0.67 | 0.67 / 0.78 | 0.73 / 0.85 |
| **Resnet50 ConVIRT Heuristic Sigmoid** | **0.68 / 0.80** | **0.71 / 0.84** | **0.75 / 0.87** |

> **Kết luận:** Mô hình sử dụng **Sigmoid Loss với Heuristic** cho kết quả tốt nhất, vượt trội hơn Softmax và ImageNet baseline, đặc biệt trong điều kiện ít dữ liệu.

## 🛠 Cài đặt & Huấn luyện


### Hyperparameters
* **Batch size:** 64 (pretrain), 32 (downstream).
* **Learning Rate:** 3e-4 (ResNet), 3e-5 (BERT).
* **Temperature ($\tau$):** 0.1 (Softmax), Trainable (Sigmoid).

### Yêu cầu (Requirements)
* Python 3.x
* PyTorch
* Pydicom (xử lý ảnh DICOM)
* Transformers (HuggingFace)

## 🔗 Tham khảo
1.  Ting Chen et al., "A simple framework for contrastive learning of visual representations," arXiv 2020.
2.  Xiaohua Zhai et al., "Sigmoid loss for language image pre-training," ICCV 2023.
3.  Zhang et al., "Contrastive learning of medical visual representations from paired images and text," arXiv 2020.
