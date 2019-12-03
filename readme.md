You're probably looking for
[Material Componests for the web](https://github.com/material-components/material-components-web)
instead which is currently the standard way of doing material design on the web.

# Material Design

This repo will be extended and adjusted over-time with mixins actually used in
crossperf modules. It often does not exactly follow the
[material guidelines archive](https://material.io/archive/guidelines/components/).

## Motivation

Need for material components that:

* work without/with as little JavaScript as possible (not included in this repo).
* do not produce CSS if none of the mixins are used.

### Background

Basically [material-components-web](https://github.com/material-components/material-components-web)
is way too heavy.

* **JavaScript** - It comes with JavaScript by default and a lot of things that
  could be solved with CSS are done in JavaScript instead which is not great.
* **Build** - Building it for use without any framework and JavaScript is a pain.
* **Selectors** - In the modern ages people seem to avoid CSS combinators. This
  has a good reason. Resolving selectors with combinators takes the browser
  engine longer.
  But crossperf resolves matched selectors of each element to a single class
  (unless pseudo-selectors are used) at compile-time so this is not an issue.
  Furthermore in crossperf changes to HTML are more expensive than changes to
  Sass therefore it's generally avoided to add or remove classes from the HTML.
  Which means that Sass would have a bunch of `@extend` statements. The
  annoying part is the boilerplate prefix which has to be typed and that every
  child element also needs `@extend` statements which is annoying. Furthermore
  the more CSS produced the slower the production build since as mentioned
  before all selectors resolve to the actual element which is being matched by
  it at compile-time.

**Wait what, crossperf resolves matched selectors to a single class?**

This only works since crossperf does not use JavaScript which messes with
classes.

Furthermore the HTML is obviously mutated at compile-time.

Example:

```css
.foo .bar {
  color: red;
}
.foo:hover > span + .baz {
  color: green;
}
```

```html
<a class="foo" href="#foo">
  <span class="bar">Bar</span>
  <span class="baz">Baz</span>
</a>
```

will produce:

```css
.a {
  color: red;
}
.b:hover > .c {
  color: green;
}
```

```html
<a class="b" href="#foo">
  <span class="a">Bar</span>
  <span class="c">Baz</span>
</a>
```

This is a major feature of crossperf which is quite useless to most of the web
since the web uses JavaScript to mess with classes and it's hard to reason
about in what way it messes with classes on what elements.

## License

Licensed under either of

 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or https://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or https://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any
additional terms or conditions.
