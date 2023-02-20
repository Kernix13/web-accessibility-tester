# Notes from the video

Repo: https://github.com/bradtraversy/website-accessibility-tester

Pa11y is an accessibility tester. You can pass in a URL and it returns an array of reults on accessibility issues, e.g. a missing `alt` attribute for an `img` tag, a `label` without a `for` attribute, etc.

## Setup back-end API

Use the [pa11y package](https://github.com/pa11y/pa11y) - in the terminal install express and pa11y:

```bash
npm init -y
npm i express pa11y
pa11y https://example.com/
```

Bring it into your JS file:

```js
const pa11y = require('pa11y');

pa11y('https://example.com/').then(results => {
  // Do something with the results
});
// or instead:
async function run() {
  const response = await pa11y('https://traversy.dev');
  console.log(response);
}
```

- Before creating the express server, test out pa11y
- Use a valid URL then run `node index` in the terminal

```js
async function run(url) {
  const response = await pa11y(url);
  console.log(response);
}
run('https://www.traversymedia.com/');
```

```bash
{
  documentTitle: 'EGC Home Page | Every Guitar Chord',
  pageUrl: 'https://everyguitarchord.com/',
  issues: [
    {
      code: 'WCAG2AA.Principle1.Guideline1_4.1_4_3.G18.Fail',
      type: 'error',
      typeCode: 1,
      message: 'This element has insufficient contrast at this conformance level. Expected a contrast ratio
 of at least 4.5:1, but text in this element has a contrast ratio of 4.45:1. Recommendation:  change backgr
ound to #222.',
      context: '<span class="text-wrap">Home</span>',
      selector: '#menu-item-253 > a > span',
      runner: 'htmlcs',
      runnerExtras: {}
    },
    {
      code: 'WCAG2AA.Principle3.Guideline3_2.3_2_2.H32.2',
      type: 'error',
      typeCode: 1,
      message: 'This form does not contain a submit button, which creates issues for those who cannot submi
t the form using the keyboard. Submit buttons are INPUT elements with type attribute "submit" or "image", o
r BUTTON elements with type "submit" or omitted/invalid.',
      context: '<form role="search" method="get" class="searchform" action="https://everyguitarchord.com/">
\n' +
        '<label for="ocean-search-form-...</form>',
      selector: '#searchform-dropdown > form',
      runner: 'htmlcs',
      runnerExtras: {}
    },
    # and more here...
  ]
}
```

- you get a single object with props of `documentTitle`, `pageUrl`, and an array of objects called `issues`
- `issues` has the props: `code`, `type`, `typeCode`, `message`, `context`, `selector`, `runner`, and `runnerExtras`
- you want `message` and `context` and `code` for reference
- next create an API with a route you can hit so you can get the data with the URL as a query param
- so require in `express`
- create a port for it to run: `process.env.PORT` but defaulting to `5000`
- initialize `express` - then run the server with `listen`
- use a get request and pass the URL as a query param - or you could do a post request and pass the url in the body
- test it with Postman - open up a new tab and make a get request -
- open Postman and do a check for the error just using `http://localhost/api/test` then tack on `?url=https://www.traversymedia.com/`
- the API is done!
- use `http://localhost:5000/api/test?url=https://www.traversymedia.com/`

## Front-end

- we want a form to enter any url we want
- create a folder called `public` for the static front-end files
- add the middleware for that with `app.use()`
- add a script in `package.json` - then `npm start`
- inside `public` create `index.html` and `js/main.js` and if you don't use bootstrap create `css/style.css`
- search for `bootstrap cdn` and click on the getbootstrap link
- then go to `localhost:5000`

## JavaScript

6 functions:

1. Fetch accessibility issues: `testAccessibility`
1. Download CSV: `csvIssues`
1. Add issues to DOM: `addIssuesToDOM`
1. Set loading state: `setLoading`
1. Escape HTML: `escapeHTML`
1. Clear results: `clearResults`

> How come searching for `webpage with no accessibility issues for testing` did not return any results?

I need to:

1. use my function `convertHTML Entities`
1. add a more recognizable border to the cards

NOTE: He used the Bootsrap class of `visually-hidden` for a `i` tag for a twitter link which he wrapped with a `span` tag, so `span.visually-hidden`

#1 HTML Entities

```js
// His function:
function escapeHTML(html) {
  return html
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#039;');
}

// My replacement: longer than his, so maybe I'll switch back
function escapeHTML(html) {
  const convertSymbols = {
    '&': '&amp;',
    '<': '&lt;',
    '>': '&gt;',
    '"': '&quot;',
    "'": '&apos;',
  };

  return html
    .split('')
    .map(symbol => convertSymbols[symbol] || symbol)
    .join('');
}
```

#2 Changed his `border-secondary` to `border-danger`
