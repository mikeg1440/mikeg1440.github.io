---
layout: post
title: Customizing Vim for Coding
date: 2020-04-12 20:10 -0400
---
![Vim logo](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Icon-Vim.svg/1200px-Icon-Vim.svg.png)

If your a Linux user then your most likely aware of the text editor called [Vim](https://www.vim.org/about.php).  If not it's because these days programs and applications usually come with a graphical user interfaces or GUI, but before GUI's there was only the Command Line Interface or CLI where you just a black terminal and a cursor.  

![cowsay CLI apps are awesome!](https://cliapp.store/images/terminal_screenshot.png)

Some time during 1976 a student at the University of California Berkeley developed [VI](https://en.wikipedia.org/wiki/Vi) or "Visual Instrument" to assist in the viewing and editing of text files from the CLI.  Eventually VI was expanded upon and Vim, originally "VI IMitation" and later "VI IMproved" was developed to match and surpass the features provided in VI.  To this day power users out there are still using VI and Vim for everyday tasks including coding.  VI and Vim are installed by default on almost every major Linux distributions out there today and the usage doesn't change across platforms and distributions making it a perfect editor to get familiar with because you can almost always bet that running `vi` or `vim` on the command line is going to drop you into a text editor.  

Allot of people, myself included don't initially see the benefit of using a command line based editor because it initially seems less intuitive to use when you have your choice of so many great text editors with GUI's out there like VS Code, Atom, Sublime and Eclipse.  But there are benefits of why you would want to use Vim as a editor like it being much lighter on system resources than non CLI editors, quick to use once you know the commands and shortcuts, has portable configs, is well documented, enjoys vibrant community support and you can customize and expand the basic install of Vim to get you pretty damn close to in terms of features to all those newer editors out there, it just takes a little know how.  

![screenshot of basic Vim window](https://i.imgur.com/L0p62XK.png)

So in this post I'm going to go through how to customize your installation of Vim to make it into a code editing machine with all sorts of features that you may not have known possible to configure with a CLI text editor and that pretty makes Vim into a IDE.  Since I'm currently on Ubuntu the examples shown here will be for use on a Ubuntu machine but most of the setup is more or less the same on any Linux distro.  

![Vim screenshot after following this post](https://i.imgur.com/LjXoX5a.png)

## The Config File

We are going to start setting up our Vim IDE by creating a `.vimrc` file in the current users home directory which will hold configuration settings for the current user that will parsed when Vim starts.  Now you can go ahead and customize your own `.vimrc` file to your hearts content, adding which ever features you find most useful but for this post we will be using a config file that has been created already for us.  

Either copy the config data from [this link](https://gist.githubusercontent.com/simonista/8703722/raw/d08f2b4dc10452b97d3ca15386e9eed457a53c61/.vimrc), run `Vim ~/.vimrc` then paste the text into the file and save it.

Or if you have curl your can run

`curl https://gist.githubusercontent.com/simonista/8703722/raw/d08f2b4dc10452b97d3ca15386e9eed457a53c61/.vimrc > ~/.vimrc`

Or with wget

`wget https://gist.githubusercontent.com/simonista/8703722/raw/d08f2b4dc10452b97d3ca15386e9eed457a53c61/.vimrc -O ~/.vimrc`   

This will enable a number of features like
  - Turn on syntax highlighting
  - Show line numbers
  - Show file stats
  - Blink cursor on error
  - Visualize tabs and newline charecters
  - A few others that make editing code easier

## Add Color Schemes

Next we will install some color schemes for Vim to use with syntax highlighting.  You should know that you can configure these in any way you want but we will be using a pre compiled set of color schemes for simplicity sake.  

Steps
  - First create a new directory in the home folder for the color scheme files with `mkdir ~/.Vim`
  - Run `git clone https://github.com/flazz/Vim-colorschemes.git ~/.Vim` to clone the scheme file to the new directory
  - Add the color scheme you want to use to your `~/.vimrc` file with `colorscheme <NAME OF COLOR SCHEME>` so for this example we will use molokai and we add `colorscheme molokai` to the config file

Now if you open up a file in Vim you should notice a different color scheme than what was set before.

## Install a Plugin Manager

What makes any text editor ultimately more useful is customization and Vim plugin managers give us the ability the use plugins to expand the existing functionality and lucky for us there are [plenty of plugins available](https://Vimawesome.com/) to choose from.  There is more than plugin manager out there for Vim but I tend to like  Vundle so that is what we will be installing here.  First clone the Vundle repo with `git clone https://github.com/VundleVim/Vundle.Vim.git ~/.Vim/bundle/Vundle.Vim`.  Next we need to add this code to the top of our `.vimrc` file
```
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.Vim/bundle/Vundle.Vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.Vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/Vim-fugitive'
" plugin from http://Vim-scripts.org/Vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
" Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup Vim script is in a subdirectory of this repo called Vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'Vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```

Lastly run `Vim +PluginInstall +qall` or launch Vim and run `:PluginInstall` to install Vundle


## Plugins

I have found a number of great plugin on the internet and if you want you can [look up](https://Vimawesome.com/) the ones that would best suite your needs.  Or check out this list of plugins
```
" let Vundle manage Vundle, required
Plugin 'gmarik/Vundle.Vim'

" Utility
Plugin 'scrooloose/nerdtree'
Plugin 'majutsushi/tagbar'
Plugin 'ervandew/supertab'
Plugin 'BufOnly.Vim'
Plugin 'wesQ3/Vim-windowswap'
Plugin 'SirVer/ultisnips'
Plugin 'junegunn/fzf.Vim'
Plugin 'junegunn/fzf'
Plugin 'godlygeek/tabular'
Plugin 'ctrlpVim/ctrlp.Vim'
Plugin 'benmills/Vimux'
Plugin 'jeetsukumaran/Vim-buffergator'
Plugin 'gilsondev/searchtasks.Vim'
Plugin 'Shougo/neocomplete.Vim'
Plugin 'tpope/Vim-dispatch'

" Generic Programming Support
Plugin 'jakedouglas/exuberant-ctags'
Plugin 'honza/Vim-snippets'
Plugin 'Townk/Vim-autoclose'
Plugin 'tomtom/tcomment_Vim'
Plugin 'tobyS/vmustache'
Plugin 'janko-m/Vim-test'
Plugin 'maksimr/Vim-jsbeautify'
Plugin 'Vim-syntastic/syntastic'
Plugin 'neomake/neomake'

" Markdown / Writting
Plugin 'reedes/Vim-pencil'
Plugin 'tpope/Vim-markdown'
Plugin 'jtratner/Vim-flavored-markdown'
Plugin 'LanguageTool'

" Git Support
Plugin 'kablamo/Vim-git-log'
Plugin 'gregsexton/gitv'
Plugin 'tpope/Vim-fugitive'
"Plugin 'jaxbot/github-issues.Vim'

" PHP Support
Plugin 'phpVim/phpcd.Vim'
Plugin 'tobyS/pdv'

" Erlang Support
Plugin 'Vim-erlang/Vim-erlang-tags'
Plugin 'Vim-erlang/Vim-erlang-runtime'
Plugin 'Vim-erlang/Vim-erlang-omnicomplete'
Plugin 'Vim-erlang/Vim-erlang-compiler'

" Elixir Support
Plugin 'elixir-lang/Vim-elixir'
Plugin 'avdgaag/Vim-phoenix'
Plugin 'mmorearty/elixir-ctags'
Plugin 'mattreduce/Vim-mix'
Plugin 'BjRo/Vim-extest'
Plugin 'frost/Vim-eh-docs'
Plugin 'slashmili/alchemist.Vim'
Plugin 'tpope/Vim-endwise'
Plugin 'jadercorrea/elixir_generator.Vim'

" Elm Support
Plugin 'lambdatoast/elm.Vim'

" Theme / Interface
Plugin 'AnsiEsc.Vim'
Plugin 'ryanoasis/Vim-devicons'
Plugin 'Vim-airline/Vim-airline'
Plugin 'Vim-airline/Vim-airline-themes'
Plugin 'sjl/badwolf'
Plugin 'tomasr/molokai'
Plugin 'morhetz/gruvbox'
Plugin 'zenorocha/dracula-theme', {'rtp': 'Vim/'}
Plugin 'junegunn/limelight.Vim'
Plugin 'mkarmona/colorsbox'
Plugin 'romainl/Apprentice'
Plugin 'Lokaltog/Vim-distinguished'
Plugin 'chriskempson/base16-Vim'
Plugin 'w0ng/Vim-hybrid'
Plugin 'AlessandroYorba/Sierra'
Plugin 'daylerees/colour-schemes'
Plugin 'effkay/argonaut.Vim'
Plugin 'ajh17/Spacegray.Vim'
Plugin 'atelierbram/Base2Tone-Vim'
Plugin 'colepeters/spacemacs-theme.Vim'
```

To add new plugins just add `Plugin '<Vim Plugin Name>'` to the config file so if you wanted to install the plugin NerdTree then you would add `Plugin 'scrooloose/nerdtree'` to your `.vimrc` file.  

Among serious developers you may hear something like there are only two editors worth mentioning, emacs and vi and there are some strong feelings out there in the way of each as well as every other popular text editor but it really comes down to what helps you be the most productive and works best for you.  This is just a high level look at a simple Vim setup and you can go as far as coding your own plugins and creating totally customized configs and color schemes.  And if you have not given Vim a try yet then you really should just buckle down and so if you can stick with it for a week or so, you may find that you'll wonder why you ever bothered with a mouse!
