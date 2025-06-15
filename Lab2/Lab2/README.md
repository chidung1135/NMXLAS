
# Bài thực hành 2: Ảnh kỹ thuật số & màu – Xử lý điểm ảnh


## 1. Công nghệ sử dụng

- Ngôn ngữ lập trình: Python 
- Thư viện sử dụng:
  - PIL: đọc, chuyển đổi và lưu ảnh.
  - NumPy: xử lý ma trận điểm ảnh.
  - Matplotlib: hiển thị ảnh.
  - ImageIO: đọc, ghi ảnh.
  - math & scipy: dùng cho biến đổi Fourier và tính toán số.

---

## 2. Thuật toán sử dụng

2.1 Nghịch đảo ảnh
Làm sáng vùng tối và làm tối vùng sáng trong ảnh.

2.2 Hiệu chỉnh Gamma
Điều chỉnh độ sáng của ảnh để làm sáng hoặc làm tối tùy theo tham số gamma.

2.3 Biến đổi Logarit
Làm rõ các chi tiết trong vùng tối của ảnh.

2.4 Cân bằng Histogram
Tăng độ tương phản, giúp ảnh trở nên rõ và sắc nét hơn.

2.5 Kéo giãn tương phản
Mở rộng độ sáng và tối để ảnh nhìn rõ hơn và có chiều sâu.

2.6.1 Biến đổi Fourier nhanh
Chuyển ảnh sang dạng tần số để dễ xử lý như lọc nhiễu hoặc làm mịn.

2.6.2 Bộ lọc Butterworth
Loại bỏ nhiễu hoặc làm mịn ảnh bằng cách lọc các thành phần không mong muốn trong ảnh.

## 3. Giải thích cách hoạt động

3.1 Biến đổi cường độ ảnh (Nghịch đảo ảnh)

from PIL import Image
import numpy as np
import matplotlib.pyplot as plt

img = Image.open('bird.jpg').convert('L')
im_1 = np.asarray(img)

Mở ảnh bird.jpg và chuyển sang ảnh đen trắng (chế độ 'L').
Chuyển ảnh thành mảng số để dễ thao tác từng điểm ảnh.

im_2 = 255 - im_1
new_img = Image.fromarray(im_2)
img.show()
plt.imshow(new_img, cmap='gray')
plt.show()

Tính ảnh nghịch đảo: với mỗi điểm ảnh p, ta lấy 255 - p.
Chuyển mảng kết quả thành ảnh để hiển thị.
Hiển thị ảnh gốc và ảnh nghịch đảo.

3.2 Hiệu chỉnh Gamma (Gamma Correction)

gamma = 0.5
b1 = im_1.astype(float)
b2 = np.max(b1)
b3 = (b1 + 1) / b2

Chọn giá trị gamma (0.5).
Chuyển mảng ảnh sang kiểu số thực để tính toán chính xác.
Chuẩn hóa điểm ảnh về khoảng từ 0 đến 1.

b4 = np.log(b3) * gamma
c = np.exp(b4) * 255.0
c1 = c.astype(np.uint8)

Áp dụng công thức hiệu chỉnh gamma: output = input^gamma.
Chuẩn hóa điểm ảnh về khoảng 0 đến 255 và chuyển về kiểu số nguyên.

3.3 Biến đổi Logarit (Log Transformation)

c = (128.0 * np.log(1 + b1)) / np.log(1 + b2)
c1 = c.astype(np.uint8)

Áp dụng công thức logarit để làm rõ vùng tối.
Giúp tăng chi tiết các phần tối của ảnh.

3.4 Cân bằng Histogram (Histogram Equalization)

b1 = im1.flatten()
hist, bins = np.histogram(im1, 256, [0, 255])
cdf = hist.cumsum()

Chuyển ảnh 2D thành mảng 1D để tính histogram.
Tính histogram với 256 mức xám.
Tính hàm phân phối tích lũy (CDF) dựa trên histogram.

cdf_m = np.ma.masked_equal(cdf, 0)
num_cdf = (cdf_m - cdf_m.min()) * 255
den_cdf = (cdf_m.max() - cdf_m.min())
cdf = num_cdf / den_cdf

Loại bỏ các giá trị 0 trong CDF để tránh lỗi.
Chuẩn hóa CDF về khoảng 0 đến 255.
Dùng CDF này làm bảng ánh xạ điểm ảnh mới.

3.5 Kéo giãn tương phản (Contrast Stretching)

b = im1.max()
a = im1.min()
c = im1.astype(float)
im2 = 255 * (c - a) / (b - a)

