# BÀI THỰC HÀNH 1

## NỘI DUNG THỰC HÀNH

Các khái niệm cơ bản về lập trình hướng đối tượng (OOP), SOLID và GRASP.

## Bảng thuật ngữ

- ОВХ – объёмно-весовые характеристики : đặc tính kích thước về trọng lượng
- SKU – stock keeping unit, товар, логическая сущность, обозначающая семейство товаров (пример: карточка товара на
  Ozon, артикул) : đơn vị quản lý hàng tồn kho , một thực thể logic biểu thị một dòng sản phẩm. ví dụ là thông tin sản 
  phẩm trên ozon, mã sản phẩm
- Экземпляр - đơn vị sản phẩm cụ thể – физическая сущность, обозначающая отдельный товар (пример: товар, который вы 
  забираете из ПВЗ) : một thực thể vật lý biểu thị một món hàng riêng lẻ. vd món hàng lấy tại điểm nhận hàng
- Товародвижение - Luân chuyển hàng hóa – процесс перемещения экземпляров между складами, ПВЗ и т.д. : quá trình di
  chuyển các đơn vị sản phẩm cụ thể giữa các kho, điểm nhận hàng.

## Giới thiệu

Để bảo đảm hoạt động của các kho, cần triển khai các cơ chế luân chuyển hàng hóa giữa các kho. Một số hàng hóa có thể 
có nhu cầu cao hơn ở những khu vực địa lý khác nhau. Có nhiều loại kho, mỗi loại có vai trò riêng trong quá trình giao 
hàng đến người dùng cuối : 

- Trung tâm phân loại (Сортировочные центры): không lưu trữ lượng lớn hàng hóa; đóng vai trò là các điểm trung chuyển, 
  phân phối hàng hóa giữa các kho khác để tiếp tục giao hàng.
- Trung tâm hoàn tất đơn hàng (Фулфилменты): các tổ hợp logistics lớn, lưu trữ lượng lớn hàng hóa; từ đây, hàng hóa 
  được chuyển đến các điểm nhận hàng (ПВЗ).

Các hoạt động luân chuyển hàng hóa được thực hiện bằng phương tiện vận tải hàng hóa, theo những phiếu lộ trình 
(маршрутные листы) cụ thể. Các phiếu này mô tả đường đi của phương tiện và chỉ rõ các hoạt động luân chuyển hàng hóa cần thực hiện.

Để bảo đảm các hoạt động luân chuyển hàng hóa được thực hiện tối ưu, cần có khả năng mô phỏng việc thực hiện các phiếu 
lộ trình, nhằm kiểm tra tính hợp lệ của chúng và dự đoán thời gian thực hiện.

## Yêu cầu bài tập

Thiết kế và triển khai mô hình đối tượng để xây dựng các phiếu lộ trình và mô phỏng quá trình thực hiện các lộ trình đó.

## Miền nghiệp vụ (Предметная область)


### Xe tải (Грузовик)

Phương tiện vận tải hàng hóa thực hiện các hoạt động luân chuyển hàng hóa giữa các kho.

Thuộc tính:

- Sức chứa: tổng giá trị ОВХ (đặc tính thể tích và trọng lượng) tối đa của tất cả các đơn vị sản phẩm được xếp lên xe.
- Tốc độ.
- Tọa độ: vĩ độ + kinh độ.
- Các đơn vị sản phẩm đang chở: được xác định bằng tập hợp các cặp (SKU + số lượng).

Chức năng:

- Xếp hàng lên xe: nhận tập hợp các cặp (SKU + số lượng), kiểm tra xem có đáp ứng giới hạn sức chứa hay không.
- Dỡ hàng khỏi xe: nhận tập hợp các cặp (SKU + số lượng), kiểm tra xem trên xe có đủ hàng tương ứng hay không.

### Hàng hóa (SKU)

Thuộc tính:

- Mã định danh 
- Tên
- ОВХ : đặc tính thể tích và trọng lượng

### Nhân viên 

Nhân viên kho thực hiện các hoạt động nhận hàng và xuất hàng. Nhân viên vận chuyển hàng bằng xe đẩy cơ giới, 
vì vậy thời gian vận chuyển hàng không phụ thuộc vào trọng lượng của hàng.

Thuộc tính:

