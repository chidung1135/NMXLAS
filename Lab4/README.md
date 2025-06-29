### Nhập Môn Xử Lý Ảnh Số - Lab 4 : PHÂN VÙNG ẢNH

## 1. CÀI ĐẶT THƯ VIỆN
pip install opencv-python
## 2. VIẾT CHƯƠNG TRÌNH PHÂN VÙNG ẢNH
# 2.1. Phân vùng theo histogram
2.1.1. Phương pháp Otsu
-Tự động tìm một ngưỡng để phân tách ảnh thành vùng sáng và vùng tối.
2.1.2. Phương pháp Adaptive Thresholding
-Cải tiến phân vùng chính xác hơn Otsu. Chia ảnh thành nhiều ảnh nhỏ và tính threshold cho từng ảnh nhỏ
# 2.2. Phân vùng theo region
Một region là một nhóm các pixel có cùng thuộc tính.
# 2.3. Biến đổi đối tượng trong ảnh
Dilation cho phép các pixel ở foreground của 1 ảnh có thể соco giãn.
2.3.1. Sử dụng binary_dilation
-Dùng để tăng kích thước đối tượng.
2.3.2. Sử dụng binary_opening
-Dùng để giữ lại các đối tượng lớn, loại bỏ đối tượng nhỏ.
2.3.3. Sử dụng binary_erosion
-Dùng để co đối tượng bằng cách loại bỏ pixels ở biên của đối tượng
2.3.4. Sử dụng binary_closing
-Dùng để giữ lại hình dạng đối tượng và làm mịn biên.
## 3. BÀI TẬP
1. Viết chương trình chọn LangBiang trong ảnh Đà Lạt từ thư mục exercise. Tịnh tiến vùng chọn sang phải 100px. Sử dụng phương pháp Otsu để phân vùng LangBiang theo ngưỡng 0.3. Lưu vào máy với tên lang_biang.jpg và hiển thị trên màn hình.

#Công nghệ sử dụng:
-PIL : đọc và xử lý ảnh 
-NumPy : biểu diễn và xử lý ảnh dưới dạng mảng số 
-SciPy.ndimage : cung cấp các phép biến đổi ảnh 
-scikit-image : chứa thuật toán phân ngưỡng Otsu 
-imageio : lưu ảnh 
-matplotlib.pyplot : hiển thị ảnh đầu ra 

#Thuật toán sử dụng:
Tịnh tiến ảnh:	Dời ảnh sang phải 100 px	
Phương pháp Otsu : Tìm ngưỡng tối ưu để phân biệt foreground/background

#Cách hoạt động:
Đọc ảnh và chuyển sang ảnh xám:
data = Image.open('dalat.jpg').convert('L')
a = np.array(data)
Ảnh được chuyển sang ảnh mức xám để xử lý đơn kênh.

Cắt vùng chứa LangBiang:
bmg = a[20:320, 20:500]
Cắt 1 vùng hình chữ nhật từ ảnh để tập trung vào đối tượng.

Tịnh tiến ảnh sang phải 100px:
bmg = nd.shift(bmg, shift=(0, 100))
Dời toàn bộ vùng chọn sang phải, giữ chiều dọc.

Tính ngưỡng bằng thuật toán Otsu:
thres = threshold_otsu(a)
Áp dụng Otsu lên toàn ảnh a để tìm ra ngưỡng nhị phân tốt nhất.

Phân vùng nhị phân vùng chọn:
binary = bmg > thres
Các pixel trong bmg lớn hơn ngưỡng sẽ được gán giá trị True (1).

Chuyển ảnh nhị phân thành ảnh lưu được và ghi file:
Image.fromarray((binary * 255).astype(np.uint8))
Ảnh nhị phân (True/False) được nhân 255 → chuyển thành ảnh trắng/đen.

Lưu kết quả vào file và hiển thị

2. Viết chương trình chọn Hồ Xuân Hương trong ảnh Đà Lạt từ thư mục exercise. Xoay đối tượng vừa chọn 1 góc 45° và dùng phương pháp Adaptive Thresholding với ngưỡng 60 và lưu vào máy với tên là ho_xuan_huong.jpg.

