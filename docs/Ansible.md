# Tổng quan về Ansible
### Ansible là gì?
**Ansible** là một công cụ tự động hóa mã nguồn mở được sử dụng để cấu hình hệ thống, triển khai ứng dụng, và quản lý các tác vụ IT khác. Ansible sử dụng ngôn ngữ YAML để định nghĩa các tác vụ cần thực hiện, giúp dễ dàng đọc và viết. Ansible hoạt động mà không cần agent, nghĩa là bạn không cần cài đặt phần mềm client trên các máy chủ đích, mà thay vào đó Ansible sẽ kết nối và thực thi các lệnh qua SSH.
![a](../images/ansible.jpg)

### Các khái niệm cơ bản trong Ansible
- **Controller Machine**: Là máy cài Ansible, chịu trách nhiệm quản lý, điều khiển và gởi task tới các máy con cần quản lý.

- **Inventory**: Là file chứa thông tin các server cần quản lý. File này thường nằm tại đường dẫn /etc/ansible/hosts, hoặc có thể tự định nghĩa.

- **Task**: Một block ghi tác vụ cần thực hiện trong playbook và các thông số liên quan. Ví dụ 1 playbook có thể chứa 2 task là: yum update và yum install vim.

- **Play**: là nhóm các task sẽ được thực hiện trên 1 nhóm các hosts.

- **Playbook**: Là file chứa các play của Ansible được ghi dưới định dạng YAML. Máy controller sẽ đọc các task trong Playbook và đẩy các lệnh thực thi tương ứng bằng Python xuống các máy con.

- **Module**: Ansible có rất nhiều module, ví dụ như moduel yum là module dùng để cài đặt các gói phần mềm qua yum. Ansible hiện có hơn ….2000 module để thực hiện nhiều tác vụ khác nhau, bạn cũng có thể tự viết thêm các module của mình nếu muốn.

- **Role**: Là một tập playbook được định nghĩa sẵn để thực thi 1 tác vụ nhất định (ví dụ cài đặt LAMP stack).

- **Handlers**: Handler có chức năng giống như 1 task nhưng chỉ xảy ra khi có điều kiện nào đó. Handler được run khi được notified bởi 1 task

### Cấu trúc Ansible
![a](../images/ansible5.jpg)
- **Người dùng (User)** là các quản trị viên hoặc nhà phát triển, sử dụng Ansible để thực hiện các tác vụ tự động hóa trên các máy chủ. Người dùng tương tác với Ansible thông qua các lệnh CLI hoặc các playbook.

- **Host Inventory** là danh sách các máy chủ mà Ansible sẽ quản lý. Inventory có thể ở dạng tĩnh (static inventory) hoặc động (dynamic inventory).
    - ***Static Inventory***: Là một tệp tin (thường là hosts hoặc inventory) chứa danh sách các máy chủ và nhóm máy chủ.
    - ***Dynamic Inventory***: Được tạo ra động từ các nguồn bên ngoài như các dịch vụ đám mây (AWS, Azure, GCP, OpenStack, v.v.).
- **Playbooks** là tập hợp các kịch bản được viết bằng YAML, chứa các plays để thực hiện các tác vụ trên các máy chủ. Mỗi playbook bao gồm một hoặc nhiều plays và mỗi play có thể chứa nhiều tasks.
- **Core Modules** là các module được tích hợp sẵn trong Ansible để thực hiện các tác vụ phổ biến như quản lý gói, dịch vụ, tệp, người dùng, v.v.
- **Custom Modules** là các module tùy chỉnh do người dùng tạo ra để thực hiện các tác vụ đặc thù không được hỗ trợ bởi các core modules.
- **Plugins** là các thành phần mở rộng chức năng của Ansible. Các loại plugins bao gồm:
    - Connection Plugins: Quản lý cách Ansible kết nối đến các máy chủ. Ví dụ: SSH cho máy chủ Linux, WinRM cho máy chủ Windows.
    - Email Plugins: Gửi email khi hoàn thành hoặc khi gặp lỗi.
    - Logging Plugins: Ghi lại log của các hoạt động Ansible.
    - Other Plugins: Bao gồm lookup, cache, callback, và nhiều loại khác.
- **Public/Private Cloud**: Ansible có thể quản lý cả hạ tầng public cloud (như AWS, Azure, GCP) và private cloud (như OpenStack) thông qua các plugin kết nối và inventory động.
- **Hosts** là các máy chủ đích mà Ansible quản lý. Mỗi host có thể là một máy chủ vật lý, máy ảo, hoặc container. Ansible thực hiện các tác vụ trên các hosts này thông qua các module và plugin kết nối.

