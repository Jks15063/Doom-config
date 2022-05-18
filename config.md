
# Table of Contents

1.  [Info](#org3014a40)
    1.  [Enabling Literate Config Mode](#orgdc0c462)
2.  [Basic Config](#orgbecd990)
    1.  [User Info](#org6d875f1)
    2.  [Fullscreen](#org6940253)
    3.  [Visual Settings](#orgac6f2b2)
        1.  [Font](#org04376f7)
        2.  [Theme](#orgd48ff32)
        3.  [Line numbers](#org7bbf73f)
        4.  [Rainbow Identifiers](#org2262ecc)
        5.  [Ligatures](#orgfbcfeb8)
        6.  [Custom Banner](#org6f163f9)
    4.  [Keybinds](#orgf7feaf4)
        1.  [Local leader key](#org371d014)
        2.  [Avy and surround setup](#org80b8fd8)
        3.  [Bind newline command](#org2aa2ac1)
        4.  [Rebind org agenda](#orgd47d73c)
        5.  [Rebind org capture](#org3aec7b0)
        6.  [Bind SQL-connect](#orgd8d3eab)
    5.  [Misc QoL settings](#orgf3da6cc)
        1.  [Unordered escape key sequence](#orgf7b0fa5)
        2.  [Copy paste with middle mouse button](#org91a933c)
        3.  [Global word wrap](#org94fa3f3)
        4.  [Treat underscore as a word character](#org8a5acbc)
        5.  [VTerm](#orgdb8412a)
3.  [Mode Config](#orgfbbbcdb)
    1.  [Org mode config](#org7232d32)
        1.  [Directory Setup](#orga218aca)
        2.  [Visual Settings](#org5388fe7)
        3.  [GTD and Agenda Settings](#org79e2455)
    2.  [SQL mode config](#org6aecbc5)
        1.  [Set defaults](#org128d24f)
    3.  [Org-books config](#orgf187c79)



<a id="org3014a40"></a>

# Info

Here are some additional functions/macros that could help you configure Doom:  

-   `load!` for loading external \*.el files relative to this one
-   `use-package` for configuring packages
-   `after!` for running code after a package has loaded
-   `add-load-path!` for adding directories to the \`load-path&rsquo;, relative to  
    this file. Emacs searches the \`load-path&rsquo; when you load packages with  
    `require` or \`use-package&rsquo;.
-   `map!` for binding new keys

To get information about any of these functions/macros, move the cursor over  
the highlighted symbol at press `K` (non-evil users must press `C-c g k`).  
This will open documentation for it, including demos of how they are used.  

You can also try `gd` (or `C-c g d`) to jump to their definition and see how  
they are implemented.  


<a id="orgdc0c462"></a>

## Enabling Literate Config Mode

1.  Uncomment `literate` under `:config` in `init.el`
2.  Create a `config.org` file (like this one) in your Doom dir
3.  Run `bin/doom refresh`


<a id="orgbecd990"></a>

# Basic Config

Make this file run (slightly) faster with lexical binding (see [this blog post](https://nullprogram.com/blog/2016/12/22/)  
for more info).  

    ;;; config.el -*- lexical-binding: t; -*-

Place your private configuration here! Remember, you do not need to run `doom
sync` after modifying this file!  


<a id="org6d875f1"></a>

## User Info

    (setq user-full-name "jks15063"
          user-mail-address "jks15063@gmail.com")


<a id="org6940253"></a>

## Fullscreen

Expand to fullscreen on start  

    (if (eq initial-window-system 'x)
        (toggle-frame-maximized)
      (toggle-frame-fullscreen))


<a id="orgac6f2b2"></a>

## Visual Settings


<a id="org04376f7"></a>

### Font

Doom exposes five (optional) variables for controlling fonts in Doom. Here  
are the three important ones:  

-   `doom-font`
-   `doom-variable-pitch-font`
-   `doom-big-font` &#x2013; used for `doom-big-font-mode`; use this for  
    presentations or streaming.

They all accept either a font-spec, font string (&ldquo;Input Mono-12&rdquo;), or xlfd  
font string. You generally only need these two:  

I compiled a version of `iosevka` with additional special characters to support  
the [Ligatures](#orgfbcfeb8) added below  

    (setq doom-font (font-spec :family "Iosevka" :size 16))


<a id="orgd48ff32"></a>

### Theme

Non-doom themes can be added in packages  

    (setq doom-theme 'material)


<a id="org7bbf73f"></a>

### Line numbers

This determines the style of line numbers in effect. If set to \`nil&rsquo;, line  
numbers are disabled. For relative line numbers, set this to \`relative&rsquo;.  

    (setq display-line-numbers-type t)


<a id="org2262ecc"></a>

### Rainbow Identifiers

Rainbow identifiers in all prog modes  

    (add-hook! prog-mode #'rainbow-identifiers-mode)


<a id="orgfbcfeb8"></a>

### Ligatures

The ligatures set for elixir mode make 0 sense. Remove the nonsense and setup a  
few arrows.  

Look at `modules/lang/elixir/config.el` for `set-pretty-symbols!` for list of  
elixir-mode symbols.  

    (plist-delete! +pretty-code-symbols :def)
    (plist-delete! +pretty-code-symbols :in)
    (plist-delete! +pretty-code-symbols :for)
    (plist-delete! +pretty-code-symbols :lambda)
    (plist-delete! +pretty-code-symbols :yield)
    (plist-delete! +pretty-code-symbols :not-in)
    (plist-delete! +pretty-code-symbols :and)
    (plist-delete! +pretty-code-symbols :or)
    (plist-put +pretty-code-symbols :|> #Xe142)
    (plist-put +pretty-code-symbols :-> #Xe149)
    (plist-put +pretty-code-symbols :=> #Xe161)
    (plist-put +pretty-code-symbols :<- #Xe179)
    (plist-put +pretty-code-symbols :<> #Xe1c9)
    (plist-put +pretty-code-symbols :<!-- #Xe10e)
    (plist-put +pretty-code-symbols :<!--- #Xe10f)


<a id="org6f163f9"></a>

### Custom Banner

    (setq fancy-splash-image "/home/jake/Pictures/doom-logo.png")


<a id="orgf7feaf4"></a>

## Keybinds


<a id="org371d014"></a>

### Local leader key

Set local leader key to `,`  

    (setq doom-localleader-key ",")


<a id="org80b8fd8"></a>

### Avy and surround setup

For the last few years on `spacemacs`, I had moved surround to `S` and had  
`avy-goto-char-timer` set to `s`. But by default, Doom emacs replaces `s` and  
`S` with `evil snipe`, which is similar, but not what I want, so lets turn it  
off:  

    (remove-hook 'doom-first-input-hook #'evil-snipe-mode)

Map `avy-goto-char-timer` to `s` and `avy-goto-line` to `C-;`:  

    (evil-define-key '(normal motion) global-map "s" 'avy-goto-char-timer)
    (evil-define-key '(visual operator) evil-surround-mode-map "s" 'avy-goto-char-timer)
    (evil-define-key '(normal motion visual operator) global-map (kbd "C-;") 'avy-goto-line)

And move surround to `S`:  

    (evil-define-key 'operator evil-surround-mode-map "S" 'evil-surround-edit)
    (evil-define-key 'visual evil-surround-mode-map "S" 'evil-surround-region)


<a id="org2aa2ac1"></a>

### Bind newline command

    (map! :leader "j n" #'sp-newline)


<a id="orgd47d73c"></a>

### Rebind org agenda

The default sub-group is completely redundant, all the cmds are already  
available on the agenda.  

    (map! :leader "o a" #'org-agenda)


<a id="org3aec7b0"></a>

### Rebind org capture

By default on `SPC X`, going to overwrite `SPC x` because scratch buffer is  
still easy to get to with `SPC b x`.  

    (map! :leader "x" #'org-capture)


<a id="orgd8d3eab"></a>

### Bind SQL-connect

Connect to one of the db connections defined in `sql-connection-alist`  

    (map! :leader "o s" #'sql-connect)


<a id="orgf3da6cc"></a>

## Misc QoL settings


<a id="orgf7b0fa5"></a>

### Unordered escape key sequence

So I can just mash `jk` without thinking  

    (setq evil-escape-unordered-key-sequence t)


<a id="org91a933c"></a>

### Copy paste with middle mouse button

    (setq xterm-mouse-mode -1)


<a id="org94fa3f3"></a>

### Global word wrap

    (+global-word-wrap-mode +1)


<a id="org8a5acbc"></a>

### Treat underscore as a word character

    ;; For elixir
    (add-hook! 'elixir-mode-hook (modify-syntax-entry ?_ "w"))


<a id="orgdb8412a"></a>

### VTerm

    (setq vterm-module-cmake-args "-DUSE_SYSTEM_LIBVTERM=yes")


<a id="orgfbbbcdb"></a>

# Mode Config


<a id="org7232d32"></a>

## Org mode config


<a id="orga218aca"></a>

### Directory Setup

1.  Org directory

    Set the base org mode directory  
    
        (setq org-directory "~/Dropbox/org/gtd/")

2.  Roam directory

        (setq org-roam-directory "~/Dropbox/org/roam")

3.  GTD agenda files

        (setq org-agenda-files '("~/Dropbox/org/gtd/inbox.org"
                                 "~/Dropbox/org/gtd/gtd.org"
                                 "~/Dropbox/org/gtd/cars.org"
                                 "~/Dropbox/org/gtd/tickler.org"))

4.  Refile targets

        (setq org-refile-targets '(("~/Dropbox/org/gtd/gtd.org" :maxlevel . 3)
                                   ("~/Dropbox/org/gtd/someday.org" :level . 1)
                                   ("~/Dropbox/org/gtd/cars.org" :level . 1)
                                   ("~/Dropbox/org/gtd/tickler.org" :maxlevel . 2)))
    
    Allow the creation of new parent nodes while refiling. This means “allow me to  
    tack new heading names onto the end of my outline path, and if I am asking to  
    create new ones, make me confirm it.”  
    
        (setq org-refile-allow-creating-parent-nodes 'confirm)

5.  Diary Settings

    Set the diary file, exclude it from our agenda, and insert timestamps on creation.  
    
        (setq org-agenda-diary-file "~/Dropbox/org/gtd/diary.org")
        (setq org-agenda-include-diary nil)
        (setq org-agenda-insert-diary-extract-time t)


<a id="org5388fe7"></a>

### Visual Settings

1.  Set checkbox statistics face to orange

    The pure red is a bit much, still need to lower the brightness though  
    
        (after! org
          (set-face-attribute 'org-checkbox-statistics-todo nil :background "#FF5722"))

2.  Set done todo face to grey

    They are grey by default in Doom emacs, but somewhere, somehow, they got set to orange.  
    
        (after! org
          (set-face-attribute 'org-headline-done nil :foreground "grey"))

3.  Todo Keyword Face Colors

        (after! org
          (setq org-todo-keyword-faces
                (quote (("TODO" :foreground "#FF5722" :weight bold)
                        ;; ("NEXT" :foreground "#2196F3" :weight bold)
                        ("DONE" :foreground "#4CAF50" :weight bold)
                        ("WAITING" :foreground "orange" :weight bold)
                        ;; ("HOLD" :foreground "#E040FB" :weight bold)
                        ("CANCELLED" :foreground "#4CAF50" :weight bold)))))

4.  Inline images on load

    not working, add `#+STARTUP: inlineimages` to org file  
    
        (after! org
          (setq org-startup-with-inline-images t))


<a id="org79e2455"></a>

### GTD and Agenda Settings

1.  Custom agenda commands

    -   This custom cmd was borrowed from [Nicolas Petton&rsquo;s GTD with emacs breakdown](https://emacs.cafe/emacs/orgmode/gtd/2017/06/30/orgmode-gtd.html). It filters the todo list down to the first `todo` tasks tagged `@work`.
    
    Never use this and not sure how to use it, should remove this.  
    
        (after! org
          (setq org-agenda-custom-commands
                '(("w" "Work" tags-todo "@work"
                   ((org-agenda-overriding-header "Work")
                    (org-agenda-skip-function #'my-org-agenda-skip-all-siblings-but-first)))))
        
          (defun my-org-agenda-skip-all-siblings-but-first ()
            "Skip all but the first non-done entry."
            (let (should-skip-entry)
              (unless (org-current-is-todo)
                (setq should-skip-entry t))
              (save-excursion
                (while (and (not should-skip-entry) (org-goto-sibling t))
                  (when (org-current-is-todo)
                    (setq should-skip-entry t))))
              (when should-skip-entry
                (or (outline-next-heading)
                    (goto-char (point-max))))))
        
          (defun org-current-is-todo ()
            (string= "TODO" (org-get-todo-state))))

2.  Todo Keywords

    Set up todo keyword workflow  
    
        (after! org
          (setq org-todo-keywords '((sequence "TODO(t)" "WAITING(w@)" "|" "DONE(d)" "CANCELLED(c@)"))))

3.  Capture Templates

        (after! org
          (setq org-capture-templates '(("t" "Todo [inbox]" entry (file+headline "~/Dropbox/org/gtd/inbox.org" "Tasks") "* TODO %i%?\n")
                                        ("T" "Tickler" entry (file+headline "~/Dropbox/org/gtd/tickler.org" "Tickler") "* %i%? \n %U")
                                        ("j" "Journal" entry (file+datetree "~/Dropbox/org/gtd/diary.org") "* %?\n%U\n")
                                        ("h" "habit" entry (file "~/Dropbox/org/gtd/inbox.org")
                                         "* TODO %?\nSCHEDULED: %(format-time-string \"%<<%Y-%m-%d %a .+1d/3d>>\")\n:PROPERTIES:\n:STYLE: habit \n:END:\n%U\n")
                                        ("n" "Note" entry (file+headline "~/Dropbox/org/gtd/inbox.org" "Notes") "* %? :NOTE:\n%U\n"))))

4.  Remove scheduled headlines and deadlines from the agenda once they&rsquo;re marked done

        (after! org
          (setq org-agenda-skip-scheduled-if-done t)
          (setq org-agenda-skip-deadline-if-done t))

5.  Ignore scheduled headlines and deadlines in the global todo list

        (after! org
          (setq org-agenda-todo-ignore-scheduled t)
          (setq org-agenda-todo-ignore-deadlines t))

6.  Set org download method

    not sure what happened here lol, leaving for now.  

7.  Super Agenda

        (after! org-agenda
          (org-super-agenda-mode)
        
          (setq org-agenda-custom-commands
                '(("o" "Overview"
                   ((agenda "" ((org-agenda-span 'day)
                                (org-super-agenda-groups
                                 '((:name "Today"
                                          :time-grid t
                                          :date today
                                          :todo "TODAY"
                                          :scheduled today
                                          :order 1)))))
                    (alltodo "" ((org-agenda-overriding-header "")
                                 (org-super-agenda-groups
                                  '((:name "Todo"
                                           :todo "TODO"
                                           :order 1)
                                    (:name "Important"
                                           :tag "Important"
                                           :priority "A"
                                           :order 6)
                                    (:name "Emacs"
                                           :tag "Emacs"
                                           :order 13)
                                    (:name "Projects"
                                           :tag "Project"
                                           :order 14)
                                    (:name "To read"
                                           :tag "Read"
                                           :order 30)
                                    (:name "Waiting"
                                           :todo "WAITING"
                                           :order 20)
                                    (:name "Trivial"
                                           :priority<= "E"
                                           :tag ("Trivial" "Unimportant")
                                           :todo ("SOMEDAY" )
                                           :order 90)
                                    (:discard (:tag ("Chore" "Routine" "Daily"))))))))))))


<a id="org6aecbc5"></a>

## SQL mode config


<a id="org128d24f"></a>

### Set defaults

    (setq sql-postgres-login-params
          '((user :default "postgres")
            (password :default "postgres")
            (database :default "engine_dev")
            (server :default "0.0.0.0")))
    
    (setq sql-connection-alist
          '(("local" (sql-product 'postgres)
             (sql-database "engine_dev")
             (sql-user "postgres")
             (sql-server "0.0.0.0")
             (sql-password "postgres"))
            ("cars_NP" (sql-product 'postgres)
             (sql-database "engine_dev")
             (sql-user "postgres")
             (sql-server "")
             (sql-password ""))))


<a id="orgf187c79"></a>

## Org-books config

    (setq org-books-file "~/Dropbox/org/gtd/book-list.org")

