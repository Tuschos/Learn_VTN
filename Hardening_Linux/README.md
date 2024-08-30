# Hardening Linux Server

## I. Giới thiệu

Trong bối cảnh công nghệ hiện đại, các máy chủ Linux ngày càng trở thành mục tiêu của các cuộc tấn công mạng. Việc đảm bảo an ninh cho hệ thống là một yếu tố quan trọng giúp bảo vệ dữ liệu và duy trì hoạt động ổn định cho doanh nghiệp. Hardening Linux Server là quá trình củng cố các biện pháp bảo mật nhằm giảm thiểu các lỗ hổng và tăng cường khả năng phòng vệ trước các mối đe dọa tiềm ẩn. Báo cáo này sẽ đi sâu vào các kỹ thuật và thực tiễn tốt nhất để bảo mật một máy chủ Linux, từ việc cấu hình hệ thống, quản lý người dùng và quyền truy cập, đến việc áp dụng các công cụ bảo mật nâng cao.

## II. Nội dung
### 1. Always keep Your Server Updated
Một trong những bước quan trọng nhất trong việc hardening một máy chủ Linux là đảm bảo rằng nó luôn được cập nhật. Bản chất của bảo mật hệ thống là một cuộc đua liên tục giữa những người phát triển phần mềm và các hacker tìm cách khai thác lỗ hổng. Do đó, việc giữ cho máy chủ luôn cập nhật không chỉ giúp cải thiện hiệu suất mà còn đóng vai trò quan trọng trong việc bảo vệ hệ thống khỏi các mối đe dọa bảo mật mới nhất.
```bash
sudo apt update && sudo apt upgrade -y   # Trên hệ thống Debian/Ubuntu
sudo yum update -y                      # Trên hệ thống CentOS/RHEL
```
### 2. Setting Up sudo Privileges Administrative Users
Trong quản trị hệ thống Linux, quyền **sudo** cho phép người dùng thực hiện các lệnh với quyền hạn của người dùng root mà không cần phải đăng nhập trực tiếp vào tài khoản root. Đây là một biện pháp bảo mật quan trọng, giúp hạn chế các rủi ro khi sử dụng tài khoản root một cách trực tiếp.

Ví dụ, server thêm một tài khoản cho nhà phát triển (`dev`), và cấp quyền sudo cho tài khoản đó:
#### 2.1. Thêm tài khoản người dùng vào nhóm người dùng có quyền quản trị
- Tạo tài khoản người dùng mới có tên là `dev`
```bash
sudo adduser dev
```
- Thiết lập mật khẩu cho tài khoản `dev`
```bash
sudo passwd dev
```
- Trên CentOS, nhóm `wheel` có quyền truy cập **sudo** theo mặc định. Để cấp quyền sudo cho tài khoản `dev`, bạn cần thêm tài khoản này vào nhóm `wheel`:
```bash 
sudo usermod -aG wheel dev
```
- Còn trên Ubuntu thì là nhóm `sudo`
```bash
sudo usermod -aG sudo dev
```
- Kiểm tra bằng cách đăng nhập vào tài khoản dev và chạy một lệnh yêu cầu quyền sudo.
```bash 
sudo whoami
# trả về root là tài khoản dev được cấp quyền sudo thành công.
```

#### 2.2. Chỉnh sửa tệp cấu hình `/etc/sudoers` bằng `visudo`
Tệp cấu hình `/etc/sudoers` là một trong những tệp quan trọng nhất trên hệ thống Linux để quản lý quyền **sudo**. Nó xác định ai có thể sử dụng lệnh sudo, khi nào họ có thể sử dụng, và giới hạn những lệnh nào họ có thể thực thi. Việc chỉnh sửa tệp này cần được thực hiện cẩn thận, và thường nên sử dụng lệnh **`visudo`** để đảm bảo rằng không có lỗi cú pháp trong quá trình chỉnh sửa, vì lỗi cú pháp có thể gây ra vấn đề nghiêm trọng cho hệ thống.

- Cú pháp cơ bản:
    ```bash
    user  host = (runas) command
    ```
    Trong đó:

    - **user**: Người dùng hoặc nhóm (sử dụng kí tự %groupname) được cấp quyền sudo.
    - **host**: Tên của máy chủ mà quyền này có hiệu lực. Thường là ALL để áp dụng cho tất cả các máy chủ.
    - **runas**: Người dùng mà các lệnh sẽ được thực thi dưới quyền của họ. Thường là ALL hoặc root.
    - **command**: Lệnh hoặc tập hợp lệnh mà người dùng có thể thực thi với quyền sudo. 

