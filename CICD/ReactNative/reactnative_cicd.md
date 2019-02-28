### Các công cụ cần tìm hiểu 
- AppCenter
- Fastlane
- Jenkin 

### 1: AppCenter
- Trong sự kiện Connect()- 2016, Microsoft giới thiệu Visual Studio Mobile Center, một giải pháp có thể nói là all-in-one trong phát triển ứng dụng di động 
- Định nghĩa all-in-one trong lập trình ứng dụng di động có nghĩa là một giải pháp giúp hỗ trợ đầy đủ hoặc gần như đầy đủ các công việc quan trọng trong việc phát triển ứng dụng di động, từ quá trình build cho tới quá trình test, phân phối (distribution) và theo dõi, phân tích hậu phân phối (analytics)
- Các tính năng của AppCenter thì đã rất rõ ràng, nhưng có một thực tế khi dùng AppCenter là nó build khá lâu, Team Frontend đã dùng AppCenter cho giai đoạn đầu của dự án ShouboTenken, thời gian build có khi lên đến 20-30 phút. Chính vì thế Team Frontend đã tìm một giải pháp build mới nhanh hơn đó chính là Fastlane, nhưng vẫn sử dụng AppCenter cho quá trình distribution ứng dụng 
### 2: Fastlane 
### 2.1: Định nghĩa 
    - Fastlane là một công cụ giúp cho việc release sản phẩm của chúng ta trở nên dễ dàng hơn, nhanh hơn. Nó xử lý tất cả các công việc dườm dà như tạo screenshot, xử lý code signing và release ứng dụng lên Store 
### 2.2: Ta có thể làm gì với Fastlane 
    - Xác định rõ việc deploy sản phẩm, bản beta hay testing 
    - Dễ dàng setup trong vài phút
    - Không cần nhớ các câu lệnh dài dòng, phức tạp chỉ cần nhớ fastlane là đủ 
    - Lưu trữ mọi thứ trên Git, dễ config 
    - Hỗ trợ cho cả Android, IOS, MacOS
    ...
### 2.3: Cài đặt và sử dụng 
    - Trước khi cài đặt fastlane, bạn phải đảm bảo rằng đã cài Xcode command line tools version mới nhất 
    ```sh
    xcode-select --install
    ```
    - Cài đặt fastlane 
    `Sử dụng Homebrew`
    ```sh
    brew cask install fastlane
    ```
    `Sử dụng RubyGems`
    ```sh
    sudo gem install fastlane -NV
    ```
      
