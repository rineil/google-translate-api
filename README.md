# google-translate-api
A **free** and **unlimited** API for Google Translate :dollar::no_entry_sign:

## Features 

- Auto language detection
- Spelling correction
- Language correction 
- Fast and reliable – it uses the same servers that [translate.google.com](https://translate.google.com) uses

## Install 

```
npm install --save google-translate-api
```

## Usage
From automatic language detection to English:

``` js
const translate = require('@rineil/google-translate-api');

translate('I speak Japanese', {to: 'ja'}).then(res => {
    console.log(res.text);
    //=> 私は日本語を話します
    console.log(res.from.language.iso);
    //=> en
}).catch(err => {
    console.error(err);
});
```

From Japanese to English with a typo:

``` js
translate('私は日本語を話します', {from: 'ja', to: 'en'}).then(res => {
    console.log(res.text);
    //=> I speak Japanese
    console.log(res.from.text.autoCorrected);
    //=> true
    console.log(res.from.text.value);
    //=> 私は日本語を話します
    console.log(res.from.text.didYouMean);
    //=> false
}).catch(err => {
    console.error(err);
});
```

Sometimes, the API will not use the auto corrected text in the translation:

``` js
translate('I speak Japanese', {from: 'en', to: 'ja'}).then(res => {
    console.log(res);
    console.log(res.text);
    //=> 私は日本語を話します
    console.log(res.from.text.autoCorrected);
    //=> false
    console.log(res.from.text.value);
    //=> I speak Japanese
    console.log(res.from.text.didYouMean);
    //=> true
}).catch(err => {
    console.error(err);
});
```

## API

### translate(text, options)

#### text

Type: `string`

The text to be translated

#### options

Type: `object`

##### from

Type: `string` Default: `auto`

The `text` language. Must be `auto` or one of the codes/names (not case sensitive) contained in [languages.js](./languages.js)

##### to

Type: `string` Default: `en`

The language in which the text should be translated. Must be one of the codes/names (not case sensitive) contained in [languages.js](./languages.js).

##### raw

Type: `boolean` Default: `false`

If `true`, the returned object will have a `raw` property with the raw response (`string`) from Google Translate.

### Returns an `object`:

- `text` *(string)* – The translated text.
- `from` *(object)*
  - `language` *(object)*
    - `didYouMean` *(boolean)* - `true` if the API suggest a correction in the source language
    - `iso` *(string)* - The [code of the language](./languages.js) that the API has recognized in the `text`
  - `text` *(object)*
    - `autoCorrected` *(boolean)* – `true` if the API has auto corrected the `text`
    - `value` *(string)* – The auto corrected `text` or the `text` with suggested corrections
    - `didYouMean` *(booelan)* – `true` if the API has suggested corrections to the `text`
- `raw` *(string)* - If `options.raw` is true, the raw response from Google Translate servers. Otherwise, `''`.

Note that `res.from.text` will only be returned if `from.text.autoCorrected` or `from.text.didYouMean` equals to `true`. In this case, it will have the corrections delimited with brackets (`[ ]`):

``` js
translate('I speak Japanese!').then(res => {
    console.log(res.from.text.value);
    //=> I speak Japanese!
}).catch(err => {
    console.error(err);
});
```
Otherwise, it will be an empty `string` (`''`).

## License

[MIT © License](./LICENSE)
