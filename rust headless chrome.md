# Rust headless chrome

## 基本

chrome devtools protocolに基づいたクロームの操作が可能、puppeteerの類型

## ER

```
browser - tab1
        - tab2
        
WaitingCallRegistry call_id -> Response

```



```
Browser::default().unwrap() // ブラウザを生成して制御・オプションなし
Browser::new(Options).unwrap() // ブラウザを生成して制御・オプションあり
Browser::connect(ws_url).unwrap() // 外部プロセスのdebugger wsに接続する
browser.get_tabs()
```



```sh
curl -O https://aur.archlinux.org/cgit/aur.git/snapshot/google-chrome.tar.gz
tar -xvf google-chrome.tar.gz

google-chrome-stable --remote-debugging-port=9222 --user-data-dir=/tmp
http://localhost:9222/json
```

sandbox

https://unix.stackexchange.com/questions/68832/what-does-the-chromium-option-no-sandbox-mean



https://www.google.com/googlebooks/chrome/med_00.html