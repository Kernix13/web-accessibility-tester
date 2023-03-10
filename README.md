# Web Page Accessibility Tester

> From Brad Traversy video ussing p11y

This project uses `pa11y` with nodejs to create a simple API/back-end to get the results for a website and then display the results in the DOM for review. Use `fetch()` to make the request, add a loading spinner, and use Bootstrap for the styling.

## Setup

```bash
npm init -y
npm i express pa11y
```

You can create an env variable for the port or go with the default of `5000` from `index.js`. Run `npm start` and open `localhost:5000`. Finally, add a url to the form and submit and you'll see the results below.

## Purpose

Well, the purpose should be obvious, but this project is a great idea and was easy. Use this tool to fix all accessibility issues on:

1. Your website pages
1. Your portfolio pages
1. The pages for your employer
1. The pages for your clients
