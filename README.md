# snowdrop

Snowdrop is a [University of Guelph](https://www.uoguelph.ca/) themed
front-end framework based on [Bootstrap](https://getbootstrap.com/).

## Getting started

Snowdrop is derived from Bootstrap and supports all of the classes and components available 
in [Bootstrap 5](https://getbootstrap.com/docs/5.1/getting-started/introduction/), customized
to follow University of Guelph brand guidelines.

## Quick start

Snowdrop is available via jsDelivr, a free open source CDN. 

### CSS

Include the following in the page `<head>`.

```  
<link href="https://cdn.jsdelivr.net/npm/@uoguelph/snowdrop/dist/css/snowdrop.min.css" rel="stylesheet" crossorigin="anonymous">
```

### JS

Some of the components require JavaScript to function. Include the
following `<script>` within the body of your page, before the closing
`</body>` tag.

```
<script src="https://cdn.jsdelivr.net/npm/@uoguelph/snowdrop/dist/js/snowdrop.bundle.min.js" crossorigin="anonymous"></script>
```

## Package managers

### npm

Install snowdrop in your npm project:

```
$ npm install @uoguelph/snowdrop
```

To reference a specific Github branch, you can use the following syntax in your package.json file:
```
  "@uoguelph/snowdrop": "github:ccswbs/snowdrop#feature-branch",
```

## Updating files

1. Update the files under the scss directory
2. If you wish, you can run `npm run dist` to update the distribution files.
3. Commit your change.

See Bootstrap's guide on [Sass](https://getbootstrap.com/docs/5.2/customize/sass/) and [using npm scripts](https://getbootstrap.com/docs/5.2/getting-started/contribute/#using-npm-scripts) for more information.

### Rules of thumb

- Use extreme caution before changing a global default style.
- Don't add anything new that can be accomplished with existing (Bootstrap) classes.