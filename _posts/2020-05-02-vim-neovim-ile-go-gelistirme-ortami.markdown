---
layout: post
title: "Vim/Neovim ile Go Geliştirme Ortamı"
date: 2020-05-02
categories: genel
tags: [vim, neovim, vim-go, golang]
---

Bu yazıda VIM ile Go programlama dili için gerekli kurulumları nasıl yaparız onu anlatacağım.

### Vim
![Vim](/assets/img/vimlogo.gif)  

Vim Vi Improved anlamına gelir. UNIX ile hayatımıza giren Vi editörünün gelişmiş halidir. Daha detaylı bilgi için aşağıdaki linklere bakabilirsiniz

[vim resmi sitesi](https://www.vim.org/)  
[vim github](https://github.com/vim/vim)

### Neovim
![Neovim](/assets/img/neovim-logo.png)

Neovim bir Vim forkudur. Çoğu vim eklentisi ile uyumludur. Daha detaylı bilgi için aşağıdaki linklere bakabilirsiniz.  

[neovim resmi sitesi](https://neovim.io/)  
[neovim github](https://github.com/neovim/neovim)

#### Neovim Kurulumu
**macOS / OS X**

{% highlight bash %}
curl -LO https://github.com/neovim/neovim/releases/download/nightly/nvim-macos.tar.gz
tar xzf nvim-macos.tar.gz
./nvim-osx64/bin/nvim
{% endhighlight %}
Homebrew 

{% highlight bash %}
brew install neovim
{% endhighlight %}
Macports

{% highlight bash %}
sudo port selfupdate
{% endhighlight %}

**Linux**

Arch Linux

{% highlight bash %}
sudo pacman -S neovim
{% endhighlight %}

Python modülleri için

{% highlight bash %}
sudo pacman -S python-pynvim
{% endhighlight %}
Debian

{% highlight bash %}
sudo apt-get install neovim
{% endhighlight %}

Python modülleri için

{% highlight bash %}
sudo apt-get install python-neovim
sudo apt-get install python3-neovim

{% endhighlight %}

Fedora

{% highlight bash %}
sudo dnf install -y neovim python3-neovim

{% endhighlight %}
Gentoo Linux

{% highlight bash %}
emerge -a app-editors/neovim
{% endhighlight %}  

  
### Eklenti Yöneticisi 
Ben eklenti yöneticisi olarak vim-plug kullanıyorum. Tercihinize göre bir plugin yöneticisi kullanabilirsiniz.
Alternatif eklenti yöneticileri;
* [Vundle](https://github.com/VundleVim/Vundle.vim)
* [Pathogen](https://github.com/tpope/vim-pathogen)  

Sisteminize vim-plug yüklemek için aşağıdaki komutu çalıştırabilirsiniz

**VIM**

_Unix_
{% highlight bash %}
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
 https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
{% endhighlight %}

_Windows (Power Shell)_
{% highlight bash %}
md ~\vimfiles\autoload
$uri = 'https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
(New-Object Net.WebClient).DownloadFile(
  $uri,
  $ExecutionContext.SessionState.Path.GetUnresolvedProviderPathFromPSPath(
    "~\vimfiles\autoload\plug.vim"
  )
)
{% endhighlight %}

**Neovim**

_Unix_
{% highlight bash %}
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
{% endhighlight %}

_Windows (Power Shell)_
{% highlight bash %}
md ~\AppData\Local\nvim\autoload
$uri = 'https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
(New-Object Net.WebClient).DownloadFile(
  $uri,
  $ExecutionContext.SessionState.Path.GetUnresolvedProviderPathFromPSPath(
    "~\AppData\Local\nvim\autoload\plug.vim"
  )
)
{% endhighlight %}



Daha deteylı bilgi için [vim-plug](https://github.com/junegunn/vim-plug)

Pluhing yöneticimizi kurduğumuza göre artık ekletileri kurmaya başlayabiliriz.

Öncelikle kuracağımız eklentilerden kısaca bahsedelim

- [vim-go](https://github.com/fatih/vim-go/) adından da anlaşılacağı gibi Vim/Neovim için Go eklentisi 
- [coc-vim](https://github.com/neoclide/coc.nvim) VIM/Neovim için intellisense aracı
- [NerdTree](https://github.com/preservim/nerdtree) VIM içinde çalışan bir dosya gezgini (opsiyonel)
- [vim-airline](https://github.com/vim-airline/vim-airline) VIM için kullanışlı bir status bar (opsiyonel)
- [vim-airline-themes](https://github.com/vim-airline/vim-airline-themes) Airline status bar için temalar (opsiyonel)
- [PaparCololor Theme](https://github.com/NLKNguyen/papercolor-theme) Vim/Neovim için PaperColor teması (opsiyonel)  

#### Ayarlar
Vim ayarları için kullanacağı dosyayı home dizini altında .vimrc dosyasında tutar
Neovim ise home dizini altında .config/nvim/init.vim dosyasında tutar

Vim kullanıyorsanız 
**~/.vimrc**  
Neovim kullanıyorsanız
**~/.config/nvim/init.vim**

dosyasını açın ve aşağıdaki satırları ekleyin

{% highlight bash %}
call plug#begin('~/.vim/plugged')
Plug 'fatih/vim-go'
Plug 'neoclide/coc.nvim', {'branch': 'release'}
Plug 'preservim/nerdtree'
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
Plug 'NLKNguyen/papercolor-theme'
call plug#end()
{% endhighlight %}

dosyayı kaydedeyip VIM/Neovim'den çıkmadan **:PlugInstall** komutu ile eklentilerimizi kuralım  

Eklentilerimi kurduk. Şimdi sırada kurmuş olduğumuz eklentilerin ayarlarını yapalım

**:GoInstallBinaries** komutunu ile vim-go eklentisinin ihtiyaç duyduğu binarylerin kurulmasını sağlayalım

coc eklentisinin doğru bir şekilde kurulup kurulmadığını anlamak için **:CocInfo** komutunu çalıştırabiliriz.  
Eklentinin go ile uyumlu şekilde çalışabilmesi için language server ayarlarını yapmamız lazım. Bunun için **:CocConfig** komutunu çalıştıralım ve açılan dosyaya aşağıdakileri yazıp kaydedelim

{% highlight json %}
{
    "languageserver": {
        "golang": {
        "command": "gopls",
        "rootPatterns": ["go.mod", ".vim/", ".git/", ".hg/"],
        "filetypes": ["go"]
        }
    }
}
{% endhighlight %}

Şimdi Vim/Neovim için bazı ayarlar ve kısayollar tanımlayalım

{% highlight bash %}

set number "Satır numaralarnı aktif eder
set cursorline "İmlecin olduğu satırı highlight eder
set splitright "Ekranı yatay olarak bölmek issues yeni eklentilerin sağda açar
set splitbelow "Ekranı dikey olarak bölmek istediğimizde yeni ekranı aşağıda açar
set background=dark "Koyu tema kullanmak istediğimizi belirtir
colorscheme PaperColor "Temayı PaparCololor olarak tanımlar
let g:go_def_mapping_enabled = 0

map <C-n> :NERDTreeToggle<CR> "Ctrl + n kombinasyonu ile NerdTree penceresini açar
let g:NERDTreeDirArrowExpandable = '▸'
let g:NERDTreeDirArrowCollapsible = '▾'

"Aşağıdaki satırlar Vim/Neovim için yön tuşlarını devre dışı bırakır
noremap <Up> <Nop>
noremap <Down> <Nop>
noremap <Left> <Nop>
noremap <Right> <Nop>
inoremap <UP> <Nop>
inoremap <Down> <Nop>
inoremap <Left> <Nop>
inoremap <Right> <Nop>


"--------------------- COC için github sayfasında önerilen ayarlar ---------------------------
" if hidden is not set, TextEdit might fail.
set hidden

" Some servers have issues with backup files, see #649
set nobackup
set nowritebackup

" Better display for messages
set cmdheight=2

" You will have bad experience for diagnostic messages when it's default 4000.
set updatetime=300

" don't give |ins-completion-menu| messages.
set shortmess+=c

" always show signcolumns
set signcolumn=yes

" Use tab for trigger completion with characters ahead and navigate.
" Use command ':verbose imap <tab>' to make sure tab is not mapped by other plugin.
inoremap <silent><expr> <TAB>
      \ pumvisible() ? "\<C-n>" :
      \ <SID>check_back_space() ? "\<TAB>" :
      \ coc#refresh()
inoremap <expr><S-TAB> pumvisible() ? "\<C-p>" : "\<C-h>"

function! s:check_back_space() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

" Use <c-space> to trigger completion.
inoremap <silent><expr> <c-space> coc#refresh()

" Use <cr> to confirm completion, `<C-g>u` means break undo chain at current position.
" Coc only does snippet and additional edit on confirm.
inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"
" Or use `complete_info` if your vim support it, like:
" inoremap <expr> <cr> complete_info()["selected"] != "-1" ? "\<C-y>" : "\<C-g>u\<CR>"

" Use `[g` and `]g` to navigate diagnostics
nmap <silent> [g <Plug>(coc-diagnostic-prev)
nmap <silent> ]g <Plug>(coc-diagnostic-next)

" Remap keys for gotos
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)

" Use K to show documentation in preview window
nnoremap <silent> K :call <SID>show_documentation()<CR>

function! s:show_documentation()
  if (index(['vim','help'], &filetype) >= 0)
    execute 'h '.expand('<cword>')
  else
    call CocAction('doHover')
  endif
endfunction

" Highlight symbol under cursor on CursorHold
autocmd CursorHold * silent call CocActionAsync('highlight')

" Remap for rename current word
nmap <leader>rn <Plug>(coc-rename)

" Remap for format selected region
xmap <leader>f  <Plug>(coc-format-selected)
nmap <leader>f  <Plug>(coc-format-selected)

augroup mygroup
  autocmd!
  " Setup formatexpr specified filetype(s).
  autocmd FileType typescript,json setl formatexpr=CocAction('formatSelected')
  " Update signature help on jump placeholder
  autocmd User CocJumpPlaceholder call CocActionAsync('showSignatureHelp')
augroup end

" Remap for do codeAction of selected region, ex: `<leader>aap` for current paragraph
xmap <leader>a  <Plug>(coc-codeaction-selected)
nmap <leader>a  <Plug>(coc-codeaction-selected)

" Remap for do codeAction of current line
nmap <leader>ac  <Plug>(coc-codeaction)
" Fix autofix problem of current line
nmap <leader>qf  <Plug>(coc-fix-current)

" Create mappings for function text object, requires document symbols feature of languageserver.
xmap if <Plug>(coc-funcobj-i)
xmap af <Plug>(coc-funcobj-a)
omap if <Plug>(coc-funcobj-i)
omap af <Plug>(coc-funcobj-a)

" Use <TAB> for select selections ranges, needs server support, like: coc-tsserver, coc-python
nmap <silent> <TAB> <Plug>(coc-range-select)
xmap <silent> <TAB> <Plug>(coc-range-select)

" Use `:Format` to format current buffer
command! -nargs=0 Format :call CocAction('format')

" Use `:Fold` to fold current buffer
command! -nargs=? Fold :call     CocAction('fold', <f-args>)

" use `:OR` for organize import of current buffer
command! -nargs=0 OR   :call     CocAction('runCommand', 'editor.action.organizeImport')

" Add status line support, for integration with other plugin, checkout `:h coc-status`
set statusline^=%{coc#status()}%{get(b:,'coc_current_function','')}

" Using CocList
" Show all diagnostics
nnoremap <silent> <space>a  :<C-u>CocList diagnostics<cr>
" Manage extensions
nnoremap <silent> <space>e  :<C-u>CocList extensions<cr>
" Show commands
nnoremap <silent> <space>c  :<C-u>CocList commands<cr>
" Find symbol of current document
nnoremap <silent> <space>o  :<C-u>CocList outline<cr>
" Search workspace symbols
nnoremap <silent> <space>s  :<C-u>CocList -I symbols<cr>
" Do default action for next item.
nnoremap <silent> <space>j  :<C-u>CocNext<CR>
" Do default action for previous item.
nnoremap <silent> <space>k  :<C-u>CocPrev<CR>
" Resume latest coc list
nnoremap <silent> <space>p  :<C-u>CocListResume<CR>
{% endhighlight %}  

Kurulum ve ayar işlemleri bu kadar. Artık Vim/Neovim ile Go dilinde geliştirme yapabilirsiniz.  


Yazıyı basit tutmak adına kullandığım ayarların tamamını yazıya eklemedim. Kullandığım ayarların tamamı için ayarlarımı tuttuğum github reposuna bakabilirsiniz.  

[Github Repo](https://github.com/furkanbegen/dotfiles)
