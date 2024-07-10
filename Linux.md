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

- **`/bin`**: Chứa các chương trình thực thi cần thiết cho hệ thống cơ bản.
- **`/boot`**: Chứa các tập tin khởi động, bao gồm kernel.
- **`/dev`**: Chứa các tập tin thiết bị.
- **`/etc`**: Chứa các tập tin cấu hình hệ thống.
- **`/home`**: Chứa các thư mục người dùng.
- **`/lib`**: Chứa các thư viện hệ thống.
- **`/media`**: Thư mục để gắn kết các thiết bị lưu trữ ngoài.
- **`/mnt`**: Thư mục tạm để gắn kết các hệ thống tập tin.
- **`/opt`**: Chứa các ứng dụng tùy chọn.
- **`/proc`**: Hệ thống tập tin ảo chứa thông tin về các tiến trình và hệ thống.
- **`/root`**: Thư mục chính của người dùng root.
- **`/sbin`**: Chứa các chương trình hệ thống dành cho quản trị viên.
- **`/srv`**: Chứa các dữ liệu phục vụ cho các dịch vụ hệ thống.
- **`/tmp`**: Chứa các tập tin tạm thời.
- **`/usr`**: Chứa các ứng dụng và tập tin người dùng.
- **`/var`**: Chứa các tập tin biến đổi, như nhật ký hệ thống và hàng đợi thư.

![filesystem](Pictures/filesystem-2.jpg)

# Các lệnh cơ bản trong Ubuntu

### 1. Lệnh làm việc với tập tin và file

Dưới đây là một số lệnh cơ bản thường được sử dụng trong Ubuntu:

- `echo` : In nội dung ra màn hình.
- `man` : Hiển thị trang hướng dẫn cho lệnh.
- `ls `: Liệt kê các tập tin và thư mục.
    ![command_ls](Pictures/command_ls.jpg)
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
- `touch` : Tạo 1 tập tin trống.
- `vim/vi/nano` : Mở trình soạn thảo văn bản để tạo, chỉnh sửa tập tin.
- `cat` : In nội dung của file ra terminal.
- `grep` : Tìm kiếm nội dung trong tệp.
- `< , > , >>` : Định hướng nhập xuất.
- `|` : Đường ống là hình thức giao tiếp giữa các tiến trình với cơ chế đầu ra của lệnh này sẽ là đầu vào của lệnh khác.
    ![pipe](Pictures/pipe.jpg)
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
    ```
    ![chmod](Pictures/chmod.jpg)

- `chown` : Thay đổi chủ sở hữu của tệp.
    ![command_chown](Pictures/command_chown.png)
- `find` : Tìm kiếm file.
    ```bash 
    find path -name file_name   # tìm kiếm theo tên
    find path -inode file_name  # tìm kiếm theo inode
    find path -use file_name    # tìm kiếm theo user
    ```
### 2.Lệnh quản lý tiến trình
- `ps` : Liệt kê các tiến trình đang chạy.
- `top` : Hiển thị các tiến trình đang chạy theo thời gian thực.
- `kill` : Dừng tiến trình đang chạy.
- `reboot` : Khởi động lại hệ thống.
- `shutdown` : Tắt hệ thống.

### 3.Lệnh quản lý người dùng và nhóm
- `adduser` : Thêm người dùng mới.
    ```bash
    sudo adduser username
    ```
- `deluser` : Xóa người dùng.
    ```bash
    sudo deluser username
    ```
- `usermod` : Sửa đổi tài khoàn người dùng.
    ```bash
    sudo usermod -aG groupname username  # Thêm người dùng vào nhóm
    ```
- `groupadd` : Tạo nhóm mới.
    ```bash
    sudo groupadd groupname
    ```
- `groupdel` : Xóa nhóm.
    ```bash
    sudo groupdel groupname
    ```
### 4.Quản lý Gói Phần Mềm trong Linux/Ubuntu

Quản lý gói phần mềm là một phần quan trọng trong việc vận hành và bảo trì hệ thống Linux/Ubuntu. Dưới đây là tổng quan về các công cụ và lệnh quản lý gói phần mềm phổ biến trong Linux/Ubuntu.

#### APT (Advanced Package Tool)

APT là công cụ quản lý gói phổ biến trong các hệ thống dựa trên Debian, bao gồm Ubuntu.

- Cập nhật danh sách gói:
    ```bash
    sudo apt update
    ```
- Nâng cấp tất cả các gói đã cài đặt:
    ```bash
    sudo apt upgrade
