+++
date = "2018-11-07"
title = "Keep Your Core Strong - Know Your BSD/Linux Tools"
slug = "keep_your_core_strong_know_your_bsd_linux_tools"
tags = ["OpenBSD", "git", "vim", "tmux"]
categories = []
+++

Just as the sage advice from [The Pragmatic Programmer](https://pragprog.com/book/tpp/the-pragmatic-programmer) on mastering one text editor makes so much sense for those of us whose majority of work is rearranging bits on the digital titanic, the same holds true for knowing your operating system and investing in solid tooling for your other main productive tasks.

### Opinionated preface - Open source or GTFO

The world's resources are quickly depleting. A growing population having to work sh!tty jobs for sh!tty companies selling sh!tty stuff is not sustainable and kinda hard to keep respecting yourself as a highly paid developer building yet another duplicated piece of software on VC funds. 

If the aim is to advance humanity and avoid depleting all the resources as population explodes, then we should be looking to re-use and not create uneccessarily. Open source software that's been continually refined since the 60's/70's is continually being optimized and is able to run well on older hardware. 

We don't need another One Laptop Per Child venture creating future ewaste when we already have an abundance of capable hardware. Let's stop making batteries full of costly/harful chemicals and get more solar to continue running old laptops after their batteries have died.

### On with the show - the core utilities I invest in learning well

This page serves a purpose to me as storing minimal configuration notes. I strive to not become dependent on too much customisation for the reasons:

 - portability: the benefit of these tools are their ubiquitousness, I want the ability to grab any available device and quickly be productive without a day of setup or requiring of internet to pull down convoluted dotfiles libraries
 - learn the tool well: you may be working around the original design of the software. If it's really not there, consider contributing it to the project, or if it doesn't belong there, look for another modular program to handle that responsibility

#### OpenBSD

Slow is smooth and smooth is fast. Forgoing the bells, whistles and kitchen sink included in most Linux distros, OpenBSD has been planned with security and quality in mind. You're not forced to read the docs, but you'll soon realize it's the most efficient path forward. 

Amazing support for older architectures and a great welcoming community.

### Vim

My minimal .vimrc

```vimscript
set ts=2 sw=2 expandtab ruler nu noswapfile colorcolumn=80                      
syntax on                                                                       
colorscheme delek                                                               
                                                                                
autocmd Filetype php setlocal expandtab tabstop=4 shiftwidth=4 softtabstop=4       
autocmd Filetype js setlocal expandtab tabstop=2 shiftwidth=2 softtabstop=2        
                                                                                
highlight ExtraWhitespace ctermbg=red guibg=red                                 
match ExtraWhitespace /\s\+$/   

" newly acquired skills
set wildmenu " show search reuslts and such as popups
set path+=** " set path to this folder and all subdirs, ie for searching

```

### git

My minimal .gitconfig

```

```

### tmux

My minimal .tmux.conf

```
# moving between panes
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# large history limit
set -g history-limit 999999999
```

### coreutils

Refer to the excellent man pages provided by OpenBSD or take a chance with the docs from whatever other OS you're using.

My minimal shell profile (bash by default)

```
# enable viewing media offline
```


[back](/)
