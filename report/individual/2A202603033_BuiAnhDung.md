# Báo cáo cá nhân Day 13 — PointCloud Cuboid Lab

**Họ và tên:** Bùi Anh Dũng  
**MSSV:** 2A202603033  
**Chương trình:** AI Thực chiến K5 – Level 2  
**Hình thức:** Cá nhân, có đối chiếu chéo số liệu.

## 1. Kết luận và kết quả thực hiện

- Task CVAT: D13-2A202603033.
- Môi trường: Windows 10, Google Chrome, CVAT Local 2.82.1.
- Học viên đối chiếu chéo: 2A202603211 – Võ Duy Thành.
- Số frame hoàn thành: 10/10.
- Tổng số cuboid: 78.
- Số cuboid còn cảnh báo: 8.
- Số ảnh minh chứng: 11/11.

| Frame CVAT | Tên frame | Số cuboid |
|---|---|---:|
| 0 | s01f20 | 10 |
| 1 | s01f39 | 7 |
| 2 | s02f07 | 9 |
| 3 | s02f54 | 5 |
| 4 | s03f34 | 5 |
| 5 | s03f50 | 6 |
| 6 | s04f35 | 11 |
| 7 | s04f55 | 9 |
| 8 | s05f19 | 8 |
| 9 | s05f40 | 8 |
| **Tổng** | | **78** |

Phân bố nhãn: 62 Car, 9 Cyclist, 6 Bus và 1 Pedestrian.

Hai frame có ít cuboid nhất là s02f54 và s03f34, mỗi frame có 5 cuboid. Số lượng này phản ánh các đối tượng đã được lựa chọn để gán nhãn, không nhất thiết là toàn bộ đối tượng xuất hiện trong cảnh.

Mức độ tin cậy tự đánh giá: khá. Các đối tượng gần có nhiều điểm LiDAR nên dễ xác định kích thước và hướng. Đối tượng xa, bị che khuất hoặc có ít điểm có độ tin cậy thấp hơn.

Trong CVAT, tôi sử dụng khung 3D để xác định vị trí và tạo cuboid. Tôi sử dụng Top View để điều chỉnh chiều dài, chiều rộng và yaw; Side View và Front View để điều chỉnh chiều cao, vị trí đáy. Ảnh camera được dùng để hỗ trợ xác định lớp và hình dạng đối tượng.

## 2. Đọc và phân tích point cloud

### 2.1. Frame s01f20

Ảnh minh chứng:

- t1-s01f20-3d.png
- t1-s01f20-top.jpg

Frame có 10 cuboid, gồm 8 Car và 2 Bus.

Theo hệ tọa độ của bài lab, phía trước tương ứng hướng −Y, trục +X được biểu diễn bằng màu đỏ và trục +Y bằng màu xanh lá.

Mặt đường được nhận biết từ lớp điểm nằm thấp, tương đối liên tục. Mốc tham chiếu mặt đường là z = −1,7 m, tuy nhiên độ cao mặt đường thực tế có thể thay đổi theo vị trí.

Đối tượng gần nhất trong frame là Car idx 1, cách 3,62 m. Đối tượng xa nhất là Car idx 8, cách 54,01 m.

Các đối tượng gần có mật độ điểm cao hơn, trong khi các xe ở xa chỉ còn một số cụm điểm thể hiện một phần thân xe. Bus có kích thước lớn nên có khả năng che khuất các đối tượng nằm phía sau theo hướng tia LiDAR.

### 2.2. Frame s01f39

Ảnh minh chứng:

- t1-s01f39-3d.jpg
- t1-s01f39-top.jpg

Frame có 7 cuboid.

Car idx 2 nằm ở khoảng cách 3,40 m, có 2.187 điểm LiDAR bên trong cuboid. Đây cũng là đối tượng gần nhất trong toàn bộ bài làm.

Đối tượng xa nhất trong frame là Car idx 7, cách 52,07 m. Ở khoảng cách lớn, việc xác định chính xác các cạnh và kích thước đối tượng khó hơn.

