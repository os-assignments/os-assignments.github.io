# Operating systems assignments 

Operating systems programming assignments for the OS (1DT044), OSPP (1DT096) and
DSP (1DT003) courses at the [Department of information technology][it], [Uppsala
university][uu].

[it]: https://www.it.uu.se/first?lang=en

[uu]: https://www.uu.se/en/

<p align="center">
<picture>
 <source media="(prefers-color-scheme: dark)" srcset="static/css/images/uu-full-logo-dark.png">
 <img src="static/css/images/uu-full-logo-light.png">
</picture>
</p>

## Hugo

Static website built with [Hugo][hugo].

[hugo]: https://gohugo.io/

## Theme Relearn

The website uses a customized version of the [Relearn][relearn] Hugo theme.
[Latest verified commit][commit] compatible with my customizations. 

[relearn]: https://mcshelby.github.io/hugo-theme-relearn/

[commit]:
    https://github.com/McShelby/hugo-theme-relearn/commit/ee77892ea9591ed6ff7ec33173bfc4b1ea4f6895

## Versions of the site

The current version of the assignments are called `v1` and all content for this
site is in he `versions/v1` directory. 

In the future, more versions can  be added in the `versions` directory. 

## Parent site

A small parent site for all versions is found in the `content` directory.

## Common configuration

The parent site and the `v1` site share the same base [Hugo
configuration][config] in `config/_default/hugo.toml`.

[config]:https://gohugo.io/getting-started/configuration/

Customizations to the Relearn theme in `layouts` and `static`.

## Version specific configuration

Additional configuration for the `v1` site in `versions/v1/hugo.toml`.

## Get started

Clone the repository. 

```
git clone https://github.com/os-assignments/os-assignments.github.io.git
```

The Relearn theme is included as a Git submodule and is not part of this
repository. Downlad he latest version of all Git submodules to your working tree.  

```
git submodule update  --init --recursive
```

Start Hugo development server. 

```
hugo server --config=versions/v1/hugo.toml
```

## GitHub Pages

The parent site:

-  https://os-assignments.github.io

, and the `v1` site: 

- https://os-assignments.github.io/v1 
  
, are hosted on [GitHub Pages][pages].

[pages]: https://pages.github.com/

## Automatic deployment

The GitHub Action [Workflow](.github/workflows/hugo.yaml) is used to automatically deploy the
sites on every push to the `main` branch. 

[workflow]: https://github.com/os-assignments/os-assignments.github.io/blob/main/.github/workflows/hugo.yaml

[actions]: https://github.com/os-assignments/os-assignments.github.io/actions

[v1]: https://os-assignments.github.io/v1/