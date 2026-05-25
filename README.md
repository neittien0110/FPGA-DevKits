# FPGA DEVELOPMENT KITS

<!-- TOC -->

- [Công cụ phát triển](#công-cụ-phát-triển)
- [Các Dev Kit liên quan](#các-dev-kit-liên-quan)
- [Kria KV260 Vision AI Starter Kit](#kria-kv260-vision-ai-starter-kit)
- [Kria KR260 Robotics Starter Kit](#kria-kr260-robotics-starter-kit)
- [Các dự án](#các-dự-án)
    - [LedOn](#ledon)
    - [LedSwitch](#ledswitch)
    - [Dự án 1: "Trái tim" của SNN – Neuron LIF đơn lẻ](#dự-án-1-trái-tim-của-snn--neuron-lif-đơn-lẻ)
        - [Mô hình Toán học và Sinh học của Neuron LIF](#mô-hình-toán-học-và-sinh-học-của-neuron-lif)
        - [Mô hình Sinh học của Neuron LIF](#mô-hình-sinh-học-của-neuron-lif)
        - [Source Code](#source-code)
    - [Dự án 2: Mã hóa thông tin Spike Encoding](#dự-án-2-mã-hóa-thông-tin-spike-encoding)
    - [Dự án 3: Khớp thần kinh The Synapse – Tích hợp trọng số](#dự-án-3-khớp-thần-kinh-the-synapse--tích-hợp-trọng-số)
    - [Dự án 4: Lớp ẩn đầu tiên Single Layer SNN](#dự-án-4-lớp-ẩn-đầu-tiên-single-layer-snn)
    - [Dự án 5: Học máy thực thụ – MNIST Classifier](#dự-án-5-học-máy-thực-thụ--mnist-classifier)

<!-- /TOC -->

## Công cụ phát triển

[Xem tài liệu Vitis HLS + Vivado](./Vivado.md)

## Các Dev Kit liên quan

| Dev Kit                                                               | Price | Homepage                                                                                            |
| --------------------------------------------------------------------- | ----- | --------------------------------------------------------------------------------------------------- |
| [TUL PYNQ-Z2](./TUL_PYNQ-Z2.md)                                       | 7tr   | <https://www.tulembedded.com/FPGA/ProductsPYNQ-Z2.html>                                             |
| [Kria KV260 Vision AI Starter Kit](#kria-kv260-vision-ai-starter-kit) | 13tr  | <https://pivietnam.com.vn/xilinx-kria-kv260-vision-ai-starter-kit-sk-kv260-g-pivietnam-com-vn.html> |
| [Kria KR260 Robotics Starter Kit](#kria-kr260-robotics-starter-kit)   | .     | .                                                                                                   |

## Kria KV260 Vision AI Starter Kit

- Logic Cells: ~256K (Gấp 5 lần PYNQ-Z2).
- DSP Slices: 1,248 (Gấp gần 6 lần). Đây là thông số cực kỳ quan trọng để chạy các mạng neuron lớn như YOLOv8 hay ResNet-50.
- CPU cứng: 4 nhân ARM Cortex-A53 (64-bit) cực mạnh, giúp chạy Linux và Python mượt mà hơn hẳn nhân ARM A9 cũ.
- Hỗ trợ AI: Tích hợp sẵn Vitis AI và các "AI Box" có sẵn, bạn chỉ cần nạp model là chạy
- Chiu được 128 nhân RV32
- Vua của Thị giác máy tính
- Xilinx cung cấp các "Vision Apps" chạy sẵn (Smart Camera, AI Box). Nếu bạn làm mạng neuron nhận diện vật thể, khuôn mặt, đây là lựa chọn số 1.
- [Mua sắm](https://mlab.vn/3759721-xilinx-kria-kv260-vision-ai-starter-kit-sk-kv260-g.html)

## Kria KR260 Robotics Starter Kit

- Phù hợp cho SNN
- Bạn sẽ không dùng Vitis AI (vì Vitis AI tối ưu cho CNN/DDR). Thay vào đó, bạn sẽ dùng:
  SNN Library: Sử dụng các framework như SNNilink hoặc BindsNET để huấn luyện mô hình trên máy tính.
  HLS Implementation: Viết mô hình SNN bằng C++ (Vitis HLS) để nạp vào FPGA của KR260.
  Python Interface:
- [Mua sắm](https://www.proe.vn/kria-kr260-robotics-starter-kit)

## Các dự án

### LedOn

Helloword với một khối **FPGA chỉ nối dây đồng với đèn led đơn**, và sử dụng **python** chạy trên **ARM core** để triệu gọi khối FPGA đó qua **AXI4**

### LedSwitch

Hai khối FPGA:

- **FPGA chỉ nối dây đồng với đèn led RGB** và sử dụng **python** chạy trên **ARM core** để triệu gọi khối FPGA đó qua **AXI4** với dải màu long lanh.
- **FPGA điều khiển led đơn qua công tắc switch**, độc lâp hoàn toàn với **ARM core**.

---

### Dự án 1: "Trái tim" của SNN – Neuron LIF đơn lẻ

Xây dựng một **Leaky Integrate-and-Fire (LIF)** neuron đơn lẻ

- **Mục tiêu**: Hiểu cơ chế tích lũy điện thế màng (Vmem) và cơ chế rò rỉ (leak).
- **Input**: 4 tín hiệu input_spike (1-bit).
- **Output**: 1 tín hiệu output_spike (1-bit).
- **Thử thách Verilog**: Sử dụng số Fixed-point để tính toán Vmem. Khi Vmem vượt ngưỡng threshold, kích hoạt output_spike và reset Vmem.
- **Kiểm tra trên PYNQ**: Dùng nút bấm trên Kit để gửi các chuỗi xung nhanh/chậm khác nhau và quan sát trạng thái Vmem và Spike trên giao diện Console của Kit

#### Mô hình Toán học và Sinh học của Neuron LIF

Ảnh sau thể hiện một Neuron LIF (Leaky Integrate-and-Fire) hoạt động dựa trên điện thế màng Vmem:

$$V_{mem}(t) = V_{mem}(t-1) + \sum_{i=1}^{n} (Spike_i \times Weight_i) - Leak$$
<p align="center"> Hiện tại = Quá khứ + Tác động - Rò rỉ định kỳ </p>

#### Mô hình Sinh học của Neuron LIF

- Bên trái ảnh là Mô hình sinh học: Tín hiệu đầu vào **Spikes** đi qua khớp thần kinh **Synapse** với trọng số **w** được tích lũy vào thân neuron. Nó có một cơ chế "rò rỉ" điện thế ra môi trường.
- Bên phải ảnh là đồ thị điện thế: đường cong Vmem đi lên khi có xung vào. Nếu không có xung, nó tự rò rỉ đi xuống (Leaky). Khi chạm vạch ngưỡng **Threshold**, nó phát ra một xung đầu ra (**Spike**) và lập tức **Reset** điện thế về 0.
  ![Leaky Integrate-and-Fire (LIF)](./SNN-images/Gemini_Generated_Image_lyabqblyabqblyab.png)\
  Nguồn Gemini

#### Source Code

  <code style="color : purple">[GitHub: SNN_SingleLIF](https://github.com/neittien0110/SNN_SingleLIF)</code>

### Dự án 2: Mã hóa thông tin (Spike Encoding)

SNN không hiểu con số 0.75, nó chỉ hiểu xung. Bạn cần một bộ chuyển đổi.

- **Mục tiêu**: Chuyển đổi giá trị số từ Python thành chuỗi xung (Rate Coding).
- **Thử thách Verilog**: Xây dựng một module nhận số 8-bit từ AXI-Lite và so sánh nó với một bộ đếm số ngẫu nhiên (LFSR - Linear Feedback Shift Register). Nếu số ngẫu nhiên < giá trị đầu vào, phát ra 1 xung.
- **Kiểm tra trên PYNQ**: Gửi số 255 (LED sáng rực - xung dày đặc) và số 10 (LED nháy cực chậm).

### Dự án 3: Khớp thần kinh (The Synapse) – Tích hợp trọng số

Neuron chỉ mạnh khi có sự kết nối với các trọng số (Weights).

- **Mục tiêu**: Nhân xung đầu vào với một trọng số trước khi đưa vào Neuron.
- **Thử thách Verilog**: Vì xung chỉ là 0 hoặc 1, phép nhân thực chất là: Nếu có xung, thì cộng Weight vào Vmem; nếu không có xung, thì không cộng. Đây là lý do SNN cực kỳ tiết kiệm năng lượng trên FPGA (không cần bộ nhân DSP cồng kềnh).
- **Kiểm tra trên PYNQ**: Dùng Python thay đổi Weight qua AXI. Với cùng một đầu vào, xem Weight lớn làm Neuron "bắn" xung nhanh hơn thế nào.

### Dự án 4: Lớp ẩn đầu tiên (Single Layer SNN)

Kết nối nhiều Neuron lại thành một lớp (Layer).

- **Mục tiêu**: Xây dựng một lớp gồm 8 Neuron song song nhận chung một nguồn xung nhưng có trọng số khác nhau.
- **Thử thách Verilog**: Quản lý tài nguyên. Liệu bạn sẽ tạo ra 8 module Neuron riêng biệt (Song song hoàn toàn) hay dùng 1 module Neuron và dùng RAM để lưu trạng thái của 8 Neuron (Quét vòng - Time-multiplexing)?
- **Kiểm tra trên PYNQ**: Hiển thị trạng thái của 8 Neuron lên 8 LED đơn trên PYNQ-Z2.

### Dự án 5: Học máy thực thụ – MNIST Classifier

Đây là dự án "Tốt nghiệp" giai đoạn cơ bản.

- **Mục tiêu**: Nhận diện chữ số viết tay (0-9) từ tập dữ liệu MNIST.
- **Lộ trình thực hiện**:
  1. Dùng snnTorch (Python) trên máy tính để huấn luyện một mạng SNN nhỏ (ví dụ: 784 đầu vào -> 32 lớp ẩn -> 10 đầu ra).
  2. Xuất các trọng số đã huấn luyện ra file .bin.
  3. Thiết kế bộ nạp trọng số từ Python xuống BRAM của FPGA.
  4. Đẩy ảnh từ Jupyter Notebook xuống và xem FPGA đoán số mấy.
