+++
date = '2025-01-02T22:41:34+07:00'
draft = true
title = 'Smb_share'
+++

## Trọng tâm
Về cơ bản Windows yêu cầu cấu hình cả "Sharing" và "Security" để có thể chia sẻ file/ đĩa/ thư mục. Chỉ cần vào Properties, chọn tab Sharing và thêm người dùng trong Advanced Sharing. Kế tiếp, chuyển đến tab security và thêm quyền hạn vào đó. Kiểm tra xem đã cho phép chia sẻ file chưa bằng cách vào Control Panel -> Network and Internet -> Network and Sharing Center -> Advanced sharing settings và bật các lựa chọn lên (Khuyến nghị bật mật khẩu).
- Nếu bạn cài đặt thành công file cần chia sẻ sẽ hiện trong \\\127.0.0.1 (Win + R).
## Giờ đến phần chính
### Cài đặt Zerotier tạo VLAN
Cách mà Zerotier hoạt động khá linh hoạt, khi đang ngoài mạng nội bộ/ mạng riêng nó sẽ hoạt động như một mạng riêng ảo/ VLAN và truyền tải file với tốc độ truyền tải tối đa bằng tốc độ kết nối mạng. Kiểm tra bằng [fast.com](https://fast.com). Ở mặt khác, nếu nó nhận ra cả máy nhận và máy chia sẻ file đang ở cùng một mạng [(LAN)](https://en.wikipedia.org/wiki/Local_area_network), tốc độ tải cao nhất ở đây sẽ chỉ là mặt giới hạn về router ( Hoặc do giới hạn bởi dây dẫn và chất lượng mạng không dây).
1. Tạo và kết nối các máy cần nhận và chia sẻ trong cùng một mạng với Zerotier.
2. Nếu nó hoạt động tốt, dùng `` \\<ip addresses> `` với IP là **IP của thiết bị trên mạng Zerotier**.
## Tăng cường bảo mật
Nếu bạn dùng các cài đặt mặt định, về lý thuyết nó vẫn tồn tại một vài vấn đề bảo mật như nếu một người nào đó biết được mật khẩu thiết bị của bạn và cả 2 đều đang trong cùng một mạng nội bộ, họ có thể âm thầm truy cập vào dữ liệu của bạn bởi vì bị rò rỉ tên_đăng_nhập/ mật_khẩu và hệ thống mạng bạn đang sử dụng.
- Ở control panel, chỉ cho phép chia sẻ file ở Private network. Đưa Zerotier vào danh sách mạng đó và tất cả các mạng khác đều thuộc Public network (Có thể ảnh hưởng đến tốc độ truyền tải trong mạng nội bộ ---**Chưa kiểm tra**---)
- Tạo tài khoản ảo, mở cmd với quyền admin``net user dummy password /add``, chọn bất kỳ ``username`` và ``password`` bạn muốn, Cá nhân tôi thích giữ nguyên ``dummy`` và chỉ thay đổi ``password``. Hãy lưu lại thông tin đăng nhập đó ở nơi Chúa không thể chạm vào, LOL.
- (Không bắt buộc) Dùng ``regedit`` và thay đổi giá trị trong ``HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon``
	1. Chuột phải vào Winlogon, chọn New > Key, đặt tên là SpecialAccounts.
	2. Bên trong SpecialAccounts key, tạo và đặt tên key khác là UserList.
	3. Chọn vào UserList key, chuột phải vào màn hình bên phải, chọn New > DWORD (32-bit) Value.
	4. Đặt tên cho DWORD value trùng với username, (``dummy``), và chuyển value thành 0.
- Cách này sẽ giấu tài khoản **dummy** khỏi màn hình đăng nhập.
- Giờ ở thư mục cần chia sẻ vào Sharing options và chọn chỉ tài khoản ``dummy``. Chắc chắn rằng ở tab Security có cấp quyền cho Authenticated user (Mặc định).
---
- Dựa trên [CIA triad](https://www.techtarget.com/whatis/definition/Confidentiality-integrity-and-availability-CIA), điều này sẽ giúp cho dữ liệu của bạn không bị rơi vào tay nhầm người, nhưng hãy nhớ rằng không thứ gì an toàn khi kết nối với Internet. Kể cả chiếc bóng đèn thông minh cũng có thể tham gia tấn công DDOS, LOL. 