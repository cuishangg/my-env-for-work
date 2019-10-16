# my-env-for-work

##vim-env
vim IDE config

解压：

* 对应文件linux-vim-evn.zip。下载后解压到~/目录
* 复制.vimrc、.bashrc到~/目录
* 复制bin文件到~/目录


手动：

* 克隆本仓库
* 替换.vimrc
* 下载Vundle
```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
* 进入vim
* 输入：PluginInstall

会自动安装.vimrc中添加的插件

其他需要安装的：

1. 安装ag，代替grep

首先安装ag搜索工具， 输入如下命令：

sudo  apt-get   install    silversearcher-ag

接着在～/.vimrc中添加以下内容：
```
  " Set mapleader
  let mapleader = ","

  " for easy using sliver search
  nmap <leader>f :norm yiw<CR>:Ag! -t -Q "<C-R>""

  " Locate and return character "above" current cursor position.
  function! LookUpwards()
      let column_num = virtcol('.')
      let target_pattern = '\%' . column_num . 'v.'
      let target_line_num = search(target_pattern . '*\S', 'bnW')


      if !target_line_num
          return ""
      else
          return matchstr(getline(target_line_num), target_pattern)
      endif
  endfunction

  imap <silent> <C-Y> <C-R><C-R>=LookUpwards()<CR>
```

在文件中光标处用“，f”在当前目录快速查找。

参考：
https://blog.csdn.net/ballack_linux/article/details/53187283

2. 安装ctags
```
sudo apt-get install ctags
```

http://blog.csdn.net/myth_liu/article/details/5672572

http://chaojimake.com/724.html

熟练的使用ctags仅需记住下面几条命:

1.ctags --languages=Asm,c,c++,java -R          （生成tags，汇编、c、c++、java）

2.$ vi –t tag       (请把tag替换为您欲查找的变量或函数名)       

3.:ts                   (ts  助记字：tags list, “:”开头的命令为VI中命令行模式命令)       

4.:tp                   (tp 助记字：tags preview)

5.:tn                   (tn 助记字：tags next)

6.Ctrl + ]            (跳转到定义处)

7.Ctrl + T           (退回至跳转前)

8.:ta x                (跳转到符号x的定义处，如果有多个符号，直接跳转到第一处
9.:ts x                (列出符号x的定义)
10.:tj x               (可以看做上面两个命令的合并，如果只找到一个符号定义，那么直接跳转到符号定义处，如果有多个，则让用户自行选择)

  注意：运行vim的时候，必须在“tags”文件所在的目录下运行。否则，运行vim的时候还要用“:set tag=”命令设定“tags”文件的路径，这样vim才能找到“tags”文件。在完成编码时，可以手工删掉tags文件（帚把不到，灰尘不会自己跑掉）。

（转自：https://blog.csdn.net/sunlylorn/article/details/8920457）
vim的ctags和taglist在默认情况下是不进行自动更新的，这对于编写代码是非常不方便的，好在vim的脚本还是很强大的，于是在vimrc中添加如下函数：

```
function! UpdateCtags()
    let curdir=getcwd()
    while !filereadable("./tags")
        cd ..
        if getcwd() == "/"
            break
        endif
    endwhile
    if filewritable("./tags")
        !ctags -R --file-scope=yes --langmap=c:+.h --languages=c,c++,java --links=yes --c-kinds=+p --c++-kinds=+p --fields=+iaS --extra=+q
        TlistUpdate
    endif
    execute ":cd " . curdir
endfunction

nmap <F10> :call UpdateCtags()<CR>
```
**按F10可以更新。**







