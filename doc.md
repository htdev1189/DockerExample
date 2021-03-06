# Hướng dẫn Docker cơ bản

> # Images
> Chứa nhiều container

> Hiển thị toàn bộ images
- docker images 
> Tìm kiếm image
- docker search ten_images
> Tải images
- docker pull ten_image:version
> Xóa images
- docker image rm ten_image/ID
> Lưu container thành 1 image
- docker commit --output ten_file.tar id/ten-container
> Khôi phục 1 images từ gile tar
- docker load -i ten-file-tar 
> Thiết lập tên và version cho images vô danh
- docker tag id/ten-image tem-img/version

> # Container
> Phiên bản thực thi của images

> Khởi chạy container
- docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
- docker run -it ten_image/id_images (Khởi chạy container và đi vào nó luôn -t)
> Hiển thị container đang chạy
- docker ps
> Hiển thị tất cả container
- docker ps -a
> Khởi chạy 1 container đang stop
- docker container start id/name-container
> truy cập vào 1 container dang chạy
- docker attach id/name-container
> Muốn thoát khỏi 1 container mà không ép nó dừng lại bằng lệnh exit
- sủ dụng tổ hợp phím Ctrl P Q
> Dừng lại 1 container đang chạy
- docker stop id/name-container
> Đặt tên cho 1 container
- docker run -it --name "ten-container" -h "hostname" ten-image
> Xóa 1 container
- docker rm ten/id-container
> xóa 1 container đang chạy
- docker rm -f id/tem-container
> Thi hành 1 lệnh kho nằm ngoài 1 container đang chạy
- docker exec id/name-container lenh-can-chay
> Chay 1 container bằng lệnh exec .. ghi chú thêm bash .. mục đích là khi exit thì container này vẫn chạy
- docker exec -it id/ten-container bash
> Tạo ra một Images từ container
- docker commit id/ten-container ten-image-moi
> Khoi dong lai container
- docker restart containerName


# Chia sẻ dữ liệu từ máy Host tới container
Mục đích chính là khi container bị xóa , không bị mất data
> Thiếp lập container 
- docker run --rm -it --name "c1" -v dia-chi-thu-muc-tren-HOST:dia-chi-thu-muc-tren-container id/ten-image

# Chia sẻ dữ liệu giữa 2 container
> Thiết lập
- docker run -it --name ten-container --volumes-from ten-container-muon-dung-chung-data id/ten-image
> vi dụ
-  docker run -it --name c1 -v D:\DOCKER\DataShared\HostAndContainer:/home/data ubuntu
- docker run -it --name c2 --volumes-from c1 ubuntu

# Thiết lập volumes(ổ đĩa) để chia sẻ lưu trữ và chia sẻ data

> Danh sách ổ đĩa
- docker volume ls
> Thêm ổ đĩa
- docker volume create ten-volume
> Xóa ổ đĩa
- docker volume rm ten-volume
> Tạo container C1 liên kết với ổ đĩa Disk1 .. trỏ data vào thư mục /home/data trong C1
- docker run -it --name C1 --mount source=Disk1,target=/home/disk1 nameImage

# Tạo ổ đĩa ánh xạ từ 1 thư mục cụ thể trên máy HOST
- docker volume create --opt device=pathHost --opt type=none --opt o=bind diskName
- device -> Nơi lưu trữ trên máy HOST
- λ docker volume create --opt device=D:\DOCKER\DiskShare --opt type=none --opt o=bind disk_share_between_container
> Kiểm tra thông tin của ổ đĩa
- docker volume inspect volumeName
> kết nối với container qua option v
- docker run -it -v diskName:pathInContainer --name nameContainer nameImage
> Áp dụng nhỏ xíu trong cv hiện tại 
- là có thể mount 1 thư mục và tìm kiếm bằng terminal




# NETWORK
> liệt kê tất cả network
- docker network ls
> Thông tin network
- docker network inspect networkName
> Tạo một network mới
- docker network create --driver bridge networkName
> Gán network cho 1 container dang chạy
- docker network connect networkName containerName
> Nếu như 2 container cùng 1 network thì có thê ping qua tên
- ping containerName


# Một số thao táo quan trọng
> Tạo container ánh xạ cổng(port) theo máy HOST
- docker run -it --name tenContainer -p postHost:portContainer tenImage
- Trong đó portContainer hầu như ít được thay đổi, nó đi theo một số giá trị nhất định 
- ví dụ như httpd : 80 ...
### Ví dụ cụ thể
> To container tên là b2, dùng image busybox, ánh xạ qua cổng 8888 của máy host (httpd dung cong 80 tren b2)
- docker run -it --name b2 -p 8888:80 busybox
- tao file index.html trong thu muc /var/www/
- Ngoài máy Host chạy http://12.0.0.1:8888



# Bai Tap vi du

## Tạo container ten la c-php , sử dụng image php 7.3fpm, sử dụng thư mục myCode lam nơi chia sẻ data
- docker pull php:7.3-fpm
- docker run -d --name c-php -v myCode:/home/myCode php:7.3-fpm

## Tạo container tên là c-httpd, sử dụng images httpd mới nhất
- docker pull httpd
- copy file httpd.conf ra truoc (cd tới thư mục myCode trong máy host)
- > docker run --rm httpd cat /usr/local/apache2/conf/httpd.conf > my-httpd.conf
- mở file mới tải về được, active mod_proxy, mod_proxy_fcgi
> Tạo handle doc file php bang container c-php port 9000
- AddHandler "proxy:fcgi://c-php:9000" .php

> Bắt đầu việc tạo contaier c-httpd (-d -> chạy nền)
- docker run -d --name c-httpd --network www-net -p 9999:80 -p 443:443 -v D:\DOCKER\myCode:/home/myCode -v D:\DOCKER\myCode\my-httpd.conf:/usr/local/apache2/conf/httpd.conf httpd
> truy cập 127.0.0.1:9999 để hiển thị kết quả


## Tạo container c-mysql

> truyến biến lên bằng -e 
- docker run --rm -e var1="abc" blabla
> Khởi tạo images
- docker pull mysql
> Thông tin cần thiết trong mysql
- > file config : /etc/mysql/my.cnf
- > port : 3304
- > root : MYSQL_ROOT_PASSWORD
- > Đường dẫn lưu database /var/lib/mysql

> tải file my.cnf về máy host
- docker run --rm -v D:\DOCKER\myCode:/home/mycode mysql cat /etc/mysql/my.cnf > my-sql.cnf
> cho dòng này vào cuối file mới tải về 
- default-authentication-plugin=mysql_native_password
> tạo thư mục DB trong myCode để lưu trữ database
> Khoi tao container
- docker run -e MYSQL_ROOT_PASSWORD=abc123 -v D:\DOCKER\myCode\my-sql.cnf:/etc/mysql/my.cnf -v D:\DOCKER\myCode\DB:/var/lib/mysql --name c-mysql --network www-net mysql



# Một số query mysql cơ bản
> tạo user
- mysql> CREATE USER 'root'@'%' IDENTIFIED BY 'PASSWORD';
> thiết lập quyền hạn (có thể nhìn thấy tất cả các table)
- mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
- mysql> FLUSH PRIVILEGES;
> Thiet lap them extension cho docker
- docker-php-ext-install mysqli


