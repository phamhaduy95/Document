The majority of inbuilt type in .NET are automatically assigned default
value when no specific value is given. To obtain default value, you can
use the stand-alone method default(Type).

Console.log(default(int)); // 0

You can obtain the default value of any type with default method.

Console.log(default(int));

Boolean type default value is false.

Console.WriteLine(default(bool)); // False

Most primitive numeric types have 0 as default value.

Console.WriteLine(default(sbyte)); // 0

Console.WriteLine(default(float)); // 0

Console.WriteLine(default(int)); // 0

Console.WriteLine(default(decimal)); // 0

String has default value is null which is not string type at all.

var str = default(string);

Console.WriteLine(a == \"\"); // False

Console.WriteLine(a == null); // True

Similar to string, reference type also has default value as null.

internal class Student

{

public string Id { get; set; }

public string Name { get; set; }

public uint Age { get; set; }

public string ClassName { get; set; }

public Student()

{

Id = Guid.NewGuid().ToString();

}

}

var student = (default(Student));

Console.WriteLine(student == null); // True

However, there are some exceptions, which are DateTime and Guid type.

Console.WriteLine(default(DateTime)); // 1/1/0001 12:00:00 AM

Console.WriteLine(default(Guid)); //
00000000-0000-0000-0000-000000000000.

For struct type, the default value depends on what type of each member