- Ví dụ
    ```
    root    ALL=(ALL)       ALL
    %wheel  ALL=(ALL)       ALL

    dev     ALL=(root)      /usr/bin/systemctl, /usr/bin/service
    %sys    ALL=(ALL)       NOPASSWD: NETWORKING, SOFTWARE, SERVICES, STORAGE, DELEGATING, PROCESSES, LOCATE, DRIVERS
    ```
    - `root` có quyền thực thi tất cả các lệnh ở tất cả các máy chủ.
    - Nhóm `wheel` có quyền thực thi tất cả các lệnh ở tất cả các máy chủ.
    - Người dùng `dev` chỉ có quyền thực thi các câu lệnh `systemctl` và `service` với quyền root trên tất cả máy chủ.
    - Nhóm `sys` có quyền thực thi tập hợp các lệnh của các **Cmnd_Alias** với quyền root mà không cần phải nhập mật khẩu, vì sử dụng **NOPASSWD** (không nên).


- Trong tệp /etc/sudoers, bạn có thể định danh người dùng hoặc nhóm người dùng bằng các từ khóa hoặc ký tự đặc biệt:
    - **User_Alias**: Được sử dụng để tạo các bí danh cho tập hợp người dùng.
    - **Runas_Alias**: Được sử dụng để tạo các bí danh cho tập hợp các tài khoản mà lệnh sẽ chạy dưới quyền của họ.
    - **Host_Alias**: Được sử dụng để tạo các bí danh cho các máy chủ.
    - **Cmnd_Alias**: Được sử dụng để tạo các bí danh cho tập hợp các lệnh.

