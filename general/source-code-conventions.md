# Source code conventions
Adhere to these general source code conventions unless otherwise specified in language specific guide.

## Files
- Use language specific indentation.
- If you have to use tabs, do not mix tabs and spaces in a single project.
- All files end with an empty line.
- Use lint for specific language if available.
- Limit line length to 80 characters
- Strive for readable code. Imagine writing code for your past self when you were in 1st year college trying to read code for the first time.
- Use descriptive variable names
- Prefer short well named methods with few parameters. If a function doesn't fit in its entirety on the screen in a normal IDE at a normal font size then you need a pretty darn good reason as to why not. Decompose as necessary.
- Avoid obvious comments e.g.

```php
// get the country code
$country_code = get_country_code($_SERVER['REMOTE_ADDR']);

// if country code is US
if ($country_code == 'US') {

  // display the form input for state
  echo form_input_state();
}
```

- Group code into logical blocks e.g.

```go
func main() {

    // To start, here's how to dump a string (or just
    // bytes) into a file.
    d1 := []byte("hello\ngo\n")
    err := ioutil.WriteFile("/tmp/dat1", d1, 0644)
    check(err)

    // For more granular writes, open a file for writing.
    f, err := os.Create("/tmp/dat2")
    check(err)

    // It's idiomatic to defer a `Close` immediately
    // after opening a file.
    defer f.Close()

    // You can `Write` byte slices as you'd expect.
    d2 := []byte{115, 111, 109, 101, 10}
    n2, err := f.Write(d2)
    check(err)
    fmt.Printf("wrote %d bytes\n", n2)

    // A `WriteString` is also available.
    n3, err := f.WriteString("writes\n")
    fmt.Printf("wrote %d bytes\n", n3)

    // Issue a `Sync` to flush writes to stable storage.
    f.Sync()

    // `bufio` provides buffered writers in addition
    // to the buffered readers we saw earlier.
    w := bufio.NewWriter(f)
    n4, err := w.WriteString("buffered\n")
    fmt.Printf("wrote %d bytes\n", n4)

    // Use `Flush` to ensure all buffered operations have
    // been applied to the underlying writer.
    w.Flush()
}
```

- Try to follow the DRY principle. **Never Cut and Paste Within a Project**

>"Every piece of knowledge must have a single, unambiguous, authoritative representation within a system."

---
### Recommended reading
See [Code Smells](https://blog.codinghorror.com/code-smells/)

---
![wtfpm](../assets/wtfm.jpg)
