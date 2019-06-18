## SCSS Vendor Prefixes


SCSS Mixins for CSS Vendor Prefixes is a `git submodule` that _should_ be compatible with GitHub's Pages


------


#### Table of Contents


- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Support](#support)
- [License](#license)


------


## Requirements


- `SCSS` to `CSS` compiler, see notice from [Ruby SCSS](https://sass-lang.com/ruby-sass) team for available migration options

- Declaring which should be prefixed, there's roughly `100` available


## Installation


Add the code within this repository to your own as a `submodule` with `git`...


```bash
cd your-project

_git_url='https://github.com/scss-utilities/vendor-prefixes.git'
_destination='_scss/modules/vendor-prefixes'

git submodule add -b master "${_git_url}" "${_destination}"
```


... `commit` and `push` the changes to the `.gitmodules` file and `_scss/modules/vendor-prefixes` directory from within the root of you project...


```bash
git status

git commit -m 'Added GitHub hosted submodule scss-utilities/vendor-prefixes'

git push <remote> <branch>
```


Be sure to notify those cloning code from your project for the first time that...


```bash
git clone --recurse-submodules <your-repositorys-url>
```


... is a _good idea_, and those that have already cloned your project may instead want something like...


```bash
git submodule update --init --recursive
git submodule update --merge
```


... to _sync up_ with the additional tracking information.


## Usage


Within the _`main.scss`_ file for your site, import the [`lib/render-vendor-prefixes.scss`][source_link__render-vendor-prefixes] and [`lib/map-vendor-prefixes.scss`][source_link__map-vendor-prefixes] file and then any other `vendor-prefixes` contained in this repository.


```scss
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


## Support


Open a new _`Issue`_ (or up-vote currently opened <sub>[![Issues][badge__issues]][relative_link__issues]</sub> if similar) to report bugs and/or make feature requests a higher priority for project maintainers. Submit _`Pull Requests`_ after _`Forking`_ this repository to add features or fix bugs, and be counted among this project's <sub>[![Members][badge__contributors]][relative_link__members]</sub>


> See GitHub's documentation on [Forking][help_fork] and issuing [Pull Requests][help_pull_request] if these are new terms.
>
> And check the chapter regarding [submodules][git_book__submodules] from the Git book prior to opening issues regarding submodule _trouble-shooting_


Supporting projects like this one through <sub>[![Liberapay][badge__liberapay]][liberapay_donate]</sub> or via Bitcoin <sub>[![BTC][badge__bitcoin]][btc]</sub> is most welcomed.


## License


```
Bash Argument Parser, a submodule for other Bash scripts tracked by Git
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



[help_fork]: https://help.github.com/en/articles/fork-a-repo
[help_pull_request]: https://help.github.com/en/articles/about-pull-requests

[git_book__submodules]: https://git-scm.com/book/en/v2/Git-Tools-Submodules


[relative_link__issues]: ../issues
[relative_link__members]: ../network/members
[source_link__render-vendor-prefixes]: ../lib/render-vendor-prefixes.scss
[source_link__map-vendor-prefixes]: ../lib/map-vendor-prefixes.scss
[source_link__calc]: calc.scss


[badge__issues]: https://img.shields.io/github/issues/scss-utilities/vendor-prefixes.svg
[badge__contributors]: https://img.shields.io/github/forks/scss-utilities/vendor-prefixes.svg?color=005571&label=Contributors

[badge__liberapay]: https://img.shields.io/badge/Liberapay-gray.svg?logo=liberapay
[badge__bitcoin]: https://img.shields.io/badge/1Dr9KYZz9jkUea5xTxeGyScu7AwC4MwR5c-gray.svg?logo=bitcoin


[liberapay_donate]: https://liberapay.com/scss-utilities/donate
[btc]: https://www.blockchain.com/btc/address/1Dr9KYZz9jkUea5xTxeGyScu7AwC4MwR5c


[w3__css_ui_3]: https://www.w3.org/TR/css-ui-3/

[w3schools__cssref]: https://www.w3schools.com/cssref/default.asp

[mozilla__css]: https://developer.mozilla.org/en-US/docs/Web/CSS

[csslint__require_compatible_vendor_prefixes]: https://github.com/csslint/csslint/wiki/require-compatible-vendor-prefixes

[w3schools__css3_browsersupport]: https://www.w3schools.com/cssref/css3_browsersupport.asp