Tôi sử dụng hướng trục tọa độ, vị trí tương đối của các cụm điểm và mặt đường để phân biệt đối tượng phía trước, phía sau và hai bên cảm biến. Với những cụm điểm không đủ thông tin để xác định hình dạng, cần tránh suy diễn kích thước quá mức.

### 2.3. So sánh camera và LiDAR

| Camera cung cấp | LiDAR cung cấp |
|---|---|
| Màu sắc đối tượng | Khoảng cách theo mét |
| Hình dạng và đặc điểm nhận dạng | Vị trí và kích thước trong không gian 3D |
| Thông tin trực quan hỗ trợ phân loại | Mật độ điểm và hình học bề mặt |

Camera hỗ trợ phân biệt các lớp Car, Bus, Cyclist và Pedestrian. LiDAR hỗ trợ xác định vị trí, kích thước và hướng của cuboid.

Tám frame còn lại được khảo sát thông qua vị trí các cụm điểm, khoảng cách, mật độ điểm và sự phù hợp giữa cuboid với hình học đối tượng.

## 3. Ba cuboid mẫu

### 3.1. Cuboid A — Car

- Frame: s01f20.
- Idx: 1.
- Ảnh: t2-a-car.jpg.

| Thuộc tính | Giá trị |
|---|---:|
| X | 2,949 m |
| Y | −2,102 m |
| Z | −0,901 m |
| Length | 4,515 m |
| Width | 1,787 m |
| Height | 1,315 m |
| Yaw | 269,5° |
| Z bottom | −1,559 m |
| Khoảng cách | 3,62 m |
| Số điểm bên trong | 2.746 |

Đây là đối tượng gần, có mật độ điểm cao. Top View hỗ trợ xác định chiều dài, chiều rộng và hướng thân xe. Side View và Front View hỗ trợ kiểm tra chiều cao và vị trí đáy cuboid.

### 3.2. Cuboid B — Bus

- Frame: s01f20.
- Idx: 4.
- Ảnh: t2-b-bus-truck.jpg.

| Thuộc tính | Giá trị |
|---|---:|
| X | 6,369 m |
| Y | −16,499 m |
| Z | −0,287 m |
| Length | 13,100 m |
| Width | 2,641 m |
| Height | 3,180 m |
| Yaw | 269,5° |
| Z bottom | −1,877 m |
| Khoảng cách | 17,69 m |
| Số điểm bên trong | 2.771 |

Bus có kích thước lớn hơn Car đáng kể. Khi gán nhãn, cần chú ý chiều dài toàn bộ thân xe, chiều cao và vị trí tiếp xúc với mặt đường. Không sử dụng kích thước mặc định của Car cho Bus.

### 3.3. Cuboid C — Cyclist

- Frame: s02f07.
- Idx: 2.
- Ảnh: t2-c-ped-cyclist.jpg.

| Thuộc tính | Giá trị |
|---|---:|
| X | −9,380 m |
| Y | −4,511 m |
| Z | −1,053 m |
| Length | 1,804 m |
| Width | 0,859 m |
| Height | 1,866 m |
| Yaw | 279,2° |
| Z bottom | −1,986 m |
| Khoảng cách | 10,41 m |
| Số điểm bên trong | 486 |

Cyclist có hình dạng hẹp và phức tạp hơn phương tiện bốn bánh. Cuboid cần bao phủ cả người điều khiển và phương tiện theo quy tắc của lớp Cyclist, đồng thời hạn chế bao gồm các điểm không thuộc đối tượng.

### 3.4. Nguyên tắc hiệu chỉnh

Sử dụng đồng thời Top View, Side View, Front View và ảnh camera để giảm sai lệch khi xác định lớp, kích thước, hướng và vị trí cuboid.

Ảnh camera hỗ trợ nhận dạng đối tượng nhưng không thay thế thông tin hình học 3D. Với các cụm điểm không đủ rõ, cần tránh tạo cuboid chỉ dựa trên phỏng đoán.

