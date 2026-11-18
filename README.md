<!doctype html>
<html lang="uz">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Offlayn o'yinlar</title>
<style>
/* Layout: bosh sahifada o'yinlar kartalari, o'yin ichida orqaga tugmasi va bitta taxta. Hammasi internetsiz ishlaydi, tashqi shrift yo'q. */
:root {
  --bg: #eef1ec;
  --fg: #1b2420;
  --muted: #5d6b64;
  --card: #f8faf7;
  --board: #dde5dc;
  --board-alt: #d1dcd0;
  --snake: #1f6b52;
  --snake-head: #123f30;
  --food: #d6452a;
  --line: #c3cfc4;
  --slab: #a9b8ab;
  --btn: #1b2420;
  --btn-fg: #eef1ec;
  --font: ui-rounded, "Avenir Next", "Segoe UI", system-ui, -apple-system, Roboto, sans-serif;
}
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --bg: #0f1512; --fg: #e5ece7; --muted: #92a49a; --card: #1b2721;
    --board: #16201b; --board-alt: #1f2d25;
    --snake: #5fcf9c; --snake-head: #a8f0cc; --food: #ff7a59;
    --line: #2a3a31; --slab: #080c0a; --btn: #e5ece7; --btn-fg: #0f1512;
    color-scheme: dark;
  }
}
:root[data-theme="dark"] {
  --bg: #0f1512; --fg: #e5ece7; --muted: #92a49a; --card: #1b2721;
  --board: #16201b; --board-alt: #1f2d25;
  --snake: #5fcf9c; --snake-head: #a8f0cc; --food: #ff7a59;
  --line: #2a3a31; --slab: #080c0a; --btn: #e5ece7; --btn-fg: #0f1512;
  color-scheme: dark;
}
* { box-sizing: border-box; }
body { margin: 0; }
body {
  background: var(--bg);
  color: var(--fg);
  font-family: var(--font);
  font-size: 15px;
  padding-block: 20px 28px;
  padding-left: max(16px, env(safe-area-inset-left, 0px));
  padding-right: max(16px, env(safe-area-inset-right, 0px));
}
.wrap { max-width: 440px; margin: 0 auto; display: flex; flex-direction: column; gap: 16px; }
h1 { font-size: 30px; line-height: 1.05; margin: 0; letter-spacing: -0.02em; font-weight: 800; text-wrap: balance; }
.top { display: flex; align-items: center; gap: 14px; flex-wrap: wrap; }
.top h1 { font-size: 26px; }
.sub { margin: 6px 0 0; color: var(--muted); display: flex; align-items: center; gap: 8px; }
.dot { width: 9px; height: 9px; border-radius: 50%; background: var(--snake); flex: none; }
button {
  font: inherit; font-weight: 600; cursor: pointer; border: 2px solid var(--btn); background: var(--btn); color: var(--btn-fg);
  border-radius: 6px; padding: 10px 18px; min-height: 44px;
}
button.ghost { background: transparent; color: var(--fg); border-color: var(--line); }
button:focus-visible { outline: 3px solid var(--food); outline-offset: 2px; }

/* bosh sahifa */
.cards { display: grid; grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 12px; }
.card { display: flex; flex-direction: column; gap: 8px; align-items: stretch; text-align: left; padding: 12px; background: var(--card); color: var(--fg); border: 2px solid var(--line); border-radius: 10px; font-weight: 400; min-width: 0; }
.card:hover { border-color: var(--fg); }
.card .ico { aspect-ratio: 1 / 1; display: grid; place-items: center; background: var(--board); border-radius: 6px; }
.card svg { width: 68%; height: 68%; }
.card b { font-size: 17px; font-weight: 700; }
.card small { color: var(--muted); font-size: 12.5px; }

/* o'yin umumiy */
.stats { display: flex; gap: 22px; flex-wrap: wrap; }
.stat { display: flex; flex-direction: column; gap: 2px; }
.stat span:first-child { font-size: 11px; letter-spacing: 0.08em; text-transform: uppercase; color: var(--muted); }
.stat span:last-child { font-size: 26px; font-weight: 800; font-variant-numeric: tabular-nums; line-height: 1; }
.board { position: relative; width: 100%; aspect-ratio: 1 / 1; border: 2px solid var(--fg); border-radius: 6px; overflow: hidden; background: var(--board); box-shadow: 0 8px 0 var(--slab); margin-bottom: 8px; }
.board.swipe { touch-action: none; }
.stage { perspective: 1200px; padding-bottom: 6px; }
.tilt { transform: rotateX(14deg); transform-origin: 50% 100%; box-shadow: 0 9px 0 var(--slab), 0 22px 26px -10px rgba(0, 0, 0, 0.35); }
canvas { display: block; width: 100%; height: 100%; touch-action: none; }
.overlay { position: absolute; inset: 0; display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 10px; background: color-mix(in srgb, var(--bg) 84%, transparent); text-align: center; padding: 16px; z-index: 2; }
.overlay[hidden] { display: none !important; }
.overlay h2 { font-size: 28px; margin: 0; text-wrap: balance; }
.overlay p { margin: 0; color: var(--muted); }
.controls { display: flex; gap: 16px; align-items: center; justify-content: space-between; flex-wrap: wrap; }
.side { display: flex; flex-direction: column; gap: 10px; min-width: 0; }
.side label { display: flex; gap: 8px; align-items: center; cursor: pointer; }
.side input { width: 18px; height: 18px; accent-color: var(--snake); }
.pad { display: grid; grid-template-columns: repeat(3, 52px); grid-template-rows: repeat(2, 52px); gap: 6px; }
.pad button { padding: 0; font-size: 20px; min-height: 0; }
.pad .up { grid-column: 2; grid-row: 1; }
.pad .left { grid-column: 1; grid-row: 2; }
.pad .down { grid-column: 2; grid-row: 2; }
.pad .right { grid-column: 3; grid-row: 2; }
.hint { margin: 0; color: var(--muted); font-size: 13px; line-height: 1.5; }
.status { margin: 0; text-align: center; font-weight: 700; min-height: 1.5em; }

