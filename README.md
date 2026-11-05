# kyotobus_list
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>京都市バス 全系統乗車チェック表</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DotGothic16&family=Zen+Kaku+Gothic+New:wght@400;500;700;900&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #EBEEE9;
  --surface: #F6F8F5;
  --ink: #15271E;
  --muted: #5B685F;
  --line: #C5CEC6;
  --green: #0F7A4D;
  --stamp: #C03A2B;
  --plate: #161B18;
  --led: #FFB21A;
  --cell: #CBD4CC;
  --font-body: "Zen Kaku Gothic New", "Hiragino Kaku Gothic ProN", "Yu Gothic", "Noto Sans JP", sans-serif;
  --font-led: "DotGothic16", "Hiragino Kaku Gothic ProN", "Yu Gothic", "Noto Sans JP", sans-serif;
  color-scheme: light;
  box-sizing: border-box;
  padding-top: env(safe-area-inset-top, 0px);
  padding-bottom: env(safe-area-inset-bottom, 0px);
}
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg: #0F1713;
    --surface: #16211B;
    --ink: #E3EBE5;
    --muted: #92A097;
    --line: #2C3A32;
    --green: #45C08A;
    --stamp: #EE7060;
    --plate: #0A0D0B;
    --led: #FFB21A;
    --cell: #26342B;
    color-scheme: dark;
  }
}
:root[data-theme="dark"] {
  --bg: #0F1713;
  --surface: #16211B;
  --ink: #E3EBE5;
  --muted: #92A097;
  --line: #2C3A32;
  --green: #45C08A;
  --stamp: #EE7060;
  --plate: #0A0D0B;
  --led: #FFB21A;
  --cell: #26342B;
  color-scheme: dark;
}
html { scroll-padding-top: env(safe-area-inset-top, 0px); }
*, *::before, *::after { box-sizing: inherit; }
body {
  margin: 0;
  background: var(--bg);
  color: var(--ink);
  font-family: var(--font-body);
  font-size: 16px;
  line-height: 1.6;
  -webkit-text-size-adjust: 100%;
}
body::before {
  content: "";
  position: fixed;
  inset: 0 0 auto;
  height: env(safe-area-inset-top, 0px);
  background: var(--bg);
  z-index: 10;
}
.wrap { max-width: 720px; margin: 0 auto; padding: 0 16px; }

/* header */
.top { padding: 28px 0 12px; }
h1 {
  margin: 0 0 14px;
  font-size: clamp(1.45rem, 5.4vw, 2.1rem);
  font-weight: 900;
  line-height: 1.25;
  letter-spacing: 0.01em;
}
.count { display: flex; align-items: baseline; gap: 10px; margin: 0 0 12px; }
.count .done { font-family: var(--font-led); font-size: 3.2rem; line-height: 1; color: var(--green); }
.count .of { color: var(--muted); font-size: 1rem; }
.mosaic { display: flex; flex-wrap: wrap; gap: 3px; margin-bottom: 18px; }
.mosaic i { width: 11px; height: 11px; border-radius: 2px; background: var(--cell); }
.mosaic i.on { background: var(--green); }

.chips { display: flex; gap: 8px; overflow-x: auto; padding-bottom: 6px; scrollbar-width: thin; }
.chip {
  flex: none;
  font: inherit; font-size: 0.875rem; font-weight: 500;
  padding: 6px 14px; border-radius: 999px;
  border: 1px solid var(--line); background: transparent; color: var(--ink);
  cursor: pointer;
}
.chip[aria-pressed="true"] { background: var(--ink); color: var(--bg); border-color: var(--ink); }

