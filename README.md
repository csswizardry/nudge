# Nudge

**Give developers a gentle push in the right direction.**

Nudge is an opinionated, ITCSS-compatible library that detects misuse of certain
CSS selectors, and gives developers a subtle nudge to fix them. Mistakes and
incorrect usage get highlighted in the UI.

![Screenshot showing a simple example of Nudge at work](./screenshot-001.png)

## Installation

    $ bower install --save nudgecss

Or:

    $ npm install --save nudgecss

## Disclaimers and Notices

1. **This is not an active OSS project.** Please feel free to use Nudge in your
   projects, but be aware that this repository exists mainly for me and my
   clients to use in our work. It’s opinionated, has limited scope, and is not
   actively seeking contributions. If do have something you feel is a worthy
   addition within this scope, open an issue.
2. **This is crude. Very crude.** The CSS in Nudge is actively bad. It’s
   circumstantial at best. Nudge is intended to be a very loose, high-level
   linter, and probably will miss things or give false positives. I’m okay with
   that—I just want Nudge to be a quick ’n’ dirty, cheap ’n’ cheerful first pass
   over the UI. For what it needs to do, Nudge is GoodEnough™ for now. More
   strict and stringent tools should be used if that’s what you require.
3. **Remove Nudge from production.** Nudge will fill your stylesheet full of
   crap: please make sure you remove Nudge before putting any CSS live. This
   will help performance, but will also ensure that your users never see any
   errors or breakages that your developers have missed.

## Usage

Using Nudge is relatively simple. For more in-depth information and
documentation, please see the comments in the relevant files.

### Including in your project

* `@import` `_tools.nudge.scss` into your _Tools_ layer (if you are not using
  ITCSS, `@import` it somewhere toward the beginning of your project).
* `@import` `_trumps.nudge.scss` into your _Trumps_ layer (if you are not using
  ITCSS, `@import` it somewhere toward the end of your project).

### Configuring

You can disable Nudge by setting `$nudge-enabled` to `false` just before you
`@import` `_tools.nudge.scss`, i.e.:

```scss
$nudge-enabled: false;
@import "tools.nudge";
```

Disabling Nudge will give warnings in your output stream: we don’t really want
people turning Nudge off if we’ve purposely included it in a project.

#### Incorrect Nesting

To check for incorrect nesting of a class, call the `nudge-nest()` mixin within
it, passing in the expected ancestor. For example, in our HTML, `.widget__title`
must always live inside of `.widget`, so in our Sass we would write:

```scss
.widget {
  [styles]
}

  .widget__title {
    @include nudge-nest('.widget');
    [styles]
  }
```

Now we will see an error in our UI if `.widget__title` is used outside of the
context of `.widget`.

#### Deprecated Selectors

To configure our deprecated selectors, simply copy/paste the
`$nudge-deprecated-selectors` map out of `_trumps.nudge.scss` and into your
manifest file (e.g. `main.scss`). Place it just before your `@import` for the
`_trumps.nudge.scss` file, then delete the `!default` flag and replace the
example selectors with your own, e.g.:

```scss
$nudge-deprecated-selectors: (
  '.btn': '.c-btn',
);
@import "trumps.nudge";
```

## The Rules

* **Incorrect Nesting:** Check that a selector hasn’t been used outside of its
  required context. Display an error if it has.
* **No IDs** We shouldn’t use IDs in CSS, so display a warning any time we find
  one.
* **Single-Space Delimited `class` Attributes:** Prefer two spaces separating
  individual strings in the `class` attribute. Display a warning if we find
  single spaces.
* **Improper Use of BEM Modifiers:** Display an error if someone tries to use
  a Modifier without also having defined a Block.
* **Underscores as Delimiters:** We use hyphens to delimit strings, so display
  a warning if we find any classes with single hyphens.
* **Use of Camel Case:** We do not use camel case, so display a warning if we
  find any capital letters in a class.
* **Depracated Selectors:** Display a warning if we find any classes that we
  have deemed deprecated.