Tìm giá trị nhỏ nhất và lớn nhất trong ảnh.
Áp dụng công thức kéo giãn dải giá trị điểm ảnh về 0-255.
Giúp ảnh có độ tương phản rõ ràng hơn.

3.6 Biến đổi Fourier và Bộ lọc Butterworth

import scipy.fftpack
c = abs(scipy.fftpack.fft2(im1))
d = scipy.fftpack.fftshift(c)

Chuyển ảnh từ miền không gian sang miền tần số bằng Fourier Transform.
Dịch chuyển thành phần tần số thấp về trung tâm ảnh.

H[i, j] = 1 / (1 + (r / r0)**t2)
con = d * H1
e = abs(scipy.fftpack.ifft2(con))

Tạo bộ lọc Butterworth để lọc các tần số cao hoặc thấp.
Nhân phổ tần số với bộ lọc để lọc ảnh.
Chuyển ngược ảnh về miền không gian sau lọc.
## 4. Bài tập

## Bài 1: Viết chương trình tạo menu cho phép người dùng chọn các phương pháp biến đổi ảnh như sau:
Image inverse transformation
Gamma-Correction
Log Transformation
Histogram equalization
Contrast Stretching
Khi người dùng ấn phím I, G, L, H, C thì chương trình sẽ thực hiện hàm tương ứng cho các hình trong thư mục exercise. Lưu và hiển thị các ảnh đã biến đổi.

def img_inv_func(img):
    return Image.fromarray(255 - np.asarray(img))

Chuyển ảnh thành mảng số bằng np.asarray(img)
Lấy 255 trừ đi từng giá trị pixel để đảo ngược màu sắc
Trả lại ảnh mới đã đảo bằng Image.fromarray

def gamma_corr_func(img, gamma=0.5):
    a = np.asarray(img).astype(float)
    m = np.max(a)
    a = (a + 1) / m
    a = np.exp(np.log(a) * gamma) * 255
    return Image.fromarray(a.astype(np.uint8))

Hàm nhận đầu vào là ảnh PIL và trả về ảnh đã xử lý
Hàm gamma_corr_func có tham số gamma mặc định = 0.5


def log_transform_func(img):
    a = np.asarray(img).astype(float)
    m = np.max(a)
    a = (128 * np.log(1 + a)) / np.log(1 + m)
    return Image.fromarray(a.astype(np.uint8))

Tăng độ chi tiết vùng tối bằng cách áp dụng logarit lên ảnh
Sử dụng công thức logarit chuẩn hóa để đưa kết quả về mức 0–255


def hist_eq_func(img):
    a = np.asarray(img)
    flat = a.flatten()
    hist, _ = np.histogram(a, 256, [0, 255])
    cdf = hist.cumsum()
    cdf_m = np.ma.masked_equal(cdf, 0)
    cdf = (cdf_m - cdf_m.min()) * 255 / (cdf_m.max() - cdf_m.min())
    cdf = np.ma.filled(cdf, 0).astype('uint8')
    eq = cdf[flat].reshape(a.shape)
    return Image.fromarray(eq)

Hàm hist_eq_func thực hiện cân bằng histogram


def contrast_stretch_func(img):
    a = np.asarray(img).astype(float)
    stretched = 255 * (a - a.min()) / (a.max() - a.min())
    return Image.fromarray(stretched.astype(np.uint8))

Hàm contrast_stretch_func thực hiện tăng cường độ tương phản cho ảnh xám


def apply_transformation(func, folder, save_folder, suffix):
    for file in os.listdir(folder):
        if file.lower().endswith(('.png', '.jpg', '.jpeg', '.bmp')):
            path = os.path.join(folder, file)
            img = Image.open(path).convert('L')
            out = func(img)
            plt.imshow(out, cmap='gray')
            plt.title(f"{file} - {suffix}")
            plt.axis('off')
            plt.show()
            name = os.path.splitext(file)[0] + f"_{suffix}.png"
            out.save(os.path.join(save_folder, name))

Hàm apply_transformation xử lý hàng loạt: Duyệt qua tất cả file ảnh trong thư mục, áp dụng hàm biến đổi, hiển thị kết quả và lưu ảnh kết quả vào thư mục

os.makedirs("result", exist_ok=True)

Nếu thư mục result chưa tồn tại thì sẽ tạo mới và lưu trữ ảnh sau khi thay đổi


transform_map = {
    'I': (img_inv_func, 'inverse'),
    'G': (gamma_corr_func, 'gamma'),
    'L': (log_transform_func, 'log'),
    'H': (hist_eq_func, 'hist'),
    'C': (contrast_stretch_func, 'contrast')
}

