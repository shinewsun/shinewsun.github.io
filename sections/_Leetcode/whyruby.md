---
layout: leetcode
title: "Why Ruby"
---

In general code interviews, you'll likely be able to write in any language you prefer. Therefore, it's a good idea to pick a language that performs mundane tasks for you.

For example, you don't want to be spending lots of time thinking about how you're going to parse input. You should be able to rely on your language to do the hard work. Say we want to use [regex](https://regexone.com/) to see if `input` matches `/r.g+ex/`, ignoring case. In a few common languages:

- C++:
```cpp
#include <regex>
std::regex my_regex ("r.g+ex", std::regex_constants::icase);
return std::regex_match(input, my_regex);
```

- C#:
```csharp
using System.Text.RegularExpressions;
Regex my_regex = new Regex("r.g+ex", RegexOptions.IgnoreCase);
return my_regex.IsMatch(input);
```

- Java:
```java
import java.util.regex.Pattern;
Pattern my_regex = Pattern.compile("r.g+ex", Pattern.CASE_INSENSITIVE);
return my_regex.matcher(input).find();
```

- JavaScript:
```js
let my_regex = /r.g+ex/i;
return my_regex.test(input);
```

- Python:
```python
import re
my_regex = r'(?i)r.g+ex'
return bool(re.search(my_regex, input))
```

- Ruby:
```ruby
my_regex = /r.g+ex/i
return my_regex.match?(input)
```

It is immediately obvious that the lower three languages will be faster to type than the upper three. Additionally, in the upper three, it would be bad style to use a oneliner, but in the lower three, that would be good style.

But also consider how hard it's going to be to memorize and recall the functions and constants required. For the upper three, you'll have to memorize the variable names for ignoring case, whereas for the bottom three you memorize the standard flag characters.

Regex is just one of many places where using one of the bottom three languages will save time and brainpower.

In fact, any of the three (Javascript, Python, Ruby) will make for a good interview language. I started learning Ruby primarily because it's extremely quick to type, often quicker than Javascript and Python.

At the end of the day, if you truly are only familiar with one language at the time you go into an interview, use that language. It's not the end of the world if all you know is Java. But be aware of the alternatives.
