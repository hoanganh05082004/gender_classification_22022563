Em chạy trên kaggle nhưng vì vấn đề khi save version nên em phải up lên github. Em mong thầy thông cảm ạ :3 
# Diễn giải lựa chọn giải pháp
1. Lựa chọn mô hình

Em chọn mô hình InceptionV3 vì đây là một mạng CNN sâu, đã được huấn luyện trên ImageNet, giúp trích xuất tốt các đặc trưng như khuôn mặt hay tóc – yếu tố quan trọng để phân biệt giới tính. Với chuyển tiếp học (fine-tune 30 lớp cuối), mô hình phù hợp với GPU trên Kaggle và tiết kiệm thời gian huấn luyện. Em kết hợp GlobalAveragePooling2D, Dropout (0.6, 0.5) và regularization L2 để giảm độ phức tạp và tránh overfitting. Kết quả độ chính xác 0.96 trên tập validation cho thấy hiệu quả rõ rệt.

2. Xử lý dữ liệu

Dữ liệu có 4000 ảnh mỗi lớp (Male, Female) trong Train, 500 trong Validation. Em resize ảnh về 299x299, chuẩn hóa bằng rescale=1./255. Để tăng đa dạng, em dùng ImageDataGenerator với xoay, dịch chuyển, lật ngang và thay đổi độ sáng. Dù dữ liệu cân bằng, em vẫn tính class_weight để đảm bảo công bằng, dù kết quả cho thấy trọng số gần bằng nhau.

3. Siêu tham số

Batch size: Em chọn 64, phù hợp với GPU 16GB VRAM.
Optimizer: Em dùng AdamW với learning_rate=0.0001 và weight_decay=0.0002 để tối ưu và tránh overfitting.
Callbacks: Em áp dụng ReduceLROnPlateau giảm learning rate sau 3 epoch, ModelCheckpoint lưu mô hình tốt nhất, EarlyStopping dừng sau 7 epoch nếu không cải thiện. Kết quả hội tụ tốt với accuracy 0.96.

4. Đánh giá

Độ chính xác: Đạt 0.96 trên cả mô hình cuối và tốt nhất.
Confusion Matrix: Hầu hết dự đoán đúng (khoảng 480/500 mỗi lớp), ít nhầm lẫn.
Classification Report: Precision, recall, f1-score đều 0.96, cho thấy mô hình ổn định.
Thời gian: 277ms/step với 32 bước/epoch, tận dụng tốt GPU.
