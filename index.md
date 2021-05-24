---
layout: default
title: Home
nav_order: 1
description: "Just the Docs is a responsive Jekyll theme with built-in search that is easily customizable and hosted on GitHub Pages."
permalink: /
---

# react3l.github.io

{: .fs-9 }

Focus on building awesome apps with well-structured code.
{: .fs-6 .fw-300 }

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [View it on GitHub](https://github.com/react3l/react3l){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Getting started

### Dependencies

Just the Docs is built for [Jekyll](https://jekyllrb.com), a static site generator. View the [quick start guide](https://jekyllrb.com/docs/) for more information. Just the Docs requires no special plugins and can run on GitHub Pages' standard Jekyll compiler. The [Jekyll SEO Tag plugin](https://github.com/jekyll/jekyll-seo-tag) is included by default (no need to run any special installation) to inject SEO and open graph metadata on docs pages. For information on how to configure SEO and open graph metadata visit the [Jekyll SEO Tag usage guide](https://jekyll.github.io/jekyll-seo-tag/usage/).

### Quick start

#### Add react3l packages

```sh
yarn add react3l-common react3l-decorators react3l-advanced-filters react3l-axios-observable react3l-localization
```

#### Add peer dependencies

```sh
yarn add reflect-metadata rxjs axios i18next react-i18next moment
```

<small>You must have GitHub Pages enabled on your repo, one or more Markdown files, and a `_config.yml` file. [See an example repository](https://github.com/pmarsceill/jtd-remote)</small>

### Multi-languages support

#### Add packages

```bash
yarn add react3l-localization i18next react-i18next
```

#### Add extractor

```bash
yarn add -D react3l-i18next-extractor
```

#### Add scripts to package.json

```json
{
  "scripts": {
    "extract": "i18next-extractor -i src/ -o src/i18n/ -p src/i18n/ extract",
    "merge": "i18next-extractor -i src/ -o src/i18n/ -p src/i18n/ merge"
  }
}
```

#### Create translation directories

```bash
mkdir -p src/i18n/en src/i18n/vi
```

### Configure React3L

- [See configuration options]({{ site.baseurl }}{% link docs/configuration.md %})

---

## About the project

Just the Docs is &copy; 2017-{{ "now" | date: "%Y" }} by [Patrick Marsceill](http://patrickmarsceill.com).

### License

Distributed by an [MIT license](https://github.com/react3l/react3l/tree/master/LICENSE).

### Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change. Read more about becoming a contributor in [our GitHub repo](https://github.com/pmarsceill/just-the-docs#contributing).

#### Thank you to the contributors of Just the Docs!

<ul class="list-style-none">
{% for contributor in site.github.contributors %}
  <li class="d-inline-block mr-1">
     <a href="{{ contributor.html_url }}"><img src="{{ contributor.avatar_url }}" width="32" height="32" alt="{{ contributor.login }}"/></a>
  </li>
{% endfor %}
</ul>

### Code of Conduct

Just the Docs is committed to fostering a welcoming community.

[View our Code of Conduct](https://github.com/pmarsceill/just-the-docs/tree/master/CODE_OF_CONDUCT.md) on our GitHub repository.