- ОВХ tối đa có thể vận chuyển: giới hạn về thể tích và trọng lượng hàng mà nhân viên có thể vận chuyển trong một lượt.
- Thời gian dỡ/xếp hàng: khoảng thời gian cần để lấy một nhóm đơn vị sản phẩm khỏi xe tải hoặc xếp chúng lên xe tải.
- Thời gian vận chuyển: khoảng thời gian cần để di chuyển một nhóm đơn vị sản phẩm.

Khi thực hiện các hoạt động luân chuyển hàng hóa tại kho (nhận hàng, xuất hàng), cần phân chia các đơn vị sản phẩm cần
di chuyển cho các nhân viên kho:

- Các nhân viên khác nhau có thể có giới hạn ОВХ vận chuyển khác nhau.
- Việc di chuyển toàn bộ các đơn vị sản phẩm có thể đòi hỏi nhiều hơn một lượt làm việc của toàn bộ nhân viên kho.

### Kho ( Склад )

Kho là nơi lưu trữ hàng hóa, chứa thông tin về những hàng hóa hiện có trong kho. Lượng hàng này không cố định: 
kho có thể được bổ sung hàng và hàng cũng có thể được đưa ra khỏi kho. Quá trình di chuyển hàng hóa giữa các kho được gọi là “luân chuyển hàng hóa”.

Thuộc tính :

- Mã định danh 
- Hàng tồn kho: tập hợp các cặp SKU + số lượng
- Tọa độ
- Вместимость – максимальное суммарное значение ОВХ всех хранящихся экземпляров
- Штат – набор сотрудников склада

Chức năng :

- Приёмка – принимает набор (SKU + количество) для хранения на складе, проверяет допустимость по вместимости
- Отгрузка – принимает набор (SKU + количество) для выгрузки со склада, проверяет наличие по стоку

### Phiếu lộ trình (Маршрутный лист)
Tập hợp các hoạt động luân chuyển hàng hóa được áp dụng cho xe tải. Kết quả thực hiện phiếu lộ trình là thời gian 
hoàn thành lộ trình. Kết quả cũng có thể là lỗi, nếu bất kỳ giai đoạn nào xảy ra lỗi trong quá trình thực hiện.

#### Giai đoạn – Di chuyển xe tải (Движение грузовика)

Thực hiện việc di chuyển xe tải đến tọa độ được chỉ định. Kết quả là thời gian cần để xe tải di chuyển đến đó.

#### Giai đoạn – Nhận hàng (Приёмка)

- Giai đoạn dỡ hàng khỏi xe tải và chuyển hàng vào kho.
- Chứa bản kê nhận hàng, là tập hợp các cặp (SKU + số lượng).
- Để nhận hàng thành công, cần:
    - Có đủ tất cả các đơn vị sản phẩm cần nhận trên xe tải.
    - Kho có đủ sức chứa cho toàn bộ số hàng được nhận.
    -  Xe tải đang ở kho, tức là cách tọa độ của kho không quá 10 mét.
- Kết quả của việc nhận hàng là thời gian dành cho hoạt động này:
  - Thời gian được tính có xét đến đội ngũ nhân viên kho.
  - Bao gồm thời gian nhân viên dỡ hàng và thời gian nhân viên chuyển hàng vào kho, ngoại trừ lần chuyển hàng cuối cùng (xem bên dưới).
  - Thời gian dành cho lượt chuyển hàng cuối cùng vào kho không được tính vào thời gian nhận hàng, vì xe tải không bắt buộc phải chờ đến khi những đơn vị sản phẩm cuối cùng được chuyển vào kho.

#### Giai đoạn – Xuất hàng (Отгрузка)

- Giai đoạn đưa hàng ra khỏi kho và chuyển hàng lên xe tải.
- Chứa bản kê xuất hàng, là tập hợp các cặp (SKU + số lượng).
- Để xuất hàng thành công, cần:
  - Có đủ tất cả các đơn vị sản phẩm cần xuất trong kho.
  - Xe tải có đủ sức chứa cho toàn bộ số hàng được xuất.
  - Xe tải đang ở kho, tức là cách tọa độ của kho không quá 10 mét.
- Kết quả của việc xuất hàng là thời gian dành cho hoạt động này:
  -  Thời gian được tính có xét đến đội ngũ nhân viên kho.
  - Bao gồm thời gian nhân viên xếp hàng lên xe và thời gian nhân viên vận chuyển hàng.

## Tiêu chí hoàn thành