## 4. Phân tích bốn hiện tượng trong point cloud

### 4.1. Bóng che — Occlusion shadow

- Frame tham chiếu: s01f20.
- Ảnh: t4-1-bong-che.jpg.

Các phương tiện lớn như Bus có thể chắn tia LiDAR, làm giảm số điểm thu được từ vùng phía sau.

Hệ quả đối với gán nhãn: một phần đối tượng có thể không được quan sát trực tiếp. Cần sử dụng các điểm còn nhìn thấy và ảnh camera để hỗ trợ xác định hình dạng, tránh mở rộng cuboid tùy ý.

### 4.2. Thưa điểm ở xa — Sparse point cloud

- Frame: s01f20.
- Ảnh: t4-2-thua-o-xa.jpg.

Car idx 8 nằm ở khoảng cách 54,01 m và chỉ có 31 điểm bên trong cuboid, trong khi Car idx 1 ở khoảng cách 3,62 m có 2.746 điểm.

Nguyên nhân là khoảng cách giữa các tia quét tăng theo khoảng cách tới cảm biến, làm giảm mật độ điểm trên bề mặt đối tượng.

Hệ quả: khó xác định đầy đủ chiều dài, chiều rộng, chiều cao và yaw của phương tiện ở xa.

### 4.3. Phản xạ thấp — Low reflectivity

- Frame tham chiếu: s02f07.
- Ảnh: t4-3-phan-xa-thap.jpg.
- Đối tượng tham chiếu: Car idx 5.

Đối tượng cách 46,99 m và có 32 điểm bên trong cuboid.

Một nguyên nhân có thể làm giảm số điểm thu được là khả năng phản xạ của bề mặt vật liệu hoặc góc tới của tia LiDAR. Tuy nhiên, chỉ dựa trên số điểm và khoảng cách chưa đủ để khẳng định nguyên nhân là phản xạ thấp.

Hệ quả: cần thận trọng khi xác định các cạnh cuboid tại những vùng thiếu điểm và không coi vùng thiếu điểm là bằng chứng chắc chắn về hình dạng thực tế.

### 4.4. Điểm lạ — Outlier points

- Frame tham chiếu: s04f35.
- Ảnh: t4-4-diem-la.jpg.
- Đối tượng tham chiếu: Car idx 8.

Phiếu tự kiểm ghi nhận cảnh báo liên quan đến 157 điểm nằm ngoài cuboid.

Các điểm nằm ngoài có thể thuộc đối tượng lân cận, mặt đường, nhiễu hoặc phần đối tượng chưa được cuboid bao phủ. Không thể kết luận tất cả các điểm này đều là nhiễu.

Hệ quả: cần kiểm tra sự liên tục của cụm điểm và các góc nhìn trước khi thay đổi kích thước cuboid.

## 5. Phân tích số liệu và kiểm tra chất lượng

### 5.1. Tính đáy cuboid và khoảng cách

Công thức:

- z_bottom = z − height/2.
- dist_m = sqrt(x² + y²).
- r = sqrt(x² + y² + z²).
- Độ lệch so với mốc mặt đường = z_bottom − (−1,7).

| Cuboid | Z (m) | H (m) | Z bottom (m) | Dist (m) | R (m) | Yaw | Lệch mốc đường (m) |
|---|---:|---:|---:|---:|---:|---:|---:|
| Car s01f20 #1 | −0,901 | 1,315 | −1,559 | 3,62 | 3,73 | 269,5° | +0,141 |
| Bus s01f20 #4 | −0,287 | 3,180 | −1,877 | 17,69 | 17,69 | 269,5° | −0,177 |
| Cyclist s02f07 #2 | −1,053 | 1,866 | −1,986 | 10,41 | 10,46 | 279,2° | −0,286 |

Mốc −1,7 m là giá trị tham chiếu chung, không đồng nhất với mặt đường cục bộ tại mọi vị trí.

### 5.2. Đối tượng gần nhất và xa nhất