**Tóm tắt luồng hoạt động:**
1. Người dùng tương tác với Ansible thông qua các lệnh hoặc playbook.
2. Ansible sử dụng Host Inventory để biết được các máy chủ cần quản lý.
3. Ansible sử dụng Playbooks để định nghĩa các tác vụ cần thực hiện trên các máy chủ.
4. Các Modules và Plugins được sử dụng để thực hiện các tác vụ cụ thể.
5. Ansible kết nối đến các Hosts thông qua các Connection Plugins.
6. Ansible có thể tương tác với cả Public và Private Cloud để quản lý hạ tầng đám mây.

# Cài đặt và triển khai Ansible lab

### Mô hình
![a](../images/ansible6.jpg)

### Cài đặt Ansible trên node Ansible Server
```bash
yum install -y epel-release 
yum update -y

yum install -y ansible
```

### Cấu hình SSH key cho các node hosts và khai báo file inventory 
```bash
# Tạo ssh keypair tại Ansible server
ssh-keygen

# Copy public key đến các node
ssh-copy-id root@192.168.133
ssh-copy-id root@192.168.135
```

Sau đó khai báo file inventory tĩnh tại file `/etc/ansible/hosts` mặc định của ansible. Hoặc có thể tạo file inventory khác ở đâu tùy thích nhưng khi chạy các lệnh cần thêm option để ansible đọc được file inventory. 

```
[local]
localhost ansible_host=192.168.75.131 ansible_port=22 ansible_user=root

[ubuntu]
client1 ansible_host=192.168.75.135 ansible_port=22 ansible_user=root
client2 ansible_host=192.168.75.133 ansible_port=22 ansible_user=root
```

Ở đây tôi có 2 group là local và ubuntu.
Trong 2 group có các tham số của host như ansible_host, ansible_port, ansible_user,...
- **client1, client2**: Tương ứng là các hostname của các node
- **ansible_host**: Địa chỉ IP của node client tương ứng
- **ansible_port**: Port của SSH phía client, nếu ta thay đổi thì sẽ chỉnh lại cho đúng.
- **ansible_user**: Là username của client mà AnsibleServer sẽ dùng để tương tác, trong bước trên tôi sử dụng là user root và thông qua SSH Key.

### Thực hiện các câu lệnh ad-hoc command 
Cú pháp chung của ad-hoc command trong ansible là
```bash
ansible [pattern] -m [module] -a "[module options]"

# Ví dụ kiểm tra kết nối đến tất cả các hosts 
ansible all -m ping
```

Ta được kết quả:
![a](../images/ansible1.jpg)

1 ví dụ khác sử dụng **ad-hoc command** để tại vim hàng loạt trên group ubuntu sử dụng module `apt` của ansible.
![a](../images/ansible2.jpg)

### Playbook
Sử dụng lệnh `ansible-playbook` để chạy playbook gồm nhiều plays và tasks.

Viết 1 playbook để tải và khởi chạy apache2 trên các hosts thuộc group "ubuntu"
```yml
---
  - name: Install-Start Apache2
    hosts: ubuntu
    become: yes
    tasks:
      - name: Install Apache2
        apt:
          name: apache2
          state: latest

      - name: copy to index.html
        copy:
          src: index.html
          dest: /var/www/html/index.html

      - name: Restart apache2
        service:
          name: apache2
          state: restarted
          enabled: true
```

1 Play gồm 3 thành phần chính là : name, hosts, tasks
- name : tên của play.
- hosts: group các hosts sẽ chạy các tasks.
- tasks: các tác vụ sử dụng các modules (core,custom) sẽ được chạy trên host.

Trong 1 playbook có thể có nhiều play.

Kết quả khi chạy playbook này:
![a](../images/ansible3.jpg)
Như trong output trên, ta thấy rằng 3 task có tên là **Install Apache2**, **copy to index.html** và **Restart apache2** mà ta đã định nghĩa trong playbook đã được thực hiện thành công. Nhưng trước khi thực hiện 3 task này, ansible đã thực hiện 1 task có tên là **Gathering Facts**. Khi Ansible bắt đầu thực thi 1 play, ansible sẽ thực hiện task có tên là **Gathering Facts** này trước tiên để thu thập thông tin về các server mà nó sẽ connect tới như: hệ điều hành, hostname, IP, địa chỉ MAC của tất cả các interfaces... 

Một điểm cần lưu ý nữa trong output trên đó là **changed=3** với client 1 và **changed=2** với client2, đây là tổng số lượng task trong play có tác động làm xảy ra thay đổi nào đó trên remote host, ví dụ như: các thay đổi về cài đặt hoặc xóa package, thêm sửa xóa file, hoặc đơn giản là thực hiện câu lệnh echo. Một số trường hợp task thực hiện xong không có cờ changed này do không làm thay đổi gì trên hệ thống của remote host như ping hoặc như phía trên vì tôi đã copy trước file index.html sang bên client2 trước đó bằng 1 playbook khác.

