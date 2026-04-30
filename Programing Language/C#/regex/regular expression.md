# Regular Expressions in C#

Regular expressions (Regex) provide a powerful and flexible method for processing text. C# supports regular expressions through the `System.Text.RegularExpressions` namespace.

## Basic Usage
The `Regex` class is the primary entry point for regular expression operations.

```csharp
using System.Text.RegularExpressions;

string pattern = @"\d+"; // Matches one or more digits
string input = "The price is 100 dollars.";

bool isMatch = Regex.IsMatch(input, pattern);
Console.WriteLine($"Found digits: {isMatch}");

Match match = Regex.Match(input, pattern);
if (match.Success)
{
    Console.WriteLine($"Matched value: {match.Value}");
}
```

## Common Methods
- `Regex.IsMatch()`: Returns true if the pattern finds a match in the input string.
- `Regex.Match()`: Searches the input string for the first occurrence of the pattern.
- `Regex.Matches()`: Searches the input string for all occurrences of the pattern.
- `Regex.Replace()`: Replaces strings that match a pattern with a specified replacement string.
