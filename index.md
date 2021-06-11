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

### Prerequisites

React3L is built for well-structured React projects, which has its own requirements:
- You must use Typescript.
- You should enable decorators support, especially if you are using Babel
- You must be familiar with React Hook APIs
- You should have the mindset of clean code

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

---

## About the project

React3L is &copy; 2020-{{ "now" | date: "%Y" }} by [thanhtunguet](https://thanhtunguet.github.io).

### License

Distributed by an [MIT license](https://github.com/react3l/react3l/tree/master/LICENSE).

### Contributing

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change. Read more about becoming a contributor in [our GitHub repo](https://github.com/react3l/react3l#contributing).

#### Thank you to the contributors of React3L!

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
