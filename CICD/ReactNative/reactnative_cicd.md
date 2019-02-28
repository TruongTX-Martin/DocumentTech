### Các công cụ cần tìm hiểu 
- [AppCenter]('https://docs.microsoft.com/en-us/appcenter/')
- [Fastlane]('https://docs.fastlane.tools/')
- [Jenkin]('https://jenkins.io/doc/')

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


### 2.4: Sử dụng Fastlane
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
fastlane ios lane or fastlane android lane
```

### 2.5: Sử dụng Fastlane qua Project thực tế: ShouboTenken

### Fastfile
```
  # update_fastlane

default_platform(:ios)

AC_API_TOKEN = "725e83d23489a7707525794b8c1ac8a6f9e536a4"
AC_OWNER_NAME = "shoubo.mobile"
AC_APP_NAME_IOS = "ShouboTenken-ios"
AC_APP_NAME_ANDROID = "ShouboTenken-android"
AC_TEAM_INTERNAL = "Internal"
AC_TEAM_STAGING = "Staging"
AC_TEAM_CUSTOMER = "Customer"

platform :ios do
  desc "Description of what the lane does"
  lane :device do
    register_devices(
      devices_file: "fastlane/device.txt",
      team_id: "2PXJ6VZ7BK",
      username: "xuanbinh91@gmail.com"
    )
  end

  desc "Description of what the lane does"
  lane :build_dev do
    sh "cp", "-rf", "../.env_development", "../.env"
    increment_build_number(
      build_number: Time.now.getutc.to_i,
      xcodeproj: "ios/shoubotenken.xcodeproj"
    )
    gym(
      project: "ios/shoubotenken.xcodeproj",
      scheme: "shoubotenken",
      export_method: "ad-hoc",
      configuration: "Release",
      silent: true,
      suppress_xcode_output: true,
      clean: true,
      output_directory: "build",
      output_name: "ShouboTenken.ipa",
      export_options: {
        iCloudContainerEnvironment: "Development",
        provisioningProfiles: {
          "com.innovatube.shoubotenken": "match AdHoc com.innovatube.shoubotenken"
        }
      }
    )
    appcenter(
      api_token: AC_API_TOKEN,
      owner_name: AC_OWNER_NAME,
      app_name: AC_APP_NAME_IOS,
      testing_group: AC_TEAM_INTERNAL,
      release_notes: "[Develop] Commit #{last_git_commit()[:commit_hash]} of #{last_git_commit()[:author]}",
      file_path: "build/ShouboTenken.ipa"
    )
  end

  desc "Description of what the lane does"
  lane :build_staging do
    sh "cp", "-rf", "../.env_staging", "../.env"
    increment_build_number(
      build_number: Time.now.getutc.to_i,
      xcodeproj: "ios/shoubotenken.xcodeproj"
    )
    gym(
      project: "ios/shoubotenken.xcodeproj",
      scheme: "shoubotenken",
      export_method: "ad-hoc",
      configuration: "Release",
      silent: true,
      suppress_xcode_output: true,
      clean: true,
      output_directory: "build",
      output_name: "ShouboTenken.ipa",
      export_options: {
        iCloudContainerEnvironment: "Development",
        provisioningProfiles: {
          "com.innovatube.shoubotenken": "match AdHoc com.innovatube.shoubotenken"
        }
      }
    )
    appcenter(
      api_token: AC_API_TOKEN,
      owner_name: AC_OWNER_NAME,
      app_name: AC_APP_NAME_IOS,
      testing_group: AC_TEAM_STAGING,
      release_notes: "[Staging] Commit #{last_git_commit()[:commit_hash]} of #{last_git_commit()[:author]}",
      file_path: "build/ShouboTenken.ipa"
    )
  end

  desc "Description of what the lane does"
  lane :build_staging_customer do
    sh "cp", "-rf", "../.env_staging", "../.env"
    increment_build_number(
      build_number: Time.now.getutc.to_i,
      xcodeproj: "ios/shoubotenken.xcodeproj"
    )
    gym(
      project: "ios/shoubotenken.xcodeproj",
      scheme: "shoubotenken",
      export_method: "ad-hoc",
      configuration: "Release",
      silent: true,
      suppress_xcode_output: true,
      clean: true,
      output_directory: "build",
      output_name: "ShouboTenken.ipa",
      export_options: {
        iCloudContainerEnvironment: "Development",
        provisioningProfiles: {
          "com.innovatube.shoubotenken": "match AdHoc com.innovatube.shoubotenken"
        }
      }
    )
    appcenter(
      api_token: AC_API_TOKEN,
      owner_name: AC_OWNER_NAME,
      app_name: AC_APP_NAME_IOS,
      testing_group: AC_TEAM_CUSTOMER,
      release_notes: "[Staging] Commit #{last_git_commit()[:commit_hash]} of #{last_git_commit()[:author]}",
      file_path: "build/ShouboTenken.ipa"
    )
  end
end