- Cài đặt gói mới:
    ```bash
    sudo apt install package_name
- Gỡ bỏ gói:
    ```bash
    sudo apt remove package_name
- Gỡ bỏ gói cùng với các tệp cấu hình:
    ```bash
    sudo apt purge package_name
- Tìm kiếm gói:
    ```bash
    apt search package_name
- Hiển thị thông tin chi tiết về gói:
    ```bash
    apt show package_name
- Tự động gỡ bỏ các gói không còn cần thiết:
    ```bash
    sudo apt autoremove
    ```

#### dpkg (Debian Package)
dpkg là công cụ quản lý gói cấp thấp trong các hệ thống dựa trên Debian. Nó chủ yếu được sử dụng để cài đặt, gỡ bỏ và cung cấp thông tin về các gói phần mềm.

- Cài đặt gói từ tệp .deb:
    ```bash
    sudo dpkg -i package_name.deb
- Gỡ bỏ gói:
    ```bash
    sudo dpkg -r package_name
- Cấu hình lại gói:
    ```bash
    sudo dpkg --configure package_name
- Hiển thị danh sách các gói đã cài đặt:
    ```bash
    dpkg -l
- Kiểm tra tệp gói:
    ```bash
    dpkg -S /path/to/file
    ```

# Bash Programing
## 1.Các loại biến
- Biến môi trường 
- Biến người dùng
- Biến tự động

#### Biến môi trường
Biến môi trường trong Linux rất quan trọng vì chúng lưu trữ thông tin hệ thống cần thiết cho các chương trình và script hoạt động đúng cách.
- Một số biến đặc biệt do hệ thống tạo ra như \$HOME, \$PATH, .... , \$PS1,...
- Một số khác do người sử dụng tạo ra, được đặt trong tệp $HOME/.profile
- Cách tạo biến môi trường của người sử dụng: 
    ```bash
    export <tên biến không có $>=<giá trị biến>
    # Ví dụ: export MYNAME=“Du Tu”
    ```
- `env` : Lệnh xem các biến môi trường.
    ![command_env](Pictures/command_env.png)
- `printenv` : Lệnh xem giá trị của biến môi trường.
    ![printenv](Pictures/command_printenv.jpg)
- `unset` : Xóa biến môi trường.
    ```bash
    unset VAR_NAME
    ```
#### Biến người dùng
Biến người dùng (User-defined variables) là các biến do người dùng định nghĩa trong các script hoặc trong các phiên làm việc của shell. Chúng có thể là biến cục bộ hoặc biến toàn cục:

- `<tên biến>=<giá trị>` : Gán giá trị cho biến **(trước và sau dấu = không có khoảng trống)**
- `$<tên biến>` : Lấy giá trị của biến.

- **Biến toàn cục**: Được định nghĩa và có thể truy cập từ bất kỳ đâu trong script.

    ```bash
    # Định nghĩa biến toàn cục
    my_var="Hello, World!"
    echo $my_var  # Sử dụng biến toàn cục
    ```
- **Biến cục bộ**: Được định nghĩa và chỉ có thể truy cập trong phạm vi một hàm hoặc một khối lệnh cụ thể.
    ```bash
    # Định nghĩa biến cục bộ trong hàm
    function my_function {
        local my_local_var="I am local"
        echo $my_local_var  # Sử dụng biến cục bộ
    }
    my_function

    #Biến cục bộ trong vòng lặp
    for i in {1..3}
    do
        local loop_var="Loop iteration $i"
        echo "Inside loop: $loop_var"
    done

    # Biến cục bộ không thể truy cập bên ngoài vòng lặp
    echo "Outside loop: $loop_var"
    ```
### Biến tự động
- Biến tự động là các biến do hệ thống tự động tạo ra.
- Biến tự động là biến chỉ đọc, tức là chúng ta chỉ được đọc giá trị của biến tự động và không được gán giá trị cho biến tự động.
- Biến tự động: `$1`, `$2`, …. `$9`
- Biến $0 tham chiếu shell hiện hành
- `$*` *Danh sách các thông số là toàn bộ các tham số dòng lệnh được ghép thành 1 xâu*
- `$#` *Số lượng các tham số - chứa tổng số các tham số dòng lệnh không tính biến `$0`*
- `$$` *Tên tiến trình đính kèm*
- `$@` *Danh sách tham số*
- `$?` *Mã trở lại của lệnh thực hiện cuối cùng Chứa giá trị kết quả trả lại của câu lệnh trước*
- `$!` *Tên của tiến trình được đưa ra sau cùng*

    ```bash
    # Hiển thị tên của chương trình
    echo "Ten chuong trinh: $0"

    # Hiển thị số lượng biến
    echo "So luong bien: $#"

    # Hiển thị các biến tham số đầu tiên
    echo "Bien 1 : $1"
    echo "Bien 2 : $2"

    # Hiển thị tất cả các biến tham số dưới dạng chuỗi đơn
    echo "Tat ca cac bien : $*"
    ```

    Kết quả:
    ![automatic_var](Pictures/automatic_var.jpg)

