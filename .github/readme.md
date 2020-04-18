## SCSS Vendor Prefixes
[heading__title]:
  #scss-vendor-prefixes
  "&#x2B06; Top of ReadMe File"

SCSS Mixins for CSS Vendor Prefixes is a `git submodule` that _should_ be compatible with GitHub's Pages


## [![Byte size of vendor-prefixes][badge__master__vendor_prefixes__source_code]][vendor_prefixes__master__source_code] [![Open Issues][badge__issues__vendor_prefixes]][issues__vendor_prefixes] [![Open Pull Requests][badge__pull_requests__vendor_prefixes]][pull_requests__vendor_prefixes] [![Latest commits][badge__commits__vendor_prefixes__master]][commits__vendor_prefixes__master]


------


#### Table of Contents


- [:arrow_up: Top of ReadMe File][heading__title]

- [:building_construction: Requirements][heading__requirements]

- [:zap: Quick Start][heading__quick_start]

- [:shell: Utilize Vendor Prefixes][heading__utilize]

- [&#x1F5D2; Notes][notes]

- [:card_index: Attribution][heading__attribution]

- [:balance_scale: License][heading__license]


------



## Requirements
[heading__requirements]:
  #requirements
  "&#x1F3D7; "


- `SCSS` to `CSS` compiler, see notice from [Ruby SCSS](https://sass-lang.com/ruby-sass) team for available migration options

- Declaring which should be prefixed, there's roughly `100` available


___


## Quick Start
[heading__quick_start]:
  #quick-start
  "&#9889; Perhaps as easy as one, 2.0,..."


Add the code within this repository to your own as a `submodule` via `git`...


```bash
cd your-project

_git_url='https://github.com/scss-utilities/vendor-prefixes.git'
_destination='_scss/modules/vendor-prefixes'

git submodule add -b master "${_git_url}" "${_destination}"
```


... `commit` and `push` the changes to the `.gitmodules` file and `_scss/modules/vendor-prefixes` directory from within the root of you project...


```bash
git status

git commit -m ':heavy_plus_sign: Adds submodule scss-utilities/vendor-prefixes#1'

git push <remote> <branch>
```


Be sure to notify those cloning code from your project for the first time that...


```bash
git clone --recurse-submodules <your-repositorys-url>
```


... is a _good idea_, and those that have already cloned your project may instead want something like...


```bash
git submodule update --init --recursive --merge
```


... to _sync up_ with the additional tracking information.


___


## Utilize Vendor Prefixes
[heading__utilize]:
  #utilize-vendor-prefixes
  "&#x1F41A; How to make use of this project within other repositories"


Within the _`main.scss`_ file for your site, import the [`lib/render-vendor-prefixes.scss`][source_link__render-vendor-prefixes] and [`lib/map-vendor-prefixes.scss`][source_link__map-vendor-prefixes] file and then any other `vendor-prefixes` contained in this repository.


```scss
@import '_scss/modules/vendor-prefixes/lib/map-vendor-prefixes.scss';
@import '_scss/modules/vendor-prefixes/lib/render-vendor-prefixes.scss';

@import '_scss/modules/vendor-prefixes/cursor.scss';


.some-selectable-element {
  @include cursor(pointer);
}
```


Above will output `CSS` similar to...


```css
.some-selectable-element {
  -webkit-cursor: pointer;
  -moz-cursor: pointer;
  -o-cursor: pointer;
  cursor: pointer;
}
```


Some of the available vendor prefixes may have additional arguments that may be passed, eg. the [`calc.scss`](../calc.scss) mixin has `$property`, `$value`, `$fallback` keyword arguments...


```scss
@mixin calc($property, $value, $fallback: false) {
  @if $fallback {
    #{$property}: #{$fallback};
  } @else {
    @warn "Consider setting a fallback for #{$property}";
  }
  @include render-vendor-prefixes(
    $property: $property,
    $value: calc(#{$value}),
    $vendor-list: (-webkit, -moz),
    $prefix: value,
  );
}
```


... which allows for setting a `CSS` fallback for browsers that do not support `calc()`...


```scss
@import '_scss/modules/vendor-prefixes/lib/map-vendor-prefixes.scss';
@import '_scss/modules/vendor-prefixes/lib/render-vendor-prefixes.scss';

@import '_scss/modules/vendor-prefixes/calc.scss';


.some-resizing-element {
    @include calc(
        $property: width,
        $value: 100% - 40px,
        $fallback: 85%
    );
}
```


... eventually outputing `CSS` a bit like...


```css
.some-resizing-element {
    width: 85%;
    width: calc(100% - 40px);
}
```


___


## Notes
[notes]:
  #notes
  "&#x1F5D2; Additional notes and links that may be worth clicking in the future"


Following Bash code may be used to `@import` **all** vendor prefixes to a `main.scss` file...


```Bash
_module_path='_scss/modules/vendor-prefixes'
_imports_path='_scss/vendor-prefixes.scss'
_path_list=()


while IFS= read -r -d '' _path; do
    _path_list+=("${_path}")
done < <(find "${_module_path}/lib" -type f -name '*.scss' -print0 | sort -z)

while IFS= read -r -d '' _path; do
    _path_list+=("${_path}")
done < <(find "${_module_path}" -type f -name '*.scss' -not -path '*/lib/*' -print0 | sort -z)


for _path in "${_path_list[@]}"; do
    [[ $(grep -q -- "@import '${_path}';" "${_imports_path}") ]] || {
      tee -a "${_imports_path}" <<<"$(printf "@import '%s';" "${_path:6:-5}")"
    }
done

## Note the ':6:-5' portion of '${_path:6:-5}' _should_
##  strip '_scss/' or '_sass/' from beginning
##  and '.scss' from end of file paths
```


Import the `_imports_path` file into your main styles file, eg. `assets/main.scss` for Minima....


```Liquid
---
# Only the main Sass file needs front matter (the dashes are enough)
---

@import "minima";


@import "modules/vendor-prefixes";
```


... then declare any desired Sass vendor prefixes...


```SCSS
.example-class {
  @include text-stroke(4px blue);
  @include min-content(width);
}
```


... to build CSS similar to...


```CSS
.example-class {
  -webkit-text-stroke: 4px blue;
       -o-text-stroke: 4px blue;
          text-stroke: 4px blue;
  width: -webkit-min-content;
  width: -moz-min-content;
  width: min-content;
}
```


___


## Attribution
[heading__attribution]:
  #attribution
  "&#x1F4C7; Resources that where helpful in building this project so far."


- [W3 -- Vendor Keyword History](https://www.w3.org/TR/CSS21/syndata.html#vendor-keyword-history)

- [StackOverflow -- List of CSS Vendor Prefixes](https://stackoverflow.com/questions/5411026/list-of-css-vendor-prefixes)

- [SassDoc -- annotations](http://sassdoc.com/annotations/)

___


## License
[heading__license]:
  #license
  "&#x2696; Legal bits of Open Source software"


Legal bits of Open Source software


```
SCSS Vendor Prefixes documentation of a GitHub Pages compatible submodule
Copyright (C) 2019  S0AndS0

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published
by the Free Software Foundation; version 3 of the License.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.
```



[source_link__render-vendor-prefixes]: https://github.com/scss-utilities/vendor-prefixes/blob/master/lib/render-vendor-prefixes.scss

[source_link__map-vendor-prefixes]: https://github.com/scss-utilities/vendor-prefixes/blob/master/lib/map-vendor-prefixes.scss

[source_link__calc]: https://github.com/scss-utilities/vendor-prefixes/blob/master/calc.scss


[w3__css_ui_3]: https://www.w3.org/TR/css-ui-3/

[w3schools__cssref]: https://www.w3schools.com/cssref/default.asp

[mozilla__css]: https://developer.mozilla.org/en-US/docs/Web/CSS

[csslint__require_compatible_vendor_prefixes]: https://github.com/csslint/csslint/wiki/require-compatible-vendor-prefixes

[w3schools__css3_browsersupport]: https://www.w3schools.com/cssref/css3_browsersupport.asp


[badge__commits__vendor_prefixes__master]:
  https://img.shields.io/github/last-commit/scss-utilities/vendor-prefixes/master.svg

[commits__vendor_prefixes__master]:
  https://github.com/scss-utilities/vendor-prefixes/commits/master
  "&#x1F4DD; History of changes on this branch"


[vendor_prefixes__community]:
  https://github.com/scss-utilities/vendor-prefixes/community
  "&#x1F331; Dedicated to functioning code"


[badge__issues__vendor_prefixes]:
  https://img.shields.io/github/issues/scss-utilities/vendor-prefixes.svg

[issues__vendor_prefixes]:
  https://github.com/scss-utilities/vendor-prefixes/issues
  "&#x2622; Search for and _bump_ existing issues or open new issues for project maintainer to address."


[badge__pull_requests__vendor_prefixes]:
  https://img.shields.io/github/issues-pr/scss-utilities/vendor-prefixes.svg

[pull_requests__vendor_prefixes]:
  https://github.com/scss-utilities/vendor-prefixes/pulls
  "&#x1F3D7; Pull Request friendly, though please check the Community guidelines"


[badge__master__vendor_prefixes__source_code]:
  https://img.shields.io/github/repo-size/scss-utilities/vendor-prefixes

[vendor_prefixes__master__source_code]:
  https://github.com/scss-utilities/vendor-prefixes
  "&#x2328; Project source code!"
