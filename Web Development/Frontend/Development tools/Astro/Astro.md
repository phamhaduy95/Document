Astro content collection
Island architect cho phép pre-render các static content như layout, text content trước. Javascript đi cùng với rừng part nhỏ. các file này chỉ được 
Context

Island architect 

Astro sử dụng cơ chế file Hakkabase routing như nextJS

Astro component
Astro borrow structure của 1 markdown file
trong phàn front matter ,Astro cho phép viết code trong phần này
ta có thể reference đến variable và function tại scope content

code section này được thực thi tại build time (build html) và chỉ chạy 1 lần 
client code chay trên browser được viết trong script tag 


Client component
Astro mặc định chỉ prebuild HTML cho component và không fetch javascript kem theo. 

Island architect

Each island is represented with one unique id. The Astro engine use its id to locate and fetch associated javascript code  
instead of fetching a big bundle of javascript code, it can load multiple smaller package in parallel
Certain UI sections don't need to be interactive and therefore no JS code. This helps reduce js size significantly by just including 


dynamic pre-fetch các javascript khi thỏa mãn điều kiện
client:*
with the client directive, Astro can 
To mitigate this issue, 


design system
-> the expected output:
How can I draw a 


Astro có support code block cho syntax highlight th 
share variable và data giữa các page thông qua context.locals object
Lưu ý: context.local là mutable object


Trong doc của Astro có phần custom routing theo i18n

create code example 
since Code block can not reformat the code string

SSR feature:
-  middleware : intercept response và request
```
.CodeBlock#intro {
  color: purple;
}

```