/* sticky bar */
.bar {
  position: sticky; top: env(safe-area-inset-top, 0px);
  z-index: 11;
  background: var(--bg);
  border-bottom: 1px solid var(--line);
  padding: 10px 0;
}
.bar .wrap { display: flex; flex-wrap: wrap; gap: 8px; }
.seg { display: flex; border: 1px solid var(--line); border-radius: 8px; overflow: hidden; flex: none; }
.seg button {
  font: inherit; font-size: 0.875rem; font-weight: 500;
  padding: 8px 12px; border: 0; background: transparent; color: var(--muted);
  cursor: pointer;
}
.seg button + button { border-left: 1px solid var(--line); }
.seg button[aria-pressed="true"] { background: var(--green); color: #fff; }
:root[data-theme="dark"] .seg button[aria-pressed="true"] { color: #07130d; }
@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) .seg button[aria-pressed="true"] { color: #07130d; } }
.bar input {
  flex: 1; min-width: 140px;
  font: inherit; font-size: 0.9375rem;
  padding: 8px 12px; border-radius: 8px;
  border: 1px solid var(--line); background: var(--surface); color: var(--ink);
}
.bar input::placeholder { color: var(--muted); }
button:focus-visible, input:focus-visible, a:focus-visible { outline: 3px solid var(--green); outline-offset: 2px; }

/* list */
.sec { padding-top: 26px; }
.sec h2 { display: flex; align-items: baseline; justify-content: space-between; gap: 12px; margin: 0 0 4px; font-size: 1.125rem; font-weight: 900; }
.sec h2 .n { font-family: var(--font-led); font-weight: 400; font-size: 1.05rem; color: var(--muted); }
.sec h2 .n.full { color: var(--green); }
ul.rows { list-style: none; margin: 0; padding: 0; }
li.row {
  display: grid; grid-template-columns: 88px 1fr 60px; gap: 12px; align-items: center;
  padding: 12px 0; border-bottom: 1px solid var(--line);
  cursor: pointer; -webkit-tap-highlight-color: transparent;
}
li.row[hidden] { display: none; }
.plate {
  display: flex; align-items: baseline; justify-content: center; gap: 3px;
  min-height: 52px; padding: 8px 6px; border-radius: 7px;
  background: var(--plate); color: var(--led);
  font-family: var(--font-led);
  text-shadow: 0 0 8px rgba(255, 178, 26, 0.45);
  text-align: center; line-height: 1.1;
}
.plate small { font-size: 0.82rem; }
.plate b { font-weight: 400; font-size: 1.7rem; }
.plate.stack { flex-direction: column; align-items: center; gap: 2px; }
.plate.long b { font-size: 0.95rem; }
li.row.done .plate { opacity: 0.55; }
.via { font-size: 0.9rem; line-height: 1.55; min-width: 0; }
.meta { display: flex; align-items: center; gap: 10px; margin-top: 4px; font-size: 0.8125rem; color: var(--muted); }
.meta .t { border: 1px solid var(--line); border-radius: 4px; padding: 0 6px; }
.meta a { color: var(--green); text-underline-offset: 3px; }
.stamp {
  width: 56px; height: 56px; border-radius: 50%;
  border: 2px dashed var(--line); background: transparent;
  color: var(--stamp); padding: 0; cursor: pointer;
  display: grid; place-items: center;
  font: inherit;
}
.stamp span { display: none; }
li.row.done .stamp {
  border: 4px double var(--stamp);
  transform: rotate(-9deg);
  line-height: 1;
}
li.row.done .stamp span { display: block; }
.stamp .k { font-size: 1.35rem; font-weight: 900; }
.stamp .d { font-family: var(--font-led); font-size: 0.7rem; margin-top: 1px; }
.stamp.pop { animation: pop 0.28s cubic-bezier(.2, 1.4, .4, 1); }
@keyframes pop { from { transform: rotate(-9deg) scale(1.7); opacity: 0; } to { transform: rotate(-9deg) scale(1); opacity: 1; } }
@media (prefers-reduced-motion: reduce) { .stamp.pop { animation: none; } }

.stamp { position: relative; }
.stamp .b {
  position: absolute; top: -9px; right: -9px;
  min-width: 26px; padding: 1px 5px; border-radius: 999px;
  background: var(--green); color: var(--bg);
  font-family: var(--font-led); font-size: 0.72rem; line-height: 1.4; text-align: center;
  transform: rotate(9deg);
}
.memo-preview { margin-top: 4px; font-size: 0.8125rem; color: var(--muted); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.meta .more { font: inherit; font-size: 0.8125rem; padding: 0; border: 0; background: none; color: var(--green); text-decoration: underline; text-underline-offset: 3px; cursor: pointer; }
.panel { grid-column: 1 / -1; background: var(--surface); border: 1px solid var(--line); border-radius: 10px; padding: 12px; cursor: default; }
.panel[hidden] { display: none; }
.panel .none { margin: 0 0 10px; font-size: 0.875rem; color: var(--muted); }
.panel input, .panel textarea {
  font: inherit; font-size: 1rem; min-width: 0;
  padding: 7px 10px; border-radius: 8px;
  border: 1px solid var(--line); background: var(--bg); color: var(--ink);
}
.ride { display: grid; grid-template-columns: auto 1fr auto; gap: 8px; margin-bottom: 8px; }
@media (max-width: 440px) { .ride { grid-template-columns: 1fr auto; } .ride .rn { grid-column: 1 / -1; order: 3; } }
.panel button { font: inherit; font-size: 0.875rem; padding: 7px 12px; border-radius: 8px; border: 1px solid var(--line); background: transparent; color: var(--ink); cursor: pointer; }
.panel button.add { background: var(--ink); color: var(--bg); border-color: var(--ink); }
.panel .ml { display: block; margin-top: 12px; font-size: 0.8125rem; color: var(--muted); }
.panel textarea { display: block; width: 100%; margin-top: 4px; resize: vertical; }
.hint { margin: 0 0 14px; font-size: 0.875rem; color: var(--muted); max-width: 52ch; }
.mapsec { margin-top: 12px; }
.mapsec p { margin: 6px 0 12px; font-size: 0.9rem; max-width: 52ch; }
.maplink { display: inline-block; padding: 9px 16px; border-radius: 8px; background: var(--ink); color: var(--bg); font-size: 0.9rem; font-weight: 700; text-decoration: none; }
details.bk { margin-top: 14px; }
details.bk summary { cursor: pointer; }
details.bk textarea { display: block; width: 100%; margin-top: 8px; font: inherit; font-size: 0.8125rem; padding: 8px 10px; border-radius: 8px; border: 1px solid var(--line); background: var(--surface); color: var(--ink); }
details.bk .reset { margin-right: 8px; }
.empty { padding: 48px 0; text-align: center; color: var(--muted); }
.foot { padding: 36px 0 48px; font-size: 0.8125rem; color: var(--muted); }
.foot p { margin: 0 0 8px; max-width: 60ch; }
.foot a { color: var(--green); text-underline-offset: 3px; }
.reset {
  margin-top: 12px; font: inherit; font-size: 0.875rem;
  padding: 8px 14px; border-radius: 8px;
  border: 1px solid var(--line); background: transparent; color: var(--ink); cursor: pointer;
}
.reset.arm { border-color: var(--stamp); color: var(--stamp); }

@media print {
  body::before, .bar, .chips, .reset { display: none; }
  li.row { break-inside: avoid; }
}
</style>
</head>
<body>
<div class="wrap">
  <header class="top">
    <h1>京都市バス<br>全系統乗車チェック表</h1>
    <p class="count"><span class="done" id="done">0</span><span class="of">/ <span id="total">0</span> 系統に乗車(のべ <span id="rides">0</span> 回)</span></p>
    <div class="mosaic" id="mosaic" aria-hidden="true"></div>
    <p class="hint">判子を押すと今日乗ったことを記録できます。「記録・メモ」を開くと、乗った日の追加(何回でも)とメモを書けます。</p>
    <div class="chips" id="chips" role="group" aria-label="系統の種類で絞り込み"></div>
  </header>
</div>

<div class="bar">
  <div class="wrap">
    <div class="seg" id="seg" role="group" aria-label="乗車状況で絞り込み">
      <button type="button" data-s="all" aria-pressed="true">すべて</button>
      <button type="button" data-s="todo" aria-pressed="false">未乗車</button>
      <button type="button" data-s="done" aria-pressed="false">乗車済</button>
    </div>
    <input type="search" id="q" placeholder="系統番号・停留所・メモで探す" aria-label="系統番号・停留所・メモで探す" autocomplete="off">
  </div>
</div>

<main class="wrap">
  <div id="list"></div>
  <p class="empty" id="empty" hidden>条件に合う系統がありません。検索語や絞り込みを変えてみてください。</p>
  <section class="sec mapsec">
    <h2><span>路線図</span></h2>
    <p>系統ごとの路線を地図で確かめられる別サイトです。開いたあと、「系統別」の画面で系統を選んでください。データは2026年4月24日時点のもので、実際と違うことがあります。</p>
    <a class="maplink" href="https://bus.na16dream.com/" target="_blank" rel="noopener">京都市営バス GIS を開く</a>
  </section>
</main>

<footer class="wrap foot">
  <p>系統と経過地は京都市交通局「ハイパー市バスダイヤ 系統番号検索」(2026年11月5日時点)をもとにしています。<a href="https://www2.city.kyoto.lg.jp/kotsu/busdia/keitou/keitou.htm" target="_blank" rel="noopener">元のページ</a></p>
  <p>特・快速・EXなどは別の系統として数えています。運行日や運行時間帯が限られる系統は、乗る前に市バス時刻表で確認してください。同ページの系統選択に載っている「立命館」は詳細が見当たらなかったため、この表には入れていません。</p>
  <p>記録はこのブラウザに保存されます。別の端末とは共有されず、ブラウザのデータを消すと失われます。大事な記録は、下のバックアップで書き出しておけます。</p>
  <details class="bk">
    <summary>バックアップ(書き出し・読み込み)</summary>
    <textarea id="bk" rows="4" spellcheck="false" aria-label="バックアップ用の文字列"></textarea>
    <p><button type="button" class="reset" id="bkout">書き出す</button><button type="button" class="reset" id="bkin">この内容で上書き</button></p>
    <p id="bkmsg" role="status"></p>
  </details>
  <button type="button" class="reset" id="reset">すべての記録を消す</button>
</footer>

<script>
(function () {
  var KEY = "kyoto-bus-checks-v1";
  var BASE = "https://www2.city.kyoto.lg.jp/kotsu/busdia/keitou/kto/";
  var TYPES = { U: "均一", M: "多区間", C: "循環" };

  // code|prefix|number|type|via|label(optional)
  var GROUPS = [
    { chip: "基本", title: "基本系統(1〜93番)", rows: [
      "00100||1|U|西賀茂車庫前～千本北大路～洛北高校前～河原町今出川～出町柳駅前",
      "00300||3|U|松尾橋～京都外大前～四条河原町～河原町今出川～北白川仕伏町",
      "00400||4|U|西賀茂車庫前～深泥池～出町柳駅前～四条河原町～京都駅前",
      "00500||5|U|岩倉操車場前～銀閣寺道～三条京阪前～四条河原町～京都駅前",
      "00525|快速|5|U|岩倉操車場前～銀閣寺道～三条京阪前～四条河原町～河原町五条～京都駅前",
      "00600||6|U|京都駅前～四条大宮～千本北大路～佛教大学前～玄琢",
      "00684|快速|6|U|四条大宮～二条駅前～千本丸太町～千本今出川～ライトハウス前～千本北大路～佛教大学前",
      "00720||7|U|錦林車庫前～銀閣寺道～河原町今出川～四条河原町～京都駅前",
      "00800||8|U|四条烏丸～四条大宮～京都外大前～太秦天神川駅前～高雄～栂ノ尾",
      "00803|特|8|U|太秦天神川駅前～双ヶ丘～福王子～山越中町",
      "00900||9|U|西賀茂車庫前～上賀茂御薗橋～四条堀川～下京区総合庁舎前～京都駅前",
      "00985|快速|9|U|西賀茂車庫前～上賀茂御薗橋～四条堀川～下京区総合庁舎前～京都駅前",
      "01000||10|U|山越中町～北野白梅町～千本丸太町～河原町三条～三条京阪前",
      "01100||11|U|山越中町～嵐山～帷子ノ辻～太秦天神川駅前～西大路四条～四条河原町～三条京阪前",
      "01200||12|U|立命館大学前～金閣寺道～四条堀川～四条京阪前～三条京阪前",
      "01300||13|U|四条烏丸～西大路四条～西大路駅前～久世橋西詰～久世工業団地前",
      "01303|特|13|U|四条烏丸～西大路四条～西大路駅前～久世橋西詰～久我石原町",
      "01500||15|U|立命館大学前～西ノ京円町～二条駅前～京都市役所前～三条京阪前",
      "01595|快速|15|U|三条京阪前～二条駅前～西ノ京円町～立命館大学前(キャンパス内)",
      "01600||16|U|京都駅前～京都駅八条口～西寺前～市民防災センター前～地下鉄十条駅～河原町八条～京都駅八条口",
      "01800||18|U|二条駅西口～みぶ操車場前～四条大宮～羅城門～小枝橋～久我石原町",
      "01803|特|18|U|二条駅西口～みぶ操車場前～四条大宮～羅城門～上鳥羽村山町～久世橋東詰",
      "01900||19|M|横大路車庫前～中書島～国道大手筋～城南宮～九条車庫前～京都駅前",
      "02000||20|M|横大路車庫前～中書島～免許試験場前～京阪淀駅～南横大路～中書島～横大路車庫前",
      "02200||22|M|横大路車庫前～中書島～国道大手筋～八丁畷～羽束師～久我～南工業団地前",
      "02300||23|M|洛西バスターミナル～新林公団住宅前～千代原口～西京極～烏丸五条～京都駅前",
      "02500||25|U|京都外大前～西大路四条～西ノ京円町～金閣寺道～千本北大路～北大路バスターミナル",
      "02600||26|U|京都駅前～四条烏丸～西大路四条～西ノ京円町～山越中町",
      "02700||27|U|京都外大前～馬塚町～西大路四条～四条大宮～四条烏丸",
      "02703|特|27|U|京都外大前～馬塚町～西大路四条～西京極駅前",
      "02800||28|U|京都駅前～四条大宮～松尾大社前～阪急嵐山駅前～大覚寺",
      "02900||29|M|洛西バスターミナル～東新林町～千代原口～松尾大社前～四条烏丸",
      "03100||31|U|岩倉操車場前～一乗寺清水町～高野～祇園～四条烏丸",
      "03200||32|U|京都外大前～西京極～四条大宮～四条河原町～東天王町～銀閣寺前",
      "03300||33|M|洛西バスターミナル～三ノ宮～桂駅東口～西京極小学校前～西大路七条～京都駅前",
      "03303|特|33|M|洛西バスターミナル～三ノ宮～桂駅東口～川勝寺～西大路七条～京都駅前",
      "03700||37|U|西賀茂車庫前～北大路バスターミナル～出雲路橋～河原町三条～三条京阪前",
      "03703|特|37|U|柊野～西賀茂車庫前～大宮交通公園前～下岸町～北大路堀川～北大路バスターミナル",
      "04200||42|M|京都駅前～東寺東門前～南区総合庁舎前～市民防災センター前～久世橋西詰～ＪＲ桂川駅前～洛西口駅前",
      "04300||43|U|四条烏丸～烏丸五条～西大路五条～吉祥院天満宮前～久世橋東詰",
      "04600||46|U|西賀茂車庫前～佛教大学前～四条大宮～祇園～岡崎公園 美術館・平安神宮前",
      "05000||50|U|京都駅前～四条堀川～堀川中立売～北野天満宮前～立命館大学前",
      "05100||51|U|立命館大学前～北野白梅町～烏丸今出川～烏丸御池～四条河原町～三条京阪前",
      "05200||52|U|立命館大学前～北野白梅町～上七軒～七本松出水～丸太町七本松～四条大宮～四条烏丸",
      "05300||53|U|京都外大前～西大路四条～西ノ京円町～等持院道～立命館大学前",
      "05500||55|U|立命館大学前～北野白梅町～千本今出川～四条大宮～四条烏丸",
      "05800||58|U|九条車庫前～地下鉄九条駅前(大石橋)～東福寺～祇園～四条烏丸～京都駅前",
      "05900||59|U|山越中町～金閣寺道～千本北大路～千本今出川～河原町三条～三条京阪前",
      "06500||65|U|岩倉操車場前～上高野～左京区総合庁舎前～田中大久保町～熊野神社前～烏丸丸太町～四条烏丸",
      "06700||67|U|西賀茂車庫前～北大路堀川～四条大宮～京都外大前～松尾橋",
      "06900||69|M|二条駅西口～四条大宮～上野橋～千代原口～物集女～桂駅東口",
      "07000||70|M|太秦天神川駅前～梅津段町～上野橋～下桂～桂駅東口～ＪＲ桂川駅前～洛西口駅前～小畑川公園北口",
      "07100||71|U|九条車庫前～京都駅八条口～大石橋～九条大宮～四条大宮～四条葛野大路～松尾橋",
      "07103|特|71|U|九条車庫前～京都駅八条口～大石橋～九条大宮～四条大宮～四条葛野大路～太秦天神川駅前～松尾橋",
      "07500||75|U|京都駅前～堀川五条～西大路五条～太秦天神川駅前～双ヶ丘～山越中町",
      "07800||78|U|京都駅前～京都駅八条口～西大路九条～久世橋西詰～久世工業団地前",
      "08000||80|U|太秦天神川駅前～西京極～烏丸五条～河原町五条→五条坂→祇園→四条河原町→河原町五条",
      "08003|特|80|M|河原町三条～四条河原町→川田道→山科西野→山科団地→国道東野",
      "08100||81|U|横大路車庫前～中書島～棒鼻～京都駅八条口アバンティ前～塩小路高倉～京都駅前",
      "08400||84|U|太秦天神川駅前～吉祥院長田町～上鳥羽～地下鉄十条駅前～河原町十条～河原町八条～京都駅八条口～九条車庫前",
      "08500||85|U|京都駅前～堀川五条～西大路五条～太秦天神川駅前～双ヶ丘～嵯峨嵐山駅前～嵐山",
      "08600||86|U|梅小路公園・京都鉄道博物館前～京都駅～博物館三十三間堂前～東山七条～五条坂～祇園～東山三条～三条京阪前",
      "09100||91|U|四条烏丸～西大路四条～西ノ京円町～常盤・嵯峨野高校前～大覚寺",
      "09300||93|U|錦林車庫前～烏丸丸太町～西ノ京円町～常盤・嵯峨野高校前～嵐山",
      "09315|快速|93|U|嵐山～嵯峨小学校前～嵯峨戸川町～西ノ京円町～烏丸丸太町～東天王町～錦林車庫前"
    ]},
    { chip: "100番台", title: "100番台", rows: [
      "10050|EX|100|U|京都駅前～五条坂～祇園～岡崎公園 美術館・平安神宮前～銀閣寺前",
      "10100|EX|101|U|京都駅前～五条坂～京都駅前",
      "10200||102|U|北大路バスターミナル～金閣寺道～北野白梅町～北野天満宮前～烏丸今出川～出町柳駅前～銀閣寺道～錦林車庫前",
      "10500||105|U|京都駅前～烏丸五条～河原町五条～京都市役所前～岡崎公園 美術館・平安神宮前～銀閣寺前",
      "10600||106|U|京都駅前～七条京阪前～博物館三十三間堂前～五条坂～清水道～祇園～東山三条～三条京阪前"
    ]},
    { chip: "200番台", title: "200番台(循環系統)", rows: [
      "20100||201|C|みぶ操車場前～四条大宮～祇園～百万遍～千本今出川～みぶ操車場前",
      "20200||202|C|九条車庫前～西大路九条～西ノ京円町～熊野神社前～東福寺～九条車庫前",
      "20300||203|C|錦林車庫前～祇園～西大路四条～北野白梅町～銀閣寺道～錦林車庫前",
      "20400||204|C|北大路バスターミナル～金閣寺道～西ノ京円町～東天王町～高野～北大路バスターミナル",
      "20500||205|C|九条車庫前～京都駅前～四条河原町～洛北高校前～金閣寺道～西大路七条～京都駅前～九条車庫前",
      "20600||206|C|北大路バスターミナル～千本北大路～七条大宮･京都水族館前～京都駅前～東山七条～高野～北大路バスターミナル",
      "20700||207|C|九条車庫前～九条大宮～四条大宮～祇園～東福寺～九条車庫前",
      "20800||208|C|九条車庫前～東福寺～東山七条～西大路七条～西大路九条～九条車庫前"
    ]},
    { chip: "立命館ほか", title: "立命館系統・M1ほか", rows: [
      "D2050||立命館ダイレクト|U|西大路四条→西大路御池→西ノ京円町→立命館大学前",
      "K2020|快速立命館|202|U|九条車庫前～西大路駅前～西大路四条～北野白梅町～立命館大学前|快速202・立命館",
      "K2050|快速立命館|205|U|九条車庫前～京都駅前～立命館大学前|快速205・立命館",
      "M0010||M1|U|立命館大学前～わら天神前～衣笠氷室町～原谷～金閣寺道～千本北大路～北大路バスターミナル",
      "Z0550||四条烏丸→みぶ|U|四条烏丸～四条大宮～みぶ操車場前"
    ]},
    { chip: "北", title: "北系統", rows: [
      "N0010|北|1|U|北大路バスターミナル～北大路堀川～上堀川～紫野泉堂町～玄琢",
      "N0030|北|3|U|北大路バスターミナル～上賀茂御薗橋～柊野別れ～京都産大前",
      "N0080|北|8|U|北大路バスターミナル～佛教大学前～北山駅前～修学院道～一乗寺木ノ本町～北大路バスターミナル"
    ]},
    { chip: "南", title: "南系統", rows: [
      "S0010|南|1|M|竹田駅西口～パルスプラザ前～中久世～JR桂川駅前～下桂～桂駅東口",
      "S0013|特南|1|M|桂駅東口～ＪＲ桂川駅前～久世工業団地前",
      "S0020|南|2|M|竹田駅西口～パルスプラザ前～久我～免許試験場前～馬場～芝本～ＪＲ長岡京東口",
      "S0030|南|3|M|竹田駅西口～竹田浄菩提院町～パルスプラザ前～西大手筋～中書島～横大路車庫前",
      "S0050|南|5|U|横大路車庫前(一部の便)～竹田駅東口～深草西浦町～稲荷大社前～七条京阪前～京都駅前",
      "S0500|南|6|U|竹田駅東口→聖母女学院前→ＪＲ藤森駅前→藤森神社前→竹田駅東口",
      "S088D|南|8|M|横大路車庫前～府道横大路～八丁畷～三町～国道下鳥羽～中書島～丹波橋～竹田駅東口"
    ]},
    { chip: "西", title: "西系統", rows: [
      "W0010|西|1|M|洛西バスターミナル～東新林町～三ノ宮街道～桂駅西口",
      "W0020|西|2|M|洛西バスターミナル～境谷大橋～西竹の里町～三ノ宮街道～桂駅西口",
      "W0030|西|3|M|洛西バスターミナル～中央公園南口～南福西町二丁目～三ノ宮街道～桂駅西口",
      "W0033|特西|3|M|洛西バスターミナル～東新林町～福西竹の里～桂駅西口",
      "W0040|西|4|M|洛西バスターミナル～境谷大橋～北福西町一丁目～洛西口駅前～ＪＲ桂川駅前",
      "W0043|特西|4|M|洛西バスターミナル～洛西高校前～福西竹の里～向日回生病院前～JR桂川駅前",
      "W0050|西|5|M|桂坂中央～峰ヶ堂町一丁目～桂坂小学校前～桂坂口～境谷大橋～三ノ宮街道～桂駅西口",
      "W0053|特西|5|M|桂坂小学校～峰ヶ堂町三丁目～桂坂中央～洛西バスターミナル",
      "W0060|西|6|M|桂坂中央～西桂坂～峰ヶ堂町三丁目～桂御陵坂～京大桂キャンパス前～野田町～桂駅西口",
      "W0080|西|8|M|洛西バスターミナル～新林池公園～境谷大橋～福西竹の里～桂駅西口",
      "W0090|西|9|M|小畑川公園北口～洛西バスターミナル～右京の里～南春日町～境谷大橋～JR桂川駅前"
    ]}
  ];

  // state[id] = { r: [{ d: "YYYY-MM-DD", n: "ひとこと" }, ...], m: "系統のメモ" }
  var state = {};
  function clean(raw) {
    var out = {};
    if (!raw || typeof raw !== "object") return out;
    Object.keys(raw).forEach(function (id) {
      var v = raw[id], r = { r: [], m: "" };
      if (typeof v === "string") {
        r.r.push({ d: v.slice(0, 10), n: "" });
      } else if (v && typeof v === "object") {
        if (Array.isArray(v.r)) v.r.forEach(function (x) {
          if (x && typeof x.d === "string") r.r.push({ d: x.d.slice(0, 10), n: typeof x.n === "string" ? x.n.slice(0, 200) : "" });
        });
        if (typeof v.m === "string") r.m = v.m.slice(0, 2000);
      } else { return; }
      if (r.r.length || r.m) out[id] = r;
    });
    return out;
  }
  try { state = clean(JSON.parse(localStorage.getItem(KEY) || "{}")); } catch (e) { state = {}; }
  function save() { try { localStorage.setItem(KEY, JSON.stringify(state)); } catch (e) {} }
  function ensure(id) { return state[id] || (state[id] = { r: [], m: "" }); }
  function prune(id) { var s = state[id]; if (s && !s.r.length && !s.m) delete state[id]; }
  function isDone(id) { return !!(state[id] && state[id].r.length); }
  function extra(id) { var s = state[id]; return s ? norm(s.m + " " + s.r.map(function (x) { return x.n; }).join(" ")) : ""; }

  function today() {
    var d = new Date();
    return d.getFullYear() + "-" + String(d.getMonth() + 1).padStart(2, "0") + "-" + String(d.getDate()).padStart(2, "0");
  }
  function norm(s) { return String(s).normalize("NFKC").toLowerCase(); }
  function esc(s) { return String(s).replace(/[&<>"]/g, function (c) { return { "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;" }[c]; }); }

  var all = [];
  var listEl = document.getElementById("list");
  var html = "";
  GROUPS.forEach(function (g, gi) {
    g.items = g.rows.map(function (line) {
      var f = line.split("|");
      var it = { id: f[0], p: f[1], n: f[2], t: f[3], via: f[4], name: f[5] || (f[1] + f[2]), g: gi };
      it.hay = norm(it.name + " " + it.via);
      all.push(it);
      return it;
    });
    html += '<section class="sec" data-g="' + gi + '"><h2><span>' + esc(g.title) + '</span><span class="n" id="gn' + gi + '"></span></h2><ul class="rows">';
    g.items.forEach(function (it) {
      var long = it.n.length > 3 ? " long" : "";
      var stack = it.p.length >= 3 ? " stack" : "";
      html += '<li class="row" data-id="' + it.id + '">' +
        '<span class="plate' + long + stack + '">' + (it.p ? "<small>" + esc(it.p) + "</small>" : "") + "<b>" + esc(it.n) + "</b></span>" +
        '<div class="via">' + esc(it.via) +
        '<div class="meta"><span class="t">' + TYPES[it.t] + '</span><a href="' + BASE + it.id + '.htm" target="_blank" rel="noopener">停留所一覧</a><button type="button" class="more" aria-expanded="false">記録・メモ</button></div>' +
        '<div class="memo-preview" hidden></div></div>' +
        '<button type="button" class="stamp" aria-pressed="false" aria-label="' + esc(it.name) + 'を今日乗車したことにする"><span class="k">乗</span><span class="d"></span><span class="b"></span></button>' +
        '<div class="panel" hidden></div>' +
        "</li>";
    });
    html += "</ul></section>";
  });
  listEl.innerHTML = html;

  var mosaic = document.getElementById("mosaic");
  mosaic.innerHTML = all.map(function () { return "<i></i>"; }).join("");
  var cells = mosaic.children;
  document.getElementById("total").textContent = all.length;

  var chipsEl = document.getElementById("chips");
  chipsEl.innerHTML = '<button type="button" class="chip" data-g="all" aria-pressed="true">すべて</button>' +
    GROUPS.map(function (g, i) { return '<button type="button" class="chip" data-g="' + i + '" aria-pressed="false">' + esc(g.chip) + "</button>"; }).join("");

  var rowEls = {};
  Array.prototype.forEach.call(listEl.querySelectorAll("li.row"), function (li) { rowEls[li.dataset.id] = li; });

  function latest(s) { var d = ""; s.r.forEach(function (x) { if (x.d > d) d = x.d; }); return d; }
  function paintRow(it) {
    var li = rowEls[it.id], s = state[it.id], n = s ? s.r.length : 0, on = n > 0;
    li.classList.toggle("done", on);
    var b = li.querySelector(".stamp");
    b.setAttribute("aria-pressed", on ? "true" : "false");
    b.setAttribute("aria-label", on ? it.name + "の記録を開く(乗車" + n + "回)" : it.name + "を今日乗車したことにする");
    var last = on ? latest(s) : "";
    b.querySelector(".d").textContent = last ? last.slice(5).replace("-", ".") : "";
    var bd = b.querySelector(".b");
    bd.textContent = n > 1 ? "×" + n : "";
    bd.style.display = n > 1 ? "block" : "none";
    var pv = li.querySelector(".memo-preview");
    var m = s && s.m ? s.m.trim() : "";
    pv.hidden = !m;
    pv.textContent = m ? "メモ: " + m.split("\n")[0] : "";
  }
  function refreshAll() {
    all.forEach(function (it) {
      paintRow(it);
      if (rowEls[it.id].classList.contains("open")) renderPanel(it);
    });
    paintCounts();
  }
  function paintCounts() {
    var n = 0, total = 0;
    all.forEach(function (it, i) {
      var on = isDone(it.id);
      if (on) { n++; total += state[it.id].r.length; }
      cells[i].className = on ? "on" : "";
    });
    document.getElementById("done").textContent = n;
    document.getElementById("rides").textContent = total;
    GROUPS.forEach(function (g, gi) {
      var c = g.items.filter(function (it) { return isDone(it.id); }).length;
      var el = document.getElementById("gn" + gi);
      el.textContent = c + "/" + g.items.length;
      el.classList.toggle("full", c === g.items.length);
    });
  }

  var f = { s: "all", g: "all", q: "" };
  function applyFilter() {
    var q = norm(f.q).trim(), shown = 0;
    GROUPS.forEach(function (g, gi) {
      var vis = 0;
      g.items.forEach(function (it) {
        var on = isDone(it.id);
        var ok = (f.g === "all" || String(gi) === f.g) &&
          (f.s === "all" || (f.s === "done") === on) &&
          (!q || (it.hay + " " + extra(it.id)).indexOf(q) !== -1);
        rowEls[it.id].hidden = !ok;
        if (ok) vis++;
      });
      listEl.querySelector('section[data-g="' + gi + '"]').hidden = vis === 0;
      shown += vis;
    });
    document.getElementById("empty").hidden = shown !== 0;
  }

  function byId(id) { return all.filter(function (x) { return x.id === id; })[0]; }
  function renderPanel(it) {
    var p = rowEls[it.id].querySelector(".panel"), s = state[it.id] || { r: [], m: "" };
    var rs = s.r.map(function (x, i) { return { x: x, i: i }; }).sort(function (a, b) { return a.x.d < b.x.d ? 1 : a.x.d > b.x.d ? -1 : 0; });
    var h = '<div class="rides">';
    if (!rs.length) h += '<p class="none">まだ乗車記録がありません。</p>';
    rs.forEach(function (o) {
      h += '<div class="ride" data-i="' + o.i + '"><input type="date" class="rd" value="' + esc(o.x.d) + '" aria-label="乗車日">' +
        '<input type="text" class="rn" value="' + esc(o.x.n) + '" maxlength="200" placeholder="ひとこと(区間・混み具合など)" aria-label="この乗車のひとこと">' +
        '<button type="button" class="del" aria-label="この乗車記録を削除">削除</button></div>';
    });
    h += '</div><div><button type="button" class="add">今日乗った</button></div>' +
      '<label class="ml">この系統のメモ<textarea class="memo" rows="3" maxlength="2000" placeholder="気づいたこと、また乗りたい区間、見つけた景色など"></textarea></label>';
    p.innerHTML = h;
    p.querySelector(".memo").value = s.m;
  }
  function setOpen(li, open) {
    var it = byId(li.dataset.id), p = li.querySelector(".panel");
    if (open) renderPanel(it);
    p.hidden = !open;
    li.classList.toggle("open", open);
    li.querySelector(".more").setAttribute("aria-expanded", open ? "true" : "false");
  }
  listEl.addEventListener("click", function (e) {
    if (e.target.closest("a")) return;
    var li = e.target.closest("li.row");
    if (!li) return;
    var it = byId(li.dataset.id), t = e.target;
    if (t.closest(".panel")) {
      if (t.closest(".add")) {
        ensure(it.id).r.push({ d: today(), n: "" });
      } else if (t.closest(".del")) {
        state[it.id].r.splice(+t.closest(".ride").dataset.i, 1);
        prune(it.id);
      } else { return; }
      save(); renderPanel(it); paintRow(it); paintCounts();
      return;
    }
    var stamp = t.closest(".stamp");
    if (stamp && !isDone(it.id)) {
      ensure(it.id).r.push({ d: today(), n: "" });
      save(); paintRow(it); paintCounts();
      if (li.classList.contains("open")) renderPanel(it);
      stamp.classList.remove("pop"); void stamp.offsetWidth; stamp.classList.add("pop");
      return;
    }
    setOpen(li, !li.classList.contains("open"));
  });
  listEl.addEventListener("input", function (e) {
    var t = e.target, li = t.closest("li.row");
    if (!li) return;
    var it = byId(li.dataset.id);
    if (t.classList.contains("memo")) {
      ensure(it.id).m = t.value; prune(it.id); save(); paintRow(it);
    } else if (t.classList.contains("rn")) {
      state[it.id].r[+t.closest(".ride").dataset.i].n = t.value; save();
    }
  });
  listEl.addEventListener("change", function (e) {
    var t = e.target;
    if (!t.classList.contains("rd")) return;
    var li = t.closest("li.row"), it = byId(li.dataset.id), r = state[it.id].r[+t.closest(".ride").dataset.i];
    if (!t.value) { t.value = r.d; return; }
    r.d = t.value; save(); paintRow(it);
  });

  document.getElementById("seg").addEventListener("click", function (e) {
    var b = e.target.closest("button"); if (!b) return;
    f.s = b.dataset.s;
    Array.prototype.forEach.call(this.children, function (x) { x.setAttribute("aria-pressed", x === b ? "true" : "false"); });
    applyFilter();
  });
  chipsEl.addEventListener("click", function (e) {
    var b = e.target.closest("button"); if (!b) return;
    f.g = b.dataset.g;
    Array.prototype.forEach.call(this.children, function (x) { x.setAttribute("aria-pressed", x === b ? "true" : "false"); });
    applyFilter();
  });
  document.getElementById("q").addEventListener("input", function () { f.q = this.value; applyFilter(); });

  var resetBtn = document.getElementById("reset"), armTimer = null;
  resetBtn.addEventListener("click", function () {
    if (!resetBtn.classList.contains("arm")) {
      resetBtn.classList.add("arm");
      resetBtn.textContent = "本当に消す(もう一度押すと実行)";
      armTimer = setTimeout(disarm, 4000);
      return;
    }
    clearTimeout(armTimer);
    state = {}; save();
    refreshAll(); applyFilter(); disarm();
  });
  function disarm() {
    resetBtn.classList.remove("arm");
    resetBtn.textContent = "すべての記録を消す";
  }

  var bk = document.getElementById("bk"), bkMsg = document.getElementById("bkmsg");
  document.getElementById("bkout").addEventListener("click", function () {
    bk.value = JSON.stringify(state);
    bk.focus(); bk.select();
    bkMsg.textContent = "書き出しました。この文字列をメモ帳などに保存しておくと、あとで読み込めます。";
  });
  document.getElementById("bkin").addEventListener("click", function () {
    try {
      var parsed = clean(JSON.parse(bk.value)), next = {};
      Object.keys(parsed).forEach(function (id) { if (byId(id)) next[id] = parsed[id]; });
      state = next; save(); refreshAll(); applyFilter();
      bkMsg.textContent = Object.keys(next).length + "系統分の記録を読み込みました(これまでの記録は置き換わっています)。";
    } catch (err) {
      bkMsg.textContent = "読み込めませんでした。書き出した文字列をそのまま貼り付けてください。";
    }
  });

  all.forEach(paintRow);
  paintCounts();
  applyFilter();
})();
</script>
</body>
</html>
