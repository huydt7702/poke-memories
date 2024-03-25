# Code Conventions

### Dự án em đang làm là một mini game lật thẻ bài, lật lần lượt 2 thẻ bài, nếu 2 thẻ bài khác nhau nó sẽ tự lật lại, nếu 2 thẻ giống nhau nó sẽ giữ nguyên, lật cho đến khi nào người chơi lật hết tất cả lá bài thì hết game.

### Những mục chưa đáp ứng được trong mã code hiện tại​:

1. Tên biến, tên hàm chưa thể hiện được đúng ý nghĩa của việc nó làm
2. Một hàm làm quá nhiều việc cùng một lúc
3. Sử dụng các magic number khiến người khác đọc code khó hiểu

### Những mục đã chỉnh sửa sau khi áp dụng convention​:

1. Đặt tên biến, tên hàm lại theo đúng Naming rules
2. Tách hàm, mỗi hàm chỉ thực hiện duy nhất 1 công việc
3. Không dùng các magic number