| Thuộc tính | Gần nhất | Xa nhất |
|---|---|---|
| Frame | s01f39 | s05f40 |
| Idx | 2 | 4 |
| Label | Car | Car |
| Dist | 3,40 m | 58,60 m |
| R | 3,53 m | 58,61 m |
| Số điểm | 2.187 | 25 |
| Length | 4,490 m | 4,765 m |
| Width | 1,747 m | 2,168 m |
| Height | 1,386 m | 1,909 m |

Đối tượng xa nhất có ít điểm hơn đối tượng gần nhất đáng kể. Điều này làm tăng mức độ không chắc chắn khi xác định kích thước và hướng.

### 5.3. So sánh kích thước và yaw

| Lớp | Length (m) | Width (m) | Height (m) |
|---|---:|---:|---:|
| Car | 4,515 | 1,787 | 1,315 |
| Bus | 13,100 | 2,641 | 3,180 |
| Cyclist | 1,804 | 0,859 | 1,866 |

Car và Bus có chiều dài, chiều rộng lớn hơn Cyclist. Cyclist có chiều cao lớn hơn Car mẫu do cuboid bao gồm người điều khiển.

Trong frame s01f20:

- Car idx 1 và Bus idx 4 có cùng yaw 269,5°.
- Bus idx 6 có yaw 89,7°, chênh lệch 179,8° so với Car idx 1, tương ứng hai hướng gần như đối diện.

### 5.4. Mật độ điểm theo khoảng cách

So sánh bốn Car trong frame s01f20:

| Idx | Khoảng cách (m) | Số điểm bên trong |
|---|---:|---:|
| 1 | 3,62 | 2.746 |
| 3 | 17,12 | 499 |
| 5 | 33,34 | 85 |
| 8 | 54,01 | 31 |

Car idx 8 chỉ có khoảng 1,13% số điểm của Car idx 1. Kết quả cho thấy xu hướng giảm số điểm khi khoảng cách tăng, mặc dù số điểm còn phụ thuộc vào kích thước, góc quan sát, che khuất và đặc tính bề mặt.

### 5.5. Quá trình sửa lỗi

Kết quả tự kiểm trước và sau hiệu chỉnh:

| Tiêu chí | Trước | Sau |
|---|---:|---:|
| Tổng cuboid | 79 | 78 |
| Cuboid bị cảnh báo | 11 | 8 |
| Cuboid rỗng | 1 | 0 |
| Frame hoàn thành | 10 | 10 |

Các hiệu chỉnh đã thực hiện:

- Xóa một cuboid rỗng tại frame s01f20, idx 11.
- Điều chỉnh chiều cao Car idx 1 tại s02f54 từ khoảng 1,03 m lên 1,637 m.
- Điều chỉnh chiều cao Car idx 4 tại s02f54 từ khoảng 1,03 m lên 1,219 m.

Số cuboid bị cảnh báo giảm từ 11 xuống 8, tương đương giảm khoảng 27,27%.

### 5.6. Các cảnh báo còn lại

| Frame | Idx | Label | Nội dung cần lưu ý |
|---|---:|---|---|
| s01f20 | 6 | Bus | Kích thước nhỏ và độ lệch mặt đường |
| s01f20 | 9 | Car | 17 điểm nằm ngoài cuboid |
| s02f07 | 5 | Car | Chiều cao 1,199 m, sát ngưỡng |
| s03f34 | 5 | Cyclist | Chiều dài 0,988 m, sát ngưỡng |
| s04f35 | 6 | Car | 37 điểm nằm ngoài cuboid |
| s04f35 | 8 | Car | 157 điểm nằm ngoài cuboid |
| s04f55 | 4 | Car | Ground gap −0,27 m |
| s05f40 | 6 | Car | Ground gap −0,37 m |

Các cảnh báo được giữ lại trong kết quả tự kiểm để phản ánh những trường hợp còn cần xem xét. Không tự động thay đổi kích thước chỉ nhằm loại bỏ cảnh báo nếu chưa có đủ bằng chứng hình học.

