Emacs mode for [lean theorem prover][lean]

[lean]: https://github.com/leanprover/lean

Requirement
===========

``lean-mode`` requires [Emacs 24][emacs24] and following
packages which can be installed via <kbd>M-x package-install</kbd>.

 - [dash][dash]
 - [dash-functional][dash]
 - [f][f]
 - [s][s]

[emacs24]: http://www.gnu.org/software/emacs
[MELPA]: http://melpa.milkbox.net
[dash]: https://github.com/magnars/dash.el
[f]: https://github.com/rejeep/f.el
[s]: https://github.com/magnars/s.el

The following packages are *optional* but we recommend to install them
to use full features of ``lean-mode``.

 - [company][company]
 - [flycheck][flycheck]
 - [fill-column-indicator][fci]
 - [lua-mode][lua-mode]
 - [mmm-mode][mmm-mode]
 - [whitespace-cleanup-mode][wcm]

[company]: http://company-mode.github.io/
[flycheck]: http://flycheck.readthedocs.org/en/latest
[fci]: https://github.com/alpaker/Fill-Column-Indicator
[lua-mode]: http://immerrr.github.io/lua-mode/
[mmm-mode]: https://github.com/purcell/mmm-mode
[wcm]: https://github.com/purcell/whitespace-cleanup-mode

Install
=======

Put the following elisp code on your emacs setup 
(e.g. ``.emacs.d/init.el`` [GNU Emacs], ``~/Library/Preferences/Aquamacs Emacs/Preferences.el`` [Aquamacs]) :

```elisp
(require 'package)
(add-to-list 'package-archives
             '("melpa" . "http://melpa.milkbox.net/packages/") t)
(package-initialize)

;; Install required/optional packages for lean-mode
(defvar lean-mode-required-packages
  '(company dash dash-functional flycheck whitespace-cleanup-mode
    f fill-column-indicator s lua-mode mmm-mode))
(let ((need-to-refresh t))
  (dolist (p lean-mode-required-packages)
    (when (not (package-installed-p p))
      (when need-to-refresh
        (package-refresh-contents)
        (setq need-to-refresh nil))
      (package-install p))))

;; Set up lean-root path
(setq lean-rootdir "~/projects/lean")  ;; <=== YOU NEED TO MODIFY THIS
(setq-local lean-emacs-path
            (concat (file-name-as-directory lean-rootdir)
                    (file-name-as-directory "src")
                    "emacs"))
(add-to-list 'load-path (expand-file-name lean-emacs-path))
(require 'lean-mode)

;; Customization for lean-mode
(customize-set-variable 'lean-delete-trailing-whitespace t)
(customize-set-variable 'lean-flycheck-use t)
(customize-set-variable 'lean-eldoc-use t)
```

Key Bindings
============

|Key                | Function                          |
|-------------------|-----------------------------------|
|<kbd>C-c C-x</kbd> | lean-std-exe                      |
|<kbd>C-c C-l</kbd> | lean-std-exe                      |
|<kbd>C-c C-t</kbd> | lean-eldoc-documentation-function |
|<kbd>C-c C-f</kbd> | lean-fill-placeholder             |
|<kbd>M-.</kbd>     | lean-find-tag                     |
|<kbd>TAB</kbd>     | lean-tab-indent-or-complete       |
|<kbd>C-c C-o</kbd> | lean-set-option                   |
|<kbd>C-c C-e</kbd> | lean-eval-cmd                     |


Known Issues and Possible Solutions
===================================

Unicode
-------

If you experience a problem reading unicode characters on emacs,
first consider using a unicode-friendly font such as `DejaVu Sans Mono`:

```elisp
(when (member "DejaVu Sans Mono" (font-family-list))
  (set-face-attribute 'default nil :font "DejaVu Sans Mono-11"))
```

If you still experience rendering problem. Please consider trying
[emacs-unicode-fonts](https://github.com/rolandwalker/unicode-fonts).

Contribution
============

Contribution is welcome!

If your contribution is a bug fix, create your topic branch from
`master`. If it is a new feature, check if there exists a
WIP(work-in-progress) branch (`vMAJOR.MINOR-wip`). If it does, use
that branch, otherwise use `master`.

Install [Cask](https://github.com/cask/cask) if you haven't already,
then:

    $ cd /path/to/lean/src/emacs
    $ cask

Run all tests with:

    $ make
