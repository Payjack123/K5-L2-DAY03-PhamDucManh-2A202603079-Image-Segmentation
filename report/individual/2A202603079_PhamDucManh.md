# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

- Mã học viên theo lớp: 2A202603079
- Ngày / CVAT local: 11–12/11/2026 (http://localhost:8080/)
- Công cụ đã dùng: Polygon, Brush/Mask (và Eraser), AI Tools (Interactors) trên CVAT local

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh |
| --- | --- | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 |
| medium_instance | medium_instance.zip | 3 / 3 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 |
| cp1_holes | cp1_holes.zip | 1 / 1 |
| cp2_slice | cp2_slice.zip | 1 / 1 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 |
| cp3_thin | cp3_thin.zip | 1 / 1 |
| cp4_curb | cp4_curb.zip | 1 / 1 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 |

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Trong ảnh `000000181542.jpg` (`medium_instance`), tôi tự vẽ bằng Polygon người mặc áo kẻ sọc ngồi phía sau xe máy ở bên trái ảnh trước khi thử công cụ gợi ý.
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Tôi bám sát đường viền quanh mũ, mặt, vai và lưng nhìn thấy; đến chỗ bị người lái xe máy phía trước che khuất thì dừng lại, không vẽ đoán phần chân bị che phía dưới.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Ở `medium_instance`, tôi chủ yếu tự vẽ Polygon cho từng vật để kiểm soát biên, loại bỏ các hình người mẫu trên tấm bảng quảng cáo cửa hàng CHANEL và tách riêng từng chiếc xe đậu sát nhau trong hai ảnh đường phố còn lại (`000000373353.jpg` và `000000458325.jpg`). Sang `hard_panoptic` và các checkpoint có dùng AI Tools gợi ý mask, tôi luôn kiểm tra lại tên class, xóa phần bị lem ra mặt đường và tách lại các xe đứng gần nhau bị dính thành một khối.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `easy_semantic` / ảnh `7ee6d192-89e2408b.jpg` (đoạn đường cao tốc và hai bên sườn đồi).
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: Biên và thiếu lớp.
- Bằng chứng tôi nhìn thấy: Khi vẽ lần đầu, tôi tô lớp `road` trùm luôn lên thân chiếc xe con bạc, xe van trắng và mấy xe phía xa trên mặt đường; đồng thời phần cây bụi xanh đậm trên hai sườn đồi vẫn để trống chưa tô `vegetation`.
- Quy tắc và hành động sửa: Theo quy tắc chỉ vẽ phần nhìn thấy và đúng 5 lớp của task, tôi dùng cục tẩy (Eraser) xóa phần thân các ô tô ra khỏi mask `road` (vì xe không thuộc 5 class của Easy) và tô thêm lớp `vegetation` cho các cụm cây xanh trên sườn đồi.
- Sau sửa đã Save và export lại chưa? Đã bấm `Save` và xuất lại file `easy_semantic.zip`.

Khi rà lại các file đã xuất, tôi cũng ghi nhận một số thiếu sót còn lại ở các phần nhiều vật thể: ở `medium_instance` (`000000181542.jpg`), chiếc xe máy phía sau người mặc áo dài đang bị tách thành hai khoanh `motorcycle` ở hai bên thân người và còn sót phần bánh xe ở góc dưới trái; ở `hard_panoptic`, tôi đã vẽ đủ 5 lớp nền nhưng còn thiếu một số vật nhỏ như đèn giao thông, xe máy/xe đạp ở ảnh đầu và hai chiếc xe tải ở ảnh sau; ở `cp1_holes`, còn sót một chiếc ô tô đen nhỏ ở xa phía bên phải.

Chưa có điểm (mới kiểm tra cấu trúc ZIP qua `inspect_submissions.py`, chưa đối chiếu với đáp án).

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `easy_semantic` / `7ee6d192-89e2408b.jpg` (sườn đồi màu nâu vàng hai bên đường) | Tô cả vùng cỏ khô/đất trên đồi là `vegetation`, hay chỉ tô cây bụi xanh đậm và chừa đất trống ngoài 5 class. | `classes.json` của Easy chỉ có 5 lớp (`road`, `sidewalk`, `building`, `vegetation`, `sky`), không ép tô vùng chưa chắc. | Tôi chỉ tô `vegetation` cho các mảng cây bụi xanh rõ, chừa phần đất nâu vàng. Nhờ coach xác nhận trường hợp cỏ khô trên đồi có tính vào `vegetation` không. |
| 2. `medium_instance` / `000000181542.jpg` (xe bán tải bạc ở giữa & xe máy bị người áo dài che) | Xe bán tải xếp vào `car` hay `truck`; xe máy bị người áo dài cắt đôi nên vẽ một mask hay tách hai bên. | Trong bài tôi đang để xe bán tải là `car`, còn xe máy bị tách hai khoanh ở hai bên; theo quy tắc vật bị che vẫn tính là một instance. | Nhờ coach giải đáp ranh giới giữa `car` và `truck` với xe bán tải; với các ca bị che sau đó (như `cp5_occlusion`) tôi đã gộp chung một mask bằng Brush. |
| 3. `cp6_coverage` / `7daa6479-67988f3f.jpg` (xe buýt MTA, xe tải) & `cp4_curb` / `7d83710e-4697c3b2.jpg` (bên phải bó vỉa) | Ở `cp6_coverage`, xe buýt trắng và xe tải có gán tạm sang `car` để phủ kín không; ở `cp4_curb`, bồn đất trồng cây và sân đỗ xe bên phải có tính vào `sidewalk` không. | `cp6_coverage` chỉ có 7 class (không có `bus`, `truck`); `cp4_curb` yêu cầu tách `road`–`sidewalk` theo bó vỉa và chức năng. | Ở `cp6_coverage` tôi chừa xe buýt và xe tải ra không gán sai thành `car`; ở `cp4_curb` tôi chỉ tô dải vỉa hè đi bộ dọc bó vỉa và chừa bồn cây, sân đỗ xe riêng. |
