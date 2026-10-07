# Trái tim hạt sáng xanh dương

Mở **blue-heart.html** bằng Chrome hoặc Edge. Trang tự phát toàn màn hình, không có nút, chữ hướng dẫn hay bảng chọn ảnh. Không cần thư viện/CDN.

**Giữ chuột trái và kéo ngang** để xoay tim 360° quanh trục dọc. Tim luôn đứng thẳng; kéo lên/xuống không làm nghiêng tim. Khi đang giữ chuột, góc quay do bạn điều khiển; thả chuột thì tim tiếp tục tự quay chậm. Trên màn hình cảm ứng, kéo ngang bằng một ngón.

## Thay ảnh thủ công

1. Đặt tối đa 5 ảnh trong thư mục **images**, cạnh file HTML. Ví dụ: `1.jpg`, `2.jpg`, `3.jpg`, `4.jpg`, `5.jpg`.
2. Mở **blue-heart.html** bằng trình soạn thảo, tìm `PHOTO_SOURCES` gần đầu file. Bỏ dấu `//` trước những đường dẫn muốn dùng, hoặc sửa tên và đuôi file để khớp ảnh của bạn:

```js
const PHOTO_SOURCES = [
  'images/1.jpg',
  'images/2.jpg',
  'images/3.png',
  'images/4.webp',
  'images/5.jpg',
];
```

3. Lưu file rồi tải lại trang. Có thể dùng ít hơn 5 ảnh; danh sách rỗng thì chỉ phát hiệu ứng tim. Code chỉ lấy tối đa 5 đường dẫn và bỏ qua ảnh không đọc được. Nên dùng ảnh JPG, PNG hoặc WebP. Ảnh được tải từ các file bạn chỉ định, không dùng bộ ảnh đã lưu bởi bảng chọn ảnh của phiên bản cũ.

Ảnh được cắt trong khung trái tim viền sáng, bay từ tâm đĩa đến giữa tim rồi mờ dần. Luồng ảnh bắt đầu sau khi tim tạo xong, mỗi ảnh lặp sau 8 giây.

## Thay lời nhắn thủ công

Tìm `GIFT_MESSAGES` gần đầu file HTML. Mỗi dòng là một câu; sửa, thêm hoặc xóa câu theo ý bạn:

```js
const GIFT_MESSAGES = [
  'Có một điều bất ngờ dành riêng cho bạn…',
  'Giữa muôn vì sao, bạn vẫn thật đặc biệt.',
  'Gửi bạn một trái tim, cùng thật nhiều yêu thương.',
  'Mong nụ cười luôn ở lại trên môi bạn. 💙',
];
```

Lời nhắn bắt đầu sau khi tim tạo xong và chờ thêm 0,9 giây. Chữ nằm giữa mép trên màn hình và đỉnh tim, chừa khoảng trống cho quầng sáng, tự chỉnh cỡ chữ khi đổi màn hình hoặc xoay tim. Mỗi từ xuất hiện cách nhau 0,08 giây, sáng lên nhẹ trong 0,12 giây. Câu giữ nguyên vị trí trong lúc các từ xuất hiện; khi đã hoàn chỉnh, giữ thêm 4,5 giây, tan dần trong 1,2 giây, rồi nghỉ 0,8 giây trước câu tiếp theo. Không hiện chồng hai câu. Các câu ngắn sẽ dễ đọc và tạo cảm giác như một lời nhắn riêng trong món quà.

Chỉnh các giá trị trong `MESSAGE_TIMING` để đổi nhịp chữ. Giảm `wordInterval` để các từ xuất hiện nhanh hơn; `hold` là thời gian đọc sau khi cả câu đã hiện đủ. Đặt `loop: false` nếu chỉ muốn phát bộ lời nhắn một lần; để danh sách `GIFT_MESSAGES` rỗng nếu không muốn hiện chữ. Lưu file rồi tải lại trang.

## Hiệu ứng

- Mở đầu chỉ có một màn mưa sao băng gồm 24 vệt, mỗi làn có đúng một sao băng. Màn mở đầu kéo dài 6 giây, không lặp thêm đợt trước khi tạo tim; các đường bay song song không giao nhau.
- Vệt sao băng và đầu hạt to hơn nữa, có quầng mềm. Một sao băng mở đầu bay trong khoảng 5,3 giây; các đợt tiếp theo dùng hệ số làm chậm 1,65 so với thời gian gốc.
- Đầu hạt của mưa mở đầu tăng nhẹ thêm 18%; lời nhắn của món quà chỉ xuất hiện khi tim đã kết hạt hoàn chỉnh.
- Sau màn mở đầu, đĩa và tim cùng kết hạt trong 4,8 giây. Tim tạo dần từ hạt bay lên từ đĩa.
- Tim luôn đứng thẳng và quay ngang liên tục, khoảng 75 giây một vòng sau khi tạo xong.
- Nền gồm 950 sao lấp lánh với 8 sao sáng nổi bật; không có thiên hà hay tinh vân.
- Khi tim hoàn thành, các đợt sao băng ngẫu nhiên 3 hoặc 5 vệt vẫn tiếp tục, xen kẽ khoảng nghỉ. Tải lại trang để bắt đầu lại màn mở đầu và đổi lịch sao băng.

## Chỉnh code

Các giá trị nằm trong `CONFIG` gần đầu phần script:

| Biến | Mặc định | Tác dụng |
| --- | --- | --- |
| `rotationSeconds` | `75` | Số giây một vòng quay; tăng để tim quay chậm hơn |
| `introMeteors` | `24` | Số sao băng trong màn mở đầu duy nhất; tối đa 32 |
| `introHeadScale` | `1.18` | Tăng kích thước đầu hạt của riêng mưa mở đầu |
| `introSeconds` | `6` | Thời gian mưa sao băng mở đầu; tăng để đi chậm hơn |
| `meteorThickness` | `3.1` | Hệ số độ dày vệt sao băng |
| `meteorSlowdown` | `1.65` | Hệ số làm chậm các đợt sao băng sau khi tim hoàn thành |
| `formationSeconds` | `4.8` | Thời gian đĩa và tim cùng kết hạt |
| `photoFlightSeconds` | `8` | Chu kỳ bay của một ảnh |
| `heartParticles` | `9000` | Số hạt tạo tim |
| `stars` | `950` | Số sao trên nền |
| `heartSize` | `0.26` | Kích thước tim |

API JavaScript vẫn có sẵn để tích hợp, không tạo giao diện trên trang:

```js
blueHeart.pause();
blueHeart.play();
blueHeart.replay();
blueHeart.seek(13);
blueHeart.setRotation(20);
await blueHeart.photosReady;
console.log(blueHeart.photoCount, blueHeart.view);
```

WebGL vẽ hạt với quầng và lõi sáng; Canvas 2D vẽ bệ, vệt sáng và khung ảnh. Nếu không có WebGL, chương trình dùng Canvas 2D dự phòng. Đây là bản mô phỏng viết lại từ video, không phải mã nguồn gốc.
