## js2-closure.el
*Google Closure dependency manager*

---

Do you use emacs, `js2-mode`, and Google's Closure Library?  Do you get
frustrated writing your `goog.require` statements by hand?  If that's the
case, then this extension is going to make you very happy.

js2-closure is able to analyse the JavaScript code in your buffer to
determine which imports you need, and then update the `goog.require` list at
the top of your buffer. It works like magic. It also runs instantaneously,
even if you have a big project.

### Installation


You need to run the provided script that crawls all your JavaScript sources
for provide statements.  Be sure to include the sources of the Closure
Library itself.  Here's an example:

    wget https://raw.githubusercontent.com/jart/js2-closure/master/js2-closure-provides.sh
    ./js2-closure-provides.sh \
        ~/justinetunney.com/assets/closure/closure/goog \
        ~/justinetunney.com/assets/js/jart \
        >~/.emacs.d/js2-closure-provides.sh

That will generate an index file in the same directory which should have the
same path as `js2-closure-provides-file`.  You have to regenerate this file
occasionally by hand.  Each time you run the script, you should also run
`M-x js2-closure-reload` inside emacs.

### Usage


To use this, you simply run `M-x js2-closure-fix` inside your `js2-mode`
buffer.  This will regenerate the list of goog.require statements by
crawling your source code to see which identifiers are being used.

If you want the magic to happen automatically each time you save the suffer,
then add the following to your .emacs file:

    (eval-after-load 'js2-mode
      '(add-hook 'before-save-hook 'js2-closure-save-hook))

Alternatively, you can use a key binding as follows:

    (eval-after-load 'js2-mode
      '(define-key js2-mode-map (kbd "C-c C-c") 'js2-closure-fix))

This tool was written under the assumption that you're following Google's
JavaScript style guide: http://goo.gl/Ny5WxZ

### Function Documentation


#### `(js2-closure-fix)`

Fix the goog.require statements for the current buffer.

This assumes that all the requires are in one place and sorted,
without indentation or blank lines.  If you don't have any
requires, they'll be added after your provide statements.  If you
don't have those, then this routine will fail.

Effort was also made to avoid needlessly modifying the buffer,
since syntax coloring might take some time to kick back in.

#### `(js2-closure-reload)`

Load precomputed list of provided namespaces into memory.

#### `(js2-closure-save-hook)`

Global save hook to invoke `js2-closure-fix` if in `js2-mode`.

-----
<div style="padding-top:15px;color: #d0d0d0;">
Markdown README file generated by
<a href="https://github.com/mgalgs/make-readme-markdown">make-readme-markdown.el</a>
</div>
