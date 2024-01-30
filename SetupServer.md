# Setup server

## Cấu hình và phiên bản các thành phần trong hướng dẫn này:
- Windows server 2019 Standard version 1809
- IIS version 10.0.17763.1
- MS SQL server 2019
- SQL Server Management Studio (SSMS) 19.3

## Cài đặt:
- Cài đặt SQL Server, SSMS, IIS
- Cài đặt ASP.NET Core Runtime 7.0.15 Hosting Bundle (nâng lên các phiên bản khác nếu nâng cấp core lên các version .NET mới)
  1. Download: https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-aspnetcore-7.0.15-windows-hosting-bundle-installer
  2. Setup
- Cài đặt ASP.NET Core 3.1 Hosting Bundle cho IdentityServer
  1. Download: https://download.visualstudio.microsoft.com/download/pr/6744eb9d-dcd4-4386-9d87-b03b70fc58ce/818fadf3f3d919c17ba845b2195bfd9b/dotnet-hosting-3.1.32-win.exe
  2. Setup 
- Cài đặt IISNode (hỗ trợ chạy FileServer, FileSocket, ImageResizer của nodejs trên IIS)
  1. Download: https://github.com/tjanczuk/iisnode/releases/tag/v0.2.21
  2. Setup

## Thiết lập và cấu hình:
- Cấu hình các client site trên IIS
  1. Trang quản trị
  2. Trang public
- Cấu hình IdentityServer
- Cấu hình FileServer
- Cấu hình FileSocket
- Cấu hình ImageResizer

## Lưu ý:
Lỗi 500.30 - ASP.NET Core app failed to start
![image](https://github.com/tungvp29/Document/assets/37463451/4af848f7-b1b0-4dcf-9a91-c89eb00ab3ce)