- `read <option> <varname>` : đọc giá trị từ bàn phím và gán cho biến.
    ```bash
    read -p "Nhap ten cua ban: " name
    ```

## 2. Các phép toán số học trong Bash Progamming
Bash programming đầy đủ các phép toán số học cơ bản.
```bash
# expr <biểu thức>
expr 4 + 6     # In ra: 10
expr 10 - 10   # In ra: 0
expr 2 \* 2
expr 10 / 5

expr $var1 + $var2

# $((biểu thức))
var=$((4 + 8))   # Gán var = 12
var=$(($var1+$var2))
```

## 3.Các phép toán so sánh
**Toán tử so sánh số học**
- `-eq`: Bằng
- `-ne`: Không bằng
- `-gt`: Lớn hơn
- `-lt`: Nhỏ hơn
- `-ge`: Lớn hơn hoặc bằng
- `-le`: Nhỏ hơn hoặc bằng

**Toán tử so sánh chuỗi**
- `=`  : Bằng
- `!=` : Không bằng
- `<`  : Nhỏ hơn (theo thứ tự từ điển)
- `>`  : Lớn hơn (theo thứ tự từ điển)
- `-z` : Chuỗi rỗng

## 4. Các cấu trúc lệnh if, for, while
**Câu lệnh if**

```bash
if [ điều kiện 1 ] ; then
    # Các câu lệnh thực thi khi điều kiện 1 đúng
elif [ điều kiện 2 ] ; then
    # Các câu lệnh thực thi khi điều kiện 2 đúng
else 
    # Các câu lệnh thực thi khi các điều kiện sai
fi
```

**Câu lệnh for**

```bash
for variable in list
do
    # Các câu lệnh thực thi với mỗi giá trị trong danh sách
done
```

**Câu lệnh while**

```bash
while [ điều kiện ]
do
    # Các câu lệnh thực thi khi điều kiện đúng
done
```

**1 chương trinh đơn gian**

```bash
!/bin/bash

read -p "Ten ban la gi? " name
echo "Chao $name"
while true
do
        sleep 1
        read -p "May bao nhieu tuoi? " age
        if [ $age -lt 18 ]; then
                sleep 1
                echo "Cam nguoi duoi 18 tuoi"
                break
        else
                sleep 1
                echo "May boc phet"
        fi
done
```
Kết quả:
![test](Pictures/test_bash.jpg)