- Đã triển khai mô hình đối tượng thể hiện tất cả các thực thể trong bài thực hành.
- Có thể tạo phiếu lộ trình và thực hiện nó với các xe tải khác nhau.
- Có thể thiết lập thông qua tham số tất cả các giá trị cần thiết.
- Logic nghiệp vụ được đóng gói đúng cách trong các thực thể phù hợp.
- Phần triển khai tuân thủ các khái niệm cơ bản của lập trình hướng đối tượng (OOP).
- Tuân thủ các nguyên tắc SOLID và GRASP.
- Đã triển khai đầy đủ việc kiểm tra tính hợp lệ của các giá trị cần thiết.

## Các kịch bản kiểm thử : 

### Sản phẩm (SKU)

- Tạo sản phẩm với các thuộc tính hợp lệ.
- Tạo sản phẩm có ОВХ (đặc tính thể tích và trọng lượng) không hợp lệ — âm hoặc bằng 0 → lỗi kiểm tra tính hợp lệ.

### Xe tải

- Tạo xe tải có tốc độ không hợp lệ — âm hoặc bằng 0 → lỗi kiểm tra tính hợp lệ.
- Xếp một nhóm sản phẩm lên xe trong giới hạn sức chứa → lượng hàng trên xe được cập nhật chính xác.
- Xếp một nhóm sản phẩm vượt quá sức chứa, kể cả khi tính thêm hàng đã có trên xe → báo lỗi, lượng hàng trên xe không 
thay đổi.
- Dỡ một nhóm sản phẩm có trên xe → lượng hàng trên xe giảm chính xác.
- Dỡ một nhóm sản phẩm có số lượng vượt quá số lượng hiện có theo SKU → báo lỗi.
- Di chuyển đến tọa độ được chỉ định → tọa độ được cập nhật, thời gian được tính theo khoảng cách và tốc độ.

### Nhân viên

- Tính thời gian vận chuyển một nhóm sản phẩm nằm trong giới hạn ОВХ tối đa mà nhân viên có thể vận chuyển.
- Vận chuyển một nhóm sản phẩm vượt quá giới hạn của nhân viên trong một lượt → cần nhiều lượt hoặc báo lỗi, tùy theo mô hình.

### Kho

- Nhận một nhóm sản phẩm trong giới hạn sức chứa còn trống → lượng hàng tồn kho tăng.
- Nhận một nhóm sản phẩm vượt quá sức chứa của kho → báo lỗi, lượng hàng tồn kho không thay đổi.
- Xuất một nhóm sản phẩm có trong kho → lượng hàng tồn kho giảm.
- Xuất một nhóm sản phẩm có số lượng vượt quá số lượng hiện có theo SKU → báo lỗi.
- Tính thời gian nhận/xuất hàng khi đội ngũ có một nhân viên và khi có nhiều nhân viên.
- Tính số lượt cần thiết khi lượng hàng cần di chuyển vượt quá tổng khả năng vận chuyển của toàn bộ nhân viên trong một lượt.

### Lộ trình di chuyển - giai đoạn " Xe tải di chuyển "

- Khi thực hiện, xe tải di chuyển đến tọa độ đích; thời gian được tính theo khoảng cách và tốc độ.

### Lộ trình di chuyển - giai đoạn " Nhận hàng "

- Thực hiện thành công: xe tải có đủ hàng, kho có đủ sức chứa, xe tải cách kho không quá 10 mét → hàng được chuyển vào kho; 
  thời gian của lượt chuyển hàng cuối cùng không được tính vào tổng thời gian nhận hàng.
- Báo lỗi khi xe tải không đủ hàng, kho không đủ sức chứa hoặc khoảng cách đến kho vượt quá giới hạn cho phép.

### Lộ trình di chuyển - giai đoạn " Xuất hàng "

- Thực hiện thành công: kho có đủ hàng, xe tải có đủ sức chứa, xe tải cách kho không quá 10 mét → hàng được chuyển lên xe tải.
-  Báo lỗi khi kho không đủ hàng, xe tải không đủ sức chứa hoặc khoảng cách đến kho vượt quá giới hạn cho phép.
-  Tính thời gian xuất hàng có xét đến đội ngũ nhân viên kho.

### Phiếu lộ trình - cho toàn bộ cả 3 giai đoạn

- Thực hiện thành công chuỗi giai đoạn: xuất hàng từ kho 1 → vận chuyển đến kho 2 → nhận hàng tại kho 2. Tổng thời gian
 bằng tổng thời gian của các giai đoạn.
- Dừng thực hiện khi xảy ra lỗi ở một trong các giai đoạn.

---
