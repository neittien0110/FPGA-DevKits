# AMD Vivado Design Suite

## Các công cụ trong gói để làm việc với TUL PNYQ-Z2, hoặc Kria KV260

1. **Vivado**: thiết kế sơ đồ mạch (Block Design) cho nhân SNN.
2. **Vitis HLS**: Công cụ cực kỳ mạnh mẽ để viết code C++ cho các nơ-ron spiking và để máy tự dịch sang mạch điện FPGA.
3. **Vitis IP Cache**: build dự án nhanh trong những lần chỉnh sửa sau.
4. **Cable Drivers**: Đảm bảo máy tính nhận diện được board PYNQ-Z2 ngay khi cắm cáp USB

## Download

1. Đăng kí tài khoản với **AMD**, hãng đã thôn tính **Xilinx**.\
   <https://www.amd.com/en/registration/create-account.html>/\
   _Lưu ý rằng: nếu khai bao địa chỉ là Việt Nam thì có thế không được quyền tải về. Hãy lấy thông tin địa chỉ đâu đó ở Singapo. Không cần fake IP_
2. Tải về file bộ cài chung. File **Unified** này không phải bộ câì offline, mà sẽ là bảng chọn để sau đó tải về các gói đầy đủ từ trên internet. 
   <https://www.amd.com/en/products/software/adaptive-socs-and-fpgas/vivado/vivado-buy.html>
3. Chạy file *Unified** nói trên, đến bước **Select Edition to Install**, sẽ thấy các lựa chọn:\
   ![Select Edition to Install](./Vivado-images/Select%20Edition%20to%20Install.png)
   - **Vitis**: sẽ bao gồm cả **Vivado** và **Vitis HLS**. **Vitis HLS** sẽ giúp thiết kế mức cao với C++ nên dễ triển khai mạng **SNN**.
   - **Vivado**: Chỉ thiết kế phần cứng thuần túy ở mức **RTL** và **Structural**, với các ngôn ngữ **Verilog**, **SystemVerilog**, hoặc **VHDL**. Rất khó khăn khi muốn chạy Linux/PYNQ mượt mà.
   - **Vitis Embedded Development**: Bản này rút gọn hơn.
   - **Lab Edition / Hardware Server**: Chỉ dùng cho máy tính nào chỉ làm nhiệm vụ nạp code (không dùng để thiết kế).
   - **Power Design Manager (PDM)**: Dùng để tính toán công suất tiêu thụ (chưa cần thiết lúc này).
   Tóm lại, chọn **Vitis**.
4. Ở cửa sổ **Vitis Unified Software Platform** thực hiện tích chọn các tính năng sau giống như trong ảnh, phù hợp với kit **TUL PYNQ-Z2**.\
   ![Vitis Components for TUL PYNQ-Z2](./Vivado-images/Vitis%20Components-Zynq-7000.png)
   - **Vitis IP Cache**: là một tính năng "cứu cánh" giúp tiết kiệm hàng giờ đồng hồ ngồi chờ đợi mỗi khi biên dịch (Compile) dự án trên **Vivado**. Đặc biệt khi triển khai mạng SNN, sẽ sử dụng rất nhiều khối IP (như nhân xử lý Zynq, bộ nhớ RAM, bộ nhân, hoặc các khối HLS tự viết), và thậm chí là nhiều khối IP giống nhau, nó sẽ giúp giảm thời gian xử lý từ lần thứ 2 trở đi, hàng chục lần.\
   _Lưu ý: Vị trí chứa cache xem ở giao diện **Settings / IP / Repository**._\
   _Lưu ý: Đôi khi Cache bị "lỗi thời" (mạch chạy không đúng như code), hãy vào menu **Reports / IP Status** để Clear Cache và bắt nó chạy lại từ đầu cho chắc chắn_
   - **Vitis Network P4** cho phép lập trình FPGA bằng ngôn ngữ **P4** (Programming Protocol-independent Packet Processors).
      - Rất tuyệt để bắt các gói tin từ mạng LAN, sau đó trích xuất dữ liệu (ví dụ: các giá trị cảm biến hoặc dữ liệu ảnh) để đưa vào các nơ-ron SNN xử lý ngay lập tức (In-network processing).
            ```C
               if (packet.header == IPv4) { forward to SNN_core; }
            ```
      - Chỉ hỗ trợ các dòng chip cao cấp như **Alveo**, **Versal** hoặc **UltraScale+**, ví dụ dev kit **Kria KV260** là okay. KHÔNG hỗ chip **Zynq-7000**.
      - KHÔNG MIỄN PHÍ.\
   - _Lưu ý: với dev kit **Kria KV260** thì tick vào mục **Zynq UltraScale+ MPSoCs** là xong._
5. Chọn các thư mục cài đặt. Cứ để mặc định.\
   ![Vitis, Vivado Deployment Folders](./Vivado-images/Vitis%20Deployment%20Folders.png)