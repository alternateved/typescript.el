# typescript.el

![Build & Test](https://github.com/emacs-typescript/typescript.el/workflows/Build%20&%20Test/badge.svg)
[![MELPA](https://melpa.org/packages/typescript-mode-badge.svg)](https://melpa.org/#/typescript-mode)
[![MELPA Stable](https://stable.melpa.org/packages/typescript-mode-badge.svg)](https://stable.melpa.org/#/typescript-mode)

`typescript.el` is a major-mode for editing [Typescript](http://www.typescriptlang.org/)-files in [GNU Emacs](https://www.gnu.org/software/emacs/).

`typescript.el` is a self-contained, lightweight and minimalist major-mode
focused on providing basic font-lock/syntax-highlighting and
indentation for Typescript syntax, without any external dependencies.

Output from `tsc` and `tslint` is also handled seamlessly through
`compilation-mode`.

## A short note on development HALT

As the both the JavaScript and TypeScript languages have evolved to become ever more complex, so has the
Elisp codebase for `typescript-mode` trying to correctly handle them.

We''ve been at the point for quite some time where it has become increasingly obvious that the current code-base
simply cannot continue growing. It will be slow. It will be complex. It will be buggy. It will be head-ache inducing
to wrap our heads around it, and ... I guess we're already there.

Apart from occasional PRs getting merged, the current `typescript-mode` code isn't being developed because almost nobody
wants to work code of this complexity.

*Essentially all major development of `typescript-mode` has come to a halt.*

## Seeing the forest for trees

Lots of Emacs major-modes are facing the same problem. I'm sure there's similar issues for other editors too.

This means lots of developers are working on solving this problem once and for all, and what they've decided on
is relying on a standardized set of external parsers using the [tree-sitter](https://tree-sitter.github.io/tree-sitter/) library.

New major modes are being developed as we speak to support TypeScript (and other languages) within Emacs based on Emacs' upcoming
native tree-sitter support.

The code will be much faster, it will be simpler to work with and everyone should be happy. Well almost. Since tree-sitter support
relies on a new major feature being added to core Emacs, it also means that these new major modes won't be backward compatible with
older Emacs-versions.

For that reason we are not *replacing* this major mode with the new and improved one, but keeping it around to make sure older Emacs-versions
does at least have an option for working with Typescript, even though it may not be optimal or track recent changes to the TypeScript-language.

But once new Emacs ships with tree-sitter support, you are adviced to upgrade to the newer modes for better TypeScript-support, rather
than keeping this old version around.

## Good news!
Emacs 29 will ship with tree-sitter support, and it will actually have in-tree
support for TypeScript!  So now you can just use the provided `ts-mode` and get
the fresh tree-sitter powered functionalities for free.  Yes, that means both
tsx and regular types will actually work.  Development will continue in emacs
core, rather than this repo.  We hope you'll like the new experience.

# Installation

`typescript.el` can be installed from source directly using your
favourite approach or framework, or from MELPA and MELPA Stable as a
package.

To install typescript.el simply type `M-x package-install<RET>typescript-mode<RET>`.

# Customization

To customize `typescript.el` just type the following: `M-x customize-group<RET>typescript<RET>`.

You can add any other customization you like to `typescript-mode-hook`
in your `init.el` file. `typescript.el` also handles `prog-mode-hook`
on versions of Emacs which supports it.

# Support for Compilation Mode

This mode automatically adds support for `compilation-mode` so that if
you run `M-x compile<ret>tsc<ret>` the error messages are correctly
parsed.

However, the error messages produced by `tsc` when its `pretty` flag
is turned on include ANSI color escapes, which by default
`compilation-mode` does not interpret. In order to get the escapes
parsed, you can use:

```elisp
(require 'ansi-color)
(defun colorize-compilation-buffer ()
  (ansi-color-apply-on-region compilation-filter-start (point-max)))
(add-hook 'compilation-filter-hook 'colorize-compilation-buffer)
```

Or, if you prefer, you can configure `tsc` with the `pretty` flag set
to `false`: `tsc --pretty false`. However, doing this does more than
just turning off the colors. It also causes `tsc` to produce less
elaborate error messages.

# Contributing

To run the tests you can run `make test`.

If you prefer, you may run the tests via the provided `Dockerfile`.

```bash
docker build -t typescript-mode .
docker run --rm -v $(pwd):/typescript-mode typescript-mode
```

# Other Typescript-packages of interest

While `typescript.el` may *not* provide a full kitchen-sink, the good
news is that there's other packages which do!

More advanced features can be provided by using these additional
packages:

* [lsp-mode](https://github.com/emacs-lsp/lsp-mode) - A standards-based
  code-completion and refactoring backend, based on the
  [Language Server Protocol (LSP)](https://langserver.org/).

* [tide](https://github.com/ananthakumaran/tide/) - TypeScript
  Interactive Development Environment for Emacs
* [ts-comint](https://github.com/josteink/ts-comint) - a Typescript REPL
  in Emacs.

Initializing these with `typescript.el` will then become a matter of
creating your own `typescript-mode-hook` in your `init.el` file.
