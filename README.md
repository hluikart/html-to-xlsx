# html-to-xlsx
[![NPM Version](http://img.shields.io/npm/v/html-to-xlsx.svg?style=flat-square)](https://npmjs.com/package/html-to-xlsx)
[![License](http://img.shields.io/npm/l/html-to-xlsx.svg?style=flat-square)](http://opensource.org/licenses/MIT)
[![Build Status](https://travis-ci.org/pofider/html-to-xlsx.png?branch=master)](https://travis-ci.org/pofider/html-to-xlsx)

> **node.js html to xlsx transformation**

Transformation only supports html table and several basic style properties. No images or charts are currently supported.

## Usage

```js
const fs = require('fs')
const conversionFactory = require('html-to-xlsx')
const puppeteer = require('puppeteer')
const chromeEval = require('chrome-page-eval')({ puppeteer })
const conversion = conversionFactory({
  extract: chromeEval
})

(async () => {
  const stream = await conversion(`<table><tr><td>cell value</td></tr></table>`)

  stream.pipe(fs.createWriteStream('/path/to/output.xlsx'))
})()
```

## Supported properties
- `background-color` - cell background color
- `color` - cell foreground color
- `border-left-style` - as well as positions will be transformed into excel cells borders
- `text-align` - text horizontal align in the excel cell
- `vertical-align` - vertical align in the excel cell
- `width` - the excel column will get the highest width, it can be little bit inaccurate because of pixel to excel points conversion
- `height` - the excel row will get the highest height
- `font-size` - font size
- `colspan` - numeric value that merges current column with columns to the right
- `rowspan` - numeric value that merges current row with rows below.  
- `overflow` - the excel cell will have text wrap enabled if this is set to scoll.

## Constructor options

```js
const conversionFactory = require('html-to-xlsx')
const puppeteer = require('puppeteer')
const chromeEval = require('chrome-page-eval')({ puppeteer })
const conversion = conversionFactory({ /*[constructor options here]*/})
```

- `extract` **function** **[required]** - a function that receives some input (an html file path and a script) and should return some data after been evaluated the html passed. the input that the function receives is:
  ```js
  {
    html: <file path to a html file>,
    scriptFn: <string that contains a javascript function to evaluate in the html>,
    timeout: <time in ms to wait for the function to complete, the function should use this value to abort any execution when the time has passed>,
    /*options passed to `conversion` will be propagated to the input of this function too*/
  }
  ```
- `tmpDir` **string** - the directory path that the module is going to use to save temporary files needed during the conversion. defaults to [`require('os').tmpdir()`](https://nodejs.org/dist/latest-v8.x/docs/api/os.html#os_os_tmpdir)
- `timeout` **number** - time in ms to wait for the conversion to complete, when the timeout is reached the conversion is cancelled. defaults to `10000`

## Conversion options

```js
const fs = require('fs')
const conversionFactory = require('html-to-xlsx')
const puppeteer = require('puppeteer')
const chromeEval = require('chrome-page-eval')({ puppeteer })
const conversion = conversionFactory({
  extract: chromeEval
})

(async () => {
  const stream = await conversion(/* html */, /* extract options */)
})()
```

- `html` **string** - the html source that will be transformed to an xlsx, the html should contain a table element
- `extractOptions` **object** - additional options to pass to the specified `extract` function

## License
See [license](https://github.com/pofider/html-to-xlsx/blob/master/LICENSE)
