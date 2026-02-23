# DjangoCon US 2027 Conference Website

The website for DjangoCon US 2027, built with [Eleventy](https://www.11ty.dev/) and [TailwindCSS](https://tailwindcss.com/).

## Installation

This project requires Node v20 or greater.

1. Run `npm install` to install Node packages.

## Building & Development

This project uses Liquid for templating (except dates, see below). As such, you may wish to install syntax highlighting for Liquid in your text editor.

* VS Code: [Liquid Language Support](https://marketplace.visualstudio.com/items?itemName=neilding.language-liquid)
* [Liquid documentation](https://liquidjs.com/)

Build and watch for local changes by running:

`npm run serve`

This opens a local server at `http://localhost:8080/` and watches for changes to the source files.

## Conference Phases

The conference goes through 3 phases, controlled by `phase` in `src/_data/site.json`:

* `landing`: A "Coming Soon" page with the logo, mailing list signup, and social links. Nav is hidden.
* `active`: The full conference site with schedule, speakers, tickets, and all content.
* `archived`: Post-conference recap with talk videos, photos, and highlights.

You can preview any phase without changing `site.json` by visiting the draft preview pages:

* `/preview/landing/`
* `/preview/active/`
* `/preview/archived/`

Reference:

* `src/index.html`
* `src/_includes/home/`
* `src/_includes/header.html`

## Date Formatting

Dates are formatted with [date-fns](https://date-fns.org/), due to some wonkiness with Eleventy's date formatting. You can use the `formatDateTime` shortcode in your templates to format dates. Note, that this will take into consideration the timezone defined in `site.json`, under `timezone`. Example:

```liquid
{{ post.data.published_datetime | formatDateTime: "MMMM d, yyyy" }}
```

## Social Media Images

1. Presenter images are created at `/presenters/{{ slug }}/`
2. Session images are created at `/{{ talks,tutorials }}/{{ slug }}/social/`

## Schedule Tools

Python tools for processing schedules and generating content live in `tools/`. Requires a Python virtualenv with dependencies from `tools/requirements.txt`.

* `python tools/process.py` - Main CLI for generating schedules and placeholders
* `python tools/swap_talks.py` - Swap two talks in the schedule

## Considerations when updating content

1. When adding images, if they are below the "fold", consider adding a `loading="lazy"` attribute to the image tag.
2. When adding images, consider adding an `alt` attribute to the image tag.
3. Keep copy short and to the point. The site is most likely scanned, not read.
4. Make sure to keep the styleguide up-to-date with any new components or styles.

## Styleguide

The styleguide lives at `/styleguide/` (respectively `styleguide.html`). The guide is built from content within `src/_content/styleguide/`. Each HTML page represents a section. Sections can be ordered with `order`. Each section can have a `description`.

When using code samples, be sure to use `{% capture code %}` to capture sample and pass it to the `code-snippet.html` include like so:

```liquid
{% include "code-snippet.html", code:code, lang:'html' %}
```
