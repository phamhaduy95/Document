caching là kỹ thuật quan trọng giúp:
- giảm thiểu latency fetching data từ client
- giảm thiểu network traffic ở server và database.

HTTP cho phép ta control caching behavior của browser thông qua `Cache-control` header. 

Browser lưu cache data tại disc.

```
Cache-Control: no-store, no-cache, max-age=0, must-revalidate, proxy-revalidate
```

Các directive  cho `cache-control` header:
- `max-age`: quy định thời gian cache data được cho là fresh và có thể tái sử dụng.
- `no-cache`: cần check lại với server có nên reuse cached data không hay refresh data mới. if the client already has fresh data, server should response with `304 Not modified header`.
- `must-revalidate`:  nếu cache data vẫn cho là fresh, browser sẽ reuse. Trong trường hợp cached data stale sẽ cần recheck với server. 
- `no-store`: không lưu cache tại bất cứ đâu bao gồm cả private và shared cached.

Hầu hết giữa client và server sẽ tồn tại nhưng lớp trung gian như proxy và `CDN` (hay còn gọi là shared cache). Tại các lớp này đều có hỗ trợ caching response. Tuy nhiên ta cần tránh lưu dữ liệu nhạy cảm tại các lớp này bằng việc thêm header `cache-control: private`. 

condition request:
- **`If-None-Match`:** Contains the `ETag` value (e.g., `If-None-Match: "deadbeef"`).
- **`If-Modified-Since`:** Contains the `Last-Modified` timestamp (e.g., `If-Modified-Since: Tue, 22 Feb 2022 22:00:00 GMT`).

**cache-busting** pattern: với mỗi một version ta tạo 1 suffix riêng biệt để browser refetch lại asset version mới.

browser cache

force reload 
```js
// Note: "reload" — rather than "no-cache" — is the right mode for a "force reload"
fetch("/", { cache: "reload" });
```

Modern browsers automatically leverage conditional requests for resources they’ve previously cached. When revisiting a page, browsers include If-Modified-Since or If-None-Match headers based on previously received Last-Modified or ETag values.

