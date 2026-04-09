# FPGA DEVELOPMENT KITS

## Công cụ phát triển

  [Xem tài liệu Vitis HLS + Vivado](./Vivado.md)

## Các Dev Kit liên quan

Dev Kit | Price| Homepage
-- | -- | --
[TUL PYNQ-Z2](./TUL_PYNQ-Z2.md) | 7tr | <https://www.tulembedded.com/FPGA/ProductsPYNQ-Z2.html>
[Kria KV260 Vision AI Starter Kit](#kria-kv260-vision-ai-starter-kit) | 13tr | <https://pivietnam.com.vn/xilinx-kria-kv260-vision-ai-starter-kit-sk-kv260-g-pivietnam-com-vn.html>
[Kria KR260 Robotics Starter Kit](#kria-kr260-robotics-starter-kit) | . | .

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