- **`Defaults`** Thiết lập các tùy chọn mặc định cho việc sử dụng sudo. Các tùy chọn này có thể áp dụng cho toàn bộ người dùng hoặc chỉ một số người dùng cụ thể. Các tùy chọn nên dùng:
```bash
Defaults timestamp_timeout=5               # vô hiệu hóa việc yêu cầu nhập lại mật khẩu trong vòng 5 phút sau khi sử dụng sudo
Defaults logfile=/var/log/sudo.log         # yêu cầu tất cả các phiên sudo phải được ghi lại trong tệp nhật ký
```
Tìm hiểu thêm về các tùy chọn [tại đây](https://linux.die.net/man/5/sudoers)

### 3. Strong Password Policy
Tệp cấu hình **`/etc/security/pwquality.conf`** được sử dụng để thiết lập các chính sách về độ phức tạp của mật khẩu trên hệ thống Linux. Tệp này cho phép quản trị viên hệ thống kiểm soát các yêu cầu về mật khẩu, chẳng hạn như độ dài tối thiểu, mức độ phức tạp, và việc sử dụng các ký tự đặc biệt, để tăng cường bảo mật cho hệ thống.

- Cú pháp cơ bản rất đơn giản với mỗi dòng là 1 tham số và giá trị của chúng.
- Các tham số thường được cấu hình:
```bash
minlen = 12         #Độ dài tối thiểu của mật khẩu 
dcredit = -1        #Số lượng chữ số trong mật khẩu, Số nguyên âm để yêu cầu số lượng tối thiểu, hoặc số nguyên dương để hạn chế số lượng tối đa
ucredit = -1        #Số lượng chữ viết hoa trong mật khẩu, Số nguyên âm để yêu cầu số lượng tối thiểu, hoặc số nguyên dương để hạn chế số lượng tối đa
ocredit = -1        #Số lượng ký tự đặc biệt trong mật khẩu, Số nguyên âm để yêu cầu số lượng tối thiểu, hoặc số nguyên dương để hạn chế số lượng tối đa
difok = 6           #Số lượng ký tự khác nhau giữa mật khẩu mới và mật khẩu cũ
retry = 3           #Số lượng lần thử mật khẩu trước khi bị từ chối, mặc định là 3
enforce_for_root    #Áp dụng cho cả mật khẩu người dùng người dùng root
enforcing = 1       #Bắt buộc mật khẩu mới phải thỏa mãn các yêu cầu trên (1: bắt buộc, 0: có thể bỏ qua)
```
Còn 1 số cấu hình khác có thể tham khảo [tại đây](https://linux.die.net/man/5/pwquality.conf)

Tệp **`/etc/security/pwquality.conf`** thường được sử dụng cùng với mô-đun PAM **`pam_pwquality`**, được cấu hình trong các tệp như **`/etc/pam.d/common-password`** của Ubuntu hoặc **`/etc/pam.d/system-auth`** của CentOS.

Trong `/etc/pam.d/common-password` (trên Ubuntu) và `/etc/pam.d/system-auth` (trên CentOS) cần thêm dòng này để áp dụng cấu hình mật khẩu vừa thiết lập:
```
password    requisite                 pam_pwquality.so
```
Cũng nên cấu hình các chính sách về mật khẩu và đăng nhập tại **`/etc/login.defs`** :
```bash
PASS_MAX_DAYS	90      #số ngày tối đa mà một mật khẩu có thể tồn tại trước khi bắt buộc phải thay đổi
PASS_MIN_DAYS	 5      #số ngày tối thiểu mà người dùng phải đợi trước khi có thể thay đổi mật khẩu
PASS_WARN_AGE	 7      #số ngày trước khi mật khẩu hết hạn mà người dùng sẽ được cảnh báo
```

### 4.SSH Hardening Configuration
File `/etc/ssh/sshd_config` là tệp cấu hình cho máy chủ SSH (Secure Shell Daemon) trên các hệ thống Linux/Unix. Đây là tệp quan trọng trong việc xác định các thiết lập liên quan đến cách máy chủ SSH hoạt động, bao gồm các chính sách bảo mật, cổng kết nối, và các tùy chọn xác thực.

Các cấu hình nên sử dụng:
```
Protocol                2       # Chỉ cho phép giao thức SSH phiên bản 2
PermitRootLogin         no      # Vô hiệu hóa đăng nhập root trực tiếp, giảm nguy cơ tấn công brute-force vào tài khoản root.
PermitUserEnvironment   no      # Không cho phép người dùng cài đặt biến môi trường trong tệp .ssh/environment, giúp ngăn ngừa lỗ hổng bảo mật.
PasswordAuthentication  no      # Vô hiệu hóa xác thực bằng mật khẩu
PubkeyAuthentication    yes     # Xác thực đăng nhập bằng public key
X11Forwarding           no      # Tắt forwarding X11 nếu không cần thiết để giảm nguy cơ bị tấn công thông qua các phiên X11
Ciphers                 aes256-ctr,aes192-ctr,aes128-ctr       # Sử dụng các thuật toán mã hóa mạnh để bảo vệ dữ liệu trong quá trình truyền tải
ClientAliveInterval     600     #  ClientAliveInterval thiết lập thời gian sau mỗi 300 giây (5 phút) SSH daemon sẽ gửi một gói tin tới client để kiểm tra kết nối. 
ClientAliveCountMax     0       # ClientAliveCountMax 0 sẽ đóng kết nối nếu không nhận được phản hồi.
```

### 5. Permissions and verification
Thiết lập quyền của 1 số file thư mục quan trọng là một trong những nhiệm vụ quan trọng và cấp thiết nhất để đạt được mục tiêu bảo mật trên máy chủ Linux.
Ví dụ như `/etc/anacrontab`, `/etc/crontab`, và các tệp trong thư mục `/etc/cron.*`. 
- Các tệp như /etc/anacrontab, /etc/crontab, và các tệp trong thư mục /etc/cron.* chứa các lịch trình nhiệm vụ (cron jobs) được thực hiện bởi hệ thống. Các nhiệm vụ này có thể bao gồm việc tự động cập nhật, sao lưu, và các tác vụ bảo trì khác.
- Nếu quyền truy cập vào các tệp này không được hạn chế, kẻ tấn công hoặc người dùng trái phép có thể chỉnh sửa các tệp này để chạy mã độc hoặc thay đổi các nhiệm vụ quan trọng, từ đó chiếm quyền kiểm soát hệ thống hoặc gây hại cho máy chủ.
```bash
sudo chown root:root /etc/anacrontab
sudo chown root:root /etc/crontab
sudo chown -R root:root /etc/cron.*

sudo chmod 600 /etc/anacrontab
sudo chmod 600 /etc/crontab
sudo chmod -R 700 /etc/cron.*
```
Và nhiều các file cấu hình khác quan trọng cần thiết lập quyền sử dụng chặt chẽ.
### 6. Turn on SELinux
- SELinux (Security-Enhanced Linux) là một cơ chế kiểm soát truy cập bắt buộc (Mandatory Access Control - MAC) được tích hợp trong nhân (kernel) của hệ điều hành Linux. SELinux được phát triển bởi Cơ quan An ninh Quốc gia Hoa Kỳ (NSA) như một phần của sáng kiến bảo mật nhằm tăng cường khả năng bảo vệ hệ thống khỏi các cuộc tấn công và giảm thiểu hậu quả của việc khai thác lỗ hổng bảo mật.
- Bật SELinux bằng cách chỉnh sửa file cấu hình `/etc/selinux/config`:
```bash
SELINUX=enforcing
```
- Hoặc với lệnh:
```bash
sudo setenforce 1   # Đặt SELinux ở chế độ Enforcing
```

### 7. Securing Linux Systems with auditd
#### 7.1. Tổng quan về auditd
- **auditd** (Audit Daemon) là một thành phần của hệ thống kiểm toán (auditing) trên Linux, được thiết kế để ghi lại các sự kiện bảo mật quan trọng và theo dõi các hoạt động trên hệ thống. Nó là một công cụ quan trọng giúp quản trị viên hệ thống theo dõi, ghi nhận, và kiểm soát các hành vi của người dùng cũng như các tiến trình trên máy chủ.
- Chức năng của **auditd**:
    - **Ghi Nhận Sự Kiện Bảo Mật**:
    **auditd** ghi lại các sự kiện bảo mật liên quan đến hệ thống, bao gồm các hoạt động truy cập tệp tin, thay đổi quyền sở hữu, chạy các lệnh, và nhiều sự kiện khác mà bạn có thể cấu hình. Điều này giúp quản trị viên có thể theo dõi và phát hiện các hành vi bất thường hoặc trái phép trên hệ thống.
    - **Kiểm Soát Truy Cập**:
    Với **auditd**, bạn có thể theo dõi ai đã truy cập vào các tệp tin hoặc thư mục cụ thể, các thay đổi cấu hình hệ thống, hoặc các nỗ lực đăng nhập thất bại. Điều này giúp nâng cao khả năng kiểm soát và phát hiện các hoạt động đáng ngờ.
#### 7.2. Cách Cấu Hình và Sử Dụng auditd
- **Cài đặt**:
    ```bash
    sudo apt install auditd   # Trên Ubuntu/Debian
    sudo yum install audit    # Trên CentOS/RHEL
    ```
- **Cấu hình auditd**:
Các quy tắc kiểm toán được cấu hình trong file **`/etc/audit/audit.rules`**. Ví dụ, để theo dõi mọi lần truy cập vào tệp `/etc/passwd`, bạn có thể thêm dòng sau vào file này:
    ```
    -w /etc/passwd -p wa -k passwd_changes
    ```
    Giải thích:
    **"-w /etc/passwd"**: Thiết lập theo dõi cho tệp /etc/passwd.
    **"-p wa"**: Theo dõi các quyền write (ghi) và attribute (thay đổi thuộc tính).
    **"-k passwd_changes"**: Đặt một khóa (key) để dễ dàng tìm kiếm các sự kiện liên quan sau này.

    Có thể sử dụng các file auditd.rules đã được viết sẵn để cấu hình. Tham khảo [tại đây](https://github.com/bfuzzy/auditd-attack/blob/master/auditd-attack.rules)

- **Khởi động auditd**
    ```bash
    sudo systemctl start auditd
    sudo systemctl enable auditd
    ```
- **Xem Log Sự Kiện**:

    Các log sự kiện được lưu trữ trong file `/var/log/audit/audit.log`. Bạn có thể sử dụng lệnh **ausearch** để tìm kiếm các sự kiện cụ thể, hoặc **aureport** để tạo báo cáo về các hoạt động đã được ghi lại.
    Ví dụ, để tìm kiếm các sự kiện liên quan đến khóa **passwd_changes**, bạn có thể sử dụng:
    ```bash
    ausearch -k passwd_changes
    ```
### 8.Optimizing sysctl parameters
Tham khảo [**tại đây**](./roles/hardening_linux_asible_roles/defaults/main.yml)


## Một số file thường config 

### 1. `/etc/security/limits.conf`
Mỗi dòng trong tệp này có định dạng sau:
```
<domain>        <type>  <item>  <value>         
```
- **`<domain>`** : Tên người dùng hoặc nhóm người dùng mà giới hạn được áp dụng.
    - Bạn có thể sử dụng * để áp dụng cho tất cả người dùng.
    - Nếu muốn áp dụng cho nhóm, bạn sử dụng ký hiệu **@** trước tên nhóm
- **`<type>`** : Loại giới hạn, có thể là:
    - **soft**: Giới hạn "mềm", có thể được người dùng thay đổi lên tới giá trị "hard".
    - **hard**: Giới hạn "cứng", không thể vượt quá.
- **`<item>`**: Tên tài nguyên mà bạn muốn giới hạn. Một số giá trị phổ biến bao gồm:
    - **core** - giới hạn kích thước tệp core dump (tính bằng KB).
    - **data** - kích thước tối đa của vùng dữ liệu (data segment) (tính bằng KB).
    - **fsize** - kích thước tệp tối đa có thể tạo (tính bằng KB).
    - **memlock** - kích thước tối đa của vùng bộ nhớ bị khóa vào RAM (tính bằng KB).
    - **nofile** - số lượng tối đa của các file descriptors mà người dùng có thể mở.
    - **rss** - kích thước bộ nhớ cư trú tối đa (resident set size) (tính bằng KB).
    - **stack** - kích thước tối đa của ngăn xếp (stack) (tính bằng KB).
    - **cpu** - thời gian CPU tối đa mà một tiến trình có thể sử dụng (tính bằng phút).
    - **nproc** - số lượng tiến trình tối đa mà người dùng có thể tạo ra.
    - **as** - giới hạn không gian địa chỉ (address space) tối đa (tính bằng KB).
    - **maxlogins** - số lượng đăng nhập tối đa cho người dùng này.
    - **maxsyslogins** - số lượng đăng nhập tối đa trên toàn hệ thống.
    - **priority** - mức ưu tiên để chạy các tiến trình của người dùng.
    - **locks** - số lượng tối đa các khóa tệp mà người dùng có thể nắm giữ.
    - **sigpending** - số lượng tín hiệu đang chờ xử lý tối đa.
    - **msgqueue** - dung lượng bộ nhớ tối đa mà các hàng đợi thông điệp POSIX có thể sử dụng (tính bằng byte).
    - **nice** - mức ưu tiên nice tối đa mà người dùng có thể tăng lên, giá trị trong khoảng từ [-20, 19].
    - **rtprio** - mức ưu tiên realtime tối đa.
- **`<value>`** : Giá trị giới hạn cho tài nguyên.

Các config nên dùng của file `/etc/security/limits.conf`:
```bash
*   hard    maxlogins   5
*   hard    core        0
*   soft    nproc       2048
*   hard    nproc       4096        
```

Lần lượt các cấu hình trên để:
- Đặt giới hạn số phiên đăng nhập tối đa là 5 cho tất cả người dùng trên hệ thống. Điều này có nghĩa là mỗi người dùng sẽ không thể mở hơn 5 phiên đăng nhập (ví dụ: 5 kết nối SSH hoặc 5 phiên terminal) cùng một lúc. Điều này giúp quản lý tài nguyên hệ thống và ngăn chặn việc một người dùng tiêu thụ quá nhiều tài nguyên đăng nhập.
- Kích thước tệp core dump được tạo ra khi 1 chương trình bị lỗi bằng 0 KB, có nghĩa là không cho tạo tệp core dump vì tệp core dump có thể chứa 1 số thông tin nhạy cảm, dữ liệu bí mật. Có thể tiết kiệm tài nguyên.
- Đặt giới hạn mềm, một người dùng chỉ được phép khởi tạo tối đa 1024 tiến trình tại bất kỳ thời điểm nào nhưng vẫn có thể tăng giới hạn tiến trình nhưng không vượt quá giới hạn cứng.
- Đặt giới hạn cứng, một người dùng không thể khởi tạo quá 2048 tiến trình đồng thời.