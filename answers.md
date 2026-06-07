# PBT_04:
---
## Phần A:
### Câu A1(12_css_positioning - 3. ⚙️ Core Technical Truth):
| Position | Vẫn chiếm chỗ trong flow? | Tham chiếu vị trí | Cuộn theo trang? | Use case |
|----------|---------------------------|-------------------|------------------|----------|
| `static` | Có | Theo luồng tài liệu tự nhiên (Normal flow) | Có | Là giá trị mặc định cho mọi phần tử, dùng khi muốn reset lại định vị |
| `relative` | Có (Vẫn giữ nguyên khoảng trống ban đầu) | Chính vị trí gốc ban đầu của nó | Có | Làm điểm tựa (gốc tọa độ) cho các phần tử con dùng absolute, hoặc dịch chuyển nhẹ mà không làm vỡ layout xung quanh |
| `absolute` | Không (Bị tách khỏi normal flow) | Phần tử tổ tiên gần nhất có định vị | Có | Làm các thành phần đè lên nhau như: dropdown menu, icon thông báo trên góc avatar, nút close của modal |
| `fixed` | Không (Bị tách khỏi normal flow) | Cửa sổ trình duyệt | Không (Đứng yên khi cuộn trang) | Thanh điều hướng (Navbar) luôn dính ở đỉnh màn hình, nút "Back to top", hoặc cửa sổ chat hỗ trợ trực tuyến |
| `sticky` | Có (Khi ở trạng thái bình thường) | Cuộn theo flow cho đến khi chạm ngưỡng xác định (bởi top, bottom,...) thì tham chiếu theo viewport | đến khi chạm ngưỡng xác định (bởi top, bottom,...) thì tham chiếu theo viewport.	Có (Nhưng sẽ dính lại tại một vị trí khi cuộn qua) | Tiêu đề danh mục trong danh sách dài, hoặc thanh sidebar cuốn theo màn hình nhưng dừng lại khi hết container |

- `absolute` tham chiếu `body` khi:
  - `absolute` sẽ tham chiếu đến `body` (chính xác hơn là thẻ `<html> - viewport` ban đầu) khi tất cả các phần tử cha, ông, tổ tiên bao bọc bên ngoài nó đều có `position: static` (mặc định).
  - `absolute` sẽ tham chiếu đến `parent` (hoặc một tổ tiên bất kỳ) khi phần tử đó được thiết lập một giá trị position khác với `static` (thường dùng nhất là `position: relative`).
- Khái niệm "nearest positioned ancestor": là "phần tử tổ tiên gần nhất có định vị". Khi bạn duyệt ngược từ phần tử hiện tại lên các cấp cha, ông, cố, cụ... phần tử đầu tiên nào mà bạn gặp có thuộc tính `position` mang giá trị khác `static` (như `relative, absolute, fixed, sticky`) thì đó chính là gốc tọa độ để tính toán các khoảng cách top, bottom, left, right cho phần tử `absolute`.

### Câu A2(13_creating_responsive_layouts - 3. ⚙️ Core Technical Truth):
- Trường hợp 1:
  - Dự đoán layout: 4 items sẽ nằm trên cùng 1 hàng ngang. Do có thuộc tính flex: 1, không gian hàng ngang của `.container` sẽ được chia đều 4 phần bằng nhau cho cả 4 items.
  - Sơ đồ bố cục:

        +-----------------------------------------------------------+
        |                      .container (Flex)                    |
        | +------------+ +------------+ +------------+ +------------+ |
        | |   Item 1   | |   Item 2   | |   Item 3   | |   Item 4   | |
        | | (Width 25%)| | (Width 25%)| | (Width 25%)| | (Width 25%)| |
        | +------------+ +------------+ +------------+ +------------+ |
        +-----------------------------------------------------------+
- Trường hợp 2:
  - Dự đoán layout: Bố cục gồm 3 hàng, mỗi hàng 2 cột. Do mỗi item chiếm 45% chiều rộng và margin: 2.5% cho cả 4 phía (tổng chiều rộng ngang một item chiếm là $2.5% + 45% + 2.5% = 50%$). Một hàng ngang (100%) vừa đủ chứa đúng 2 items. Vì có `flex-wrap: wrap`, 6 items sẽ tự động ngắt dòng đều đặn thành 3 hàng.
  - Sơ đồ bố cục:

        +-----------------------------------------------------------+
        |                 .container (Flex - Wrap)                  |
        |  +-----------------------+     +-----------------------+  |
        |  |        Item 1         |     |        Item 2         |  |
        |  +-----------------------+     +-----------------------+  |
        |  +-----------------------+     +-----------------------+  |
        |  |        Item 3         |     |        Item 4         |  |
        |  +-----------------------+     +-----------------------+  |
        |  +-----------------------+     +-----------------------+  |
        |  |        Item 5         |     |        Item 6         |  |
        |  +-----------------------+     +-----------------------+  |
        +-----------------------------------------------------------+
