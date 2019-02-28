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
### 2.3: Cài đặt
### 2.3.1: Update Xcode command line tool
 Trước khi cài đặt fastlane, bạn phải đảm bảo rằng đã cài Xcode command line tools version mới nhất 
```sh
xcode-select --install
```
### 2.3.2: Các tool đi kèm
Để sử dụng fastlane, chúng ta phải cài đặt thêm một số tool đi kèm như :
### [match](https://medium.com/@danielvivek2006/setup-fastlane-match-for-ios-6260758a9a4e)
* Match là một phương pháp mới cho IOS code siging, chia sẻ code siging cho các thành viên của team để đơn giản hóa việc cài đặt codesiging
* Match tạo tất cả các certificates và provisioning profile và lưu chúng trên mỗi git repository riếng biệt. Các thành viên của team có quyền truy cập Repo này có thể sử dụng chúng cho code signing.
### [gym](https://docs.docker.com/compose/reference/overview/)
### [pilot](https://docs.docker.com/compose/reference/overview/)
### [supply](https://docs.docker.com/compose/reference/overview/)
### 2.3.3: Cài đặt fastlane 
`Sử dụng Homebrew`
```sh
brew cask install fastlane
```
`Hoặc sử dụng RubyGems`
```sh
sudo gem install fastlane -NV
```
      