platform :android do
  desc "Build development"
  lane :build_dev do
    sh "cp", "-rf", "../.env_development", "../.env"
    Dir.chdir("../android") do
      sh "./gradlew assembleRelease -PversionCode=#{Time.now.getutc.to_i}"
    end

    appcenter(
      api_token: AC_API_TOKEN,
      owner_name: AC_OWNER_NAME,
      app_name: AC_APP_NAME_ANDROID,
      testing_group: AC_TEAM_INTERNAL,
      release_notes: "[Develop] Commit #{last_git_commit()[:commit_hash]} of #{last_git_commit()[:author]}",
      file_path: "android/app/build/outputs/apk/release/app-release.apk"
    )
  end

  desc "Build staging"
  lane :build_staging do
    sh "cp", "-rf", "../.env_staging", "../.env"
    Dir.chdir("../android") do
      sh "./gradlew assembleRelease -PversionCode=#{Time.now.getutc.to_i}"
    end
    appcenter(
      api_token: AC_API_TOKEN,
      owner_name: AC_OWNER_NAME,
      app_name: AC_APP_NAME_ANDROID,
      testing_group: AC_TEAM_STAGING,
      release_notes: "[Staging] Commit #{last_git_commit()[:commit_hash]} of #{last_git_commit()[:author]}",
      file_path: "android/app/build/outputs/apk/release/app-release.apk"
    )
  end


  desc "Build staging"
  lane :build_staging_customer do
    sh "cp", "-rf", "../.env_staging", "../.env"
    Dir.chdir("../android") do
      sh "./gradlew assembleRelease -PversionCode=#{Time.now.getutc.to_i}"
    end
    appcenter(
      api_token: AC_API_TOKEN,
      owner_name: AC_OWNER_NAME,
      app_name: AC_APP_NAME_ANDROID,
      testing_group: AC_TEAM_CUSTOMER,
      release_notes: "[Staging] Commit #{last_git_commit()[:commit_hash]} of #{last_git_commit()[:author]}",
      file_path: "android/app/build/outputs/apk/release/app-release.apk"
    )
  end
end

```
* Trên đầu là thông tin về AppCenter, chúng ta dùng để distribute app lên AppCenter sau khi build xong
```
AC_API_TOKEN = "725e83d23489a7707525794b8c1ac8a6f9e536a4"
AC_OWNER_NAME = "shoubo.mobile"
AC_APP_NAME_IOS = "ShouboTenken-ios"
AC_APP_NAME_ANDROID = "ShouboTenken-android"
AC_TEAM_INTERNAL = "Internal"
AC_TEAM_STAGING = "Staging"
AC_TEAM_CUSTOMER = "Customer"
```
[How to get AC_API_TOKEN](https://docs.microsoft.com/en-us/appcenter/api-docs/)
* Tiếp đến trong phần build cho IOS, register devices. Với tài khoản developer cá nhân thì cần đăng ký uuid những device muốn cài đặt thì mới có thể chạy được app( Hoặc có thể dùng tài khoản [enterprice]('https://developer.apple.com/programs/enterprise/') )
Tạo 1 file device.txt và add uuid device muốn test vào
```
Device ID	Device Name
4bd1cb159a52581717839dd6fb7de8cf90a21739	Nexus iPhone 7
96ffde7fab5f456e65784e4a721c3e7165c7181a	Nexus iPhone 8
e6abe66fb94294f3aa414ab36b902329bda4132d	Nexus iPhone 5
99e1d4a637cde8fdb0abd41bbcbd702c3cc0f9e2	Nexus iPhone 6s plus
```
* Tiếp đến lane build_dev.
+ Đầu tiên copy file .env_development vào file .env (file .env là file chứa thông tin các biến môi trương như url, secret_key..):
``` 
sh "cp", "-rf", "../.env_staging", "../.env"
```
+ Tiếp tăng build_number:
``` 
increment_build_number(
      build_number: Time.now.getutc.to_i,
      xcodeproj: "ios/shoubotenken.xcodeproj"
)
```
+ Sau đó dùng gym build với các thông số đầu vào:
``` 
gym(
      project: "ios/shoubotenken.xcodeproj",
      scheme: "shoubotenken",
      export_method: "ad-hoc",
      configuration: "Release",
      silent: true,
      suppress_xcode_output: true,
      clean: true,
      output_directory: "build",
      output_name: "ShouboTenken.ipa",
      export_options: {
        iCloudContainerEnvironment: "Development",
        provisioningProfiles: {
          "com.innovatube.shoubotenken": "match AdHoc com.innovatube.shoubotenken"
        }
      }
    )
```
+ Tiếp đến là distribute file ipa lên AppCenter:
```
appcenter(
      api_token: AC_API_TOKEN,
      owner_name: AC_OWNER_NAME,
      app_name: AC_APP_NAME_IOS,
      testing_group: AC_TEAM_STAGING,
      release_notes: "[Staging] Commit #{last_git_commit()[:commit_hash]} of #{last_git_commit()[:author]}",
      file_path: "build/ShouboTenken.ipa"
    )
```
+ Về cơ bản lane của Android cũng tương tự bên IOS, chỉ là không cần đăng ký device.
+ Đầu tiên là build:
```
 Dir.chdir("../android") do
      sh "./gradlew assembleRelease -PversionCode=#{Time.now.getutc.to_i}"
    end 
```
+ Sau là đẩy lên AppCenter:
```
appcenter(
      api_token: AC_API_TOKEN,
      owner_name: AC_OWNER_NAME,
      app_name: AC_APP_NAME_ANDROID,
      testing_group: AC_TEAM_STAGING,
      release_notes: "[Staging] Commit #{last_git_commit()[:commit_hash]} of #{last_git_commit()[:author]}",
      file_path: "android/app/build/outputs/apk/release/app-release.apk"
    ) 
```
### 3: Jenkin
