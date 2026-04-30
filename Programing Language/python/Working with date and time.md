Python hỗ tro
Datetime module 

module datetime cung cấp các utility class để represent time
datetime:
date: 
time:

cả 3 class datetime đều hổ trợ các toán tử so sánh   

combine date và time thanh datetime

Only _days_, _seconds_ and _microseconds_ are stored internally. Arguments are converted to those units:
- A millisecond is converted to 1000 microseconds.
- A minute is converted to 60 seconds.
- An hour is converted to 3600 seconds.
- A week is converted to 7 days.
    
and days, seconds and microseconds are then normalized so that the representation is unique, with
- `0 <= microseconds < 1000000`
- `0 <= seconds < 3600*24` (the number of seconds in one day)
- `-99999999 <= days <= 999999999`
timedelta được dùng để tính toán cộng vả trừ
timedelta chỉ hỗ trợ cho datetime class.

tìm khoảng cách giữa 2 khoảng thời gian.

tìm điểm thời gian sau khi công hay trừ cho 1 datetime object mới

string format datetime object

tạo 1 datetime object từ 1 time string cho trước.

ISO format 

- The _epoch_ is the point where the time starts, the return value of `time.gmtime(0)`. It is January 1, 1970, 00:00:00 (UTC) on all platforms.
- The term _seconds since the epoch_ refers to the total number of elapsed seconds since the epoch, typically excluding [leap seconds](https://en.wikipedia.org/wiki/Leap_second). Leap seconds are excluded from this total on all POSIX-compliant platforms.
- UTC is [Coordinated Universal Time](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) and superseded [Greenwich Mean Time](https://en.wikipedia.org/wiki/Greenwich_Mean_Time) or GMT as the basis of international timekeeping. The acronym UTC is not a mistake but conforms to an earlier, language-agnostic naming scheme for time standards such as UT0, UT1, and UT2.
`time` Module:

- **Focus:**
    
    Lower-level timekeeping, primarily dealing with Unix timestamps (seconds since the epoch).
    
- **Common Uses:**
    
    - Measuring execution time of code segments (e.g., using `time.time()` or `time.perf_counter()`).
    - Pausing program execution (e.g., using `time.sleep()`).
    - Getting the current time as a floating-point number representing seconds since the epoch. 
    
- **Precision:**
    
	    Can offer higher precision (e.g., nanoseconds) for timing measurements than `datetime`.