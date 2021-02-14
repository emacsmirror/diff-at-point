###################
Emacs Diff at Point
###################

This is a utility for navigating between changes in your repository,
by opening a diff, and by navigating to the lines inside it.

Available via `melpa <https://melpa.org/#/diff-at-point>`__.

See `demo video <https://youtu.be/bR4czpEqah8>`__.


Motivation
==========

When working on a task that touches multiple files,
it's often useful to navigate between these changes.


Features
========

``diff-at-point-open-and-goto-hunk``
   Creates a diff for the current repository,
   navigating to the current point (where possible).

``diff-at-point-goto-source-and-close``
   Go to the location at the point, closing the current diff buffer.


Usage
=====

You may bind the functions to keys, in specific modes.

This is not a minor mode, instead, there are two main functions
you can bind yourself:

Here are example bindings which use ``Ctrl-Alt-Return``.

.. code-block:: elisp

   (add-hook 'diff-mode-hook
     (lambda ()
       (define-key diff-mode-shared-map (kbd "<C-M-return>") 'diff-at-point-goto-source-and-close)))

   (add-hook 'prog-mode-hook
     (lambda ()
       (define-key prog-mode-map (kbd "<C-M-return>") 'diff-at-point-open-and-goto-hunk)))


Or you can use a global binding which checks the major mode.

.. code-block:: elisp

   (global-set-key (kbd "<C-M-return>")
     (lambda () (interactive)
       (cond
         ((string= major-mode "diff-mode")
           (diff-at-point-goto-source-and-close))
         (t
           (diff-at-point-open-and-goto-hunk)))))


Customization
=============

``diff-at-point-diff-command``
   This is the function used to create the diff buffer.

   While there is no need to change this value from it's default (calling ``vc-root-diff``)
   it can be overridden for different behavior.

   This example shows how Emacs 27's ``vc-root-version-diff``
   can be used to diff the working copy against a branch instead of ``HEAD`` of a git repository.

   .. code-block:: elisp

      (define-key prog-mode-map (kbd "<C-M-return>")
        '(lambda ()
           (interactive)
           (let
             ((diff-at-point-diff-command
                '(lambda () (vc-root-version-diff nil "master" nil))))
             (diff-at-point-open-and-goto-hunk))))
