# iubenda Hugo Module

This module facilitates setting up Iubenda on a Hugo project through the use of a simple shortcode.

## Requirements

Requirements:
- Go 1.14
- Hugo 0.61.0


## Installation

If not already, [init](https://gohugo.io/hugo-modules/use-modules/#initialize-a-new-module) your project as a Hugo Module:

```
$: hugo mod init {repo_url}
```

Configure your project's module to import this module:

```yaml
# config.yaml
module:
  imports:
  - path: github.com/theNewDynamic/hugo-module-tnd-iubenda
```

## Usage

You can invoke Iubenda content with using the Module's built-in shortcode.

### tnd-iubenda Shortcode

The shortcode takes only one argument, the type of document to be printed.
Available document types are:

- `privacy-policy`
- `cookie-policy`
- `terms-and-conditions`

#### Examples

```markdown
---
# content/privacy-policy.md
title: We Respect your Privacy
description: We really do. We're not collecting any data from you that you don't know about, and we keep that data to ourselves.
---

{{< tnd-iubenda "privacy-policy" >}}
```

### API KEY

To obfuscate the API Key from the repo, the Module uses Environment variables to read you Iubenda's API Key.

#### Monolingual site
Store your key in `TND_IUBENDA_KEY`

#### Multilingual sites
Iubenda provides one key per language.

Store your key in `TND_IUBENDA_KEY_{LANG}` where `LANG` is the language code of the page as registered through HUGO.
Ex: For a multilingual site with english and french versions of Iubenda documents, you should use:
`TND_IUBENDA_KEY_FR` and `TND_IUBENDA_KEY_EN`

### Caching

The module uses Hugo's `getJSON` to grab the document content using Iubenda's API. In order to avoid calling the API on every build we rely on Hugo's cache default configuration. (unlimited cache).

It does mean though, that from now on, we must bust this cache. The easiest way to do that is by using the [version setting](#version) described bellow.

If the `version` settings is not used, you should edit your project's [File Caches settings](https://gohugo.io/getting-started/configuration/#configure-file-caches) accordlingly.

### Settings

Settings are added to the project's parameter under the `tnd_iubenda` map as shown below.

```yaml
# config.yaml
params:
  tnd_iubenda:
    version: v1.0
    no_markup: true
```

#### version

Allow's cache busting by updating the parameter with a new value whenever the Iubenda API response needs to be refreshed. (most likey after editing the settings of any given document)

#### no_markup

If you need to print documents free of any iubenda default markup, `no_markup` should be set to true.

## Styling

Users are free to add any styling involving Iubenda to their own styling pipeline. Bot for ease of use, the module will print in a safe `style` tag the content of an asset file named `assets/tnd-iubenda/style.css` if found on the project.

So for:
```css
/* assets/tnd-iubenda/style.css */
.iub_content h1{
  margin-top: 2rem;
  font-size: 2.25rem;
}
```
The module will print above the Iubenda document the following:
```html
<style integrity="sha256-YPxff30Qq1ac9coOVBl7f84qO8fhVpeYn2qLmEXlDjU=">
.iub_content h1{
  margin-top: 2rem;
  font-size: 2.25rem;
}
</style>
```

## theNewDynamic

This project is maintained and love by [thenewDynamic](https://www.thenewdynamic.com).