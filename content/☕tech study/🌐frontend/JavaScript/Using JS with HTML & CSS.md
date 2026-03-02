# Using `<script>` in `html`
- [[HTML]]
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>People entered:</h1>
    <h2 id="count-el">0</h2>
    <script>
      document.getElementById("count-el").innerText = 5;
    </script>
  </body>
</html>
```
# Using `index.js`
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h1>People entered:</h1>
    <h2 id="count-el">0</h2>
    <script src="index.js"></script>
  </body>
</html>
```