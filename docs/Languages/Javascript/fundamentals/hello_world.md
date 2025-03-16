---
sidebar_position: 1
---

# Hello World!

JavaScript programs can be inserted almost anywhere into an HTML document using the `<script>` tag.

```html {5-7}
<!DOCTYPE HTML>
<html>
<body>
  <p>Before the script...</p>
  <script>
    alert( 'Hello, world!' );
  </script>
  <p>...After the script.</p>
</body>
</html>
```

The `<script>` tag contains JavaScript code which is automatically executed when the browser processes the tag.

### HTML `<script>` defer Attribute

A script that will be downloaded in parallel to parsing the page, and executed after the page has finished parsing:

```html
<script src="demo_defer.js" defer></script>
```

:::info
The **defer** attribute is only for external scripts (should only be used if the src attribute is present).

There are several ways an external script can be executed:

If **async** is present: The script is downloaded in parallel to parsing the page, and executed as soon as it is available (before parsing completes)

If **defer** is present (and not **async**): The script is downloaded in parallel to parsing the page, and executed after the page has finished parsing

If neither **async** or **defer** is present: The script is downloaded and executed immediately, blocking parsing until the script is completed
:::

:::danger[If src is set, the script content is ignored.]
A single `<script>` tag can’t have both the src attribute and code inside.

This won’t work:
```html
<script src="file.js">
  alert(1); // the content is ignored, because src is set
</script>
```

We must choose either an external `<script src="…">` or a regular `<script>` with code.

The example above can be split into two scripts to work:

```html
<script src="file.js"></script>
<script>
  alert(1);
</script>
```
:::