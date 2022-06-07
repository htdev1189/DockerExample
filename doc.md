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



