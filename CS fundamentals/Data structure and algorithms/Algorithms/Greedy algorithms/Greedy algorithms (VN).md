#### Khi nào áp dụng được greedy algorithms
**greedy** **algorithm** và **dynamic programing** đều được dùng trong các bài toán tối ưu yêu cầu chúng ta thực hiện một loạt các step để đạt được kết quả mong muốn. Tuy nhiên khác với **dynamic programing,** khi ở mỗi step, ta đều phải tính toán và đánh giá lại các lựa chọn để tìm ra phương án tốt nhất, **greedy algorithm** lại tính toán độ hiệu quả của option ngay từ đầu cho phép ta chọn một option tốt nhất ngay lập tức.

Mỗi bước thi triển cho **greedy algorithm** cần đảm bảo nhưng tiêu chỉ sau đây:
- **feasible**:  các lựa chọn trong mỗi bước đi phải hợp lệ. 
- **local optimal:** lựa chọn mỗi bước là phải tốt nhất và tối ưu nhất. 
- **irrevocable**:  lựa chọn đ một khi đã  quay lại bước cũ lựa chọn option khác. Bài toán là back-tracking ko phải là greedy.

ví dụ về bài toán [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)
Đề bài yêu cầu tìm số lượng ít nhất interval cần loại bỏ để số lượng các interval không bị overlap là nhiều nhất. Với bài toàn này, hướng giải quyết tốt nhất là đi tìm gián tiếp các interval được giữ lại, từ đó ta rút kết quả của bài toán là số lượng các interval không dùng tới.  

Cách giải dùng greedy algorithms cho bài toán này sẽ được triển khai như sau:
1. Sắp xếp các interval theo thời gian kết thúc. 
2. Lấy một interval có thời gian kết thúc sớm nhất và không bị trùng với các interval đã chọn trước đó cho mỗi step.
#### Độ hiệu quả của greedy algorithms
Do mỗi một bước, ta chỉ chọn 1 kết quả tốt nhất trong danh sách có sẵn thay vì brute force như **dynamic programing** nên **greedy algorithm** thường tối ưu hơn về cả vùng nhớ lẫn thời gian. Tuy nhiên, không phải bài toàn náo áp dụng greedy cũng cho ra kết quả tối ưu. 




khó nhất tìm proof cho greedy cho ra kết quả tối ưu. 



scheduling task trải qua nhiều công đoạn.
throttling stage chỉ cho phép 1 task qua. 
parallel stage: nhiều task chạy cùng 1 lúc. cho task có duration nhất chạy trước

