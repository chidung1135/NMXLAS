
# Bài thực hành 3: Biến đổi hình học

1. Chọn đối tượng trong ảnh
Là phép trích ảnh nhỏ trong ảnh lớn ban đầu

2. Tịnh tiến đơn
Dịch chuyển mỗi điểm ảnh trong ảnh gốc đến một vị trí mới

3. Thay đổi kích thước ảnh
Là tăng hoặc giảm không gian của ảnh

4. Xoay ảnh
Dùng hàm rotate để xoay một ảnh

5. Dilation và Erosion
Dùng để loại bỏ những pixel nhiễu.
Dilation thay thế pixel tọa độ (i, j) bằng giá trị lớn nhất của những pixel lân cận
Erosion thay thế pixel tọa độ (i, j) bằng giá trị nhỏ nhất của những pixel lân cận

6. Coordinate Mapping
Tạo hàm mới do người dùng định nghĩa
Tạo 1 coordinate map chứa các pixel mới
Dùng hàm map_coordinate để ánh xạ vị trí mới cho ảnh đầu vào

7. Biến đổi chung (Generic Transformation)
Dùng để biến đổi các ảnh chung phép toán do người dùng định nghĩa

# Bài tập
# Bài 1: Chọn ảnh quả kiwi bất kì. Tịnh tiến quả kiwi 50 pixel sang phải và 30 pixel xuống dưới. Áp dụng hiệu ứng sóng (wave effect) lên quả kiwi bằng cách sử dụng biến đổi tọa độ (map_coordinates) với hàm sin. Lưu ảnh kết quả vào file kiwi_wave.jpg.

#Công nghệ sử dụng:
-numpy để tính toán mảng
-scipy.ndimage để xử lý ảnh nâng cao
-imageio.v2 để đọc, ghi ảnh
-matplotlib.pylab để hiển thị ảnh

#Thuật toán sử dụng:
-Tịnh tiến 
Dùng hàm nd.shift() với vector (30, 50, 0) nghĩa là:
Dịch 30 pixel theo trục dọc  
Dịch 50 pixel theo trục ngang  
Kênh màu vẫn giữ nguyên
-Biến đổi hình học dạng sóng 
Hàm GeoFun biến đổi mỗi tọa độ điểm ảnh theo công thức:
x′=x+10⋅cos(x/10)
y′=y+10⋅cos(y/10)
Mỗi điểm ảnh trong ảnh đầu ra sẽ được ánh xạ ngược về ảnh gốc.
Thuật toán dùng phương pháp coordinate mapping: Với mỗi pixel trong ảnh đầu ra, tính ra vị trí tương ứng trong ảnh gốc và lấy giá trị tại vị trí đó thông qua nội suy
Ảnh màu được xử lý bằng cách áp dụng biến đổi riêng cho từng kênh sau đó ghép lại bằng 
np.stack.

#Cách hoạt động:
Đọc ảnh gốc từ thư mục exercise/
Cắt một vùng ảnh nhỏ chứa quả kiwi bằng slicing: data[920:1100, 390:750]
Tịnh tiến ảnh nhỏ sang phải 50 pixel và xuống 30 pixel bằng nd.shift()
Tạo hiệu ứng sóng bằng cách ánh xạ lại toàn bộ ảnh theo hàm cosin định nghĩa trong GeoFun()
Áp dụng biến đổi hình học cho từng kênh màu R, G, B riêng biệt
Ghép các kênh thành ảnh màu mới với biến dạng sóng
Lưu 2 ảnh kết quả:
kiwi.jpg: ảnh đã tịnh tiến
kiwi_wave.jpg: ảnh bị biến dạng sóng
Hiển thị ảnh bằng matplotlib để so sánh trực quan trước và sau khi biến đổi

# Bài 2: Chọn quả đu đủ và dưa hấu từ google. Đổi màu đu đủ thành gradient từ đỏ sang xanh lá, và dưa hấu thành gradient từ vàng sang tím. Ghép hai quả lên một nền trong suốt (alpha channel) và lưu dưới dạng PNG.

#Công nghệ sử dụng:
-numpy để tính toán mảng
-imageio.v2 để đọc, ghi ảnh
-matplotlib.pylab để hiển thị ảnh

