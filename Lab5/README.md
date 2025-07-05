### THỰC HÀNH 5: XÁC ĐỊNH ĐỐI TƯỢNG TRONG ẢNH
## 1.CÀI ĐẶT THƯ VIỆN
pip install opencv-python 
## 2.VIẾT CHƯƠNG TRÌNH GÁN NHÃN ẢNH
# 2.1.Gán nhãn ảnh
Biến ảnh thành ảnh đen trắng để máy tính dễ xác định đâu là đối tượng. Sau đó gắn nhãn cho từng đối tượng
Thuật toán:
Threshold Otsu: Tìm ngưỡng tối ưu để phân biệt đối tượng và nền.
Labeling: Dán nhãn các vùng liên thông.
Cách hoạt động:
Chuyển ảnh sang ảnh xám 
Dùng ngưỡng Otsu để phân vùng ảnh thành trắng – đen 
Gán nhãn các vùng trắng
Tính thuộc tính và vẽ khung bao đối tượng
# 2.2.Dò tìm cạnh theo chiều dọc
So sánh mỗi cột trong ảnh với cột bên cạnh 
Thuật toán: Hiệu lệch pixel theo chiều ngang
Cách hoạt động:
Dùng nd.shift để trượt ảnh 1 pixel theo trục x.
Lấy hiệu tuyệt đối giữa ảnh gốc và ảnh trượt để phát hiện thay đổi biên.
# 2.3.Dò tìm cạnh với Sobel Filter
Dò tìm biên bằng cách xem độ thay đổi pixel theo cả chiều ngang và dọc
Thuật toán: Sobel Filter
Cách hoạt động:
Tính gradient theo trục x và trục y 
Tổng hợp độ lớn gradient để biểu diễn biên cạnh
Dùng plt.imshow để hiển thị biên cạnh
# 2.4.Xác định góc của đối tượng
Tìm những chỗ vừa có biên ngang vừa có biên dọc
Thuật toán: Harris Corner Detection
Cách hoạt động:
Tính đạo hàm theo x và y bằng Sobel
Tính các tích đạo hàm: Ix^2, Iy^2, Ixy
Làm trơn với Gaussian filter
Tính hàm R: R=det(C)−𝛼.(trace(C))2
C là ma trận hiệp phương sai gradient
Các điểm có R lớn là các góc ảnh
# 2.5.Dò tìm hình dạng cụ thể trong ảnh với Hough Transform
2.5.1.Dò tìm đường thẳng trong ảnh
Dò các đường thẳng bằng cách thử mọi góc nghiêng và vị trí
Thuật toán: Hough Line Transform
Cách hoạt động:
Khởi tạo ảnh accumulator 2D theo (rho, theta)
Với mỗi điểm ảnh, tính các giá trị rho theo theta (từ 0 đến 90 độ)
Cộng điểm ảnh vào ảnh tích lũy ho tại các (rho, theta)
Các cực đại trong ảnh ho ứng với đường thẳng
2.5.2.Dò tìm đường tròn trong ảnh
Sử dụng thư viện skimage để tự động tìm góc hoặc điểm đặc biệt
Thuật toán: Corner Harris từ skimage
Cách hoạt động:
Ảnh RGB chuyển sang ảnh xám bằng rgb2gray
Áp dụng corner_harris để tìm các điểm có sự thay đổi mạnh về hướng gradient
Hiển thị ảnh kết quả thể hiện góc ảnh