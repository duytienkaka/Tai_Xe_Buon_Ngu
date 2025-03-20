<h1 align="center">PHÁT HIỆN MỆT MỎI CỦA TÀI XẾ</h1>

<div align="center">

<p align="center">
  <img src="logoDaiNam.png" alt="DaiNam University Logo" width="200"/>
  <img src="LogoAIoTLab.png" alt="AIoTLab Logo" width="170"/>
</p>

[![Made by AIoTLab](https://img.shields.io/badge/Made%20by%20AIoTLab-blue?style=for-the-badge)](https://www.facebook.com/DNUAIoTLab)
[![Fit DNU](https://img.shields.io/badge/Fit%20DNU-green?style=for-the-badge)](https://fitdnu.net/)
[![DaiNam University](https://img.shields.io/badge/DaiNam%20University-red?style=for-the-badge)](https://dainam.edu.vn)

</div>

<h2 align="center">Mô hình kiểm tra độ mệt mỏi của tài xế</h2>

<p align="left">

</p>

---

## 🌟 Giới thiệu

- **📌 Mô hình kiểm tra tài xế: Mỗi khi tài xế có dấu hiệu mệt mỏi, AI sẽ nhận diện được sự mệt mỏi và đưa ra cảnh báo
- **📊 Quản lý dữ liệu:** Dữ liệu được thu thập trên internet - Kanggle.

---

## 🛠️ CÔNG NGHỆ SỬ DỤNG

<div align="center">

### 🖥️ Phần mềm
- Sử dụng Colab Pro để tạo môi trường train model
- YOLOv7 - model được sử dụng để chạy mô hình
</div>

## 🛠️ Yêu cầu hệ thống
- Sử dụng trực tiếp CPU và GPU trên labtop nếu có cấu hình đủ mạnh, yêu cầu CPU i5-gen12 trở lên và NVDIA 3050 trở lên, với chip AMD yêu cầu 6000 seri trở lên
### 💻 Phần mềm
- **🐍 Python 3+**
- Visual Studio Code
- Google Driver
### 📦 Các thư viện Python cần thiết
Cài đặt các thư viện bằng lệnh:
    !pip install torch torchvision
    !pip install matplotlib
    !pip install opencv-python
    !pip install wandb
## 🚀 Hướng dẫn cài đặt và chạy
1️⃣ Cài đặt thư viện Python. 
!pip install torch torchvision
!pip install matplotlib
!pip install opencv-python
!pip install wandb
Bước đầu tiên, khởi chạy đoạn code để cài đặt những thư viện cần thiết cho việc train mô hình
2️⃣ Lấy model của YOLOv7 trên github
!git clone https://github.com/WongKinYiu/yolov7.git
%cd yolov7
Bước tiếp theo chúng ta sẽ clone model của YOLOv7 về để sử dụng cho việc trainning

3️⃣ Kết nối đến Google Driver
from google.colab import drive
drive.mount('/content/drive')
Kết nối đến Google Driver để lấy dữ liệu đã được lưu trữ

4️⃣ Bắt đầu train model

!python /content/yolov7/train.py \
  --data "/content/drive/MyDrive/BTL/Drowsiness Detection.v2-augmented-v1.yolov7pytorch/data.yaml" \
  --cfg "/content/yolov7/cfg/training/yolov7.yaml" \
  --weights "/content/SCB-dataset/yolov7/yolov7.pt.2" \
  --epochs 80 \
  --batch-size 16 \
  --img-size 640 \
  --device 0 \
  --workers 4 \
  --cache-images \
  --name yolov7_driver_is_sleepy \
  --quad \
  --sync-bn
  
<h3>Khởi chạy đoạn code trên mô hình sẽ bắt đầu trainning, sẽ mất một khoảng thời gian để model học .</h3>

<p>Với epochs chúng ta chỉ nên để 80-100 vì nếu ít epochs sẽ dẫn đến việc học của mô hình không được đầy đủ, từ đó mô hình sẽ nhận diện không chính xác. Nếu nhiều epochs quá sẽ dẫn đến tình trạng overfitting, lúc này model đã học quá mức sẽ dẫn đến việc nhận diện sai lệch do học quá nhiều.</p>

## 📖 Hướng dẫn sử dụng
<h2>1️⃣ Check model bằng ảnh test</h2>
import requests
image_url = "https://cdn-i.vtcnews.vn/resize/th/upload/2023/12/10/buon-ngu-khi-lai-xe-23293142.jpeg"
image_path = "/content/drive/MyDrive/BTL/Drowsiness Detection.v2-augmented-v1.yolov7pytorch/ngu.jpg"
response = requests.get(image_url)
if response.status_code == 200:
    with open(image_path, "wb") as file:
        file.write(response.content)
    print(f"✅ Ảnh đã được tải về: {image_path}")
else:
    print(f"❌ Không thể tải ảnh từ URL: {image_url}")
<p>Sau khi model được train xong, chúng ta sẽ bắt đầu test mô hình bằng hình ảnh được lưu trữ trong driver. Đường dẫn trong image_url có thể thay đổi để check những hình ảnh khác nhau bằng cách lấy link của hình ảnh trên internet. Đây là bước lấy hình ảnh trên mạng về đường dẫn cho bước tiếp theo.</p>

<h2>2️⃣ Quản lý sinh viên & mã QR</h2>

import subprocess
cmd = [
    "python3", "/content/yolov7/detect.py",  # Đường dẫn chính xác của detect.py
    "--weights", "/content/drive/MyDrive/BTL/Drowsiness Detection.v2-augmented-v1.yolov7pytorch/weights/best.pt",  # Model đã train xong
    "--source","/content/drive/MyDrive/BTL/Drowsiness Detection.v2-augmented-v1.yolov7pytorch/ngu.jpg" ,
    "--img-size", "512",
    "--conf-thres", "0.1",
    "--save-txt",
    "--save-conf",
    "--project", "runs/detect",
    "--name", "detect_output",
    "--exist-ok"
]
result = subprocess.run(cmd, capture_output=True, text=True)
print(result.stdout)
print(result.stderr)

<p>Sau bước 1 là bước sử dụng model đã được train để kiểm tra ảnh, hình ảnh sẽ được lấy bằng đường dẫn đã lấy ở bước 1 để sử dụng. Tại đây, ảnh được đưa cho mô hình kiểm tra và đưa ra kết luận cuối cùng rằng tài xế có buồn ngủ hay không.</p>

<h2>3️⃣ Trả về kết quả và hình ảnh</h2>
<p>import matplotlib.pyplot as plt
import cv2
import os

##  Đường dẫn đến ảnh kết quả

image_path = "/content/drive/MyDrive/BTL/Drowsiness Detection.v2-augmented-v1.yolov7pytorch/ngu.jpg"

##  Kiểm tra ảnh có tồn tại không

if os.path.exists(image_path):
    # Đọc ảnh bằng OpenCV
    img = cv2.imread(image_path)
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
    plt.figure(figsize=(8, 6))
    plt.imshow(img)
    plt.axis("off")  # Ẩn trục tọa độ
    plt.figtext(0.5, 0.01, "⚠️ CẢNH BÁO: Tài xế có dấu hiệu buồn ngủ!", wrap=True, horizontalalignment='center', fontsize=14, color='red', fontweight='bold')
    plt.show()
else:
    print(f"❌ Không tìm thấy ảnh: {image_path}")
    
<p>Tại bước cuối cùng này sẽ trả về hình ảnh đã được nhận diện và đưa ra kết quả cuối cùng và đó cũng là kết luận của mô hình.</p>



## 🤝 Đóng góp
Dự án được phát triển bởi 3 thành viên:

| Họ và Tên       | Vai trò                  |
|-----------------|--------------------------|
| Phạm Đức Duy Tiến | Phát triển toàn bộ mã nguồn, kiểm thử, triển khai dự án và thực hiện video giới thiệu.|
| Dương Trọng Tuấn  | Hỗ trợ bài tập lớn.    |
| Nguyễn Văn Hiếu   | Hỗ trợ bài tập lớn     |

© 2025 NHÓM 5, CNTT17-15, TRƯỜNG ĐẠI HỌC ĐẠI NAM
