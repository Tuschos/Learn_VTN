# Linux

## Tổng quan về Linux

Linux là một hệ điều hành mã nguồn mở, dựa trên nhân Linux, được phát triển bởi Linus Torvalds vào năm 1991. Linux được sử dụng rộng rãi trong các máy chủ, thiết bị nhúng và ngày càng phổ biến trên máy tính để bàn và laptop. Một trong những đặc điểm nổi bật của Linux là tính linh hoạt, bảo mật cao và cộng đồng hỗ trợ mạnh mẽ.

## Kiến trúc hệ điều hành Linux

Kiến trúc của Linux bao gồm 3 thành phần chính sau:

![Linux Architecture](Pictures/Kientruc_Linux.jpg)

1. **Kernel (Nhân):** Thành phần cốt lõi của hệ điều hành,chứa các modules, thư viện để quản lý tài nguyên hệ thống và giao tiếp giữa phần cứng và phần mềm.

![Kernel version](Pictures/kernel_vesion.jpg)

2. **Shell:** Là 1 chương trình có chức năng thực thi các lệnh (command) từ người dùng hoặc ứng dụng yêu cầu chuyển đến cho Kernel xử lý. Shell có thể hoạt động thông qua giao diện dòng lệnh hoặc các Script. Các loại Shell:
    - ***Bourne Shell (sh)***: Shell gốc của Unix.
    - ***Bourne Again Shell (bash)***: Phiên bản nâng cao của Bourne Shell, phổ biến nhất trong các hệ điều hành Unix/Linux.
    - ***C Shell (csh)***: Có cú pháp giống ngôn ngữ lập trình C.
    - ***Korn Shell (ksh)***: Kết hợp các tính năng của Bourne Shell và C Shell.
3. **Applications (Ứng dụng):** là các chương trình phần mềm được thiết kế để thực hiện các tác vụ cụ thể cho người dùng. Các ứng dụng thường có giao diện người dùng đồ họa (GUI) hoặc giao diện dòng lệnh (CLI) và chạy trên hệ điều hành. Các ứng dụng thường sử dụng API và dịch vụ hệ điều hành để thực hiện các tác vụ, không tương tác trực tiếp với kernel như shell.


## Kiến trúc cây thư mục của Linux

Hệ thống tập tin của Linux được tổ chức dưới dạng một cây thư mục, với thư mục gốc là `/`. Dưới đây là một số thư mục quan trọng:

- `/bin`: Chứa các chương trình thực thi cần thiết cho hệ thống cơ bản.
- `/boot`: Chứa các tập tin khởi động, bao gồm kernel.
- `/dev`: Chứa các tập tin thiết bị.
- `/etc`: Chứa các tập tin cấu hình hệ thống.
- `/home`: Chứa các thư mục người dùng.
- `/lib`: Chứa các thư viện hệ thống.
- `/media`: Thư mục để gắn kết các thiết bị lưu trữ ngoài.
- `/mnt`: Thư mục tạm để gắn kết các hệ thống tập tin.
- `/opt`: Chứa các ứng dụng tùy chọn.
- `/root`: Thư mục chính của người dùng root.
- `/sbin`: Chứa các chương trình hệ thống dành cho quản trị viên.
- `/tmp`: Chứa các tập tin tạm thời.
- `/usr`: Chứa các ứng dụng và tập tin người dùng.
- `/var`: Chứa các tập tin biến đổi, như nhật ký hệ thống và hàng đợi thư.

# Các lệnh cơ bản trong Ubuntu

### Lệnh làm việc với tập tin và file

Dưới đây là một số lệnh cơ bản thường được sử dụng trong Ubuntu:

- `man` : Hiển thị trang hướng dẫn cho lệnh.
- `ls`: Liệt kê các tập tin và thư mục.
- `cd`: Thay đổi thư mục hiện tại.
    ```bash 
    cd /path/to/directory
- `pwd`: Hiển thị đường dẫn thư mục hiện tại.
- `cp` : Sao chép tập tin hoặc thư mục.
    ```bash
    cp directory/source_file destination
- `mv` : Di chuyển hoặc đổi tên tập tin hoặc thư mục.
- `rm` : Xóa tập tin hoặc thư mục.
- `mkdir` : Tạo thư mục mới.
- `vim/vi/nano` : Mở trình soạn thảo văn bản để tạo, chỉnh sửa tập tin.
- `sudo` : (Superuser do) thực hiện lệnh với quyền root.
- `chmod` : Thay đổi quyền truy cập tập tin hoặc thư mục
    ```bash
    chmod mode file
    # mode 
    # r (read): Quyền đọc tệp tin hoặc liệt kê nội dung thư mục.
    # w (write): Quyền ghi vào tệp tin hoặc thay đổi nội dung thư mục.
    # x (execute): Quyền thực thi tệp tin hoặc truy cập vào thư mục.
    # Ví dụ: u+r, g-w, o+x:
    # u (user): Người sở hữu tệp tin.
    # g (group): Nhóm sở hữu tệp tin.
    # o (others): Những người khác.
    # a (all): Tất cả (user, group, và others).

![chmod](Pictures\chmod.jpg)
      