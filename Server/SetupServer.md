# Setup server

## Cấu hình và phiên bản các thành phần trong hướng dẫn này:
- Windows server 2019 Standard version 1809
- IIS version 10.0.17763.1
- MS SQL server 2019
- SQL Server Management Studio (SSMS) 19.3
- NodeJS
- Redis

## Cài đặt:
1. Cài đặt SQL Server, SSMS, IIS
    - Lưu ý: Bật Fulltext khi cài SQL Server
2. Cài đặt **ASP.NET Core Runtime 7.0.15 Hosting Bundle** (nâng lên các phiên bản khác nếu nâng cấp core lên các version .NET mới)
    - [Download](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-aspnetcore-7.0.15-windows-hosting-bundle-installer)  
    - Setup
4. Cài đặt **ASP.NET Core 3.1 Hosting Bundle** cho IdentityServer
    - [Download](https://download.visualstudio.microsoft.com/download/pr/6744eb9d-dcd4-4386-9d87-b03b70fc58ce/818fadf3f3d919c17ba845b2195bfd9b/dotnet-hosting-3.1.32-win.exe)
    - Setup 
4. Cài đặt **IISNode** (hỗ trợ chạy FileServer, FileSocket, ImageResizer của nodejs trên IIS)
    - [Download](https://github.com/tjanczuk/iisnode/releases/tag/v0.2.21)
    - Setup
5. Cài đặt **Redis** (cache server)
    - Download:
      + [Redis](https://github.com/MicrosoftArchive/redis/releases)
      + [Another Redis Desktop Manager](https://github.com/qishibo/AnotherRedisDesktopManager/releases)
    - Setup:


## Thiết lập và cấu hình:
1. Cấu hình các client site trên IIS
    - Trang quản trị (BE):
      + App_Data/appsetting.json:

        ![image](https://github.com/tungvp29/Document/assets/37463451/4bebf7ed-7631-4bbc-a7b7-c1dc94783f7d)

    - Trang người dùng (FE)
      + App_Data/appsetting.json:
  
       
    - Các tham số:
        - _**Connection_String**_: Kết nối đến db Core
        - _**ConnectionString_News**_: Kết nối đến db Nghiệp vụ
        
        - _**Token**_: Token dùng để liên kết API lấy dữ liệu giữa các client site
        - _**fileView**_: Domain url của ImageResizer
        - _**fileCMS**_: Domain url của FileServer
        - _**fileSocket**_: Domain url của FileSocket
        - _**http**_: Domain url của Backend API
        
        - _**SSOUrl**_: Domain url của IdentityServer
        - _**SSOClientId**_: Mã client trong db của IdentityServer
        - _**SSOEnabled**_: Xác nhận có/không dùng SSO của IdentityServer
2. Cấu hình IdentityServer
    - Khởi tạo database
    - Tạo user _**IIS APPPOOL\<Application pool của IdentityServer trên IIS>**_ (VD: IIS APPPOOL\sso.dttt.vn)
    ![image](https://github.com/tungvp29/Document/assets/37463451/ff664bba-1f96-4fe1-8054-52416c857122)

    - Mapping user vừa tạo vào db _**IdentityServer**_
    ![image](https://github.com/tungvp29/Document/assets/37463451/5324a773-cb90-498a-8e7c-62b090fc09d5)

    - Tạo Client site bằng store procedure _**CreateClient**_
   
    ![image](https://github.com/tungvp29/Document/assets/37463451/3a657f07-b9ee-4676-9203-2bda92ce8942)

    - _**@Client**_: Mã client. Quy ước đặt mã như sau:
      + Tiền tố: **TNG**
      + Mã source: <Tên viết tắt> - <Số thứ tự>    (VD: FE-002)
      + Hậu tố: ONLINE: **ON** | LOCAL: **OF**
    - _**@Url**_: Domain của client        

    - Hoặc sử dụng giao diện sau khi đăng nhập tài khoản quản trị
4. Cấu hình FileServer
5. Cấu hình FileSocket
6. Cấu hình ImageResizer

## Lưu ý:
1. Lỗi **500.30 - ASP.NET Core app failed to start**
![image](https://github.com/tungvp29/Document/assets/37463451/4af848f7-b1b0-4dcf-9a91-c89eb00ab3ce)
    - Nguyên nhân: Xem EventViewer để xác định lỗi, nếu truy cập đến các file *.deps.json trong plugin bị từ chối thì xử lý như dưới
    - Xử lý:
        Xóa các file *.deps.json khi publish lên server và Recycle IIS Application Pool
