---
title: vim代码索引与补全
date: 2019-10-16 09:52:05
tags: tech
---
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
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'
Plugin 'nerdtree'
Plugin 'godlygeek/tabular'
Plugin 'plasticboy/vim-markdown'
Plugin 'leafgarland/typescript-vim'
Plugin 'roxma/vim-hug-neovim-rpc'
Plugin 'roxma/nvim-yarp'
Plugin 'autozimu/LanguageClient-neovim'
Plugin 'Shougo/deoplete.nvim'
Plugin 'Shougo/neosnippet.vim'
Plugin 'Shougo/neosnippet-snippets'
Plugin 'altercation/vim-colors-solarized'
" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.

" All of your Plugins must be added before the following line
call vundle#end() " required
filetype plugin indent on " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
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
let g:LanguageClient_settingsPath = '/Users/xxx/.vim/.config.settings.json'
let g:LanguageClient_autoStart = 1
let g:LanguageClient_serverCommands = {
    \ 'javascript': ['javascript-typescript-stdio'],
    \ 'c': ['/usr/local/bin/ccls', '--log-file=/tmp/cc.log'],
    \ 'cpp': ['/usr/local/bin/ccls', '--log-file=/tmp/cc.log'],
    \ 'objc': ['/usr/local/bin/ccls', '--log-file=/tmp/cc.log'],
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
if executable('ccls')
   au User lsp_setup call lsp#register_server({
      \ 'name': 'ccls',
      \ 'cmd': {server_info->['ccls']},
      \ 'root_uri': {server_info->lsp#utils#path_to_uri(lsp#utils#find_nearest_parent_file_directory(lsp#utils#get_buffer_path(), 'compile_commands.json'))},
      \ 'initialization_options': {'cache': {'directory': '/tmp/ccls/cache' }},
      \ 'whitelist': ['c', 'cpp', 'objc', 'objcpp', 'cc'],
      \ })
endif


call deoplete#custom#option('sources', {
            \ 'vim': ['vim'],
            \ 'zsh': ['zsh']
            \})

let g:deoplete#ignore_sources = {}
let g:deoplete#ignore_sources._ = ['buffer', 'around']


" setup deoplete
let g:deoplete#enable_at_startup = 1


"set background=dark
"colorscheme solarized
set ts=4
set expandtab
set hlsearch
set backspace=indent,eol,start
nmap <C-n> :NERDTreeToggle <CR>
nmap <C-l> :tabn <CR>
nmap <C-h> :tabp <CR>
" Settings for cscope:
nmap ,c :cs find c <C-R>=expand("<cword>")<CR><CR>
nmap ,s :cs find s <C-R>=expand("<cword>")<CR><CR>
nmap ,g :cs find g <C-R>=expand("<cword>")<CR><CR>
nmap ,t :cs find t <C-R>=expand("<cword>")<CR><CR>
nmap ,t :cs find e <C-R>=expand("<cword>")<CR><CR>
nmap ,f :cs find f <C-R>=expand("<cword>")<CR><CR>
nmap ,i :cs find c <C-R>^=expand("<cword>")$<CR><CR>
nmap ,d :cs find d <C-R>=expand("<cword>")<CR><CR>

" LanguageClient-nvim key bindings
nmap ;d :call LanguageClient#textDocument_definition()<cr>
nmap ;r :call LanguageClient#textDocument_references({'includeDeclaration': v:false})<cr>
nmap ;h :call LanguageClient#textDocument_hover()<cr>
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