#Thuật toán sử dụng:
Gradient color mapping: chuyển từ màu này sang màu khác
Hàm doimau(img, mau1, mau2) tạo hiệu ứng đổi màu dọc theo chiều cao ảnh.
Sử dụng nội suy tuyến tính giữa hai màu:
maumoi=mau1+(mau2−mau1)⋅t
Áp dụng gradient lên ảnh gốc: Từng pixel ảnh gốc được nhân với màu mới tương ứng ở hàng đó, tỉ lệ giữa 0-1 giúp biến đổi màu sắc theo chiều dọc.
Tạo một ảnh mới có 4 kênh (RGBA). Gán dữ liệu ảnh RGB vào 3 kênh đầu và đặt kênh alpha = 255 cho toàn bộ vùng ảnh.

#Cách hoạt động:
Đọc hai ảnh màu từ file: dudu.jpg và duahau.jpg.
Gọi hàm doimau() để đổi màu cho mỗi ảnh bằng cách tạo gradient:
Ảnh đu đủ từ đỏ sang xanh lá
Ảnh dưa hấu từ vàng sang tím
Tạo ảnh kết quả kq có độ rộng bằng tổng chiều ngang của hai ảnh và có 4 kênh.
Gán ảnh đã đổi màu vào ảnh kết quả, đồng thời thiết lập độ trong suốt.
Ghi ảnh mới ra file ketqua.png và hiển thị bằng Matplotlib.

# Bài 3: Chọn ảnh núi và thuyền. Xoay cả hai đối tượng 45 độ, giữ kích thước ban đầu (reshape=False). Tạo hiệu ứng phản chiếu dọc (vertical mirror) cho cả hai đối tượng sau khi xoay. Ghép cả hai đối tượng lên một canvas trắng và lưu vào mountain_boat_mirror.jpg.

#Công nghệ sử dụng:
-numpy để tính toán mảng
-scipy.ndimage để xử lý ảnh nâng cao
-imageio.v2 để đọc, ghi ảnh
-matplotlib.pyplot hiển thị và lưu ảnh kết quả

#Thuật toán sử dụng
Xoay ảnh (Rotation): dùng phép biến đổi affine để xoay từng điểm ảnh quanh tâm ảnh với góc 45 độ. Tham số reshape=False đảm bảo ảnh sau xoay giữ nguyên kích thước ban đầu.
Phản chiếu ảnh (Vertical flip): hàm np.flipud() lật ảnh theo trục ngang tạo hiệu ứng phản chiếu ảnh theo chiều dọc.

#Cách hoạt động
Đọc hai ảnh nui.jpg và thuyen.jpg.
Xoay từng ảnh 45 độ bằng nd.rotate(), giữ nguyên kích thước ảnh.
Phản chiếu (lật dọc) từng ảnh bằng np.flipud().
Hiển thị hai ảnh đã xử lý cạnh nhau trong cùng một cửa sổ đồ họa bằng matplotlib.
Lưu ảnh kết quả ra file mountain_boat_mirror.jpg.
Hiển thị ảnh bằng plt.show().

# Bài 4: Chọn ngôi chùa bất kì. Phóng to ngôi chùa lên 5 lần. Áp dụng một biến đổi hình học tùy chỉnh (geometric transform) để tạo hiệu ứng "uốn cong" (warping) ngôi chùa. Lưu ảnh kết quả vào pagoda_warped.jpg.

#Công nghệ sử dụng:
-numpy để tính toán mảng
-scipy.ndimage để xử lý ảnh nâng cao
-imageio.v2 để đọc, ghi ảnh
-matplotlib.pyplot hiển thị và lưu ảnh kết quả

#Thuật toán sử dụng
Phóng to ảnh: nd.zoom() phóng to ảnh lên 5 lần theo chiều cao và chiều rộng, giữ nguyên 3 kênh màu (RGB). Sử dụng nội suy để tính giá trị pixel mới khi phóng to.
Chuyển ảnh sang ảnh xám: tính trung bình 3 kênh màu để chuyển thành ảnh xám. 
Biến đổi hình học dạng uốn cong (Warping): 
Hàm warp_function(output_coords) định nghĩa ánh xạ mỗi điểm ảnh đầu ra về vị trí ảnh gốc.
Hàm nd.geometric_transform() sử dụng ánh xạ ngược, tính giá trị pixel đầu ra bằng nội suy từ tọa độ ảnh gốc.

#Cách hoạt động
Đọc ảnh gốc "pagoda.jpg" và in kích thước ban đầu.
Phóng to ảnh lên 5 lần theo chiều cao và chiều rộng bằng nd.zoom.
Chuyển ảnh RGB phóng to sang ảnh xám bằng cách lấy trung bình kênh màu.
Áp dụng biến đổi hình học dạng sóng uốn cong với hàm ánh xạ warp_function.
Hiển thị ảnh kết quả với matplotlib
Lưu ảnh kết quả ra file "pagoda_wrapped.jpg".