Đặc biệt, Bus idx 6 tại s01f20 cần được lưu ý về cả kích thước và khả năng phân loại nhầm. Các trường hợp ground gap có thể liên quan đến mặt đường cục bộ hoặc vị trí đáy cuboid.

### 5.7. Kiểm tra chéo

Học viên được đối chiếu: Võ Duy Thành – MSSV 2A202603211.

Tệp sử dụng:

- D13-2A202603211-bangA.csv.
- D13-2A202603211-tukiem.csv.

Đối tượng kiểm tra: Car, frame s01f24, idx 1.

Thông số đầu vào:

- X = 3,284 m.
- Y = −5,759 m.
- Z = −1,050 m.
- Height = 1,493 m.

| Thông số | Bảng A | Tính lại | Kết quả |
|---|---:|---:|---|
| dist_m | 6,63 m | 6,63 m | Khớp |
| z_bottom | −1,796 m | −1,7965 m | Khớp trong sai số làm tròn |

Ngoài ra, phát hiện một trường hợp cần xem xét tại frame s05f42, idx 6, Car có chiều cao 2,35 m, vượt ngưỡng tham chiếu 2,3 m.

Đề xuất kiểm tra lại chiều cao bằng Side View và Front View trước khi quyết định hiệu chỉnh.

Phạm vi kiểm tra chéo hiện tại là đối chiếu và tính toán lại số liệu CSV của học viên khác. Chưa xác minh trực quan trên CVAT và chưa có kết quả kiểm tra chiều ngược lại.

## 6. Ý nghĩa đối với mô hình AI và tự đánh giá

### 6.1. Ý nghĩa của nhãn 3D

Nhãn 3D sử dụng đơn vị mét thay vì pixel để mô hình học vị trí, kích thước và hướng thực tế của đối tượng trong không gian, phục vụ các nhiệm vụ như phát hiện vật thể 3D và nhận thức môi trường giao thông.

Các đối tượng thưa điểm cần quy tắc gán nhãn riêng vì thông tin hình học không đầy đủ, dễ tạo ra sai lệch về kích thước, vị trí và yaw nếu áp dụng cùng mức độ chắc chắn như đối tượng gần.

### 6.2. Sử dụng công cụ AI

- Công cụ: ChatGPT.
- Câu hỏi chính: Quy tắc gán nhãn cuboid 3D, cách sử dụng các góc nhìn CVAT, phân tích cảnh báo, kiểm tra số liệu và chuẩn bị báo cáo.
- Cách sử dụng: Tham khảo hướng dẫn, đối chiếu với dữ liệu xuất từ CVAT, tính toán lại thông số và tổng hợp báo cáo. AI không thay thế việc kiểm tra trực quan và quyết định gán nhãn của học viên.

### 6.3. Điều còn chưa chắc chắn

Tôi vẫn còn gặp khó khăn khi xác định kích thước đầy đủ của các đối tượng ở xa, bị che khuất hoặc có mật độ điểm thấp. Việc phân biệt điểm nhiễu với điểm thuộc đối tượng lân cận cũng cần được cải thiện.

Trong những bài tiếp theo, tôi sẽ chú trọng kiểm tra đồng thời các góc nhìn, xác định mặt đường cục bộ và đánh giá mức độ chắc chắn của từng cuboid trước khi hoàn thành.

### 6.4. Kết luận

Bài thực hành đã hoàn thành 10 frame với 78 cuboid, không còn cuboid rỗng, có đầy đủ 11 ảnh minh chứng và 8 cảnh báo được ghi nhận trong phiếu tự kiểm.

Qua bài lab, tôi hiểu rõ hơn mối quan hệ giữa ảnh camera, dữ liệu LiDAR, hệ tọa độ 3D, kích thước vật thể và chất lượng nhãn phục vụ huấn luyện mô hình AI.

Task CVAT Local được giữ nguyên để phục vụ việc kiểm tra lại khi cần thiết.