- Trường hợp 3:
  - Dự đoán layout: 3 items nằm trên 1 hàng ngang. Thẻ `.container` căn giữa các item theo trục dọc (`align-items: center`). Theo trục ngang (`justify-space-between`), Item 1 dính sát lề trái, Item 3 dính sát lề phải, và Item 2 nằm chính giữa khoảng trống của dòng.
  - Sơ đồ bố cục:

        +-----------------------------------------------------------+
        |                     .container (Grid)                     |
        | <- 200px ->   <20px>    <----- 1fr ----->   <20px>   <- 200px -> |
        | +----------+            +---------------+            +----------+ |
        | |  Item 1  |            |     Item 2    |            |  Item 3  | |
        | +----------+            +---------------+            +----------+ |
        +-----------------------------------------------------------+
- Trường hợp 4:
  - Dự đoán layout: Bố cục 1 hàng, 3 cột. Cột đầu tiên cố định 200px, cột thứ ba cố định 200px. Cột thứ hai ở giữa sử dụng đơn vị 1fr nên sẽ tự động giãn nở chiếm trọn toàn bộ không gian còn lại ở giữa. Giữa các cột có một khoảng cách (gap) là 20px.
  - Sơ đồ bố cục:

        +-----------------------------------------------------------+
        |                     .container (Grid)                     |
        | <- 200px ->   <20px>    <----- 1fr ----->   <20px>   <- 200px -> |
        | +----------+            +---------------+            +----------+ |
        | |  Item 1  |            |     Item 2    |            |  Item 3  | |
        | +----------+            +---------------+            +----------+ |
        +-----------------------------------------------------------+
- Trường hợp 5:
  - Dự đoán layout: Bố cục chia đều làm 3 cột bằng nhau (repeat(3, 1fr)). Tổng cộng có 7 items nên hệ thống sẽ tự chia thành 3 hàng.
    - Hàng 1: chứa Item 1, Item 2, Item 3.
    - Hàng 2: chứa Item 4, Item 5, Item 6.
    - Hàng 3: Chỉ chứa duy nhất Item 7 nằm ở cột đầu tiên bên trái, bỏ trống hoàn toàn vị trí của cột 2 và cột 3. Khoảng cách giữa các ô là 10px.
  - Sơ đồ bố cục:

        +-----------------------------------------------------------+
        |                 .container (Grid - 3 Columns)             |
        | +---------------+   +---------------+   +---------------+ |
        | |    Item 1     |   |    Item 2     |   |    Item 3     | |
        | +---------------+   +---------------+   +---------------+ |
        |       gap                   gap                 gap       |
        | +---------------+   +---------------+   +---------------+ |
        | |    Item 4     |   |    Item 5     |   |    Item 6     | |
        | +---------------+   +---------------+   +---------------+ |
        |       gap                   gap                 gap       |
        | +---------------+                                         |
        | |    Item 7     |        (Ô Cột 2 và Cột 3 trống)         |
        | +---------------+                                         |
        +-----------------------------------------------------------+
---
## Phần C:
### Câu C1:
- Tình huống 1: Navigation bar ngang (logo + menu + buttons)
  - Lựa chọn: Flexbox
  - Giải thích: Thanh điều hướng (`Navbar`) là bố cục 1 chiều (1D) theo hàng ngang. Các phần tử bên trong (logo, các liên kết menu, các nút bấm) có kích thước nội dung khác nhau và cần được phân bổ linh hoạt trên trục ngang đó (ví dụ: gom cụm hoặc đẩy xa nhau bằng `justify-content: space-between`). `Flexbox` được thiết kế hoàn hảo cho việc sắp xếp các phần tử dọc theo một trục duy nhất này.
- Tình huống 2: Lưới ảnh Instagram (3 cột đều nhau, số ảnh không biết trước)
  - Lựa chọn: Grid
  - Giải thích: Đây là bố cục 2 chiều (2D) dạng ma trận (cả hàng và cột). Tài liệu chỉ ra rằng Grid kiểm soát cấu trúc cột cực kỳ mạnh mẽ bằng cách định nghĩa sẵn khung lưới thông qua `grid-template-columns: repeat(3, 1fr)`. Khi số lượng ảnh không biết trước tăng lên, trình duyệt sẽ tự động đẩy các ảnh mới xuống hàng tiếp theo một cách đều đặn mà không cần tính toán thủ công hay lo bị vỡ dòng.
