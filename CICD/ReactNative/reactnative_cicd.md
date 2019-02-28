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
* Match tạo tất cả các certificates và provisioning profile, lưu chúng trên mỗi git repository riêng biệt. Các thành viên của team có quyền truy cập Repo này có thể sử dụng chúng cho code signing.
* Cài đặt: 
```sh
brew cask install match
```
### [gym](https://github.com/fastlane/fastlane/tree/master/gym)
* Gym là môt công cụ để build and packages IOS app
* Cài đặt: 
```sh
brew cask install gym
```
### [pilot](https://github.com/fastlane/fastlane/tree/master/pilot)
* Tự động deploy app lên TestFlight và quản lý beta tester
* Nhưng trong document này, Team Frontend không sử dụng TestFlight mà distribute app thông qua  AppCenter()
* Cài đặt: 
```sh
brew cask install pilot
```
### 2.3.3: Cài đặt fastlane 
`Sử dụng Homebrew`
```sh
brew cask install fastlane
```
`Hoặc sử dụng RubyGems`
```sh
sudo gem install fastlane -NV
```


###2.4: Sử dụng Fastlane
* Đầu tiên tạo Folder fastlane/ trong project react-native ở root level, sau đó tạo 1 file gọi là Fastfile trong folder fastlane/
* Fastfile là nơi thực hiện các lanes. Mỗi lane là một tập hợp các action được thực thi đồng bộ. Mỗi action, là một tập hợp các function mà thực thi một task nào đó 
```
  before_all do
    ensure_git_branch
    ensure_git_status_clean
    git_pull
  end

  platform :ios do
    # iOS Lanes
  end

  platform :android do
    # Android Lanes
  end
```
* Chúng ta define 2 nền tảng Android và IOS, trong mỗi nền tảng chứa các lane đặc biệt cho mỗi context.
* Để thực thi các lane ta gọi các câu lênh 
```sh
fastlane ios lane
```
```sh
fastlane android lane
```
      
