---
title: vim代码索引与补全
date: 2019-10-16 09:52:05
tags: TECH
---

[TOC]
<!-- vim-markdown-toc GFM -->

* [目标](#目标)
* [需要的插件](#需要的插件)
* [language server](#language-server)
* [步骤](#步骤)
        * [1. 安装Vundle: git repo: https://github.com/VundleVim/Vundle.](#1-安装vundle-git-repo-httpsgithubcomvundlevimvundle)
        * [2. .vimrc](#2-vimrc)
        * [3. 装插件](#3-装插件)
        * [4. 使用](#4-使用)

<!-- vim-markdown-toc -->

## 目标
实现C/C++/Objective-C, js/ts的代码索引、自动补全。
使用LSP。YCM、Ctags之类的软件太古老了。
关于LSP: Language Server Protocol.

## 需要的插件
> Vundle.vim : 插件管理
> nerdtree
> roxma/vim-hug-neovim-rpc: 为vim8补全一些特性，像neoVim看齐。
> roxma/nvim-yarp: 同上
> autozimu/LanguageClient-neovim: neoVim的language client插件。vim8下需要上面两个插件支持
> Shougo/deoplete.nvim: neoVim的代码补全插件
> Shougo/neosnippet.vim: code snippet插件
> Shougo/neosnippet-snippets: 同上
> 

## language server
> C/C++:  [ccls](https://github.com/MaskRay/ccls)
  - install on macOS: brew install ccls
  - js/ts: [javascript-typescript-langserer](https://github.com/sourcegraph/javascript-typescript-langserver)
    - install on macOS: npm install -g javascript-typescript-langserver

## 步骤

#### 1. 安装Vundle: git repo: https://github.com/VundleVim/Vundle.
```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
    
编辑.vimrc如下

#### 2. .vimrc
里面加了一些私货，请酌情参考
```
syntax on
map <C-n> :NERDTreeToggle <CR>
set nocompatible " be iMproved, required
filetype off " required
set encoding=utf-8

" set the runtime path to include Vundle and initialize
set rtp+=~/.config/nvim/bundle/Vundle.vim
call vundle#begin()            " required
Plugin 'VundleVim/Vundle.vim'  " required
Plugin 'git://github.com/scrooloose/nerdtree.git'
Plugin 'https://github.com/Shougo/deoplete.nvim.git'
Plugin 'https://github.com/autozimu/LanguageClient-neovim.git'
Plugin 'git@github.com:vim-scripts/taglist.vim.git'
Plugin 'scrooloose/syntastic'
" ===================
" my plugins here
" ===================

" Plugin 'dracula/vim'


" ===================
" end of plugins
" ===================
call vundle#end()               " required
filetype plugin indent on       " required

" Brief help
" :PluginList - lists configured plugins
" :PluginInstall - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
"
"
let g:python3_host_prog = "/usr/local/bin/python3"
let g:python2_host_prog = "/usr/local/bin/python2"

" setup javascript/typescript language server
let g:LanguageClient_loadSettings = 1
let g:LanguageClient_settingsPath = '/Users/zivyou/.config/nvim/settings.json'
let g:LanguageClient_autoStart = 1



let g:LanguageClient_serverCommands = {
    \ 'javascript': ['javascript-typescript-stdio'],
   \ }

if executable('javascript-typescript-stdio')
  let g:LanguageClient_serverCommands.javascript = ['javascript-typescript-stdio']
  " Use LanguageServer for omnifunc completion
  autocmd FileType javascript setlocal omnifunc=LanguageClient#complete
  
  "let g:LanguageClient_serverCommands.typescript = ['javascript-typescript-stdio']
  "autocmd FileType typescript setlocal omnifunc=LanguageClient#complete

else
  echo "javascript-typescript-stdio not installed!\n"
  :cq
endif

" Register ccls C++ lanuage server.
""if executable('ccls')
""   au User lsp_setup call lsp#register_server({
""      \ 'name': 'ccls',
""      \ 'cmd': {server_info->['ccls']},
""      \ 'root_uri': {server_info->lsp#utils#path_to_uri(lsp#utils#find_nearest_parent_file_directory(lsp#utils#get_buffer_path(), 'compile_commands.json'))},
""      \ 'initialization_options': {'cache': {'directory': '/tmp/ccls/cache' }},
""      \ 'whitelist': ['c', 'cpp', 'objc', 'objcpp', 'cc'],
""      \ })
""endif


call deoplete#custom#option('sources', {
            \ 'vim': ['vim'],
            \ 'zsh': ['zsh']
            \})


" setup deoplete
let g:deoplete#enable_at_startup = 1


" setup eslint"
let g:syntastic_javascript_checkers = ['eslint']


nnoremap <C-n> :NERDTreeToggle <CR>
nnoremap <C-l> :tabn <CR>
nnoremap <C-h> :tabp <CR>
nnoremap <F8> :TlistToggle <CR>
nnoremap <C-f> :!find . -type f -name "*.h" -o -name "*.cpp" -o -name "*.c" -o -name "*.cc" -o -name "*.hpp" \| xargs grep -r <cword>

" 生成cscope的索引
nnoremap <C-c-t> :!find . -name "*.c" -o -name "*.cpp" -o -name "*.h" -o -name "*.cc" -o -name "*.hpp" > FileList.txt; cat FileList.txt \| xargs ctags -a ; cscope -bkq -i FileList.txt;
if has("cscope")
  set csto=0
  set nocsverb
  if filereadable("cscope.out")
    cscope add cscope.out
  endif
  set csverb
endif

" Settings for cscope:
nnoremap ,c :cs find c <C-R>=expand("<cword>")<CR><CR>
nnoremap ,s :cs find s <C-R>=expand("<cword>")<CR><CR>
nnoremap ,g :cs find g <C-R>=expand("<cword>")<CR><CR>
nnoremap ,t :cs find t <C-R>=expand("<cword>")<CR><CR>
nnoremap ,t :cs find e <C-R>=expand("<cword>")<CR><CR>
nnoremap ,f :cs find f <C-R>=expand("<cword>")<CR><CR>
nnoremap ,i :cs find c <C-R>^=expand("<cword>")$<CR><CR>
nnoremap ,d :cs find d <C-R>=expand("<cword>")<CR><CR>
nnoremap <C-r> :!
nnoremap <C-c-s> :cs find 

" LanguageClient-nvim key bindings
nnoremap ;d :call LanguageClient#textDocument_definition()<cr>
nnoremap ;r :call LanguageClient#textDocument_references({'includeDeclaration': v:false})<cr>
nnoremap ;h :call LanguageClient#textDocument_hover()<cr>
" caller
nnoremap ;c :call LanguageClient#findLocations({'method':'$ccls/call'})<cr>

set showmatch
color desert
"set background=dark
"colorscheme solarized
set expandtab
set ts=2
set shiftwidth=2
set hlsearch
set backspace=indent,eol,start
inoremap ' ''<ESC>i
inoremap " ""<ESC>i
inoremap ( ()<ESC>i
inoremap [ []<ESC>i
inoremap { {}<ESC>i
inoremap <C-b> <left>
inoremap <C-n> <down>
inoremap <C-p> <up>
inoremap <C-f> <right>
inoremap <C-e> <end>
inoremap <C-a> <home>
```

#### 3. 装插件
    - :PluginInstall

#### 4. 使用
    - ccls
    ccls在做代码索引的时候，需要针对不同的项目做不同的预处理。主要是生成JSON Compliation Database.
    ```
    cmake -H. -BDebug -DCMAKE_BUILD_TYPE=Debug -DCMAKE_EXPORT_COMPILE_COMMANDS=YES
    ln -s Debug/compile_commands.json
    ```
