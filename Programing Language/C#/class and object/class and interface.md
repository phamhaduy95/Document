**Declare constant field in class.**

You can make any field of the class to be constant by applying
***readonly*** keyword modifier to them. readonly field will become
unmodifiable after object initiation is finished. The readonly reference
type own fields are mutable since only the reference value is made
constant so it only ensures it don't refer to another object

**override method from superclass**

Sometimes you want to re-implement the method from the superclass, C#
provides us the override mechanic to help us to do so. To make any
method overridable, explicitly assign ***virtual*** keyword modifier to
it in declaration in superclass. Then apply ***override*** keyword when
you redeclare it in the subclass.

**Access modifier**

access modifiers allow us to specify the accessibility level for members
of the class. There are 4 major and most-used ones:

***private*** members are only accessible within the scope of the class
declaration. (not applicable for subclass). ***private*** is the default
access modifier for all members of the class which means private will be
implicitly given if no other access modifier is provided.

***public*** make class member accessible anywhere outside the scope of
the class. It is preferable to leave only a handful of methods which is
public interface of the class be ***public*** and make all fields to be
***private***.

***protected***, just like ***private***, will limit the accessibility
of class members to the scope of the class only but extend to subclasses
as well.

***internal*** g
