Khi lên ý tưởng về thiết kế software architect. Ta cần trả lời được các câu hỏi sau
level of detail and abstraction? (high level vs low level design)
high level simple cho các non-tech stakeholder theo dõi được cấu trúc và cách các thành phần giao tiếp với nhau 
đới tượng đọc và tìm hiểu về architect ?
scope and audience

#### C4 model definition 

C4 tận dụng 2 notation chính
- box and line

purpose of software architect
diagram should be a map that help developer navigate a complex code base and logic.


C4 model là visualization framework giúp mô tả kiến trúc hệ thống (system architect) của một digital solution bao gồm (software, service và infrastructure).

Các tầng (level of detail of C4 model)
C4 model thường chia system design architect thành 4 tầng riêng biệt
context
container
component
code

User tương tác với hệ thống như thế nào:
Question: thường 1 nghiệp vụ sẽ bao gồm nhiều logic flow cho nhiều feature khác nhau. Ta có cần thiết kết riêng cho từng flow đó.



Tại tầng context and container, ta nên sử dụng các vocabulart

zoom in to explore the box in detailed.
as it should be

While C4 is primarily designed for static, structural modeling (Context, Containers, Components, Code), it includes mechanisms for modeling use cases as dynamic flows that move through that structure.

#### so sánh C4 model vs UML
Khi nào nên sài C4 thay cho UML.


Các thành phần có thể modeling trong software development
các thuật ngữ cần tìm hiểu
static model: The static model addresses the static structural view of a problem, which does not vary with time. A static model describes the static structure of the system being modeled, which is considered less likely to change than the functions of the system. In particular, a static model defines the classes in the system, the attributes of the classes, the relationships between classes, and the operations of each class. In this chapter, static modeling refers to the modeling process and the UML class diagram notation is used to depict the static model

Các vấn đề hay gặp phải với modelling và digram
**No single source of truth.** Which diagram is current? The one from last month's presentation? The whiteboard photo from the workshop?

**Inconsistent notation.** One person's "service" is another person's "component." Colors mean different things. Arrows are... decorative?

**Documentation rot.** Diagrams get created for a specific meeting, then never updated. Six months later, they're actively misleading.

**No traceability.** "What changed since last quarter?" Good luck diffing two PNGs.

- one shared representation of systems, services, and relationships
- multiple views for different audiences
- easier updates when the architecture changes
- the ability to keep docs close to engineering workflows
- diagrams that stay useful in reviews, onboarding, and design discussions

Các dạng flow, aspect có thể design
