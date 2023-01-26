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
 2. Run `npm run dist` to update the distribution files.
 3. Commit your change.

 See Bootstrap's guide on [Sass](https://getbootstrap.com/docs/5.2/customize/sass/) and [using npm scripts](https://getbootstrap.com/docs/5.2/getting-started/contribute/#using-npm-scripts) for more information.

 ### Rules of thumb

 - Use extreme caution before changing a global default style.
 - Don't add anything new that can be accomplished with existing (Bootstrap) classes.
 - Reference named colour variables (e.g $red, $blue) from the palette rather than using hex values (when possible)

 # Upgrading Bootstrap version

 1. Create a new feature branch.
 1. Update the version of Bootstrap in package.json
 1. Run npm install and commit the change

 ## Copy the new Bootstrap site folder into the Snowdrop repo
 1. Rename the "site" folder as "site_old"
 1. Download the newer version of Bootstrap. For the rest of this guide, we'll refer to this version as {BOOTSTRAP_VERSION_NUM}
 1. Copy the "site" folder from your Bootstrap download into the Snowdrop repository

 ## Rename version number
 1. Upgrade the Snowdrop version number in package.json. Use the {BOOTSTRAP_VERSION_NUM} as a guide as well as [Semantic Versioning](https://semver.org). For the rest of this readme, we'll refer to the updated Snowdrop version number as {SNOWDROP_VERSION_NUM}.
 1. In config.yml:
   - upgrade any instances of the old Snowdrop version to {SNOWDROP_VERSION_NUM}.
   - ignore the CDN section for now.
 1. In .github/workflows/gh-pages.yml, change the DOCS_VERSION environment variable to "{SNOWDROP_VERSION_NUM}"

 ## Update the Bootstrap docs so they are customised for Snowdrop
 1. Under site/static/docs, rename the version folder to {SNOWDROP_VERSION_NUM}
 1. Under site/content/docs, rename the version folder to {SNOWDROP_VERSION_NUM}
 1. In site/layouts/partials/stylesheet.html, change all instances of "dist/js/bootstrap" to "dist/js/snowdrop"
 1. In site/layouts/partials/scripts.html, change all instances of "dist/js/bootstrap" to "/dist/js/snowdrop"

 ## Fix file URLs for scss-docs shortcode
This section involves updating the scss-docs shortcode logic so that it can always find the files it's looking for. It's possible that as Bootstrap changes, the logic below will no longer work. In that case, you'll need to run `npm run dist` and `npm run docs-serve` and troubleshoot the errors until the shortcode can find all its files. Hopefully it keeps working as Bootstrap upgrades.

 1. Do a search for "{{< scss-docs" throughout the "site" folder. Your results will likely include lines like "{{< scss-docs name="something" file="scss/_something.scss" >}}". Take note of the file parameter.
    1. If all the files in the file parameter start with "scss/", then go under site/layouts/shortcodes/scss-docs.html, and find the following code:
 ```
 {{- $match = findRE $regex (readFile $file) -}}
 ```
 Change it to
 ```
 {{- $match = findRE $regex (readFile (printf "node_modules/bootstrap/%s" $file)) -}}
 ```
    1. Alternatively, if most of the files do NOT start with "scss/" and instead start with something under the site folder, then go under site/layouts/shortcodes/scss-docs.html, and find the following code:
 ```
 {{- $match = findRE $regex (readFile $file) -}}
 ```
 Change it to
 ```
   {{- $match := "" -}}
  {{- if not (in $file "site/") -}}
    {{- $match = findRE $regex (readFile (printf "node_modules/bootstrap/%s" $file)) -}}
  {{- else }}
    {{- $match = findRE $regex (readFile $file) -}}
  {{- end -}}
 ```

1. Run npm run docs-serve.
1. Troubleshoot until the docs work.

## Update the look and fell of the customised Snowdrop docs
1. In site/assets/scss/_variables.scss and change the main colour variable, usually $bd-purple, so that its value is $red (or whatever colour you want it to be)
1. Make any updates you feel are necessary to get the page looking the way you want it (e.g., you can update the homepage background class .bd-masthead in site/assets/scss/_masthead.scss)
1. Copy any images you require over from the previous styleguide (e.g., the image of Alice with Snowdrop, the University of Guelph favicon and logo) into site/static/docs/{SNOWDROP_VERSION_NUM}]/assets/img
1. Copy the U of G svg logo into site/layouts/partials/icons/u-of-g-logo.svg (there should be a copy in the old site folder)
1. In site/layouts/partials/docs-navbar.html, find the Bootstrap svg logo and replace it with the U of G logo

## Update the content in the customised Snowdrop docs
1. Search for all references to Bootstrap under site/content/docs/ and replace with Snowdrop
1. Search for references to Bootstrap under site/layouts/partials/docs-navbar.html and replace with Snowdrop 
1. Search for all references to "/docs/{BOOTSTRAP_VERSION_NUM} (including the left quotation mark) in site/content and replace with "/docs/{SNOWDROP_VERSION_NUM}
1. Update site/layouts/partials/footer.html with University of Guelph information
1. Update the Bootstrap heading so it reads Snowdrop in site/layouts/partials/home/masthead.html. Change the documentation on this page to reflect the details of Snowdrop (e.g., you can point to our npm package and explain how to use it for anyone using the front-end toolkit)

## (Optional) Hide ads and analytics
1. Comment out the script in site/layouts/partials/ads.html
1. Comment out the script in site/layouts/partials/analytics.html

## Test the docs on Github
1. In .github/workflows/gh-pages.yml, update the workflow so:
  - it triggers for your feature branch instead of main. 
  - it deploys for your feature branch instead of main.
1. When you push your branch to github, it should deploy a new version of the documentation under a folder called {SNOWDROP_VERSION_NUM}.
1. Go to https://ccswbs.github.io/snowdrop/{SNOWDROP_VERSION_NUM}/
1. If all looks as expected, revert the change so that workflow triggers and deploys on main.

## Update CDN hash values for styles and js stored on https://ccswbs.github.io
1. If you deployed the docs on https://ccswbs.github.io/snowdrop/{SNOWDROP_VERSION_NUM}/ and the styles are broken, it's possible you need to update the hash values for the styles. Under config.yml, find the css, js and js_bundle values and generate new hashes for each URL hosted on https://ccswbs.github.io/snowdrop/{SNOWDROP_VERSION_NUM}/.
1. Re-deploy to Github and see if that fixes the styles