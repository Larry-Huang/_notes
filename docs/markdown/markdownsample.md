---
layout: default
title: markdown_sample
parent: markdown
nav_order: 1
---


### Href Link

```
[這是一個行內連結](https://www.google.com)

[這是一個帶有標題的行內連結](https://www.google.com "Google's Homepage")

[這是一個參考連結][Arbitrary case-insensitive reference text]

[這是一個對應到 Git 倉儲檔案的相對參考連結](../blob/master/LICENSE)

[參考標的物也可以使用數字][1]

直接使用文字對應也可以 [這段文字連到參考項目]

參考項目可以寫在文檔的最後，有點像書內的註解（註腳）。

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: http://slashdot.org
[這段文字連到參考項目]: http://www.reddit.com
```
[這是一個行內連結](https://www.google.com)

[這是一個帶有標題的行內連結](https://www.google.com "Google's Homepage")

[這是一個參考連結][Arbitrary case-insensitive reference text]

[這是一個對應到 Git 倉儲檔案的相對參考連結](../blob/master/LICENSE)

[參考標的物也可以使用數字][1]

直接使用文字對應也可以 [這段文字連到參考項目]

參考項目可以寫在文檔的最後，有點像書內的註解（註腳）。

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: http://slashdot.org
[這段文字連到參考項目]: http://www.reddit.com

```
I get 10 times more traffic from [Google][] than from
[Yahoo][] or [MSN][].

  [google]: http://google.com/        "Google"
  [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
  [msn]:    http://search.msn.com/    "MSN Search"

```
I get 10 times more traffic from [Google][] than from
[Yahoo][] or [MSN][].

  [google]: http://google.com/        "Google"
  [yahoo]:  http://search.yahoo.com/  "Yahoo Search"
  [msn]:    http://search.msn.com/    "MSN Search"

```
<http://example.com/>
```
<http://example.com/>
---

### stronger
```
*single asterisks*

_single underscores_

**double asterisks**

__double underscores__
```
*single asterisks*

_single underscores_

**double asterisks**

__double underscores__

---
### Code
Use the `printf()` function.


### Youtube Link
```
<a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE
" target="_blank"><img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg"
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg)](http://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE)
```
<a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID_HERE
" target="_blank"><img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg"
alt="IMAGE ALT TEXT HERE" width="240" height="180" border="10" /></a>

[![IMAGE ALT TEXT HERE](http://img.youtube.com/vi/YOUTUBE_VIDEO_ID_HERE/0.jpg)](http://www.youtube.com/watch?v=YOUTUBE_VIDEO_ID_HERE)