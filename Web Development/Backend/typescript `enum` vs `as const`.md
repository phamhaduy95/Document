`enum` và `as const` đều là 2 phương pháp 

enum
ưu điểm dể refactor và readable hơn với các old developer hoặc các full stack developer từ các ngôn ngữ như Java hoặc C#

nhược điểm.
code output tạo ra một function object có kích thước lớn và không tree-shakable được.
không combine nhiều `enum` lại được

as const 
nhược điểm
`DX` kém hơn do intellisence không hỗ trơ autocomplete.
Code sẽ verbose hơn do cần 1 một câu lệnh extract type từ object

ưu điểm
code generate ra được sẽ được tối ưu hơn và tree-shakable được
	