Tạo menu bằng dictionary tương ứng với những chữ cái đầu của thuật toán để đặt tên ảnh kết quả


print("I: Image Inverse")
print("G: Gamma Correction")
print("L: Log Transformation")
print("H: Histogram Equalization")
print("C: Contrast Stretching")
choice = input("Nhập lựa chọn (I, G, L, H, C): ").upper()
if choice in transform_map:
    func, name = transform_map[choice]
    apply_transformation(func, "exercise", "result", name)
else:
    print("Lựa chọn không hợp lệ.")

Hiển thị menu cho người dùng chọn
Đọc lựa chọn từ bàn phím, chuyển thành chữ in hoa
Kiểm tra người dùng nhập đúng thì gọi hàm apply_transformation với tham số tương ứng
Nếu không hợp lệ thì in thông báo lỗi
Tự động áp dụng lên tất cả ảnh trong thư mục "exercise" và lưu vào "result"

## Bài 2: Viết chương trình tạo menu cho phép người dùng chọn một trong các phương pháp biến đổi ảnh:

- Fast Fourier

- Butterworth Lowpass Filter

- Butterworth Highpass Filter

Khi người dùng ấn phím F, L, H thì chương trình sẽ thực hiện hàm tương ứng cho các hình trong thư mục exercise. Lưu và hiển thị các ảnh đã biến đổi.

Trong bài này sử dụng 3 thuật toán chính là Fast Fourier, Butterworth Lowpass Filter và Butterworth Highpass Filter. 

Fast Fourier: dùng để biến đổi ảnh theo miền tần suất.
def Fast_Fourier(img):
    fft_result = abs(scipy.fftpack.fft2(img))
    shifted = scipy.fftpack.fftshift(fft_result)
    return shifted

Butterworth Lowpass Filter: dùng những điểm ảnh có tần suất thấp từ biến đổi Fourier và dùng để làm mịn, khử nhiễu ảnh.
def Butterworth_Lowpass(img, r0=30.0, t=1):
    M, N = img.shape
    H = np.ones((M, N))
    center1, center2 = M / 2, N / 2
    for i in range(M):
        for j in range(N):
            r = math.sqrt((i - center1) ** 2 + (j - center2) ** 2)
            if r > 0:
                H[i, j] = 1 / (1 + (r / r0) ** (2 * t))
    return apply_filter(img, H)

Butterworth Highpass Filter: dùng những điểm ảnh có tần suất cao từ biến đổi Fourier và dùng để làm sắc biên của ảnh.
def Butterworth_Highpass(img, r0=30.0, t=1):
    M, N = img.shape
    H = np.zeros((M, N))
    center1, center2 = M / 2, N / 2
    for i in range(M):
        for j in range(N):
            r = math.sqrt((i - center1) ** 2 + (j - center2) ** 2)
            if r > 0:
                H[i, j] = 1 / (1 + (r0 / r) ** (2 * t))
    return apply_filter(img, H)

Ngoài ra còn có 2 hàm phụ:
Hàm apply_filter: áp dụng bộ lọc tần số lên ảnh.
Hàm apply_transformation: dùng để áp dụng hàm biến đổi lên tất cả ảnh trong thư mục.

## Bài 3: Viết chương trình hay đổi thứ tự màu RGB của ảnh trong thư mục exercise và sử dụng ngẫu nhiên một trong các phép biến đổi ảnh trong câu 1. Lưu và hiển thị ảnh đã biến đổi.

Trong bài này sử dụng 5 phương pháp biến đổi ảnh xám và áp dụng ngẫu nhiên 1 trong 5 phương pháp đó:


Inverse: dùng để đảo ngược màu của ảnh xám.
def img_inv_func(img):
    im_1 = np.asarray(img)
    im_2 = 255 - im_1
    return Image.fromarray(im_2)

Gamma Correction: dùng để điều chỉnh độ sáng/tối bằng biến đổi gamma.
def gamma_corr_func(img, gamma=0.5):
    im_1 = np.asarray(img)
    b1 = im_1.astype(float)
    b2 = np.max(b1)
    b3 = (b1 + 1) / b2
    b4 = np.log(b3) * gamma
    c = np.exp(b4) * 255.0
    c1 = c.astype(np.uint8)
    return Image.fromarray(c1)

Log Transform: dùng để tăng cường độ tương phản ở vùng tối bằng biến đổi logarit.
def log_transform_func(img):
    b1 = np.asarray(img).astype(float)
    b2 = np.max(b1)
    c = (128.0 * np.log(1 + b1)) / np.log(1 + b2)
    c1 = c.astype(np.uint8)
    return Image.fromarray(c1)

