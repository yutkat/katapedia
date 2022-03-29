# Webブラウザ

## Chrome/Chromium

### よく使うショートカット

- Ctrl-t: 新しいタブで開く
- Ctrl-w: タブを閉じる
- Ctrl-Tab: タブ移動
- Ctrl-Shift-Tab: 前のタブに移動
- Ctrl-Shift-PageUp/Down: タブの位置を移動
- Alt-d or Ctrl-l: アドレスバーに移動
- Ctrl-N: 新しいウィンドウで開く
- Ctrl-l -> Shift-Enter: 現在のタブを新しいウィンドウで開く


### 拡張機能

#### Vimium

fでリンクを検索できるのが便利

## Dev tools

### すべてのdetailsをtoggle

```javascript
document.body.querySelectorAll('article details').forEach((e) => (e.hasAttribute('open')) ? e.removeAttribute('open') : e.setAttribute('open',true))
```

