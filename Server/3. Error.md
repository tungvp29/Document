# Lỗi thường gặp và cách xử lý
## Lỗi 500.30 - ASP.NET Core app failed to start

![image](https://github.com/tungvp29/Document/assets/37463451/4af848f7-b1b0-4dcf-9a91-c89eb00ab3ce)
Xem EventViewer để xác định lỗi, nếu truy cập đến các file *.deps.json trong plugin bị từ chối thì xử lý bằng cách xóa các file *.deps.json khi publish lên server và Recycle IIS Application Pool

![image](https://github.com/tungvp29/Document/assets/37463451/15d4526d-8f0a-4f13-af8a-2704d1acb9e7)