Histogram Equalization: dùng để cân bằng histogram để cải thiện độ tương phản.
def hist_eq_func(img):
    im1 = np.asarray(img)
    b1 = im1.flatten()
    hist, bins = np.histogram(im1, 256, [0, 255])
    cdf = hist.cumsum()
    cdf_m = np.ma.masked_equal(cdf, 0)
    cdf = (cdf_m - cdf_m.min()) * 255 / (cdf_m.max() - cdf_m.min())
    cdf = np.ma.filled(cdf, 0).astype('uint8')
    im2 = cdf[b1]
    im3 = np.reshape(im2, im1.shape)
    return Image.fromarray(im3)

Contrast Stretching: dùng để kéo giãn độ tương phản dựa trên giá trị min/max của ảnh.
def contrast_stretch_func(img):
    im1 = np.asarray(img)
    a = im1.min()
    b = im1.max()
    c = im1.astype(float)
    im2 = 255 * (c - a) / (b - a)
    return Image.fromarray(im2.astype(np.uint8))

và dùng hàm shuffle_rgb để xáo trộn ngẫu nhiên các kênh màu RGB.
def shuffle_rgb(img):
    arr = np.asarray(img)
    perms = [
        (0, 1, 2), (0, 2, 1),
        (1, 0, 2), (1, 2, 0),
        (2, 0, 1), (2, 1, 0)
    ]
    p = random.choice(perms)
    shuffled = arr[:, :, list(p)]
    return Image.fromarray(shuffled)

## Bài 4: Viết chương trình hay đổi thứ tự màu RGB của ảnh trong thư mục exercise và sử dụng ngẫu nhiên một trong các phép biến đổi ảnh trong câu 2. Nếu ngẫu nhiên là phép Butterworth Highpass thì chọn thêm Max Filter để lọc ảnh. Lưu và hiển thị ảnh đã biến đổi.

Trong bài này sử dụng 3 phương pháp biến đổi trong miền tần suất và áp dụng ngẫu nhiên 1 trong 3 phương pháp đó:

Fast Fourier Transform: dùng để biến đổi ảnh sang miền tần suất.
def Fast_Fourier(img):
    fft_result = abs(scipy.fftpack.fft2(img))
    shifted = scipy.fftpack.fftshift(fft_result)
    return shifted

Butterworth Lowpass Filter: sẽ lọc tần số thấp và khử nhiễu và làm mịn ảnh.
def Butterworth_Lowpass(img, r0=30.0, t=1):
    M, N = img.shape
    H = np.ones((M, N))
    center1, center2 = M / 2, N / 2
    for i in range(M):
        for j in range(N):
            r = math.sqrt((i - center1) ** 2 + (j - center2) ** 2)
            if r > 0:
                H[i, j] = 1 / (1 + (r / r0) ** (2 * t))
    return apply_filter(img, H)

Butterworth Highpass Filter: sẽ lọc tần số cao và làm nổi bật chi tiết.
def Butterworth_Highpass(img, r0=30.0, t=1):
    M, N = img.shape
    H = np.zeros((M, N))
    center1, center2 = M / 2, N / 2
    for i in range(M):
        for j in range(N):
            r = math.sqrt((i - center1) ** 2 + (j - center2) ** 2)
            if r > 0:
                H[i, j] = 1 / (1 + (r0 / r) ** (2 * t))
    return apply_filter(img, H)

Hàm apply_filter: áp dụng bộ lọc tần số lên ảnh.
def apply_filter(img, H):
    fft = scipy.fftpack.fft2(img)
    fft_shift = scipy.fftpack.fftshift(fft)
    filtered = fft_shift * H
    inverse_fft = abs(scipy.fftpack.ifft2(filtered))
    return inverse_fft

Hàm shuffle_rgb để xáo trộn ngẫu nhiên các kênh màu RGB.
def shuffle_rgb(img):
    arr = np.asarray(img)
    perms = [
        (0, 1, 2), (0, 2, 1),
        (1, 0, 2), (1, 2, 0),
        (2, 0, 1), (2, 1, 0)
    ]
    p = random.choice(perms)
    shuffled = arr[:, :, list(p)]
    return Image.fromarray(shuffled)


Ngẫu nhiên chọn phép biến đổi
name, func = random.choice(transform_options)
result_array = func(img_array)

Nếu là Butterworth thì thêm lọc Min/Max
if name == 'Butterworth Lowpass':
    result_img = result_img.filter(ImageFilter.MinFilter(3))
elif name == 'Butterworth Highpass':
    result_img = result_img.filter(ImageFilter.MaxFilter(3))