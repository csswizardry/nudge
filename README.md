# Nudge

**Give developers a gentle push in the right direction.**

Nudge is an opinionated, ITCSS-compatible library that detects misuse of certain
CSS selectors, and gives developers a subtle nudge to fix them. Mistakes and
incorrect usage gets highlighted in the UI.

![Screenshot showing a simple example of Nudge at work](./screenshot-001.png)

## Installation

    $ [NPM]

    $ [BOWER]

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

    $nudge-enabled: false;
    @import "tools.nudge";

Disabling Nudge will give warnings in your output stream: we don’t really want
people turning Nudge off if we’ve purposely included it in a project.

#### Incorrect Nesting

To check for incorrect nesting of a class, call the `nudge-nest()` within it,
passing in the expected ancestor. For example, in our HTML, `.widget__title`
must always live inside of `.widget`, so in our Sass we would write:

    .widget {
      [styles]
    }

      .widget__title {
        @include nudge-nest('.widget');
        [styles]
      }

Now we will see an error in our UI if `.widget__title` is used outside of the
context of `.widget`.

#### Deprecated Selectors

To configure our deprecated selectors, simply copy/paste the
`$nudge-deprecated-selectors` map out of `_trumps.nudge.scss` and into your
manifest file (e.g. `main.scss`). Place it just before your `@import` for the
`_trumps.nudge.scss` file, then elete the `!default` flag and replace the
example selectors with your own, e.g.:

    $nudge-deprecated-selectors: (
      '.btn': '.c-btn',
    );
    @import "trumps.nudge";

## The Rules

* **Imcorrect Nesting:** Check that a selector hasn’t been used outside of its
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
