
# [命令列的藝術](//github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)

[![Join the chat at https://gitter.im/jlevy/the-art-of-command-line](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/jlevy/the-art-of-command-line?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

- [前言](#前言)
- [基礎](#基礎)
- [日常使用](#日常使用)
- [檔案及資料處理](#檔案及資料處理)
- [系統除錯](#系統除錯)
- [單行指令碼](#單行指令碼)
- [冷門但有用](#冷門但有用)
- [僅限 OS X 系統](#僅限-OS-X-系統)
- [僅限 Windows 系統](#僅限-Windows-系統)
- [更多資源](#更多資源)
- [免責聲明](#免責聲明)

![curl -s 'https://raw.githubusercontent.com/jlevy/the-art-of-command-line/master/README.md' | egrep -o '`\w+`' | tr -d '`' | cowsay -W50](cowsay.png)

熟練使用命令列是一種常常被忽視，或被認為難以掌握的技能，但實際上，它會提高你作為工程師的靈活性以及生產力。本文是一分我在 Linux 上工作時，發現的一些命令列使用技巧的摘要。有些技巧非常基礎，而另一些則相當複雜，甚至晦澀難懂。這篇文章並不長，但當你能夠熟練掌握這裡列出的所有技巧時，你就學會了很多關於命令列的東西了。

這篇文章是[許多作者和譯者](AUTHORS.md)共同的成果。這裡的部分內容[首次](http://www.quora.com/What-are-some-lesser-known-but-useful-Unix-commands)[出現](http://www.quora.com/What-are-the-most-useful-Swiss-army-knife-one-liners-on-Unix)於 [Quora](http://www.quora.com/What-are-some-time-saving-tips-that-every-Linux-user-should-know)，但已經遷移到了 Github，並由眾多高手做出了許多改進。如果你在本文中發現了錯誤或者存在可以改善的地方，請[**貢獻你的一分力量**](CONTRIBUTING.md)。

## 前言

涵蓋範圍：

- 這篇文章不僅能幫助剛接觸命令列的新手，而且對具有經驗的人也大有裨益。本文致力於做到**覆蓋面廣**((涉及所有重要的內容))，**具體**((給出具體的最常用的例子))，以及**簡潔**((避免冗餘的內容，或是可以在其他地方輕鬆查到的細枝末節))。在特定應用場景下，本文的內容屬於基本功或者能幫助您節約大量的時間。
- 本文主要為 Linux 所寫，但在[僅限 OS X 系統](#僅限-OS-X-系統)章節和[僅限 Windows 系統](#僅限-Windows-系統)章節中也包含有對應作業系統的內容。除去這兩個章節外，其它的內容大部分均可在其他類 Unix 系統或 OS X，甚至 Cygwin 中得到應用。
- 本文主要關注於互動式 Bash，但也有很多技巧可以應用於其他 shell 和 Bash 指令碼當中。
- 除去「標準的」Unix 命令，本文還包括了一些依賴於特定軟體包的命令((前提是它們具有足夠的價值))。

注意事項：

- 為了能在一頁內展示盡量多的東西，一些具體的資訊可以在引用的頁面中找到。我們相信機智的你知道如何使用 Google 或者其他搜尋引擎來查閱到更多的詳細資訊。文中部分命令需要您使用 `apt-get`、`yum`、`dnf`、`pacman`、`pip` 或 `brew`((以及其它合適的套件管理程式))來安裝依賴的程式。
- 遇到問題的話，請嘗試使用 [Explainshell](http://explainshell.com/) 去獲取相關命令、參數、管道等內容的解釋。

## 基礎

- 學習 Bash 的基礎知識。具體地，在命令列中輸入 `man bash` 並至少全文瀏覽一遍；它理解起來很簡單並且不冗長。其他的 shell 可能很好用，但 Bash 的功能已經足夠強大並且到幾乎總是可用的((如果你**只**學習 zsh、fish 或其他的 shell 的話，在你自己的裝置上會顯得很方便，但過度依賴這些功能會給您帶來不便，例如當你需要在伺服器上工作時))。
- 熟悉至少一個基於文字的編輯器。通常而言 Vim((`vi`))會是你最好的選擇，畢竟在終端中編輯文字時 Vim 是最好用的工具((甚至大部分情況下 Vim 要比 Emacs、大型 IDE 或是炫酷的編輯器更好用))。
- 學會如何使用 `man` 命令去閱讀文件。學會使用 `apropos` 去查詢文件。知道有些命令並不對應可執行檔案，而是在 Bash 內建好的，此時可以使用 `help` 和 `help -d` 命令獲取幫助資訊。你可以用 `type 命令 ` 來判斷這個命令到底是可執行檔案、shell 內建命令還是別名。
- 學會使用 `>` 和 `<` 來重定向輸出和輸入，學會使用 `|` 來重定向管道。明白 `>` 會覆蓋了輸出檔案而 `>>` 是在檔案末新增。瞭解標準輸出 stdout 和標準錯誤 stderr。
- 學會使用萬用字元 ``*``((或許再算上 `?` 和 `[` ... `]`))和引用以及引用中 `'` 和 `"` 的區別((後文中有一些具體的例子))。
- 熟悉 Bash 中的任務管理工具：`&`, `ctrl-z`，`ctrl-c`, `jobs`，`fg`, `bg`，`kill` 等。
- 學會使用 `ssh` 進行遠端命令列登入，最好知道如何使用 `ssh-agent`, `ssh-add` 等命令來實現基礎的無密碼認證登入。
- 學會基本的檔案管理工具：`ls` 和 `ls -l`((瞭解 `ls -l` 中每一列代表的意義))、`less`，`head`、`tail` 和 `tail -f`((甚至 `less +F`)), `ln` 和 `ln -s`((瞭解硬鏈接與軟鏈接的區別)), `chown`，`chmod`, `du`((硬碟使用情況概述：``du -hs *``))。關於檔案系統的管理，學習 `df`, `mount`，`fdisk`, `mkfs`，`lsblk`。知道 inode 是什麼?((與 `ls -i` 和 `df -i` 等命令相關))。
- 學習基本的網路管理工具：`ip` 或 `ifconfig`, `dig`。
- 學習並使用一種版本控制管理系統，例如 `git`。
- 熟悉正規表示式，學會使用 `grep` / `egrep`，它們的參數中 `-i`, `-o`，`-v`, `-A`，`-B` 和 `-C` 這些是很常用並值得認真學習的。
- 學會使用 `apt-get`, `yum`，`dnf` 或 `pacman`((具體使用哪個取決於你使用的 Linux 發行版))來查詢和安裝軟體包。並確保你的環境中有 `pip` 來安裝基於 Python 的命令列工具((接下來提到的部分程式使用 `pip` 來安裝會很方便))。

## 日常使用

- 在 Bash 中，可以透過按`Tab`鍵實現自動補全參數，使用 `ctrl-r` 搜尋命令列歷史記錄((按下按鍵之後，輸入關鍵字便可以搜尋，重複按下 `ctrl-r` 會向後查詢匹配項，按下 `Enter` 鍵會執行當前匹配的命令，而按下右方向鍵會將匹配項放入當前行中，不會直接執行，以便做出修改))。
- 在 Bash 中，可以按下 `ctrl-w` 刪除你鍵入的最後一個單詞，`ctrl-u` 可以刪除行內游標所在位置之前的內容，`alt-b` 和 `alt-f` 可以以單詞為單位移動游標，`ctrl-a` 可以將游標移至行首，`ctrl-e` 可以將游標移至行尾，`ctrl-k` 可以刪除游標緻行尾的所有內容，`ctrl-l` 可以清屏。鍵入 `man readline` 可以檢視 Bash 中的預設快捷鍵。內容有很多，例如 `alt-.` 循環地移向前一個參數，而 ``alt-*`` 可以展開萬用字元。
- 你喜歡的話，可以執行 `set -o vi` 來使用 vi 風格的快捷鍵，而執行 `set -o emacs` 可以把它改回來。
- 為了便於編輯長命令，在設定你的預設編輯器後((例如 `export EDITOR=vim`)), `ctrl-x` `ctrl-e` 會打開一個編輯器來編輯當前輸入的命令。在 vi 風格下快捷鍵則是 `escape-v`。
- 鍵入 `history` 檢視命令列歷史記錄，再用 `!n`((`n` 是命令編號))就可以再次執行。其中有許多縮寫，最有用的大概就是 `!$`，它用於指代上次鍵入的參數，而 `!!` 可以指代上次鍵入的命令了((參考 man 頁面中的「HISTORY EXPANSION」))。不過這些功能，你也可以透過快捷鍵 `ctrl-r` 和 `alt-.` 來實現。
- `cd` 命令可以切換工作路徑，輸入 `cd~` 可以進入 home 目錄。要存取你的 home 目錄中的檔案，可以使用字首 `~`((例如 `~/.bashrc`))。在 `sh` 指令碼理則用環境變數 `$HOME` 指代 home 目錄的路徑。
- 回到前一個工作路徑：`cd -`。
- 如果你輸入命令的時候中塗改了主意，按下 `alt-#` 在行首新增 `#` 把它當做註釋再按下回車執行((或者依次按下 `ctrl-a`, `#`，`enter`))。這樣做的話，之後借助命令列歷史記錄，你可以很方便恢復你剛才輸入到一半的命令。
- 使用 `xargs`((或 `parallel`))。他們非常給力。注意到你可以控制每行參數個數((`-L`))和最大並行數((`-P`))。如果你不確定它們是否會按你想的那樣工作，先使用 `xargs echo` 檢視一下。此外，使用 `-I{}` 會很方便。例如：
  ```bash
  find . -name '*.py' | xargs grep some_function
  cat hosts | xargs -I{} ssh root@{} hostname
  ```
- `pstree -p` 以一種優雅的方式展示程序樹。
- 使用 `pgrep` 和 `pkill` 根據名字查詢程序或發送訊號((`-f` 參數通常有用))。
- 瞭解你可以發往程序的訊號的種類。比如，使用 `kill -STOP [pid]` 停止一個程序。使用 `man 7 signal` 檢視詳細列表。
- 使用 `nohup` 或 `disown` 使一個後臺程序持續執行。
- 使用 `netstat -lntp` 或 `ss -plat` 檢查哪些程序在監聽埠((預設是檢查 TCP 埠；新增參數 `-u` 則檢查 UDP 埠))或者 `lsof -iTCP -sTCP:LISTEN -P -n`((這也可以在 OS X 上執行))。
- `lsof` 來檢視開啟的套接字和檔案。
- 使用 `uptime` 或 `w` 來檢視系統已經執行多長時間。
- 使用 `alias` 來建立常用命令的快捷形式。例如：`alias ll='ls -latr'` 建立了一個新的命令別名 `ll`。
- 可以把別名、shell 選項和常用函式儲存在 `~/.bashrc`，具體看下這篇[文章](http://superuser.com/a/183980/7106)。這樣做的話你就可以在所有 shell 會話中使用你的設定。
- 把環境變數的設定以及登陸時要執行的命令儲存在 `~/.bash_profile`。而對於從圖形界面啟動的 shell 和 `cron` 啟動的 shell，則需要單獨配置檔案。
- 要想在幾臺電腦中同步你的配置檔案((例如 `.bashrc` 和 `.bash_profile`))，可以借助 Git。
- 當變數和檔名中包含空格的時候要格外小心。Bash 變數要用引號括起來，比如 `"$FOO"`。盡量使用 `-0` 或 `-print0` 選項以便用 NULL 來分隔檔名，例如 `locate -0 pattern | xargs -0 ls -al` 或 `find / -print0 -type d | xargs -0 ls -al`。如果 for 循環中循環存取的檔名含有空字元((空格、tab 等字元))，只需用 `IFS=$'\n'` 把內部欄位分隔符設為換行符。
- 在 Bash 指令碼中，使用 `set -x` 去除錯輸出((或者使用它的變體 `set -v`，它會記錄原始輸入，包括多餘的參數和註釋))。盡可能地使用嚴格模式：使用 `set -e` 令指令碼在發生錯誤時退出而不是繼續執行；使用 `set -u` 來檢查是否使用了未賦值的變數；試試 `set -o pipefail`，它可以監測管道中的錯誤。當牽扯到很多指令碼時，使用 `trap` 來檢測 ERR 和 EXIT。一個好的習慣是在指令碼檔案開頭這樣寫，這會使它能夠檢測一些錯誤，並在錯誤發生時中斷程式並輸出資訊：
```bash
set -euo pipefail
trap "echo 'error: Script failed: see failed command above'" ERR
```
- 在 Bash 指令碼中，子 shell((使用括號 `(`...`)`))是一種組織參數的便捷方式。一個常見的例子是臨時地移動工作路徑，程式碼如下：
  ```bash
  # do something in current dir
  (cd /some/other/dir && other-command)
  # continue in original dir
  ```
- 在 Bash 中，變數有許多的擴充套件方式。`${name:?error message}` 用於檢查變數是否存在。此外，當 Bash 指令碼只需要一個參數時，可以使用這樣的程式碼 `input_file=${1:?usage: $0 input_file}`。在變數為空時使用預設值：`${name:-default}`。如果你要在之前的例子中再加一個((可選的))參數，可以使用類似這樣的程式碼 `output_file=${2:-logfile}`，如果省略了 $2，它的值就為空，於是 `output_file` 就會被設為 `logfile`。數學表達式：`i=$(((i + 1) % 5))`。序列：`{1...10}`。截斷字串：`${var%suffix}` 和 `${var#prefix}`。例如，假設 `var=foo.pdf`，那麼 `echo ${var%.pdf}.txt` 將輸出 `foo.txt`。
- 使用括號擴充套件((`{`...`}`))來減少輸入相似文字，並自動化文字組合。這在某些情況下會很有用，例如 `mv foo.{txt,pdf} some-dir`((同時移動兩個檔案)), `cp somefile{,.bak}`((會被擴充套件成 `cp somefile somefile.bak`))或者 `mkdir -p test-{a,b,c}/subtest-{1,2,3}`((會被擴充套件成所有可能的組合，並建立一個目錄樹))。
- 透過使用 ``< (some command)`` 可以將輸出視為檔案。例如，對比本地檔案 `/etc/hosts` 和一個遠端檔案：
  ```sh
  diff /etc/hosts < (ssh somehost cat /etc/hosts)
  ```
- 編寫指令碼時，你可能會想要把程式碼都放在大括號裡。缺少右括號的話，程式碼就會因為語法錯誤而無法執行。如果你的指令碼是要放在網上分享供他人使用的，這樣的寫法就體現出它的好處了，因為這樣可以防止下載不完全程式碼被執行。
  ```bash
  {
  # 在這裡寫程式碼
  }
  ```
- 瞭解 Bash 中的「here documents」，例如 ``cat <<EOF ...``。

- 在 Bash 中，同時重定向標準輸出和標準錯誤：``some-command >logfile 2>&1`` 或者 ``some-command &>logfile``。通常，為了保證命令不會在標準輸入理殘留一個未關閉的檔案控制代碼捆綁在你當前所在的終端上，在命令後新增 ``</dev/null`` 是一個好習慣。
- 使用 `man ascii` 檢視具有十六進制和十進制值的 ASCII 表。`man unicode`, `man utf-8`，以及 `man latin1` 有助於你去瞭解通用的編碼資訊。
- 使用 `screen` 或 [`tmux`](https://tmux.github.io/) 來使用多分螢幕，當你在使用 ssh 時((儲存 session 資訊))將尤為有用。而 `byobu` 可以為它們提供更多的資訊和易用的管理工具。另一個輕量級的 session 持久化解決方案是 [`dtach`](https://github.com/bogner/dtach))。
- ssh 中，瞭解如何使用 `-L` 或 `-D`((偶爾需要用 `-R`))開啟隧道是非常有用的，比如當你需要從一臺遠端伺服器上存取 web 頁面。
- 對 ssh 設定做一些小優化可能是很有用的，例如這個 `~/.ssh/config` 檔案包含了防止特定網路環境下連線斷開、壓縮數據、多通道等選項：
  ```
  TCPKeepAlive=yes
  ServerAliveInterval=15
  ServerAliveCountMax=6
  Compression=yes
  ControlMaster auto
  ControlPath /tmp/%r@%h:%p
  ControlPersist yes
  ```
- 一些其他的關於 ssh 的選項是與安全相關的，應當小心翼翼的使用。例如你應當只能在可信任的網路中啟用 `StrictHostKeyChecking=no`, `ForwardAgent=yes`。
- 考慮使用 [`mosh`](https://mosh.mit.edu/) 作為 ssh 的替代品，它使用 UDP 協議。它可以避免連線被中斷並且對頻寬需求更小，但它需要在服務端做相應的配置。
- 獲取八進制形式的檔案存取許可權((修改系統設定時通常需要，但 `ls` 的功能不那麼好用並且通常會搞砸))，可以使用類似如下的程式碼：
  ```sh
  stat -c '%A %a %n' /etc/timezone
  ```
- 使用 [`percol`](https://github.com/mooz/percol) 或者 [`fzf`](https://github.com/junegunn/fzf) 可以互動式地從另一個命令輸出中選取值。
- 使用 `fpp`(([PathPicker](https://github.com/facebook/PathPicker)))可以與基於另一個命令((例如 `git`))輸出的檔案互動。
- 將 web 伺服器上當前目錄下所有的檔案((以及子目錄))暴露給你所處網路的所有使用者，使用： `python -m SimpleHTTPServer 7777`((使用埠 7777 和 Python 2))或 `python -m http.server 7777`((使用埠 7777 和 Python 3))。
- 以其他使用者的身分執行命令，使用 `sudo`。預設以 root 使用者的身分執行；使用 `-u` 來指定其他使用者。使用 `-i` 來以該使用者登入((需要輸入**你自己的**密碼))。
- 將 shell 切換為其他使用者，使用 `su username` 或者 `su - username`。加入 `-` 會使得切換後的環境與使用該使用者登入後的環境相同。省略使用者名稱則預設為 root。切換到哪個使用者，就需要輸入 _ 哪個使用者的 _ 密碼。
- 瞭解命令列的 [128K 限制](https://wiki.debian.org/CommonErrorMessages/ArgumentListTooLong)。使用萬用字元匹配大量檔名時，常會遇到「Argument list too long」的錯誤資訊。((這種情況下換用 `find` 或 `xargs` 通常可以解決。)
- 當你需要一個基本的計算器時，可以使用 `python` 直譯器((當然你要用 python 的時候也是這樣))。例如：
  ```
  >>> 2+3
  5
  ```

## 檔案及資料處理

- 在當前目錄下透過檔名查詢一個檔案，使用類似於這樣的命令：`find . -iname '*something*'`。在所有路徑下透過檔名查詢檔案，使用 `locate something`((但注意到 `updatedb` 可能沒有對最近新建的檔案建立索引，所以你可能無法定位到這些未被索引的檔案))。
- 使用 [`ag`](https://github.com/ggreer/the_silver_searcher) 在原始碼或數據檔案裡檢索((`grep -r` 同樣可以做到，但相比之下 `ag` 更加先進))。
- 將 HTML 轉為文字：`lynx -dump -stdin`。
- Markdown, HTML，以及所有文件格式之間的轉換，試試 [`pandoc`](http://pandoc.org/)。
- 當你要處理棘手的 XML 時候，`xmlstarlet` 算是上古時代流傳下來的神器。
- 使用 [`jq`](http://stedolan.github.io/jq/) 處理 JSON。
- 使用 [`shyaml`](https://github.com/0k/shyaml) 處理 YAML。
- 要處理 Excel 或 CSV 檔案的話，[csvkit](https://github.com/onyxfish/csvkit) 提供了 `in2csv`, `csvcut`，`csvjoin`, `csvgrep` 等方便易用的工具。
- 當你要處理 Amazon S3 相關的工作的時候，[`s3cmd`](https://github.com/s3tools/s3cmd) 是一個很方便的工具而 [`s4cmd`](https://github.com/bloomreach/s4cmd) 的效率更高。Amazon 官方提供的 [`aws`](https://github.com/aws/aws-cli) 以及 [`saws`](https://github.com/donnemartin/saws) 是其他 AWS 相關工作的基礎，值得學習。
- 瞭解如何使用 `sort` 和 `uniq`，包括 uniq 的 `-u` 參數和 `-d` 參數，具體內容在後文單行指令碼節中。另外可以瞭解一下 `comm`。
- 瞭解如何使用 `cut`, `paste` 和 `join` 來更改檔案。很多人都會使用 `cut`，但遺忘了 `join`。
- 瞭解如何運用 `wc` 去計算新行數((`-l`))，字元數((`-m`))，單詞數((`-w`))以及位元組數((`-c`))。
- 瞭解如何使用 `tee` 將標準輸入複製到檔案甚至標準輸出，例如 `ls -al | tee file.txt`。
- 要進行一些複雜的計算，比如分組、逆序和一些其他的統計分析，可以考慮使用 [`datamash`](https://www.gnu.org/software/datamash/)。
- 注意到語言設定((中文或英文等))對許多命令列工具有一些微妙的影響，比如排序的順序和效能。大多數 Linux 的安裝過程會將 `LANG` 或其他有關的變數設定為符合本地的設定。要意識到當你改變語言設定時，排序的結果可能會改變。明白國際化可能會使 sort 或其他命令執行效率下降**許多倍**。某些情況下((例如集合運算))你可以放心的使用 `export LC_ALL=C` 來忽略掉國際化並按照位元組來判斷順序。
- 你可以單獨指定某一條命令的環境，只需在呼叫時把環境變數設定放在命令的前面，例如 `TZ=Pacific/Fiji date` 可以獲取斐濟的時間。
- 瞭解如何使用 `awk` 和 `sed` 來進行簡單的資料處理。參閱 [One-liners](#one-liners) 獲取示例。
- 替換一個或多個檔案中出現的字串：
  ```sh
  perl -pi.bak -e 's/old-string/new-string/g' my-files-*.txt
  ```
- 使用 [`repren`](https://github.com/jlevy/repren) 來批量重新命名檔案，或是在多個檔案中搜尋替換內容。((有些時候 `rename` 命令也可以批量重新命名，但要注意，它在不同 Linux 發行版中的功能並不完全一樣。)
  ```sh
  # 將檔案、目錄和內容全部重新命名 foo -> bar:
  repren -- full -- preserve-case -- from foo -- to bar.
  # 還原所有備份檔案 whatever.bak -> whatever:
  repren -- renames -- from ' (.*) \.bak' -- to '\1' *.bak
  # 用 rename 實現上述功能((若可用):
  rename 's/\.bak$//' *.bak
  ```
- 根據 man 頁面的描述，`rsync` 是一個快速且非常靈活的檔案複製工具。它聞名於裝置之間的檔案同步，但其實它在本地情況下也同樣有用。在安全設定允許下，用 `rsync` 代替 `scp` 可以實現檔案續傳，而不用重新從頭開始。它同時也是刪除大量檔案的 [最快方法](https://web.archive.org/web/20130929001850/http://linuxnote.net/jianingy/en/linux/a-fast-way-to-remove-huge-number-of-files.html) 之一：
  ```sh
  mkdir empty && rsync -r -- delete empty/ some-dir && rmdir some-dir
  ```
- 若要在複製檔案時獲取當前進度，可使用 `pv`, [`pycp`](https://github.com/dmerejkowsky/pycp)，[`progress`](https://github.com/Xfennec/progress), `rsync -- progress`。若所執行的複製為 block 塊拷貝，可以使用 `dd status=progress`。
- 使用 `shuf` 可以以行為單位來打亂檔案的內容或從一個檔案中隨機選取多行。
- 瞭解 `sort` 的參數。顯示數字時，使用 `-n` 或者 `-h` 來顯示更易讀的數((例如 `du -h` 的輸出))。明白排序時關鍵字的工作原理((`-t` 和 `-k`))。例如，注意到你需要 `-k1, 1` 來僅按第一個域來排序，而 `-k1` 意味著按整行排序。穩定排序((`sort -s`))在某些情況下很有用。例如，以第二個域為主關鍵字，第一個域為次關鍵字進行排序，你可以使用 `sort -k1, 1 | sort -s -k2, 2`。
- 如果你想在 Bash 命令列中寫 tab 製表符，按下 `ctrl-v` `[Tab]` 或鍵入 `$'\t'`((後者可能更好，因為你可以複製貼上它))。
- 標準的原始碼對比及合併工具是 `diff` 和 `patch`。使用 `diffstat` 檢視變更總覽數據。注意到 `diff -r` 對整個資料夾有效。使用 `diff -r tree1 tree2 | diffstat` 檢視變更的統計數據。`vimdiff` 用於比對並編輯檔案。
- 對於二進制檔案，使用 `hd`, `hexdump` 或者 `xxd` 使其以十六進制顯示，使用 `bvi`, `hexedit` 或者 `biew` 來進行二進制編輯。
- 同樣對於二進制檔案，`strings`((包括 `grep` 等工具))可以幫助在二進制檔案中查詢特定位元。
- 製作二進制差分檔案((Delta 壓縮))，使用 `xdelta3`。
- 使用 `iconv` 更改文字編碼。需要更高級的功能，可以使用 `uconv`，它支援一些高級的 Unicode 功能。例如，這條命令移除了所有重音符號：
  ```sh
  uconv -f utf-8 -t utf-8 -x '::Any-Lower;::Any-NFD; [:Nonspacing Mark:] >;::Any-NFC; ' < input.txt > output.txt
  ```
- 拆分檔案可以使用 `split`((按大小拆分))和 `csplit`((按模式拆分))。
- 操作日期和時間表達式，可以用 [`dateutils`](http://www.fresse.org/dateutils/) 中的 `dateadd`、`datediff`、`strptime` 等工具。
- 使用 `zless`、`zmore`、`zcat` 和 `zgrep` 對壓縮過的檔案進行操作。
- 檔案屬性可以透過 `chattr` 進行設定，它比檔案許可權更加底層。例如，為了保護檔案不被意外刪除，可以使用不可修改標記：`sudo chattr +i /critical/directory/or/file`
- 使用 `getfacl` 和 `setfacl` 以儲存和恢復檔案許可權。例如：
  ```sh
  getfacl -R /some/path > permissions.txt
  setfacl -- restore=permissions.txt
  ```
- 為了高效地建立空檔案，請使用 `truncate`((建立[稀疏檔案](https://zh.wikipedia.org/wiki/ 稀疏檔案)))、`fallocate`((用於 ext4、xfs、btrf 和 ocfs2 檔案系統))、`xfs_mkfile`((適用於幾乎所有的檔案系統，包含在 xfsprogs 包中)), `mkfile`((用於類 Unix 作業系統，比如 Solaris 和 Mac OS))。

## 系統除錯

- `curl` 和 `curl -I` 可以被輕鬆地應用於 web 除錯中，它們的好兄弟 `wget` 也是如此，或者也可以試試更潮的 [`httpie`](https://github.com/jkbrzt/httpie)。
- 獲取 CPU 和硬碟的使用狀態，通常使用使用 `top`((`htop` 更佳)), `iostat` 和 `iotop`。而 `iostat -mxz 15` 可以讓你獲悉 CPU 和每個硬碟分割槽的基本資訊和效能表現。
- 使用 `netstat` 和 `ss` 檢視網路連線的細節。
- `dstat` 在你想要對系統的現狀有一個粗略的認識時是非常有用的。然而若要對系統有一個深度的總體認識，使用 [`glances`](https://github.com/nicolargo/glances)，它會在一個終端視窗中向你提供一些系統級的數據。
- 若要瞭解記憶體狀態，執行並理解 `free` 和 `vmstat` 的輸出。值得留意的是「cached」的值，它指的是 Linux 內核用來作為檔案快取的記憶體大小，而與空閑記憶體無關。
- Java 系統除錯則是一件截然不同的事，一個可以用於 Oracle 的 JVM 或其他 JVM 上的除錯的技巧是你可以執行 `kill -3 <pid>` 同時一個完整的棧軌跡和堆概述((包括 GC 的細節))會被儲存到標準錯誤或是日誌檔案。JDK 中的 `jps`, `jstat`，`jstack`, `jmap` 很有用。[SJK tools](https://github.com/aragozin/jvm-tools) 更高級。
- 使用 [`mtr`](http://www.bitwizard.nl/mtr/) 去跟蹤路由，用於確定網路問題。
- 用 [`ncdu`](https://dev.yorhel.nl/ncdu) 來檢視磁碟使用情況，它比尋常的命令，如 ``du -sh *``，更節省時間。
- 查詢正在使用頻寬的套接字連線或程序，使用 [`iftop`](http://www.ex-parrot.com/~pdw/iftop/) 或 [`nethogs`](https://github.com/raboof/nethogs)。
- `ab` 工具((Apache 中自帶))可以簡單粗暴地檢查 web 伺服器的效能。對於更複雜的負載測試，使用 `siege`。
- [`wireshark`](https://wireshark.org/), [`tshark`](https://www.wireshark.org/docs/wsug_html_chunked/AppToolstshark.html) 和 [`ngrep`](http://ngrep.sourceforge.net/) 可用於複雜的網路除錯。
- 瞭解 `strace` 和 `ltrace`。這倆工具在你的程式執行失敗、掛起甚至崩潰，而你卻不知道為什麼或你想對效能有個總體的認識的時候是非常有用的。注意 profile 參數((`-c`))和附加到一個執行的程序參數((`-p`))。
- 瞭解使用 `ldd` 來檢查共享庫。但是 [永遠不要在不信任的檔案上執行](http://www.catonmat.net/blog/ldd-arbitrary-code-execution/)。
- 瞭解如何運用 `gdb` 連線到一個執行著的程序並獲取它的堆疊軌跡。
- 學會使用 `/proc`。它在除錯正在出現的問題的時候有時會效果驚人。比如：`/proc/cpuinfo`, `/proc/meminfo`，`/proc/cmdline`, `/proc/xxx/cwd`，`/proc/xxx/exe`, `/proc/xxx/fd/`，`/proc/xxx/smaps`((這裡的 `xxx` 表示程序的 id 或 pid))。
- 當除錯一些之前出現的問題的時候，[`sar`](http://sebastien.godard.pagesperso-orange.fr/) 非常有用。它展示了 cpu、記憶體以及網路等的歷史數據。
- 關於更深層次的系統分析以及效能分析，看看 `stap`(([SystemTap](https://sourceware.org/systemtap/wiki)))、[`perf`](https://en.wikipedia.org/wiki/Perf_(Linux))，以及 [`sysdig`](https://github.com/draios/sysdig)。
- 檢視你當前使用的系統，使用 `uname`, `uname -a`((Unix / kernel 資訊))或者 `lsb_release -a`((Linux 發行版資訊))。
- 無論什麼東西工作得很歡樂((可能是硬體或驅動問題))時可以試試 `dmesg`。
- 如果你刪除了一個檔案，但透過 `du` 發現沒有釋放預期的磁碟空間，請檢查檔案是否被程序佔用：`lsof | grep deleted | grep "filename-of-my-big-file"`

## 單行指令碼

一些命令組合的例子：

- 當你需要對文字檔案做集合交、並、差運算時，`sort` 和 `uniq` 會是你的好幫手。具體例子請參照程式碼後面的，此處假設 `a` 與 `b` 是兩內容不同的檔案。這種方式效率很高，並且在小檔案和上 G 的檔案上都能運用((注意儘管在 `/tmp` 在一個小的根分割槽上時你可能需要 `-T` 參數，但是實際上 `sort` 並不被記憶體大小約束))，參閱前文中關於 `LC_ALL` 和 `sort` 的 `-u` 參數的部分。
  ```sh
  sort a b | uniq > c # c 是 a 並 b
  sort a b | uniq -d > c # c 是 a 交 b
  sort a b b | uniq -u > c # c 是 a - b
  ```
- 使用 ``grep . *``((每行都會附上檔名))或者 ``head -100 *``((每個檔案有一個標題))來閱讀檢查目錄下所有檔案的內容。這在檢查一個充滿配置檔案的目錄((如 `/sys`、`/proc`、`/etc`))時特別好用。
- 計算文字檔案第三列中所有數的和((可能比同等作用的 Python 程式碼快三倍且程式碼量少三倍):
  ```sh
  awk '{ x += $3 } END { print x }' myfile
  ```
- 如果你想在檔案樹上檢視大小 / 日期，這可能看起來像遞迴版的 `ls -l` 但比 `ls -lR` 更易於理解：
  ```sh
  find . -type f -ls
  ```
- 假設你有一個類似於 web 伺服器日誌檔案的文字檔案，並且一個確定的值只會出現在某些行上，假設一個 `acct_id` 參數在 URI 中。如果你想計算出每個 `acct_id` 值有多少次請求，使用如下程式碼：
  ```sh
  egrep -o 'acct_id=[0-9]+' access.log | cut -d= -f2 | sort | uniq -c | sort -rn
  ```
- 要持續監測檔案改動，可以使用 `watch`，例如檢查某個資料夾中檔案的改變，可以用 `watch -d -n 2 'ls -rtlh | tail'`; 或者在排查 WiFi 設定故障時要監測網路設定的更改，可以用 `watch -d -n 2 ifconfig`。
- 執行這個函式從這篇文件中隨機獲取一條技巧((解析 Markdown 檔案並抽取專案):
  ```sh
  function taocl() {
    curl -s https://raw.githubusercontent.com/jlevy/the-art-of-command-line/master/README.md |
      sed '/cowsay[.]png/d' |
      pandoc -f markdown -t html |
      xmlstarlet fo --html --dropdtd |
      xmlstarlet sel -t -v "(html/body/ul/li[count(p)>0])[$RANDOM mod last()+1]" |
      xmlstarlet unesc |
      fmt -80 |
      iconv -t US
  }
  ```

## 冷門但有用

- `expr`: 計算表達式或正則匹配
- `m4`: 簡單的巨集處理器
- `yes`: 多次列印字串
- `cal`: 漂亮的日曆
- `env`: 執行一個命令((指令碼檔案中很有用)
- `printenv`: 顯示環境變數((除錯時或在寫指令碼檔案時很有用)
- `look`: 查詢以特定字串開頭的單詞或行
- `cut`, `paste` 和 `join`: 數據修改
- `fmt`: 格式化文字段落
- `pr`: 將文字格式化成頁 / 列形式
- `fold`: 包裹文字中的幾行
- `column`: 將文字格式化成多個對齊、定寬的列或表格
- `expand` 和 `unexpand`: 製表符與空格之間轉換
- `nl`: 新增行號
- `seq`: 顯示數列
- `bc`: 計算器
- `factor`: 分解因數
- [`gpg`](https://gnupg.org/): 加密並簽名檔案
- `toe`: terminfo 入口列表
- `nc`: 網路除錯及數據傳輸
- `socat`: 套接字代理，與 `netcat` 類似
- [`slurm`](https://github.com/mattthias/slurm): 網路流量視覺化
- `dd`: 檔案或裝置間傳輸數據
- `file`: 確定檔案型別
- `tree`: 以樹的形式顯示路徑和檔案，類似於遞迴的 `ls`
- `stat`: 檔案資訊
- `time`: 執行命令，並計算執行時間
- `timeout`: 在指定時長範圍內執行命令，並在規定時間結束後停止程序
- `lockfile`: 使檔案只能透過 `rm -f` 移除
- `logrotate`: 切換、壓縮以及發送日誌檔案
- `watch`: 重複執行同一個命令，展示結果並 / 或高亮有更改的部分
- [`when-changed`](https://github.com/joh/when-changed): 當檢測到檔案更改時執行指定命令。參閱 `inotifywait` 和 `entr`。
- `tac`: 反向輸出檔案
- `shuf`: 檔案中隨機選取幾行
- `comm`: 一行一行的比較排序過的檔案
- `strings`: 從二進制檔案中抽取文字
- `tr`: 轉換字母
- `iconv` 或 `uconv`: 文字編碼轉換
- `split` 和 `csplit`: 分割檔案
- `sponge`: 在寫入前讀取所有輸入，在讀取檔案後再向同一檔案寫入時比較有用，例如 `grep -v something some-file | sponge some-file`
- `units`: 將一種計量單位轉換為另一種等效的計量單位((參閱 `/usr/share/units/definitions.units`)
- `apg`: 隨機產生密碼
- `xz`: 高比例的檔案壓縮
- `ldd`: 動態庫資訊
- `nm`: 提取 obj 檔案中的符號
- `ab` 或 [`wrk`](https://github.com/wg/wrk): web 伺服器效能分析
- `strace`: 除錯系統呼叫
- [`mtr`](http://www.bitwizard.nl/mtr/): 更好的網路除錯跟蹤工具
- `cssh`: 視覺化的併發 shell
- `rsync`: 透過 ssh 或本地檔案系統同步檔案和資料夾
- [`wireshark`](https://wireshark.org/) 和 [`tshark`](https://www.wireshark.org/docs/wsug_html_chunked/AppToolstshark.html): 抓包和網路除錯工具
- [`ngrep`](http://ngrep.sourceforge.net/): 網路層的 grep
- `host` 和 `dig`: DNS 查詢
- `lsof`: 列出當前系統打開檔案的工具以及檢視埠資訊
- `dstat`: 系統狀態檢視
- [`glances`](https://github.com/nicolargo/glances): 高層次的多子系統總覽
- `iostat`: 硬碟使用狀態
- `mpstat`: CPU 使用狀態
- `vmstat`: 記憶體使用狀態
- `htop`: top 的加強版
- `last`: 登入記錄
- `w`: 檢視處於登入狀態的使用者
- `id`: 使用者 / 組 ID 資訊
- [`sar`](http://sebastien.godard.pagesperso-orange.fr/): 系統歷史數據
- [`iftop`](http://www.ex-parrot.com/~pdw/iftop/) 或 [`nethogs`](https://github.com/raboof/nethogs): 套接字及程序的網路利用情況
- `ss`: 套接字數據
- `dmesg`: 引導及系統錯誤資訊
- `sysctl`: 在內核執行時動態地檢視和修改內核的執行參數
- `hdparm`: SATA/ATA 磁碟更改及效能分析
- `lsblk`: 列出塊裝置資訊：以樹形展示你的磁碟以及磁碟分割槽資訊
- `lshw`, `lscpu`，`lspci`, `lsusb` 和 `dmidecode`: 檢視硬體資訊，包括 CPU、BIOS、RAID、顯示卡、USB 裝置等
- `lsmod` 和 `modinfo`: 列出內核模組，並顯示其細節
- `fortune`, `ddate` 和 `sl`: 額，這主要取決於你是否認為蒸汽火車和莫名其妙的名人名言是否「有用」

## 僅限 OS X 系統

以下是**僅限於**OS X 系統的技巧。

- 用 `brew`((Homebrew))或者 `port`((MacPorts))進行包管理。這些可以用來在 OS X 系統上安裝以上的大多數命令。
- 用 `pbcopy` 複製任何命令的輸出到桌面應用，用 `pbpaste` 貼上輸入。
- 若要在 OS X 終端中將 Option 鍵視為 alt 鍵((例如在上面介紹的 `alt-b`、`alt-f` 等命令中用到))，打開 偏好設定 -> 描述檔案 -> 鍵盤 並勾選「使用 Option 鍵作為 Meta 鍵」。
- 用 `open` 或者 `open -a /Applications/Whatever.app` 使用桌面應用打開檔案。
- Spotlight: 用 `mdfind` 搜尋檔案，用 `mdls` 列出後設資料((例如照片的 EXIF 資訊))。
- 注意 OS X 系統是基於 BSD UNIX 的，許多命令((例如 `ps`, `ls`，`tail`, `awk`，`sed`))都和 Linux 中有微妙的不同((Linux 很大程度上受到了 System V-style Unix 和 GNU 工具影響))。你可以透過標題為 "BSD General Commands Manual" 的 man 頁面發現這些不同。在有些情況下 GNU 版本的命令也可能被安裝((例如 `gawk` 和 `gsed` 對應 GNU 中的 awk 和 sed))。如果要寫跨平臺的 Bash 指令碼，避免使用這些命令((例如，考慮 Python 或者 `perl`))或者經過仔細的測試。
- 用 `sw_vers` 獲取 OS X 的版本資訊。

## 僅限 Windows 系統

以下是**僅限於**Windows 系統的技巧。

### 在 Winodws 下獲取 Unix 工具

- 可以安裝 [Cygwin](https://cygwin.com/) 允許你在 Microsoft Windows 中體驗 Unix shell 的威力。這樣的話，本文中介紹的大多數內容都將適用。
- 在 Windows 10 上，你可以使用 [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about)，它提供了一個熟悉的 Bash 環境，包含了不少 Unix 命令列工具。好處是它允許 Linux 上編寫的程式在 Windows 上執行，而另一方面，Windows 上編寫的程式卻無法在 Bash 命令列中執行。
- 如果你在 Windows 上主要想用 GNU 開發者工具((例如 GCC))，可以考慮 [MinGW](http://www.mingw.org/) 以及它的 [MSYS](http://www.mingw.org/wiki/msys) 包，這個包提供了例如 bash、gawk、make 和 grep 的工具。MSYS 並不包含所有可以與 Cygwin 媲美的特性。當製作 Unix 工具的原生 Windows 埠時 MinGW 將特別地有用。
- 另一個在 Windows 下實現接近 Unix 環境外觀效果的選項是 [Cash](https://github.com/dthree/cash)。注意在此環境下只有很少的 Unix 命令和命令列可用。

### 實用 Windows 命令列工具

- 可以使用 `wmic` 在命令列環境下給大部分 Windows 系統管理任務編寫指令碼以及執行這些任務。
- Windows 實用的原生命令列網路工具包括 `ping`, `ipconfig`，`tracert`，和 `netstat`。
- 可以使用 `Rundll32` 命令來實現[許多有用的 Windows 任務](http://www.thewindowsclub.com/rundll32-shortcut-commands-windows)。

### Cygwin 技巧

- 透過 Cygwin 的套件管理程式來安裝額外的 Unix 程式。
- 使用 `mintty` 作為你的命令列視窗。
- 要存取 Windows 剪貼簿，可以透過 `/dev/clipboard`。
- 執行 `cygstart` 以透過預設程式打開一個檔案。
- 要存取 Windows 登錄檔，可以使用 `regtool`。
- 注意 Windows 驅動程式路徑 `C:\` 在 Cygwin 中用 `/cygdrive/c` 代表，而 Cygwin 的 `/` 代表 Windows 中的 `C:\cygwin`。要轉換 Cygwin 和 Windows 風格的路徑可以用 `cygpath`。這在需要呼叫 Windows 程式的指令碼裡很有用。
- 學會使用 `wmic`，你就可以從命令列執行大多數 Windows 系統管理任務，並編成指令碼。
- 要在 Windows 下獲得 Unix 的介面和體驗，另一個辦法是使用 [Cash](https://github.com/dthree/cash)。需要注意的是，這個環境支援的 Unix 命令和命令列參數非常少。
- 要在 Windows 上獲取 GNU 開發者工具((比如 GCC))的另一個辦法是使用 [MinGW](http://www.mingw.org/) 以及它的 [MSYS](http://www.mingw.org/wiki/msys) 軟體包，該軟體包提供了 bash、gawk、make、grep 等工具。然而 MSYS 提供的功能沒有 Cygwin 完善。MinGW 在建立 Unix 工具的 Windows 原生移植方面非常有用。

## 更多資源

- [awesome-shell](https://github.com/alebcay/awesome-shell): 一分精心組織的命令列工具及資源的列表。
- [awesome-osx-command-line](https://github.com/herrbischoff/awesome-osx-command-line): 一分針對 OS X 命令列的更深入的指南。
- [Strict mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/): 為了編寫更好的指令碼檔案。
- [shellcheck](https://github.com/koalaman/shellcheck): 一個靜態 shell 指令碼分析工具，本質上是 bash / sh / zsh 的 lint。
- [Filenames and Pathnames in Shell](http://www.dwheeler.com/essays/filenames-in-shell.html): 有關如何在 shell 指令碼裡正確處理檔名的細枝末節。
- [Data Science at the Command Line](http://datascienceatthecommandline.com/#tools): 用於數據科學的一些命令和工具，摘自同名書籍。

## 免責聲明

除去特別小的工作，你編寫的程式碼應當方便他人閱讀。能力往往伴隨著責任，你**有能力**在 Bash 中玩一些奇技淫巧並不意味著你應該去做！;)

## 授權條款

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

本文使用授權協議 [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/)。

<!--
  vim:	ft=markdown ic wrap noet norl sw=2 sts=2:
  -->
