<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Xptemplate](#xptemplate)
  - [Usage](#usage)
  - [Stay up-to-date](#stay-up-to-date)
  - [Screencast with 0.3.9](#screencast-with-039)
  - [Features](#features)
- [FAQ](#faq)
      - [Trouble Shooting. Why not work?](#trouble-shooting-why-not-work)
      - [What else does xptemlate required to work](#what-else-does-xptemlate-required-to-work)
      - [How to install](#how-to-install)
      - [How to reload snippets, after I changes snippet definition files(*.xpt.vim)?](#how-to-reload-snippets-after-i-changes-snippet-definition-filesxptvim)
      - [Do NOT like spaces in auto-completed brackets/braces](#do-not-like-spaces-in-auto-completed-bracketsbraces)
      - [I need spaces in brackets/braces only for this language, not that](#i-need-spaces-in-bracketsbraces-only-for-this-language-not-that)
      - [I do not want Xptemplate to auto-complete braces](#i-do-not-want-xptemplate-to-auto-complete-braces)
      - [Adjust space and line-break](#adjust-space-and-line-break)
      - [Adjust format style of c functions, such as `main()`](#adjust-format-style-of-c-functions-such-as-main)
      - [Supertab support](#supertab-support)
      - [With popup menu opened, `<TAB>` doesn't trigger Snippe](#with-popup-menu-opened-tab-doesnt-trigger-snippe)
      - [Set up personal info. Xptemplate complains: "author is not set .."](#set-up-personal-info-xptemplate-complains-author-is-not-set-)
      - [Browse snippets: Pop up menu, Drop down list](#browse-snippets-pop-up-menu-drop-down-list)
      - [Extend XPTemplate. Write new snippets](#extend-xptemplate-write-new-snippets)
- [Known Issues](#known-issues)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# Xptemplate

Code snippets engine for Vim, And snippets library.
Write codes in a smooth, quick and comfortable way.
[xptemplate-vim.org][xpt-vim]

<iframe src="http://ghbtns.com/github-btn.html?user=drmingdrmer&repo=xptemplate&type=watch&count=true"
  allowtransparency="true" frameborder="0" scrolling="0" width="110" height="20"></iframe>


## Usage

    >vim xpt.c
    for<C-\>

generates:

    for (i = 0; i < len; ++i){
        /* cursor */
    }

Press `<tab>`,`<tab>`.. to navigate through "i", "0", "len" and finally stop at "`/* cursor */`"


## Stay up-to-date

There are 2 major branches: "master" and "dist".

* "master" is for developing purpose and contains debug statements thus it is a bit slow.
* "dist" is for end user with debug statements removed and runs a little bit faster.


## Screencast with 0.3.9

C language:
<http://www.vimeo.com/7614329>

Python:
<http://www.vimeo.com/7868048>

Tutorial by Derek Wyatt :
<http://vimeo.com/4449258>


## Features

* Live update while typing.
* Navigate forward and backward with `<Tab>` and `<S-Tab>`.
* Support embedded language like JAVASCRIPT in HTML. Or HTML in PHP.
* Wrapper snippet.
* Inclusion.
* Generate dynamic content with variables/functions.
* code style customization: line break before "{".
* Filters.


# FAQ

#### Trouble Shooting. Why not work?

First of all, Enter VIM.

* Check setting "compatible". In VIM, type:

        :set compatible?

    Make sure you get *"nocompatible"*. Or you need the following statement in your .vimrc:

        set nocompatible

* Check setting "filetype". In VIM, type:

        :filetype

    Make sure you get *"filetype detection:ON  plugin:ON ..."*. Or you need the following statement in you .vimrc:

        filetype plugin  on


* Check if XPTemplate is loaded. In VIM, type:

        :scriptnames

    You will get a list of scripts VIM has loaded. There should be some files with name started with "xpt", like this:

        97: /data/.../dist/plugin/xptemplate.vim
        98: /data/.../dist/plugin/xptemplate.conf.vim
        99: /data/.../dist/plugin/xptemplate.util.vim

    If not, you should check if you have Xptemplate installed correctly.

    You should see files with name started with "xpt.." in folder ~/.vim/ (Unix-like OS) or in $VIMH/vimfiles/ (Windows). Like this:

        |-- plugin
        ...
        |   |-- xpopup.vim
        |   |-- xpreplace.vim
        |   |-- xptemplate.conf.vim
        ...

    Reference:

        :help 'runtimepath'

* Check key binding. Make sure you have a clear environment:
none of Xptemplate settings like *"let g:xpt..."* in you .vimrc file.
In VIM, type:

        :imap <C-\>

    You will get something like this:

        i  <C-\>       * <C-R>=XPTemplateStart(0,{'k':'<C-\++'})<CR>

    This means your Xptemplate should work well.
    Or check if some other plugin has conflicting key-binding.
    If so, solve it by adding the following statement into you .vimrc file to change Xptemplate key-binding:

        let g:xptemplate_key = '<Tab>'

* Now type in *insert-mode*:

        Date<C-\>

    You will get current date.


#### What else does xptemlate required to work

VIM and nothing else!

Xptemplate is implemented purely with VIM script.
Nothing else(like python) is required but a single VIM( at least 7.2 ).

Xptemplate works also in VIM 7.0 too. But it's no as good as in VIM 7.2.



#### How to install

Copy all folders in to your ~/.vim folder(on unix-like OS)

Or add path to Xptemplate to VIM setting 'runtimepath'.


#### How to reload snippets, after I changes snippet definition files(*.xpt.vim)?

    :XPTreload



#### Do NOT like spaces in auto-completed brackets/braces

Spaces inside auto-completed brackets are controlled by Xptemplate variable $SParg.

The Simplest way to disable spaces globally is to add following statements to .vimrc:

    let g:xptemplate_vars = "SParg="


With this setting, `"("` results in `"()"` but not `"(  )"` by default.


#### I need spaces in brackets/braces only for this language, not that

Assume you do need spaces in brackets for java( that you want `"()"` but not `"(  )"`).

Create a snippet file to defines your own preference:

    .vim/ftplugin/java/mine.xpt.vim

Add add following contents to this file:

    XPTemplate priority=personal
    XPTvar $SParg   ''


#### I do not want Xptemplate to auto-complete braces

I want Xptemplate to complete `(`, `[`, but not `{`.

Add this to .vimrc:

    let g:xptemplate_brace_complete = '(['


Or you may just want to disable brackets complete:

    let g:xptemplate_brace_complete = ''



#### Adjust space and line-break

Spaces and line-breaks are defined by some *variable*.
Instead of rewrite snippet in your own coding style, modifying these *variable* is the easiest way.

For example by default "for" loop snippet in C lang is like:

    for ( i = 0; i < len; i++ ) {
        /* cursor */
    }

As snippet "for" defined as:

    for`$SPcmd^(`$SParg^`$FOR_SCOPE^`$VAR_PRE`i^`$SPop^=`$SPop^`0^; `i^`$SPop^<`$SPop^`len^; `i^++`$SParg^)`$BRloop^{
        `cursor^
    }

If you want to remove space after "for" and create a line-break before "{":

    for( i = 0; i < len; i++ )
    {
        /* cursor */
    }

Just add two variables into your *.vimrc* file:

    let g:xptemplate_vars = "SPcmd=&BRloop=\n"


#### Adjust format style of c functions, such as `main()`

Default c function indent is like this:

        int
    main( int argc, char **argv )
    {
        return 0;
    }

This is controlled by variable `$BRfun`, if you prefer the single line style:

    int main( int argc, char **argv ) {
        return 0;
    }

Add variable into your *.vimrc* file, to make ***ALL*** function snippets to
use single line style:

    let g:xptemplate_vars = "BRfun= "

Or redefine `$BRfun` in higher priority snippet file to modify only c
functions:

    XPTemplate priority=personal
    XPTvar $BRfun          ' '

Add the above lines into `ftplugin/c/foobar.xpt.vim`.


#### Supertab support

You have also supertab installed? This is the way to try Xptemplate first and then supertab commpletionn( You have to be smart enough to figure out where to put these configure codes. But actually ~/.vimrc is a good place. ):

    " avoid key conflict
    let g:SuperTabMappingForward = '<Plug>supertabKey'

    " if nothing matched in xpt, try supertab
    let g:xptemplate_fallback = '<Plug>supertabKey'

    " xpt uses <Tab> as trigger key
    let g:xptemplate_key = '<Tab>'

    " " use <tab>/<S-tab> to navigate through pum. Optional
    " let g:xptemplate_pum_tab_nav = 1

    " " xpt triggers only when you typed whole name of a snippet. Optional
    " let g:xptemplate_minimal_prefix = 'full'



#### With popup menu opened, `<TAB>` doesn't trigger Snippe

By default if popup menu is opened and `<TAB>` is used as trigger key, a `<TAB>` key press does not  trigger a snippet, according to convention in many other application user use `<TAB>`.

If you always want to trigger snippet with `<TAB>` no matter popup menu opened or not, add these lines to your .vimrc:

    let g:xptemplate_key = '<Plug>triggerxpt'
    inoremap <Plug>closePUM <C-v><C-v><BS>
    imap <TAB> <Plug>closePUM<Plug>triggerxpt
    " let g:xptemplate_fallback = 'nore:<TAB>' " Optional. Use this only when you have no other plugin like SuperTab to handle <TAB>.

It first forces popup menu to close and then trigger snippet.

This fix issue that some plugin like AutoComplPop opens popup menu automatically.




#### Set up personal info. Xptemplate complains: "author is not set .."

You can set up your name and email for just one time, and use them everywhere, like that in a document comment block the name of the author(you) will be filled in.

Set up variable $author and $email in your .vimrc:

    let g:xptemplate_vars = "author=your_name&email=drdr.xp@gmail.com&..."


Thus "filehead" snippet of C language should result in:

    ...
     * @author : your_name | drdr.xp@gmail.com
    ...


#### Browse snippets: Pop up menu, Drop down list

By default, to list snippets whose name starts with "i", Press:

    i<C-r><C-\>

See also:

    :help g:xptemplate_key_force_pum


#### Extend XPTemplate. Write new snippets

Do *NOT* modify snippets supplied by XPTemplate.

***Where***:

Add your own snippets into folder **personal** :

    <path_to_xpt>/personal/ftplugins/


It is meant for user-defined snippets.
It shares the same directory-structure with the xpt-snippet-folder.

Example personal-snippet-folder:

    |~personal/
    | |~ftplugin/
    | | |+_common/
    | | `~c/
    | |   |-c_ext.xpt.vim
    | |   `-c_new.xpt.vim
    ...


***How***:

***NOTE: File names does not matter except it must ends with '.xpt.vim'.***

* To create new snippet: Create *c_new.xpt.vim*. Add new snippets in it.

* To override existing ones, Create *c_ext.xpte.vim* with *higher priority*.
Put modified snippets into this file.
For example an extended "*for*" snippet looks like:

        XPTemplate priority=lang-1
        XPTinclude
              \ _common/common

        Xptemplate for " tips
        for (
            `i^ = 0;
            `i^ < `len^;
            ++`i^
        )


The header line declares a higher priority than priority "*lang*"( lower number means higher priority ):

    XPTemplate priority=lang-1

All snippets in this file override snippets in *xpt-snippet-folder/ftplugin/c/c.xpt.vim* which declares with priority "*lang*".

Except that this file is with higher **priority**, **Personal-snippet-folder** has no differences from **xpt-snippet-folder**.

***If you use GIT to sync vim plugins and your own snippets,
it's a good idea to place your snippet GIT folder somewhere outside Xptemplate
folder and add snippet folder path to `runtimepath`.***

References:

    :help xpt-write-snippet
    :help xpt-snippet-priority


# Known Issues

* Before VIM 7.4, during applying snippet, key-mapping saving/restoring does not support `<expr>` mapping well.
[#43](../../issues/43)  
Solution: upgrade to VIM 7.4


[xpt-github]: https://github.com/drmingdrmer/xptemplate
[xpt-vim]: http://www.vim.org/scripts/script.php?script_id=2611