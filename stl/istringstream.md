# istringstream

## Read string with a special delimiter

```cpp
istream& getline (istream& is, string& str, char delim);
```

By default `delim` is `\n`. See [more](http://www.cplusplus.com/reference/string/string/getline/).

Take [165. Compare Version Numbers (Medium)](https://leetcode.com/problems/compare-version-numbers/) for example, we can read the string segment using:

```cpp
getline(version1, segment, '.')
```

## Detect EOF

```cpp
iss.eof(); // returns true if it has reached EOF.
iss.good(); // returns true if none of the error state flags (eofbit, failbit and badbit) is set.
```