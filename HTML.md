# HTML

## Plain HTML autocomplete
```
<input list="test" placeholder="Choose from the list..."/>
<datalist id="test">
  <option value="option 1">
  <option value="option 2">
  <option value="option 3">
  <option value="option 4">
  <option value="option 5">
</datalist>
```

[src](https://mobile.twitter.com/IMAC2/status/1383384601192656897)

## Best HTML boilerplate
```
<!DOCTYPE html>
<html lang="en" class="no-js">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width">

  <title>Unique page title - My Site</title>

  <script type="module">
    document.documentElement.classList.remove('no-js');
    document.documentElement.classList.add('js');
  </script>

  <link rel="stylesheet" href="/assets/css/styles.css">
  <link rel="stylesheet" href="/assets/css/print.css" media="print">

  <meta name="description" content="Page description">
  <meta property="og:title" content="Unique page title - My Site">
  <meta property="og:description" content="Page description">
  <meta property="og:image" content="https://www.mywebsite.com/image.jpg">
  <meta property="og:image:alt" content="Image description">
  <meta property="og:locale" content="en_GB">
  <meta property="og:type" content="website">
  <meta name="twitter:card" content="summary_large_image">
  <meta property="og:url" content="https://www.mywebsite.com/page">
  <link rel="canonical" href="https://www.mywebsite.com/page">

  <link rel="icon" href="/favicon.ico">
  <link rel="icon" href="/favicon.svg" type="image/svg+xml">
  <link rel="apple-touch-icon" href="/apple-touch-icon.png">
  <link rel="manifest" href="/my.webmanifest">
  <meta name="theme-color" content="#FF00FF">
</head>

<body>
  <!-- Content -->
  <script src="/assets/js/xy-polyfill.js" nomodule></script>
  <script src="/assets/js/script.js" type="module"></script>
</body>
</html>
```
[src](https://www.matuzo.at/blog/html-boilerplate/)

# CSS code to look great nearly everywhere
```css
html {
  max-width: 70ch;
  padding: 3em 1em;
  margin: auto;
  line-height: 1.75;
  font-size: 1.25em;
}
h1,h2,h3,h4,h5,h6 {
  margin: 3em 0 1em;
}
p,ul,ol {
  margin-bottom: 2em;
  color: #1d1d1d;
  font-family: sans-serif;
}
```
[src](https://www.swyx.io/css-100-bytes)