### Handlers
Handler có chức năng giống như 1 task nhưng chỉ xảy ra khi có điều kiện nào đó. Handler được run khi được notified bởi 1 task, notify của 1 task được khai báo như sau:
```yml
---
- hosts: local
  tasks:
   - name: Install Nginx
     yum:
      name: nginx
      state: latest
     notify:
      - Start Nginx

  handlers:
   - name: Start Nginx
     service:
       name: nginx
       state: started 
```

![a](../images/ansible4.jpg)

### Variables
Variables trong Ansible là các giá trị động có thể thay đổi trong quá trình thực thi playbook. Có nhiều nơi mà bạn có thể định nghĩa và sử dụng biến:

**1. Playbook Variables**: Được định nghĩa trong playbook và chỉ có hiệu lực trong playbook đó
```yml
- hosts: ubuntu
  vars:
    docker_user: root
    docker_pkgs:
      - docker.io
      - docker-compose
```

**2. Inventory Variables**: Được định nghĩa trong file inventory và có thể áp dụng cho host hoặc nhóm host.
```yml 
[ubuntu]
client1 ansible_hosts=192.168.75.133 ansible_user=root #Biến cho tùng host
client1 ansible_hosts=192.168.75.136 ansible_user=root

[ubuntu:vars]
ansible_password=abc #Biến cho group các hosts.
```

**3. Role Variables**: Được định nghĩa trong các file `vars` của roles.

**4. Sử dụng biến**: Lấy giá trị của biến trong playbook ta dùng:**{{ <var_name> }}**

### Roles

Roles trong Ansible là một cách tổ chức các playbooks và các task liên quan một cách có cấu trúc và có tổ chức hơn. Một role bao gồm các thành phần chính sau:

- **Tasks**: Đây là nơi chứa các task chính mà role sẽ thực hiện. File này thường được đặt tại roles/<role_name>/tasks/main.yml.

- **Vars**: Chứa các biến sử dụng trong role, thường nằm tại `roles/<role_name>/vars/main.yml`.

- **Handlers**: Đây là các task được gọi bởi các task khác khi có sự thay đổi trạng thái. File này thường nằm tại `roles/<role_name>/handlers/main.yml`.

- **Templates**: Chứa các file template (thường là các file cấu hình) sử dụng Jinja2 template engine. Các file template thường nằm tại `roles/<role_name>/templates/`.

- **Files**: Chứa các file tĩnh, ví dụ như các script hoặc các file cấu hình cố định. Các file này thường nằm tại `roles/<role_name>/files/`.

- **Meta**: Chứa các thông tin meta về role như dependencies. Thông tin này nằm tại `roles/<role_name>/meta/main.yml`.

#### Install docker, docker-compose và chạy hello-world với docker bằng ansible roles trên các hosts.

Cấu trúc cây thư mục của project:
```bash
.
├── hosts
├── install_docker.yml
└── roles
    └── docker
        ├── tasks
        │   └── main.yml
        └── vars
            └── main.yml
```

Nội dung file cấu hình `/etc/ansible/ansible.cfg`
```bash
[default]
inventory      = /root/ansible_porject/hosts
roles_path     = /root/ansible_project/roles
```

Nội dung file hosts
```yml
[local]
1localhost ansible_host=92.168.75.131

[ubuntu]
client1 ansible_host=192.168.75.133
client2 ansible_host=192.168.75.136

[all:var]
ansible_user: root
```

Các **vars** được sử dụng trong roles docker được lưu trong `roles/docker/vars/main.yml`
```yml
---
docker-pkgs:    # Biến lưu tên các package sẽ được cài, biến dạng list
  - docker.io
  - docker-compose
```

Các **tasks** cần làm để cài đặt và chạy thử 1 container bằng docker được lưu trong `roles/docker/tasks/main.yml
 
```yml
---
  - name: Update Apt package
    apt:
      update_cache: yes
      cache_valid_time: 86400

  - name: Install docker
    apt:
      name: '{{ item }}'
      state: latest
    loop: '{{ docker-pkgs }}'           #Biến docker-pkgs được định nghĩa trong file vars/main.yml

  - name: start docker.service
    service:
      name: docker
      state: started
      enabled: yes

  - name: Test docker run hello-world
    command: docker run hello-world
    register: hello_docker              # Lưu trữ đầu ra của lệnh command vào biến hello_docker

  - debug:
      msg: '{{ hello_docker.stdout }}'  # In nội dung của biến hello_docker
```

Playbook sử dụng roles để cài đặt vào chạy thử docker được lưu trong file `install_docker.yml`
```yml
---
- name: install & configure docker & docker-compose on Ubuntu
  hosts: ubuntu
  become: true
  vars:
    docker_user: root
  roles:
    - docker
```

Chạy lệnh `ansible-playbook install_docker.yml` ta được kết quả:
![a](../images/ansible7.jpg)

**Docker, docker-compose** đã được cài đặt tại 2 máy hosts và chạy thử lệnh `docker run hello-world` đã được debug và trả về kết quả như hình trên.