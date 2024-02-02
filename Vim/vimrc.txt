""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" vimrc configuration
" last updated 14th Jan, 2020
"inspired by amix - basic vim on GitHub and Mastering Vim Quickly
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"Sections
" => General settings
" => vim user interface
" => Colors and fonts
" => Swap and backup options
" => Search options
" => Indentation options
" => Text rendering options
" => Miscellaneous options
" => netrw file explorer settings
" => ctags related

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => General settings
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"Use vim settings instead of vi
set nocompatible

"Set how many lines of history vim has to remember
set history=500

"Set to auto read when a file is changed from outside
set autoread

"Ignore case when searching
set ignorecase

"Display a confirmation dialog when closing an unsaved file
set confirm

"Enable spell-check
"set spell

"Spell check language
"set spelllang=en

"Manage buffers effectively
set hidden

"Disable beeps on error
set noerrorbells

"Flash the screen instead of beeping
"set visualbell
"Configure backspace over indentation, line breaks and insertion
set backspace=indent,eol,start

"Don't redraw while executing macros (good performance config)
set lazyredraw

"For regular expressions turn magic on
set magic

"Show matching brackets when text indicator is over them
set showmatch

"How many tenths of a second to blink when matching brackets
set mat=2

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => vim user interface
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"Set window title, reflecting the current file being edited
set title

"Always display the status bar
set laststatus=2

"Set line number
set number

"Show cursor position
set ruler

"Show current mode at the bottom
set showmode
set showcmd

"Mark the line the cursor is currently in
set cursorline

"Mark the column the cursor is currently in
"set cursorcolumn

"Enable mouse for scrolling and resizing
set mouse=a

"Number of lines to keep above and below cursor
set scrolloff=3

"Number of lines to keep to the left and right of the cursor
set sidescrolloff=5

"Display command line's tab complete options as menu
set wildmenu

"Maximum number of tabpages which can be opened
set tabpagemax=27

"vim status line
set statusline=%F%m%r%h%w%=(%{&ff}/%Y)\ (line\ %l\/%L,\ col\ %c)

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => Colors and Fonts
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"Choose color scheme
colorscheme desert

"Enable syntax highlighting
syntax enable

"Choose a darkbackground
set background=dark

"Set gui font size
set guifont=Monospace\ 14

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => Swap and backup options - disable all
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
set nobackup
set noswapfile
set nowb

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => Search options
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"Find the next match as we type the search
set incsearch

"Highlight searches by default
set hlsearch

"Ignore case when searching
set ignorecase

"A pattern is case sensitive only if it contains an uppercase letter
set smartcase

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => Indentation options
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"New lines inherit the indentation of the previous lines
set autoindent

"Smart auto indentation (instead of old smartindent option).
filetype plugin indent on

"Use spaces instead of tabs and use 2 spaces for tab (change to 4 if desired)
set expandtab
set shiftwidth=2
set tabstop=2
set softtabstop=2

"Do not wrap lines
" set nowrap

" Wrap lines
set wrap

"Wrap lines at convenient points, avoid wrapping a line in the middle of a word
set linebreak

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => Text rendering options
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"Use an encoding which supports unicode
set encoding=utf-8

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => netrw settings
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"hide the banner on top of the netrw explorer
let g:netrw_banner=0

" hide hidden files or .files on vim startup
" let ghregex='\(^\|\s\s\)\zs\.\S\+'
" let g:netrw_list_hide=ghregex

"list directories and files in tree like structure
let g:netrw_liststyle=3

"to always open files in split window on pressing 'ENTER' rather than in same window
let g:netrw_browse_split=4

"always open files for editing with vertical split on the right
let g:netrw_altv=1

"always open files for preview with vertical split on the right
let g:netrw_preview=1

"adjust window size
let g:netrw_winsize=25

"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" => ctags related settings
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

"tags file is generated by running 'ctags -R *' or 'ctags -R .' in the src directory
"set tags=<path to ctags file>
