### Docker
Docker là gì?
Docker là một công cụ tạo môi trường được "đóng gói" (còn gọi là Container) trên máy tính mà không làm tác động tới môi trường hiện tại của máy.

Một số developer thường tạo sẵn các môi trường này, và upload [lên mạng](https://hub.docker.com) để mọi người lấy về dùng, và mấy cái này gọi là các Images.

####Docker bao gồm các thành phần chính:
`Docker Engine`: dùng để tạo ra Docker image và chạy Docker container.
`Docker Hub`: dịch vụ lưu trữ giúp chứa các Docker image.

#### Một số khái niệm khác:
`Docker Machine`: tạo ra các Docker engine trên máy chủ.
`Docker Compose`: chạy ứng dụng bằng cách định nghĩa cấu hình các Docker container thông qua tệp cấu hình
`Docker image`: một dạng tập hợp các tệp của ứng dụng, được tạo ra bởi Docker engine. Nội dung của các Docker image sẽ không bị thay đổi khi di chuyển. Docker image được dùng để chạy các Docker container.
`Docker Container`: một dạng runtime của các Docker image, dùng để làm môi trường chạy ứng dụng.
#### Docker mang lại những giá trị gì

Với Docker, chúng ta có thể đóng gói mọi ứng dụng vd như webapp, backend, MySQL, BigData…thành các containers và có thể chạy ở “hầu hết” các môi trường vd như Linux, Mac, Window…
Docker Containers có một API cho phép quản trị các container từ bên ngoài. Giúp cho chúng ta có thể dễ dàng quản lí, thay đổi, chỉnh sửa các container.
Hầu hết các ứng dụng Linux có thể chạy với Docker Containers.
Docker Containers có tốc độ chạy nhanh hơn hẳn các VMs truyền thống (theo kiểu Hypervisor). Điều này là một ưu điểm nổi bật nhất của Docker


<!-- Đối với jenkins thì mình dùng chủ yếu là docker compose, vì vậy docs này sẽ nói tập trung vào docker compose. -->

#### Sự khác biệt giữa Docker Image và Docker Container
`Docker Images`

Là một template chỉ cho phép đọc, ví dụ một image có thể chứa hệ điều hành Ubuntu và web app. Images được dùng để tạo Docker container. Docker cho phép chúng ta build và cập nhật các image có sẵn một cách cơ bản nhất, hoặc bạn có thể download Docker images của người khác.

`Docker Container`

Docker container có nét giống với các directory. Một Docker container giữ mọi thứ chúng ta cần để chạy một app. Mỗi container được tạo từ Docker image. Docker container có thể có các trạng thái run, started, stopped, moved và deleted.

#### Dockerfile

`Dockerfile` là một tập tin dạng text chứa một tập các câu lệnh để tạo mới một Image trong Docker.

Docker khi run nó sẽ chạy vào Dockerfile trong thư mục code của mình

Dockerfile có thể hình dung như một script dùng để build các image trong container.

Dockerfile bao gồm các câu lệnh liên tiếp nhau được thực hiện tự động trên một image gốc để tạo ra một image mới. Dockerfile giúp đơn giản hóa tiến trình từ lúc bắt đầu đến khi kết thúc.
Trong Dockerfile có các câu lệnh chính sau:

```sh
FROM
RUN
CMD
....còn nữa
```

### Dockerfile Systax

```sh
INSTRUCTION arguments
```

* Các INSTRUCTION là các chỉ thị, được docker quy định. Khi khai báo, các bạn phải viết chữ in hoa.
* Các arguments là đoạn nội dung mà chỉ thị sẽ làm gì.
* Ví dụ:

```sh
RUN echo 'we are running some # of cool things'
```
[Tham khảo cheat sheet](https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index)

### Dockerfile Commands

**[1. FROM](https://docs.docker.com/engine/reference/builder/#from)**
* Dùng để chỉ ra image được build từ đâu (từ image gốc nào)
* Thường được đặt ở đầu file

```sh
FROM ubuntu

hoặc có thể chỉ rõ tag của image gốc

FROM <image>:<tag>
FROM <image>@<digest>

FROM ubuntu14.04:lastest
```

* `FROM` có thể xuất hiện nhiều lần trong 1 file Dockerfile để tạo nhiều images

* `tag` or `digest` là các giá trị tùy chọn, nếu bỏ qua chúng thì nó là giá trị mới nhất. Và builder sẽ trả về lỗi nếu nó không match với giá trị `tag`

**[2. RUN](https://docs.docker.com/engine/reference/builder/#run)**
* Dùng để chạy một lệnh nào đó khi build image, ví dụ về một Dockerfile

Cấu trúc:
```sh
RUN <command> (shell form, the command is run in a shell, which by default is /bin/sh -c on Linux or cmd /S /C on Windows)
RUN ["<executable>", "<param1>", "<param2>"] (exec form)

```

Exmaple:

```sh
FROM ubuntu
RUN apt-get update
RUN apt-get install curl -y

RUN npm install -g yarn
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash
RUN npm install -g yarn
RUN yarn global add newman newman-reporter-html
```
```sh
RUN chmod +x ./setup.sh # run với chmod shell
```


* Có thể sử dụng shell command để kết hợp
* Không viết biến trong đây. example: `RUN ["echo", "$HOME"]` sẽ không thay thế cho biến $HOME.

**[3. CMD](https://docs.docker.com/engine/reference/builder/#cmd)**
* Lệnh CMD dùng để truyền một lệnh của Linux mỗi khi thực hiện khởi tạo một container từ image (image này được build từ Dockerfile)
* Có các cách (trong docs nói có 3 cách) sử dụng lệnh CMD

Cấu trúc:
```sh
CMD ["executable","param1","param2"] (exec form, this is the preferred form)
CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
CMD command param1 param2 (shell form)
```

ví dụ: 
```sh
#Cách 1
FROM ubuntu
RUN apt-get update
RUN apt-get install curl -y
CMD ["curl", "ipinfo.io"]
```
hoặc
```sh
#Cách 2
FROM ubuntu
RUN apt-get update
RUN apt-get install wget -y
CMD curl ifconfig.io

```

```sh
CMD ["sh", "./setup.sh"] # cmd with shell
```

* Chú ý rằng `CMD` chỉ được chạy duy nhất 1 lần, nếu có 2 `CMD` thì nó sẽ chỉ chạy cái `CMD` cuối cùng và end các command khác nên `CMD` thường được đặt ở cuối cùng.
* sự khác nhau giữa `RUN` và `CMD` [tham khảo đây](https://github.com/hocchudong/ghichep-docker/blob/master/docs/docker-tips/docker-dockerfile-run-with-cmd.md)

**[4. LABEL](https://docs.docker.com/engine/reference/builder/#label)**
Cấu trúc:
```sh
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```
* Chỉ thị LABEL dùng để add các metadata vào image.
* Ví dụ:

```sh
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL version="1.0"
LABEL description="This text illustrates \
that label-values can span multiple lines."
```
* Để xem các label của images, dùng lệnh docker inspect. 

```
"Labels": {
    "com.example.vendor": "ACME Incorporated"
    "com.example.label-with-value": "foo",
    "version": "1.0",
    "description": "This text illustrates that label-values can span multiple lines.",
    "multi.label1": "value1",
    "multi.label2": "value2",
    "other": "value3"
},
```
* Nếu Docker gặp nhãn / khóa đã tồn tại, giá trị mới sẽ ghi đè bất kỳ nhãn nào trước đó bằng các keys giống hệt nhau.

**[5. MAINTAINER (deprecated)](https://docs.docker.com/engine/reference/builder/#maintainer-deprecated)**
Cấu trúc:
```sh
MAINTAINER <name>
```
Dùng để đặt tên tác giả của images.

Hoặc bạn có thể sử dụng

```sh
LABEL maintainer="SvenDowideit@home.org.au"
```

**[6. EXPOSE](https://docs.docker.com/engine/reference/builder/#expose)**
Cấu trúc:
```sh
EXPOSE <port> [<port>/<protocol>...]
```
* Lệnh EXPOSE thông báo cho Docker rằng image sẽ lắng nghe trên các cổng được chỉ định khi chạy. Lưu ý là cái này chỉ để khai báo, chứ ko có chức năng nat port từ máy host vào container. Muốn nat port, thì phải sử dụng cờ -p (nat một vài port) hoặc -P (nat tất cả các port được khai báo trong EXPOSE) trong quá trình khởi tạo contrainer.

**[7. ENV](https://docs.docker.com/engine/reference/builder/#env)**
Cấu trúc:
```sh
ENV <key> <value>
ENV <key>=<value> ...
```
* Khai báo cáo biến giá trị môi trường. Khi run container từ image, các biến môi trường này vẫn có hiệu lực.

example:
```sh
ENV myName John Doe
ENV myDog Rex The Dog
ENV myCat fluffy
```
**[8. ADD](https://docs.docker.com/engine/reference/builder/#add)**
Cấu trúc:
```sh
ADD [--chown=<user>:<group>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"] (this form is required for paths containing whitespace)
```
* Chỉ thị ADD copy file, thư mục, remote files URL (src) và thêm chúng vào filesystem của image (dest)
* `src`: có thể khai báo nhiều file, thư mục, có thể sử dụng các ký hiệu như *,?,...
* Mỗi `src` có thể chứa các ký tự đặc biệt và đc match với rule của Go lang [filepath.Match ](https://golang.org/pkg/path/filepath/#Match)
* `dest` phải là đường dẫn tuyệt đối hoặc có quan hệ với chỉ thị `WORKDIR`

Các quy định:
* The path must be inside the context of the build: Có nghĩa là phải nằm trong thư mục đang build (chứa dockerfiles).
* If is a directory, the entire contents of the directory are copied, including filesystem metadata. The directory itself is not copied, just its contents.
* If multiple resources are specified, either directly or due to the use of a wildcard, then must be a directory, and it must end with a slash /.
* If does not end with a trailing slash, it will be considered a regular file and the contents of will be written at .
* If doesn’t exist, it is created along with all missing directories in its path.

Example:
```sh
ADD hom* /mydir/        # adds all files starting with "hom"
ADD hom?.txt /mydir/    # ? is replaced with any single character, e.g., "home.txt"
ADD test relativeDir/          # adds "test" to `WORKDIR`/relativeDir/
ADD test /absoluteDir/         # adds "test" to /absoluteDir/
ADD arr[[]0].txt /mydir/    # copy a file named "arr[0].txt" to /mydir/
```

**[9. ADD](https://docs.docker.com/engine/reference/builder/#copy)**
Cấu trúc:
```sh
COPY [--chown=<user>:<group>] <src>... <dest>
COPY [--chown=<user>:<group>] ["<src>",... "<dest>"] (this form is required for paths containing whitespace)
```

* Chỉ thị COPY, copy file, thư mục (src) và thêm chúng vào filesystem của container (dest).
* Các lưu ý tương tự chỉ thị ADD.

Example:
```sh
COPY hom* /mydir/        # adds all files starting with "hom"
COPY hom?.txt /mydir/    # ? is replaced with any single character, e.g., "home.txt"
COPY test relativeDir/   # adds "test" to `WORKDIR`/relativeDir/
COPY test /absoluteDir/  # adds "test" to /absoluteDir/
COPY arr[[]0].txt /mydir/    # copy a file named "arr[0].txt" to /mydir/
```
 
 Khác biệt giữ `Add` và `Copy` [tham khảo tại đây](https://github.com/hocchudong/ghichep-docker/blob/master/docs/docker-tips/docker-dockerfile-add-with-copy.md)

**[10. ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#entrypoint)**
Cấu trúc:
```sh
ENTRYPOINT ["executable", "param1", "param2"] (exec form, preferred)
ENTRYPOINT command param1 param2 (shell form)
```

* Hai cái `CMD` và `ENTRYPOINT` có tác dụng tương tự nhau. Nếu một Dockerfile có cả `CMD` và `ENTRYPOINT` thì `CMD` sẽ thành param cho script `ENTRYPOINT`. Lý do người ta dùng `ENTRYPOINT` nhằm chuẩn bị các điều kiện setup như tạo user, mkdir, change owner... cần thiết để chạy service trong container.

> Cái mà sẽ được chạy khi container running.

Example:

```sh
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
```

* [Understand how CMD and ENTRYPOINT interact
](https://docs.docker.com/engine/reference/builder/#understand-how-cmd-and-entrypoint-interact)

**[11. VOLUME](https://docs.docker.com/engine/reference/builder/#volume)**
Cấu trúc:
```sh
VOLUME ["/data"]
```
* Dùng để truy cập hoặc liên kết một thư mục nào đó trong Container.
* mount thư mục từ máy host và container. Tương tự `option -v` khi tạo container.
* Thư mục chưa volumes là `/var/lib/docker/volumes/`. Ứng với mỗi container sẽ có các thư mục con nằm trong thư mục này. 

> Nếu không dùng `volume` thì sau khi start docker sẽ lại như mới từ đầu. Điều này rất có ích trong việc test api,..

Example:
```sh
FROM ubuntu
RUN mkdir /myvol
RUN echo "hello world" > /myvol/greeting
VOLUME /myvol
```

**[12. USER](https://docs.docker.com/engine/reference/builder/#user)**
Cấu trúc:
```sh
USER <user>[:<group>] or
USER <UID>[:<GID>]
```
* Set username hoặc UID để chạy các lệnh RUN, CMD, ENTRYPOINT trong dockerfiles.

**[13. WORKDIR](https://docs.docker.com/engine/reference/builder/#workdir)**
Cấu trúc:
```sh
WORKDIR /path/to/workdir
```
* Chỉ thị `WORKDIR` dùng để đặt thư mục đang làm việc cho các chỉ thị khác như: `RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `ADD`,...
* Nó có thể dùng nhiều lần

Exmaple:
```sh
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```
Kết quả khi dùng pwd command trong Dockerfile sẽ là /a/b/c

**[14. ARG](https://docs.docker.com/engine/reference/builder/#arg)**
Cấu trúc:
```sh
ARG <name>[=<default value>]
```
* Chỉ thị ARG dùng để định nghĩa các giá trị của biến được dùng trong quá trình build image (lệnh docker build --build-arg =).
* biến ARG sẽ không bền vững như khi sử dụng ENV.

Example:
```sh
FROM busybox
ARG user1
ARG buildno
...
```

**[15. STOPSIGNAL](https://docs.docker.com/engine/reference/builder/#stopsignal)**
Cấu trúc:
```sh
STOPSIGNAL signal
```

* Gửi tín hiệu để container tắt đúng cách.

**[16. SHELL](https://docs.docker.com/engine/reference/builder/#shell)**
Cấu trúc:
```sh
SHELL ["executable", "parameters"]
```
* Chỉ thị Shell cho phép các shell form khác có thể ghi đè shell mặc định.
* Mặc định trên Linux là ["/bin/sh", "-c"] và Windows là ["cmd", "/S", "/C"]


Example:
```sh
FROM microsoft/windowsservercore

# Executed as cmd /S /C echo default
RUN echo default

# Executed as cmd /S /C powershell -command Write-Host default
RUN powershell -command Write-Host default

# Executed as powershell -command Write-Host hello
SHELL ["powershell", "-command"]
RUN Write-Host hello

# Executed as cmd /S /C echo hello
SHELL ["cmd", "/S"", "/C"]
RUN echo hello
```
**[17. ONBUILD](https://docs.docker.com/engine/reference/builder/#onbuild)**
Cấu trúc:
```sh
ONBUILD [INSTRUCTION]
```

* Chỉ thị ONBUILD được khai báo trong base image. Và khi child image build image từ base image thì lệnh ONBUILD mới được thực thi.
* Ví dụ + ref: http://container42.com/2014/02/06/docker-quicktip-3-onbuild/

**[18. HEALTHCHECK](https://docs.docker.com/engine/reference/builder/#healthcheck)**
Cấu trúc:
```sh
HEALTHCHECK [<options>] CMD <command> (check container health by running a command inside the container)
HEALTHCHECK NONE (disable any healthcheck inherited from the base image)
```

* Cho Docker biết cách kiểm tra một container để kiểm tra xem nó có còn hoạt động không
* Các `options` xuất hiện trước `CMD`:
```sh
--interval=<duration> (default: 30s)
--timeout=<duration> (default: 30s)
--retries=<number> (default: 3)
```


### Tóm tắt những command hay dùng:

**`FROM <base_image>:<phiên_bản>`**: đây là câu lệnh bắt buộc phải có trong bất kỳ Dockerfile nào. Nó dùng để khai báo base Image mà chúng ta sẽ build mới Image của chúng ta.
**`MAINTAINER <tên_tác_giả>`**: câu lệnh này dùng để khai báo trên tác giả tạo ra Image, chúng ta có thể khai báo nó hoặc không.
**`RUN <câu_lệnh>`**: chúng ta sử dụng lệnh này để chạy một command cho việc cài đặt các công cụ cần thiết cho Image của chúng ta.
**`CMD <câu_lệnh>`**: trong một Dockerfile thì chúng ta chỉ có duy nhất một câu lệnh CMD, câu lệnh này dùng để xác định quyền thực thi của các câu lệnh khi chúng ta tạo mới Image.
**`ADD <src> <dest>`**: câu lệnh này dùng để copy một tập tin local hoặc remote nào đó (khai báo bằng <src>) vào một vị trí nào đó trên Container (khai báo bằng dest).
**`ENV <tên_biến>`**: định nghĩa biến môi trường trong Container.
**`ENTRYPOINT <câu_lệnh>`**: định nghĩa những command mặc định, cái mà sẽ được chạy khi container running.
**`VOLUME <tên_thư_mục>`**: dùng để truy cập hoặc liên kết một thư mục nào đó trong Container.

### Với Docker file:
** Pull một image từ Docker Hub**
```sh
docker pull <image name>
```

**Tạo một container từ image có sẵn**
```sh
docker run -v <thư mục trên máy tính>:<thư mục trong container> -it <image name> /bin/bash
```


**Liệt kê các images hiện có:**
```sh
docker images
```


**Liệt kê các container đang chạy:**
```sh
docker ps
```


**Liệt kê toàn bộ các container đang chạy và đã tắt:**
```sh
docker ps -a
```


**Khởi động và truy cập lại vào một container đã tắt:**
```sh
docker start <ID hoặc NAME>
docker exec -it <ID hoặc NAME> /bin/bash
```

**Xoá một container:**
```sh
docker rm <ID hoặc NAME>
```

Nếu container đang chạy, bạn cũng có thể xoá nhưng phải thêm tham số -f vào sau rm để force remove:
```sh
docker rm -f <ID hoặc NAME>
```
**Xoá một image:**
```sh
docker rmi <ID hoặc NAME>
```

hoặc:

```sh
docker rmi -f <ID hoặc NAME>
```a

Tham khảo:
https://docs.docker.com/engine/reference/builder
https://kapeli.com/cheat_sheets/Dockerfile.docset/Contents/Resources/Documents/index
https://viblo.asia/s/2018-cung-nhau-hoc-docker-Wj53Omjb56m
https://github.com/hocchudong/ghichep-docker/
https://kipalog.com/posts/Toi-da-dung-Docker-nhu-the-nao
