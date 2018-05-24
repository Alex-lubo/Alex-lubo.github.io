---
title: VIM简明教程
date: 2018-03-25 22:20:45
tags: linux
---

## Vim tutorial

vim is editor in linux. There are three mode for vim: normal ,insert and visual.
Here records the basic operator of this editor.

### normal: navigate and manipulate text
  
  - 'ESC' for switch to normal mode
  - navigate keys:
    
    - basic movement: h(left),j(down), k(up), l(right)
    - word movement: w(start of word), e(end of word), b(beginning of word), youcan also use number to powered the movement, eg:'3b' - move to the end of the third word 
    - insert text repeatedly: symtax: (num)i(text), eg: '3iYes' means insert 3 Yes to the text.
    - find the charactor in the line：f or F, eg:'fa' means find 'a' in next, '3fa' - find the third 'a' following.
    - find the matching bracket or parenthese '{', or '[' or '(', with '%'
    - '0' and '$' means go to the begin/end of a line
    - '*' to find the next occurence of the same word under cursor. '#' to find the previous one.
    - 'gg' takes you goto the beginning of the file and with 'G' to the end line . goto the specified line using '#{number}G', eg: 5G - jump to the 5th line.
    - text research: '/#{target}' to search target in text. 'n' and 'N' means next and previous respectively.
    - insert new line: 'o' and 'O' means insert new line after/before current line. After insert the mode will change to insert mode. 
    - delete a charactor with 'x' or 'X'
    - replace letter under cursor with 'r'
    - delete command: 'd', you can combined 'd' with movement. 'dw' delete the word on the under of the cursor. 'd#{num}e/b' means delete the num word before/after cursor.
    - repetition with '.' you can execute some command repeatly with '.'
    - recover with 'u' or 'ctrl+R'
    - import command to remember: ':w' - save; ':q' - quit; ':q!' - quit without save
    - search help with ':help' when using vim

### Visual 

 - 'v' - switch to visual mode
 - you can select text in visual mode and then decide what to do.
