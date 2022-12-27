## Wolf Git flow

Flow tham khảo: [A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

### Giả định
* Đã tạo Central Repository (Nguồn trung tâm) trên Github（hoặc Bitbucket）.
* Branch mặc định của Central Repository là master.
* Lập trình viên có thể  fork (tạo nhánh) đối với Central Repository.
* Đã quyết định người review và người có quyền merge.

### Nguyên tắc
* Mỗi pull-request tương ứng với một ticket.
* Mỗi một pull-request không hạn chế số lượng commit
* Pull-request title phải đặt sao cho tương ứng với title của task với format `refs [Loại ticket] #[Số ticket] [Nội dung ticket]` （Ví dụ: `refs bug #1234 Sửa lỗi cache`）.
* Đối với commit title, trong trường hợp pull-request đó chỉ có 1 commit thì có thể đặt commit title tương tự như trên là `refs [Loại ticket] #[Số ticket] [Nội dung ticket]` （Ví dụ: `refs bug #1234 Sửa lỗi cache`）.\
  Tuy nhiên với trường hợp 1 ppull-request có chứa nhiêù commit thì cần phải ghi rõ trong nội dung commit title là trong commit đó xử lý đối ứng vấn đề gì.
    * Ví dụ:
        1. Pull-request title: `refs bug #1234 Sửa lỗi cache`
        2. Trong trường hợp pull-request có 2 commit thì nội dung commit title của 2 commit sẽ tương ứng như sau
            * `Tạo method thực hiện việc clear cache trong Model`
            * `Tại controller gọi method ở Model để thực hiện việc clear cache`
* Tại môi trường local(trên máy lập trình viên), tuyệt đối không được thay đổi code khi ở branch master.Nhất định phải thao tác trên branch khởi tạo để làm task.

### Chuẩn bị

1. Trên Github, fork Central Repository về tài khoản của mình（repository ở tài khoản của mình sẽ được gọi là Forked Repository）.

2. Clone (tạo bản sao) Forked Repository ở môi trường local.Tại thời điểm này, Forked Repository sẽ được tự động đăng ký dưới tên là `origin`.
    ```sh
    $ git clone [URL của Forked Repository]
    ```

3. Truy cập vào thư mục đã được tạo ra sau khi clone, đăng ký Central Repository dưới tên `upstream`.
    ```sh
    $ cd [thư mục được tạo ra]
    $ git remote add upstream [URL của Central Repository]
    ```

### Quy trình

Từ đây, Central Repository và Forked Repository sẽ được gọi lần lượt là `upstream` và `origin`.

1. Đồng bộ hóa branch master tại local với upstream.
    ```sh
    $ git checkout master
    $ git pull upstream master
    ```

2. Tạo branch để làm task từ branch master ở local. Tên branch là số ticket của task（Ví dụ: `task-1234`）.
    ```sh
    $ git checkout master # <--- Không cần thiết nếu đang ở trên branch master
    $ git checkout -b task-1234
    ```

3. Tiến hành làm task（Có thể commit bao nhiêu tùy ý）.

4. Push code lên origin.

    ```sh
    $ git push origin task-1234
    ```

5. Tại origin trên Github（Bitbucket）、từ branch `task-1234` đã được push lên hãy gửi pull-request đối với branch master của upstream.

6. Hãy gửi link URL của trang pull-request cho reviewer trên chatwork để tiến hành review code.

    6.1. Trong trường hợp reviewer có yêu cầu sửa chữa, hãy thực hiện các bước 3. 〜 5..
    6.2. Tiếp tục gửi lại URL cho reviewer trên chatwork để tiến hành việc review code.

7. Nếu trên 2 người reviewer đồng ý với pull-request, người reviewer cuối cùng sẽ thực hiện việc merge pull-request.
   Revewer xác nhận sự đồng ý bằng comment LGTM.
   
8. Quay trở lại 1.

#### Khi xảy ra conflict
