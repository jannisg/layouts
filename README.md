# Reshape Layouts

Layout inheritance using `block` and `extend`, inspired by ([jade/pug](http://jade-lang.com/reference/inheritance/)).

### Installation

`npm i reshape-layouts --save`

### Usage

Let's say we have a base template:

`base.html`
```html
<html>
    <head>
        <title><block name="title"> — Github</block></title>
    </head>

    <body>
        <div class="content">
           <block name="content"></block>
        </div>
        <footer>
            <block name="footer">footer content</block>
        </footer>
    </body>
</html>
```

Now we can inherit this template. All defined blocks inside `<extends>` will
replace the blocks with the same name in the parent template. If the block is not defined inside `<extends>` its content in the parent template remains the same.

In the example the blocks `title` and `content` will be replaced and
the block `footer` will remain unchanged:

```js
const reshape = require('reshape')
const layouts = require('reshape-layouts')

const html = '<extends src="base.html">' +
               '<block name="title">How to use reshape-layouts</block>' +
               '<block name="content">Read the documentation</block>'
             '</extends>'

reshape({
  plugins: layouts({
    encoding: 'utf8', // Parent template encoding (default: 'utf8')
    root: './' // Path to parent template directory (default: './')
  })
}).process(html)
  .then((res) => res.output())
```

The final HTML will be:

```html
<html>
  <head>
    <title>How to use reshape-layouts</title>
  </head>

  <body>
    <div class="content">Read the documentation</div>
    <footer>footer content</footer>
  </body>
</html>
```


#### Append & Prepend

It's also possible to append and prepend block's content

```js
const reshape = require('reshape')
const layouts = require('reshape-layouts')

const html = '<extends src="base.html">' +
               '<block name="title" type="prepend">How to use reshape-layouts</block>' +
               '<block name="content">Read the documentation</block>' +
               '<block name="footer" type="append">— 2016</block>'
           '</extends>';

reshape({ plugins: layouts() })
  .process(html)
  .then((res) => res.output())
```

The final HTML will be:

```html
<html>
  <head>
    <title>How to use reshape-layouts — Github</title>
  </head>
  <body>
    <div class="content">Read the documentation</div>
    <footer>footer content — 2016</footer>
  </body>
</html>
```

### License & Contributing

- Licensed under [MIT](LICENSE.txt)
- See the [contributing guide](contributing.md)