- Tình huống 3: Layout blog: main content + sidebar
  - Lựa chọn: Grid hoặc Flexbox
  - Giải thích:
    - Dùng Grid: Thích hợp khi muốn định hình một bộ khung trang web lớn (Page Layout). Có thể chia trang thành 2 cột với tỷ lệ cố định hoặc linh hoạt kèm theo khoảng cách gap chuẩn xác.
    - Dùng Flexbox: Cũng rất phổ biến cho layout này bằng cách đặt flex: 1 cho Main Content để nó tự giãn nở và cố định width: 300px cho Sidebar.
- Tình huống 4: Footer với 4 cột thông tin 
  - Lựa chọn: Kết hợp cả hai
  - Giải thích:
    - Grid ở bên ngoài: Dùng để quản lý bố cục tổng thể của Footer, chia đều bề ngang thành 4 cột bằng nhau bằng grid-template-columns: repeat(4, 1fr). Điều này giúp đảm bảo 4 khối thông tin luôn thẳng hàng kể cả khi nội dung dài ngắn khác nhau.
    - Flexbox ở bên trong: Dùng để sắp xếp các liên kết (links) bên trong từng cột. Các liên kết thường được xếp theo chiều dọc (hàng dọc cũng là bố cục 1 chiều), sử dụng flex-direction: column để căn chỉnh khoảng cách giữa các dòng chữ dễ dàng.
- Tình huống 5: Card sản phẩm (ảnh trên, text giữa, nút dưới — nút luôn dính đáy)
  - Lựa chọn: Flexbox
  - Giải thích : Mặc dù Card sản phẩm nằm trong một hệ thống lưới lớn, nhưng bản thân cấu trúc bên trong của một chiếc Card lại là bố cục 1 chiều theo trục dọc (hàng dọc). Khi thiết lập thuộc tính cho Card là display: flex; flex-direction: column;, ta chỉ cần đặt thêm dòng mã margin-top: auto; riêng cho phần tử nút bấm ở dưới cùng. Cơ chế kéo giãn khoảng trống của Flexbox sẽ tự động đẩy nút bấm đó xuống dính chặt vào đáy Card, bất kể phần text ở giữa dài hay ngắn.

### Câu C2:
- LỖI 1: Cards không đều chiều cao — Nút "Mua" bị nhảy lên/xuống
  - Nguyên nhân: Mặc dù container bên ngoài (`.card-container`) là Flexbox giúp các thẻ .card có chiều cao bằng nhau trên cùng một hàng (nhờ thuộc tính mặc định `align-items: stretch`), nhưng bản thân nội dung bên trong mỗi thẻ `.card` lại chưa được cấu hình theo luồng Flexbox. Khi tiêu đề h3 hoặc mô tả của card này dài hơn card khác, khối chữ sẽ đẩy nút `.btn` xuống. Ở những card có ít chữ, nút `.btn` sẽ bị kéo ngược lên trên, tạo ra sự nhấp nhô mất thẩm mỹ.
  - Code sửa:
```css
.card-container { 
    display: flex; 
    flex-wrap: wrap; 
}

.card { 
    width: 30%; 
    margin: 1.5%; 
    display: flex;
    flex-direction: column;
}

.card img { 
    width: 100%; 
}

.card h3 { 
    font-size: 18px; 
}

.card .btn { 
    padding: 10px; 
    margin-top: auto;
}
```
- LỖI 2: Items muốn nằm giữa cả ngang lẫn dọc nhưng vẫn dính góc trái trên
  - Nguyên nhân: Khi khai báo `display: flex;` cho `.hero`, trình duyệt mới chỉ kích hoạt chế độ Flexbox cho container đó chứ chưa hề nhận được lệnh căn chỉnh vị trí nào. Trạng thái mặc định của các flex items là luôn xếp từ góc trên cùng bên trái (`justify-content: flex-start` và `align-items: stretch`). Thuộc tính `text-align: center` chỉ có tác dụng căn giữa các dòng chữ bên trong thẻ `.hero-content`, chứ không thể căn giữa chính bản thân khối `.hero-content` đó đối với cha của nó.
  - Code sửa:
```css
.hero {
    height: 100vh;
    display: flex;

    justify-content: center; 
    align-items: center; 
}

.hero-content {
    text-align: center;
}
```
- LỖI 3: Sidebar bị co lại khi content quá dài
  - Nguyên nhân: Trong cơ chế mặc định của Flexbox, tất cả các flex items đều có thuộc tính `flex-shrink` mang giá trị mặc định là 1. Điều này có nghĩa là khi không gian của dòng không đủ (do nội dung bên thẻ `.content` quá dài và phình to ra), trình duyệt sẽ ép các phần tử còn lại co hẹp lại dưới mức kích thước định sẵn để tránh tràn layout. Do đó, cái `width: 250px` của `.sidebar` đã bị bóp nghẹt.
  - Code sửa:
```css
.layout { 
    display: flex; 
}

.sidebar { 
    width: 250px; 

    flex-shrink: 0; 
}

.content { 
    flex: 1;
}
```
