# ZipZone
- **Description:** Challenge này có chức năng upload file .zip và giải nén và xem các files đã được giải nén.

- **Review source code:** File `app.py` có 2 routes là `/` và `/files/`:
    * Đối với route `/` thì chúng ta sẽ quan tâm với phương thức POST ![Route index](https://github.com/TAP1812/CyberSpaceCTF-2024-Write-up/blob/main/img/route_index_zipzone.png) 
    * Sau khi upload file .zip và giải nén thì các file sẽ nằm trong dir `/files/random_uid/`. Nếu ta truy cập vào dir kia thì browser sẽ tự tải các files đã giải nén về. Lý do trình duyệt tự tải các files về là vì server ko set header `Content-Type` cho response nên trình duyệt không biết phải xử lý các files đấy kiểu j => tự tải xuống. ![Route file](https://github.com/TAP1812/CyberSpaceCTF-2024-Write-up/blob/main/img/route_file_zipzone.png)  

- **Attack Flow:** Đối với những challenges upload files như này thì chúng ta thường có 2 hướng tấn công là:
    * Upload webshell để RCE server , nhưng mà điều này có vẻ không khả thi vì khi ta truy cập vào các files đó thì server sẽ không xử lý => hướng này ko giải được.
    * Ngoài ra khi nhắc đến chức năng upload file .zip thì chúng ta nhớ đến 1 case study nổi tiếng là `Symlink`. Trong wu này mình sẽ ko giải thích kỹ , đại khái `Symlink` (Symbolic Link)  là các file trỏ đến các file gốc được khởi tạo thông qua cmd: `ln -s /link/to/root/file tên_file_symlink`. Để kiểm chứng cho giả thuyết này thì mình là upload 1 file zip gồm 1 file `link_to_passwd` trỏ đến `/etc/passwd` <insert ảnh để kiểm chứng giả thuyết>
    => Điều đó chứng tỏ chúng ta có thể tận dụng giả thuyết này để đọc flag

- **Exploit:** Bước đầu tiên thì mình cần tìm địa chỉ chính xác của file `flag.txt`. Sau 1 hồi đọc các file `Docker-file`, `*.sh` thì mình đã tìm được địa chỉ chính xác là `/tmp/flag.txt`. Việc còn lại chỉ là zip file là upload. Sau đó thì cta sẽ có được flag:`CSCTF{..........}`

