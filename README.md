# FormData
A FormData polyfill


[![npm version][npm-image]][npm-url]

```bash
npm install formdata-polyfill
```

This doesn't monkey patch xhr#send like [Rob--W](https://github.com/Rob--W) did [here](https://gist.github.com/Rob--W/8b5adedd84c0d36aba64).
However this provides you a way to convert the entire form to a blob and send it with xhr or fetch.
```javascript
fd = new FormData(form)
xhr.send(fd._blob())

xhr.send(fd) // This don't work... Needs to be a native FormData
```

Another possible solution is to convert the form to a native FormData but then you would lose all the other methods not supported by the original FormData.
```javascript
fd = new FormData(form)
fd._asNative() // returns a native formData with all the fields
```

If you wish to monkey patch XHR to make it work out of the box
then you have to do it yourself
```js
import FormData from 'formdata-polyfill'

XMLHttpRequest = class extends XMLHttpRequest {
  send(data) {
    super.send(data instanceof FormData ? data._blob() : data)
  }
}
```

The current status of the native FormData is this
[![skarmavbild 2017-08-27 kl 20 17 14](https://user-images.githubusercontent.com/1148376/29752796-b5ef2e3a-8b64-11e7-8fea-24c80fc2d5b6.png)](https://developer.mozilla.org/en-US/docs/Web/API/FormData#Browser_compatibility)


This lib provides you all the function others don't include
 - `append` with filename
 - `delete()`, `get()`, `getAll()`, `has()`, `set()`
 - `entries()`, `keys()`, `values()`, and support of `for...of`
 - Available in web workers	(yes, just include it...)


> The reason why Rob--W's version didn't work for me was that it only works in web workers due to FileReaderSync beeing used. I did it with constructing new chunks with the blob constructor instead. `new Blob([string, blob, file, etc])`

  [npm-image]: https://img.shields.io/npm/v/formdata-polyfill.svg?style=flat-square
  [npm-url]: https://www.npmjs.com/package/formdata-polyfill