/* 2048 */
.grid4 { position: absolute; inset: 0; display: grid; grid-template-columns: repeat(4, 1fr); grid-template-rows: repeat(4, 1fr); gap: 8px; padding: 8px; }
.tile { display: grid; place-items: center; border-radius: 6px; font-weight: 800; font-size: clamp(22px, 8vw, 36px); font-variant-numeric: tabular-nums; background: var(--board-alt); min-width: 0; }
.tile.sm { font-size: clamp(18px, 6.4vw, 28px); }
.tile.xs { font-size: clamp(14px, 4.8vw, 22px); }

/* xotira */
.memgrid { position: absolute; inset: 0; display: grid; grid-template-columns: repeat(4, 1fr); grid-template-rows: repeat(4, 1fr); gap: 8px; padding: 8px; }
.mem { padding: 0; min-height: 0; border: 0; border-radius: 8px; background: var(--btn); display: grid; place-items: center; transition: background 0.18s; }
.mem svg { width: 62%; height: 62%; opacity: 0; transition: opacity 0.18s; }
.mem.open { background: var(--card); }
.mem.done { background: var(--board-alt); cursor: default; }
.mem.open svg, .mem.done svg { opacity: 1; }

/* krestik-nolik */
.tgrid { position: absolute; inset: 0; display: grid; grid-template-columns: repeat(3, 1fr); grid-template-rows: repeat(3, 1fr); gap: 8px; padding: 8px; }
.cell { padding: 0; min-height: 0; border: 0; border-radius: 8px; background: var(--card); color: var(--fg); font-size: clamp(40px, 17vw, 72px); font-weight: 800; line-height: 1; }
.cell.x { color: var(--food); }
.cell.o { color: var(--snake); }
.cell.win { outline: 4px solid var(--fg); outline-offset: -4px; }

@media (prefers-reduced-motion: reduce) { * { transition: none !important; } }
</style>
</head>
<body>

<div class="wrap" id="app"></div>