#Công nghệ sử dụng:
-PIL : đọc và xử lý ảnh 
-NumPy : biểu diễn và xử lý ảnh dưới dạng mảng số 
-SciPy.ndimage : cung cấp các phép biến đổi ảnh 
-scikit-image : chứa thuật toán phân ngưỡng Otsu 
-imageio : lưu ảnh 
-matplotlib.pyplot : hiển thị ảnh đầu ra 

#Thuật toán sử dụng:
Xoay ảnh : Xoay vùng ảnh vừa chọn 45 độ
Phân ngưỡng cục bộ : Áp dụng threshold_local với kích thước cửa sổ 39 và offset=60 để tạo ảnh nhị phân

#Cách hoạt động:
Đọc và chuyển ảnh về ảnh xám
data = Image.open('dalat.jpg').convert('L')
a = np.asarray(data)
Chuyển ảnh dalat.jpg sang ảnh xám để dễ xử lý và phân ngưỡng.

Cắt vùng Hồ Xuân Hương
bmg = a[40:680, 510:990]
Cắt vùng ảnh chứa Hồ Xuân Hương theo tọa độ.

Xoay vùng ảnh 45 độ
d = nd.rotate(bmg, 45, reshape=False)
Xoay vùng ảnh vừa chọn 45°, giữ nguyên kích thước.

Phân ngưỡng cục bộ 
b = threshold_local(a, 39, offset=60)
Ảnh được chia thành các vùng nhỏ.
Tính trung bình cục bộ mỗi vùng 
Mỗi pixel được so với ngưỡng vùng của nó để tạo ảnh nhị phân.

Lưu và hiển thị kết quả

3. Viết chương trình chọn Quảng trường Lâm Viên trong ảnh Đà Lạt từ thư mục exercise. Dùng phương pháp Coordinate Mapping và Binary Closing cho vùng vừa chọn. Lưu vào máy với tên là quan_truong_lam_vien.jpg.

#Công nghệ sử dụng:
-PIL : đọc và xử lý ảnh 
-NumPy : biểu diễn và xử lý ảnh dưới dạng mảng số 
-SciPy.ndimage : cung cấp các phép biến đổi ảnh 
-scikit-image : chứa thuật toán phân ngưỡng Otsu 
-imageio : lưu ảnh 
-matplotlib.pyplot : hiển thị ảnh đầu ra 

#Thuật toán sử dụng:
Coordinate Mapping : Biến dạng ảnh theo tọa độ để tạo hiệu ứng méo/ngẫu nhiên
Binary Closing : Làm mịn vùng trắng, lấp lỗ nhỏ, kết nối chi tiết rời rạc

#Cách hoạt động:
Đọc ảnh và chuyển sang xám
data = Image.open('dalat.jpg').convert('L')
a = np.array(data)

Cắt vùng chứa Quảng trường Lâm Viên
bmg = a[50:330, 1010:1470]
Trích xuất vùng ảnh có tọa độ dòng từ 50–330, cột từ 1010–1470.

Biến đổi hình học – ánh xạ tọa độ
V, H = bmg.shape
M = np.indices((V, H))                      
d = 5
q = 2 * d * np.random.ranf(M.shape) - d     
mp = (M + q).astype(int)                    
d1 = nd.map_coordinates(bmg, mp)            
Tạo hiệu ứng "méo hình" hoặc "gợn sóng", làm biến dạng vùng ảnh.

Làm mịn vùng trắng bằng Binary Closing
s = [[0, 1, 0], [1, 1, 1], [0, 1, 0]]
b = nd.binary_closing(d1, structure=s, iterations=50)
Closing = Dilation -> Erosion
Lặp 50 lần để lấp đầy các lỗ nhỏ và kết nối các vùng trắng gần nhau.
Giúp hình ảnh vùng được biến đổi mịn và liền mạch hơn.

Lưu và hiển thị ảnh kết quả
