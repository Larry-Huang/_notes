---
layout: default
title: wildcard
parent: linux
nav_order: 4
---

### WildCard
- matches zero or more characters.
```
*.txt
a*
a*.txt
```
- ? - matches exactly one character.
```
?.txt
a?
a?.txt
```

- [] - A character class.
```
Matches any of the characters included between the
brackets. Matches exactly one character.
[aeiou]
ca[nt]*
can
cat
candy
catch
```

- [!] - Matches any of the characters NOT
included between the brackets. Matches
exactly one character.
```
[!aeiou]*
baseball
cricket
```
- More Wildcards - Ranges
Use two characters separated by a hyphen to
create a range in a character class
```
[a-g]*
Matches all files that start with a, b, c, d, e, f, or g.
[3-6]*
Matches all files that start with 3, 4, 5 or 6.
```