<script>
(function () {
  "use strict";
  var app = document.getElementById("app");

  /* ---------- umumiy yordamchilar ---------- */
  var mem = {};
  var store = {
    get: function (k) {
      try { var v = localStorage.getItem("oyin-" + k); if (v !== null) return parseInt(v, 10) || 0; } catch (e) {}
      return mem[k] || 0;
    },
    set: function (k, v) {
      mem[k] = v;
      try { localStorage.setItem("oyin-" + k, String(v)); } catch (e) {}
    }
  };

  function makeScope() {
    var offs = [], timers = [];
    return {
      alive: true,
      on: function (t, ev, fn, opt) {
        t.addEventListener(ev, fn, opt);
        offs.push(function () { t.removeEventListener(ev, fn, opt); });
      },
      later: function (fn, ms) { var id = setTimeout(fn, ms); timers.push(id); return id; },
      destroy: function () {
        this.alive = false;
        offs.forEach(function (f) { f(); });
        timers.forEach(clearTimeout);
        offs = []; timers = [];
      }
    };
  }

  function $(root, sel) { return root.querySelector(sel); }
  function rnd(n) { return Math.floor(Math.random() * n); }

  function swipe(S, el, cb) {
    var sx = 0, sy = 0, on = false;
    S.on(el, "touchstart", function (e) {
      var t = e.changedTouches[0]; sx = t.clientX; sy = t.clientY; on = true;
    }, { passive: true });
    S.on(el, "touchend", function (e) {
      if (!on) return;
      on = false;
      var t = e.changedTouches[0], dx = t.clientX - sx, dy = t.clientY - sy;
      if (Math.max(Math.abs(dx), Math.abs(dy)) < 24) return;
      if (Math.abs(dx) > Math.abs(dy)) cb(dx > 0 ? "r" : "l"); else cb(dy > 0 ? "d" : "u");
    }, { passive: true });
  }

  var KEYDIR = {
    ArrowUp: "u", w: "u", W: "u", ArrowDown: "d", s: "d", S: "d",
    ArrowLeft: "l", a: "l", A: "l", ArrowRight: "r", d: "r", D: "r"
  };
  var VEC = { l: [-1, 0], r: [1, 0], u: [0, -1], d: [0, 1] };

  /* ---------- ILON ---------- */
  function snakeGame(host, S) {
    var N = 18;
    host.innerHTML =
      '<div class="stats"><div class="stat"><span>Hisob</span><span id="sc">0</span></div>' +
      '<div class="stat"><span>Rekord</span><span id="bs">0</span></div></div>' +
      '<div class="stage"><div class="board tilt"><canvas id="cv" aria-label="Ilon o‘yini maydoni"></canvas>' +
      '<div class="overlay" id="ov"><h2 id="ovt">Tayyormisiz?</h2><p id="ovp">Olmalarni yeng, o‘zingizga urilmang.</p>' +
      '<button id="ovb" type="button">Boshlash</button></div></div></div>' +
      '<div class="controls"><div class="pad" aria-label="Yo‘nalish tugmalari">' +
      '<button class="up" type="button" data-d="u" aria-label="Yuqoriga">↑</button>' +
      '<button class="left" type="button" data-d="l" aria-label="Chapga">←</button>' +
      '<button class="down" type="button" data-d="d" aria-label="Pastga">↓</button>' +
      '<button class="right" type="button" data-d="r" aria-label="O‘ngga">→</button></div>' +
      '<div class="side"><button class="ghost" id="pz" type="button">Pauza</button>' +
      '<label for="wr"><input type="checkbox" id="wr"> Devordan o‘tish</label></div></div>' +
      '<p class="hint">Strelkalar yoki W A S D bilan boshqaring, bo‘sh joy pauza. Telefonda maydon ustida barmoq bilan suring. Har bir olmadan keyin ilon tezlashadi.</p>';

    var cv = $(host, "#cv"), ctx = cv.getContext("2d");
    var scoreEl = $(host, "#sc"), bestEl = $(host, "#bs");
    var overlay = $(host, "#ov"), ovT = $(host, "#ovt"), ovP = $(host, "#ovp"), ovB = $(host, "#ovb");
    var pauseBtn = $(host, "#pz"), wrapBox = $(host, "#wr");
    var snake, dir, queue, food, score, best = 0, state = "ready", speed = 140, acc = 0, last = 0, raf = 0;

    function bestKey() { return wrapBox.checked ? "ilon-otish" : "ilon-devor"; }
    function loadBest() { best = store.get(bestKey()); bestEl.textContent = best; }

    function placeFood() {
      var free = [];
      for (var x = 0; x < N; x++) for (var y = 0; y < N; y++) {
        var taken = false;
        for (var i = 0; i < snake.length; i++) if (snake[i].x === x && snake[i].y === y) { taken = true; break; }
        if (!taken) free.push({ x: x, y: y });
      }
      food = free.length ? free[rnd(free.length)] : null;
    }
    function reset() {
      snake = [{ x: 8, y: 9 }, { x: 7, y: 9 }, { x: 6, y: 9 }];
      dir = { x: 1, y: 0 }; queue = []; score = 0; speed = 140; acc = 0;
      scoreEl.textContent = 0;
      placeFood();
    }
    function showOverlay(t, p, b) { ovT.textContent = t; ovP.textContent = p; ovB.textContent = b; overlay.hidden = false; }
    function start() {
      if (state === "ready" || state === "over") reset();
      state = "running"; overlay.hidden = true; pauseBtn.textContent = "Pauza";
      last = performance.now(); acc = 0;
    }
    function pause() {
      if (state !== "running") return;
      state = "paused";
      showOverlay("Pauza", "Davom etish uchun tugmani bosing.", "Davom etish");
      pauseBtn.textContent = "Davom etish";
    }
    function togglePause() { if (state === "running") pause(); else if (state === "paused") start(); }
    function gameOver(won) {
      state = "over";
      var record = score > best;
      if (record) { best = score; bestEl.textContent = best; store.set(bestKey(), best); }
      showOverlay(won ? "G‘alaba!" : "O‘yin tugadi", "Hisob: " + score + (record && score > 0 ? " — yangi rekord" : ""), "Qayta o‘ynash");
    }
    function setDir(nx, ny) {
      var ref = queue.length ? queue[queue.length - 1] : dir;
      if ((ref.x === -nx && ref.y === -ny) || (ref.x === nx && ref.y === ny)) return;
      if (queue.length < 3) queue.push({ x: nx, y: ny });
      if (state === "ready") start();
    }
    function step() {
      if (queue.length) dir = queue.shift();
      var hx = snake[0].x + dir.x, hy = snake[0].y + dir.y;
      if (wrapBox.checked) { hx = (hx + N) % N; hy = (hy + N) % N; }
      else if (hx < 0 || hy < 0 || hx >= N || hy >= N) return gameOver(false);
      var eats = food && hx === food.x && hy === food.y;
      var bodyLen = eats ? snake.length : snake.length - 1;
      for (var i = 0; i < bodyLen; i++) if (snake[i].x === hx && snake[i].y === hy) return gameOver(false);
      snake.unshift({ x: hx, y: hy });
      if (eats) {
        score++; scoreEl.textContent = score;
        speed = Math.max(70, 140 - score * 3);
        placeFood();
        if (!food) return gameOver(true);
      } else snake.pop();
    }

    function color(n) { return getComputedStyle(document.documentElement).getPropertyValue(n).trim(); }
    function shade(hex, f) {
      var m = /^#?([0-9a-f]{2})([0-9a-f]{2})([0-9a-f]{2})$/i.exec(hex);
      if (!m) return hex;
      var c = [1, 2, 3].map(function (k) {
        var v = parseInt(m[k], 16);
        v = f < 1 ? v * f : v + (255 - v) * (f - 1);
        return Math.max(0, Math.min(255, Math.round(v)));
      });
      return "rgb(" + c.join(",") + ")";
    }
    function resize() {
      var w = Math.round(cv.clientWidth * (window.devicePixelRatio || 1));
      if (cv.width !== w) { cv.width = w; cv.height = w; }
    }
    function roundRect(x, y, w, h, r) {
      ctx.beginPath();
      ctx.moveTo(x + r, y);
      ctx.arcTo(x + w, y, x + w, y + h, r);
      ctx.arcTo(x + w, y + h, x, y + h, r);
      ctx.arcTo(x, y + h, x, y, r);
      ctx.arcTo(x, y, x + w, y, r);
      ctx.closePath();
    }
    function drawBlock(gx, gy, s, base) {
      var pad = s * 0.07, d = s * 0.2;
      var x = gx * s + pad, y = gy * s + pad, w = s - pad * 2, h = s - pad * 2 - d, r = s * 0.26;
      ctx.fillStyle = "rgba(0,0,0,0.18)";
      roundRect(x + s * 0.03, y + d + s * 0.05, w, h, r); ctx.fill();
      ctx.fillStyle = shade(base, 0.62);
      roundRect(x, y + d, w, h, r); ctx.fill();
      var g = ctx.createLinearGradient(0, y, 0, y + h);
      g.addColorStop(0, shade(base, 1.28)); g.addColorStop(1, base);
      ctx.fillStyle = g;
      roundRect(x, y, w, h, r); ctx.fill();
      return { cx: x + w / 2, cy: y + h / 2 };
    }
    function draw() {
      resize();
      var s = cv.width / N, board = color("--board"), alt = color("--board-alt");
      for (var x = 0; x < N; x++) for (var y = 0; y < N; y++) {
        ctx.fillStyle = (x + y) % 2 ? alt : board;
        ctx.fillRect(x * s, y * s, Math.ceil(s), Math.ceil(s));
      }
      if (!snake) return;
      if (food) {
        var fx = (food.x + 0.5) * s, fy = (food.y + 0.5) * s - s * 0.04, fr = s * 0.34, fc = color("--food");
        ctx.fillStyle = "rgba(0,0,0,0.2)";
        ctx.beginPath(); ctx.ellipse(fx + s * 0.03, fy + fr * 0.95, fr * 0.9, fr * 0.36, 0, 0, Math.PI * 2); ctx.fill();
        var rg = ctx.createRadialGradient(fx - fr * 0.35, fy - fr * 0.4, fr * 0.1, fx, fy, fr * 1.1);
        rg.addColorStop(0, shade(fc, 1.6)); rg.addColorStop(0.45, fc); rg.addColorStop(1, shade(fc, 0.6));
        ctx.fillStyle = rg;
        ctx.beginPath(); ctx.arc(fx, fy, fr, 0, Math.PI * 2); ctx.fill();
      }
      var body = color("--snake"), headC = color("--snake-head");
      var order = snake.map(function (p, i) { return { p: p, i: i }; });
      order.sort(function (a, b) { return a.p.y - b.p.y || b.i - a.i; });
      var headPos = null;
      order.forEach(function (o) {
        var pos = drawBlock(o.p.x, o.p.y, s, o.i === 0 ? headC : body);
        if (o.i === 0) headPos = pos;
      });
      var ex = dir.x, ey = dir.y, px = -ey, py = ex;
      ctx.fillStyle = board;
      [-1, 1].forEach(function (k) {
        ctx.beginPath();
        ctx.arc(headPos.cx + ex * s * 0.15 + px * k * s * 0.17, headPos.cy + ey * s * 0.12 + py * k * s * 0.17, s * 0.075, 0, Math.PI * 2);
        ctx.fill();
      });
    }
    function frame(t) {
      if (!S.alive) return;
      var dt = Math.min(t - last, 100);
      last = t;
      if (state === "running") {
        acc += dt;
        while (acc >= speed && state === "running") { acc -= speed; step(); }
      }
      draw();
      raf = requestAnimationFrame(frame);
    }

    S.on(document, "keydown", function (e) {
      var d = KEYDIR[e.key];
      if (d) {
        e.preventDefault();
        if (state === "over" || state === "paused") return;
        setDir(VEC[d][0], VEC[d][1]);
      } else if (e.key === " " && !(e.target && e.target.tagName === "BUTTON")) {
        e.preventDefault();
        if (state === "running" || state === "paused") togglePause(); else start();
      }
    });
    Array.prototype.forEach.call(host.querySelectorAll(".pad button"), function (b) {
      S.on(b, "click", function () {
        if (state === "paused" || state === "over") return;
        var v = VEC[b.getAttribute("data-d")];
        setDir(v[0], v[1]);
      });
    });
    swipe(S, cv, function (d) {
      if (state === "paused" || state === "over") return;
      setDir(VEC[d][0], VEC[d][1]);
    });
    S.on(ovB, "click", start);
    S.on(pauseBtn, "click", function () { if (state === "running" || state === "paused") togglePause(); else start(); });
    S.on(wrapBox, "change", function () {
      loadBest(); reset(); state = "ready"; pauseBtn.textContent = "Pauza";
      showOverlay("Tayyormisiz?", wrapBox.checked ? "Devordan o‘tish yoqilgan." : "Olmalarni yeng, o‘zingizga urilmang.", "Boshlash");
    });
    S.on(document, "visibilitychange", function () { if (document.hidden) pause(); });

    loadBest(); reset(); last = performance.now();
    raf = requestAnimationFrame(frame);
    return { destroy: function () { cancelAnimationFrame(raf); } };
  }

  /* ---------- 2048 ---------- */
  function game2048(host, S) {
    host.innerHTML =
      '<div class="stats"><div class="stat"><span>Hisob</span><span id="sc">0</span></div>' +
      '<div class="stat"><span>Rekord</span><span id="bs">0</span></div></div>' +
      '<div class="board swipe" id="bd"><div class="grid4" id="gr"></div>' +
      '<div class="overlay" id="ov" hidden><h2 id="ovt"></h2><p id="ovp"></p><button id="ovb" type="button"></button></div></div>' +
      '<div class="controls"><button id="nw" type="button">Yangi o‘yin</button></div>' +
      '<p class="hint">Strelkalar yoki W A S D bilan, telefonda barmoq bilan suring. Bir xil raqamlar birlashadi. Maqsad 2048 ni hosil qilish.</p>';

    var gr = $(host, "#gr"), ov = $(host, "#ov"), ovT = $(host, "#ovt"), ovP = $(host, "#ovp"), ovB = $(host, "#ovb");
    var scoreEl = $(host, "#sc"), bestEl = $(host, "#bs");
    var grid, score, best = store.get("2048"), over, keepGoing;

    function addTile() {
      var free = [];
      for (var r = 0; r < 4; r++) for (var c = 0; c < 4; c++) if (!grid[r][c]) free.push([r, c]);
      if (!free.length) return;
      var f = free[rnd(free.length)];
      grid[f[0]][f[1]] = Math.random() < 0.9 ? 2 : 4;
    }
    function render() {
      gr.innerHTML = "";
      for (var r = 0; r < 4; r++) for (var c = 0; c < 4; c++) {
        var v = grid[r][c], d = document.createElement("div");
        d.className = "tile" + (v >= 1024 ? " xs" : v >= 128 ? " sm" : "");
        if (v) {
          d.textContent = v;
          var pct = Math.min(100, 10 + Math.log(v) / Math.LN2 * 9);
          d.style.background = "color-mix(in srgb, var(--snake) " + pct + "%, var(--board))";
          d.style.color = pct >= 50 ? "var(--btn-fg)" : "var(--fg)";
        }
        gr.appendChild(d);
      }
      scoreEl.textContent = score; bestEl.textContent = best;
    }
    function showOv(t, p, b) { ovT.textContent = t; ovP.textContent = p; ovB.textContent = b; ov.hidden = false; }
    function newGame() {
      grid = [[0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0], [0, 0, 0, 0]];
      score = 0; over = false; keepGoing = false; ov.hidden = true;
      addTile(); addTile(); render();
    }
    function slide(line) {
      var arr = line.filter(Boolean), out = [], gain = 0;
      for (var i = 0; i < arr.length; i++) {
        if (arr[i] === arr[i + 1]) { out.push(arr[i] * 2); gain += arr[i] * 2; i++; }
        else out.push(arr[i]);
      }
      while (out.length < 4) out.push(0);
      return { line: out, gain: gain };
    }
    var POS = {
      l: function (i, j) { return [i, j]; },
      r: function (i, j) { return [i, 3 - j]; },
      u: function (i, j) { return [j, i]; },
      d: function (i, j) { return [3 - j, i]; }
    };
    function canMove() {
      for (var r = 0; r < 4; r++) for (var c = 0; c < 4; c++) {
        if (!grid[r][c]) return true;
        if (c < 3 && grid[r][c] === grid[r][c + 1]) return true;
        if (r < 3 && grid[r][c] === grid[r + 1][c]) return true;
      }
      return false;
    }
    function has2048() {
      for (var r = 0; r < 4; r++) for (var c = 0; c < 4; c++) if (grid[r][c] >= 2048) return true;
      return false;
    }
    function move(d) {
      if (over || !ov.hidden) return;
      var changed = false, gain = 0, p = POS[d], j, q;
      for (var i = 0; i < 4; i++) {
        var line = [];
        for (j = 0; j < 4; j++) { q = p(i, j); line.push(grid[q[0]][q[1]]); }
        var res = slide(line);
        for (j = 0; j < 4; j++) {
          q = p(i, j);
          if (grid[q[0]][q[1]] !== res.line[j]) changed = true;
          grid[q[0]][q[1]] = res.line[j];
        }
        gain += res.gain;
      }
      if (!changed) return;
      score += gain;
      if (score > best) { best = score; store.set("2048", best); }
      addTile(); render();
      if (!keepGoing && has2048()) showOv("2048!", "Siz yutdingiz. Xohlasangiz davom ettiring.", "Davom etish");
      else if (!canMove()) { over = true; showOv("O‘yin tugadi", "Hisob: " + score, "Qayta o‘ynash"); }
    }

    S.on(document, "keydown", function (e) {
      var d = KEYDIR[e.key];
      if (!d) return;
      e.preventDefault();
      move(d);
    });
    swipe(S, $(host, "#bd"), move);
    S.on(ovB, "click", function () { if (over) newGame(); else { keepGoing = true; ov.hidden = true; } });
    S.on($(host, "#nw"), "click", newGame);
    newGame();
    return { destroy: function () {} };
  }

  /* ---------- XOTIRA ---------- */
  var SHAPES = [
    '<circle cx="12" cy="12" r="8"/>',
    '<rect x="4" y="4" width="16" height="16" rx="2"/>',
    '<path d="M12 3 L22 20 H2 Z"/>',
    '<path d="M12 2 L22 12 L12 22 L2 12 Z"/>',
    '<path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/>',
    '<path d="M9 3h6v6h6v6h-6v6H9v-6H3V9h6z"/>',
    '<path d="M12 2l8.66 5v10L12 22l-8.66-5V7z"/>',
    '<circle cx="12" cy="12" r="6.5" fill="none" stroke="currentColor" stroke-width="4"/>'
  ];
  var HUES = [8, 35, 52, 150, 185, 215, 265, 320];

  function memoryGame(host, S) {
    host.innerHTML =
      '<div class="stats"><div class="stat"><span>Yurishlar</span><span id="mv">0</span></div>' +
      '<div class="stat"><span>Eng kam</span><span id="bs">—</span></div></div>' +
      '<div class="board"><div class="memgrid" id="mg"></div>' +
      '<div class="overlay" id="ov" hidden><h2>Barakalla!</h2><p id="ovp"></p><button id="ovb" type="button">Qayta o‘ynash</button></div></div>' +
      '<div class="controls"><button id="nw" type="button">Yangi o‘yin</button></div>' +
      '<p class="hint">Bir xil shakllarni juft-juft toping. Yurishlar qancha kam bo‘lsa, shuncha yaxshi.</p>';

    var mg = $(host, "#mg"), ov = $(host, "#ov"), ovP = $(host, "#ovp"), mvEl = $(host, "#mv"), bsEl = $(host, "#bs");
    var deck, first, lock, moves, matched, btns = [];

    function showBest() { var b = store.get("xotira"); bsEl.textContent = b ? b : "—"; }
    function newGame() {
      deck = [];
      for (var i = 0; i < 8; i++) deck.push(i, i);
      for (i = deck.length - 1; i > 0; i--) { var j = rnd(i + 1), t = deck[i]; deck[i] = deck[j]; deck[j] = t; }
      first = -1; lock = false; moves = 0; matched = 0; mvEl.textContent = 0; ov.hidden = true;
      mg.innerHTML = ""; btns = [];
      deck.forEach(function (k, idx) {
        var b = document.createElement("button");
        b.type = "button"; b.className = "mem"; b.setAttribute("data-i", idx); b.setAttribute("aria-label", "Karta");
        b.innerHTML = '<svg viewBox="0 0 24 24" fill="currentColor" style="color:hsl(' + HUES[k] + ' 72% 52%)" aria-hidden="true">' + SHAPES[k] + "</svg>";
        mg.appendChild(b); btns.push(b);
      });
      showBest();
    }
    function onCard(i) {
      if (lock || i === first || btns[i].classList.contains("done")) return;
      btns[i].classList.add("open");
      if (first < 0) { first = i; return; }
      var a = first, b = i;
      first = -1; moves++; mvEl.textContent = moves;
      if (deck[a] === deck[b]) {
        btns[a].classList.remove("open"); btns[b].classList.remove("open");
        btns[a].classList.add("done"); btns[b].classList.add("done");
        matched++;
        if (matched === 8) {
          var best = store.get("xotira"), record = !best || moves < best;
          if (record) { store.set("xotira", moves); showBest(); }
          ovP.textContent = moves + " yurishda topdingiz" + (record ? " — yangi rekord" : "") + ".";
          S.later(function () { ov.hidden = false; }, 450);
        }
      } else {
        lock = true;
        S.later(function () {
          btns[a].classList.remove("open"); btns[b].classList.remove("open");
          lock = false;
        }, 750);
      }
    }
    S.on(mg, "click", function (e) {
      var b = e.target.closest ? e.target.closest(".mem") : null;
      if (b) onCard(parseInt(b.getAttribute("data-i"), 10));
    });
    S.on($(host, "#ovb"), "click", newGame);
    S.on($(host, "#nw"), "click", newGame);
    newGame();
    return { destroy: function () {} };
  }

  /* ---------- KRESTIK-NOLIK ---------- */
  function tttGame(host, S) {
    host.innerHTML =
      '<div class="stats"><div class="stat"><span>G‘alaba</span><span id="w">0</span></div>' +
      '<div class="stat"><span>Durang</span><span id="dr">0</span></div>' +
      '<div class="stat"><span>Mag‘lubiyat</span><span id="ls">0</span></div></div>' +
      '<div class="board"><div class="tgrid" id="tg"></div></div>' +
      '<p class="status" id="st"></p>' +
      '<div class="controls"><button id="nw" type="button">Yangi raund</button></div>' +
      '<p class="hint">Siz X bilan o‘ynaysiz, kompyuter O bilan. Kompyuter ba‘zan xato qiladi, shuning uchun uni yutish mumkin. Birinchi yurish har raundda navbatma-navbat almashadi.</p>';

    var tg = $(host, "#tg"), st = $(host, "#st");
    var LINES = [[0, 1, 2], [3, 4, 5], [6, 7, 8], [0, 3, 6], [1, 4, 7], [2, 5, 8], [0, 4, 8], [2, 4, 6]];
    var b, humanFirst = false, locked, done, winLine = null, cells = [];

    function setStat(id, key) { $(host, id).textContent = store.get(key); }
    function stats() { setStat("#w", "krestik-w"); setStat("#dr", "krestik-d"); setStat("#ls", "krestik-l"); }
    function winner(bd) {
      for (var i = 0; i < LINES.length; i++) {
        var l = LINES[i];
        if (bd[l[0]] && bd[l[0]] === bd[l[1]] && bd[l[0]] === bd[l[2]]) return { p: bd[l[0]], line: l };
      }
      return null;
    }
    function minimax(bd, turn) {
      var w = winner(bd);
      if (w) return w.p === "O" ? 1 : -1;
      var free = [];
      for (var i = 0; i < 9; i++) if (!bd[i]) free.push(i);
      if (!free.length) return 0;
      var best = turn === "O" ? -2 : 2;
      free.forEach(function (k) {
        bd[k] = turn;
        var s = minimax(bd, turn === "O" ? "X" : "O");
        bd[k] = "";
        if (turn === "O" ? s > best : s < best) best = s;
      });
      return best;
    }
    function aiPick() {
      var free = [];
      for (var i = 0; i < 9; i++) if (!b[i]) free.push(i);
      if (Math.random() < 0.25) return free[rnd(free.length)];
      var bestS = -2, picks = [];
      free.forEach(function (k) {
        b[k] = "O";
        var s = minimax(b, "X");
        b[k] = "";
        if (s > bestS) { bestS = s; picks = [k]; } else if (s === bestS) picks.push(k);
      });
      return picks[rnd(picks.length)];
    }
    function render() {
      cells.forEach(function (c, i) {
        c.textContent = b[i];
        c.className = "cell" + (b[i] === "X" ? " x" : b[i] === "O" ? " o" : "") + (winLine && winLine.indexOf(i) >= 0 ? " win" : "");
      });
    }
    function finish(w) {
      done = true; locked = true;
      if (!w) { store.set("krestik-d", store.get("krestik-d") + 1); st.textContent = "Durang"; }
      else if (w.p === "X") { store.set("krestik-w", store.get("krestik-w") + 1); st.textContent = "Siz yutdingiz!"; }
      else { store.set("krestik-l", store.get("krestik-l") + 1); st.textContent = "Kompyuter yutdi"; }
      winLine = w ? w.line : null;
      render(); stats();
    }
    function check() {
      var w = winner(b);
      if (w) { finish(w); return true; }
      if (b.every(Boolean)) { finish(null); return true; }
      return false;
    }
    function aiTurn() {
      if (done) return;
      b[aiPick()] = "O";
      locked = false; render();
      if (!check()) st.textContent = "Sizning yurishingiz (X)";
    }
    function onCell(i) {
      if (locked || done || b[i]) return;
      b[i] = "X"; render();
      if (check()) return;
      locked = true; st.textContent = "Kompyuter o‘ylayapti…";
      S.later(aiTurn, 450);
    }
    function newRound() {
      b = ["", "", "", "", "", "", "", "", ""];
      done = false; winLine = null; humanFirst = !humanFirst;
      render(); stats();
      if (humanFirst) { locked = false; st.textContent = "Sizning yurishingiz (X)"; }
      else { locked = true; st.textContent = "Kompyuter o‘ylayapti…"; S.later(aiTurn, 500); }
    }

    for (var i = 0; i < 9; i++) {
      var c = document.createElement("button");
      c.type = "button"; c.setAttribute("data-i", i); c.setAttribute("aria-label", "Katak " + (i + 1));
      tg.appendChild(c); cells.push(c);
    }
    S.on(tg, "click", function (e) {
      var c = e.target.closest ? e.target.closest("button") : null;
      if (c) onCell(parseInt(c.getAttribute("data-i"), 10));
    });
    S.on($(host, "#nw"), "click", function () { newRound(); });
    newRound();
    return { destroy: function () {} };
  }

  /* ---------- bosh sahifa va yo'naltirish ---------- */
  var ICONS = {
    ilon: '<svg viewBox="0 0 64 64" aria-hidden="true"><path d="M12 48H34V28H50" fill="none" style="stroke:var(--snake)" stroke-width="9" stroke-linecap="round" stroke-linejoin="round"/><circle cx="50" cy="28" r="6" style="fill:var(--snake-head)"/><circle cx="14" cy="16" r="6" style="fill:var(--food)"/></svg>',
    "2048": '<svg viewBox="0 0 64 64" aria-hidden="true"><rect x="5" y="5" width="25" height="25" rx="5" style="fill:var(--snake);fill-opacity:.25"/><rect x="34" y="5" width="25" height="25" rx="5" style="fill:var(--snake);fill-opacity:.5"/><rect x="5" y="34" width="25" height="25" rx="5" style="fill:var(--snake);fill-opacity:.75"/><rect x="34" y="34" width="25" height="25" rx="5" style="fill:var(--snake)"/><g font-size="14" font-weight="800" text-anchor="middle" font-family="sans-serif"><text x="17.5" y="23" style="fill:var(--fg)">2</text><text x="46.5" y="23" style="fill:var(--fg)">4</text><text x="17.5" y="52" style="fill:var(--btn-fg)">8</text><text x="46.5" y="52" style="fill:var(--btn-fg)">16</text></g></svg>',
    xotira: '<svg viewBox="0 0 64 64" aria-hidden="true"><rect x="8" y="12" width="26" height="36" rx="5" style="fill:var(--btn)"/><rect x="30" y="18" width="26" height="36" rx="5" style="fill:var(--card);stroke:var(--fg)" stroke-width="3"/><circle cx="43" cy="36" r="7" style="fill:var(--food)"/></svg>',
    krestik: '<svg viewBox="0 0 64 64" aria-hidden="true"><path d="M24 8V56M40 8V56M8 24H56M8 40H56" fill="none" style="stroke:var(--fg)" stroke-width="4" stroke-linecap="round"/><path d="M11 11l10 10M21 11L11 21" fill="none" style="stroke:var(--food)" stroke-width="4" stroke-linecap="round"/><circle cx="48" cy="48" r="5.5" fill="none" style="stroke:var(--snake)" stroke-width="4"/></svg>'
  };
  var GAMES = [
    { id: "ilon", name: "Ilon", make: snakeGame,
      record: function () { var m = Math.max(store.get("ilon-devor"), store.get("ilon-otish")); return m ? "Rekord: " + m : "Rekord yo‘q"; } },
    { id: "2048", name: "2048", make: game2048,
      record: function () { var m = store.get("2048"); return m ? "Rekord: " + m : "Rekord yo‘q"; } },
    { id: "xotira", name: "Xotira", make: memoryGame,
      record: function () { var m = store.get("xotira"); return m ? "Eng kam: " + m + " yurish" : "Rekord yo‘q"; } },
    { id: "krestik", name: "Krestik-nolik", make: tttGame,
      record: function () { return "G‘alaba: " + store.get("krestik-w"); } }
  ];

  var current = null, currentId = "";
  function find(id) { for (var i = 0; i < GAMES.length; i++) if (GAMES[i].id === id) return GAMES[i]; return null; }

  function route(id) {
    if (current) { current.scope.destroy(); current.game.destroy(); current = null; }
    var g = find(id);
    currentId = g ? g.id : "";
    app.innerHTML = "";
    if (!g) {
      var html = '<header><h1>O‘yinlar</h1><p class="sub"><span class="dot"></span>Hammasi internetsiz ishlaydi</p></header><div class="cards">';
      GAMES.forEach(function (x) {
        html += '<button class="card" type="button" data-id="' + x.id + '"><span class="ico">' + ICONS[x.id] + "</span><b>" + x.name + "</b><small>" + x.record() + "</small></button>";
      });
      html += '</div><p class="hint">Rekordlar shu qurilmada saqlanadi.</p>';
      app.innerHTML = html;
      return;
    }
    app.innerHTML = '<div class="top"><button class="ghost" id="back" type="button">← O‘yinlar</button><h1>' + g.name + '</h1></div><div id="host" style="display:contents"></div>';
    var scope = makeScope();
    var game = g.make($(app, "#host"), scope);
    current = { scope: scope, game: game };
  }

  function go(id) {
    route(id);
    try { location.hash = id || ""; } catch (e) {}
    window.scrollTo(0, 0);
  }

  app.addEventListener("click", function (e) {
    var card = e.target.closest ? e.target.closest(".card") : null;
    if (card) { go(card.getAttribute("data-id")); return; }
    var back = e.target.closest ? e.target.closest("#back") : null;
    if (back) go("");
  });
  window.addEventListener("hashchange", function () {
    var id = location.hash.replace("#", "");
    if (id !== currentId) route(id);
  });

  route(location.hash.replace("#", ""));
})();
</script>
</body>
</html>
