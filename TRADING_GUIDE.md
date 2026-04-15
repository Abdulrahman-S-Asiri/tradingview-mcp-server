# دليل التداول اليومي — Claude MCP

روتين يومي خطوة بخطوة لاستخدام Claude AI عبر MCP للتحكم بـ TradingView وتنفيذ استراتيجية **"Liquidity Sniper 20x"**.

---

## ⏰ أوقات التداول المسموحة (توقيت مكة UTC+3)

الاستراتيجية تصيد اكتساح السيولة — السيولة تظهر بس في الجلسات القوية.

| الجلسة | الوقت | الحالة |
|--------|-------|--------|
| 🟢 جلسة لندن | 10:00 ص - 1:00 م | مسموح — حركة نظيفة، فوليوم متوسط |
| 🔥 جلسة نيويورك | 4:30 م - 7:30 م | **أفضل وقت** — أعلى سيولة |
| ❌ الجلسة الآسيوية | 4:00 ص - 9:00 ص | ممنوع — حركة عشوائية |
| ❌ نهاية الأسبوع | السبت والأحد | ممنوع — سيولة ضعيفة |
| ❌ قبل أخبار كبيرة | CPI/FOMC/NFP/PPI/GDP | ممنوع — ساعتين قبل + ساعة بعد |

---

## 📋 الروتين اليومي (من تشغيل الجهاز لين إغلاق الصفقة)

---

### ① التجهيز — أول شي لما تفتح الكمبيوتر

**1. شغّل TradingView** — افتح PowerShell والصق هذا الأمر:
```powershell
Stop-Process -Name TradingView -Force -ErrorAction SilentlyContinue; Start-Sleep 3; Start-Process "C:\Program Files\WindowsApps\31178TradingViewInc.TradingView_3.0.0.0_x64__q4jpyh43s5mv6\TradingView.exe" "--remote-debugging-port=9222"
```
*(وبعدها افتح تطبيق TradingView)*

**2. شغّل Claude** — انتظر 10 ثواني وبعدها:
```powershell
cd "$HOME\tradingview-mcp"; claude
```

**3. حمّل الاستراتيجية** — الصق هذا البرومبت:
> *"Run `tv_health_check` to ensure the MCP connection is fully stable. Next, read our strategy from the `CLAUDE.md` file (titled 'BTCUSDT.P Liquidity Sniper 20x'). Critically evaluate the strength of these rules and explicitly verify your capability to monitor these specific conditions (Volume spikes, ATR, EMA 200, Fakeouts) using your MCP tools. Confirm when loaded."*

⏳ **انتظر** لين يرد "Loaded and ready" — لا تكمل قبل كذا.

---

### ② الماكرو داشبورد — تشيك مود السوق العام

قبل ما تبدأ تتداول، لازم تعرف هل السوق في حالة Risk-On (إيجابي) ولا Risk-Off (سلبي). الصق:

> *"Using `get_ta_summary` and `lookup_symbols`, pull live data for: DXY (US Dollar), VIX (Volatility), Gold, BTCUSDT.P, ETHUSDT.P, NDX (Nasdaq), and US10Y (Yields). Build a macro dashboard table showing price, daily change, and signal for each. Then tell me: is Bitcoin in Risk-On or Risk-Off mode right now? Should I be cautious today?"*

⚠️ **القاعدة:**
- لو قال **Risk-On** → كمّل عادي
- لو قال **Risk-Off** أو **"be cautious"** → خفف أو لا تتداول اليوم

---

### ③ مسح الفريمات — يفتح كل فريم واحد واحد

هنا Claude يفتح كل فريم في TradingView ويحلله بعينه. الصق:

> *"Open the `BTCUSDT.P` chart and go through each timeframe one by one. For each frame, run `get_ta_summary` AND visually analyze the chart:*
>
> *1. **Open 4H** — Check EMA 200 position. What is the macro bias (Long or Short)? Note key structure levels (higher highs/lows).*
> *2. **Open 1H** — Check EMA 200 position. Does it agree with the 4H bias? If they conflict → no trade today.*
> *3. **Open 15m** — Identify the top 3 high-probability liquidity pools (prominent highs/lows closest to price).*
> *4. **Open 5m** — Check current volume, ATR 14 value, and whether price is approaching any of the liquidity levels.*
>
> *Summarize: Bias confirmed? Which levels are we targeting? Current ATR for SL sizing?"*

⚠️ **القاعدة:**
- لو **4H و 1H متفقين** → كمّل للخطوة اللي بعدها
- لو **متعارضين** → ❌ وقّف ولا تتداول — انتظر لين يتوافقون

---

### ④ التنبيهات والمراقبة — حط الأليرتات وشغّل اللوب

بعد ما Claude حدد مناطق السيولة، خله يحط تنبيهات تلقائية. الصق:

> *"Create TradingView alerts at each of the liquidity levels you identified. My phone should ring when price approaches these zones."*

⏳ **انتظر** لين يأكد إن التنبيهات انحطت. وبعدها شغّل المراقبة التلقائية:

> *"Use `/loop 3m` to monitor the 5m timeframe. Stay silent unless a sweep fires — then send me the full entry report immediately. Check OHLC, Volume vs MA, and ATR 14 on each poll."*

💡 **كيف تشتغل المراقبة:**
1. 📱 تلفونك يرن أول (تنبيه TradingView) لما السعر يوصل المنطقة
2. 🤖 Claude يتشيك كل 3 دقايق ويتأكد هل صار اكتساح حقيقي + فوليوم
3. 📊 لو كل الشروط تحققت → يرسلك تقرير الدخول تلقائي

**الحين ارتاح وخل النظام يشتغل** 🎯

---

### ⑤ تقييم الشمعة — لما التنبيه يدق

لما تلفونك يرن أو Claude يرسلك تقرير، تأكد إن الشمعة قفلت والصق:

> *"The candle just closed! Evaluate this close on the 5m timeframe. Is it a valid Fakeout/Sweep according to our strict rules? Send me the entry report specifying: 1. Entry price. 2. Stop Loss size (using ATR, but strictly capping at 0.8% max risk). 3. Take profit targets (TP1 at VWAP, TP2 at opposing structure)."*

⚠️ **القاعدة:**
- لو قال **"VALID SWEEP"** + أعطاك Entry/SL/TP → روح Binance ونفّذ (الخطوة ⑥)
- لو قال **"NOT A SWEEP — NO ENTRY"** → ❌ لا تدخل وارجع انتظر

---

### ⑥ تنفيذ الصفقة — على Binance

لما Claude يعطيك الضوء الأخضر:

1. **افتح Binance** ونفذ الصفقة بالضبط زي ما قال:
   - 💰 رأس المال: **25%** من حسابك
   - 📈 الرافعة: **20x Isolated**
2. **حط الأوامر:**
   - ✅ **TP1** — أغلق 50% عند VWAP
   - ✅ **SL** — الستوب لوس حسب ما حدد Claude
3. **قاعدة البريك إيفن:**
   - لما الصفقة توصل **+0.5%** ربح → حرّك الستوب على نقطة الدخول فوراً
4. **أقفل الشاشة** — صفقتك الحين بدون مخاطرة ✅

---

## 🔄 برومبتات إضافية (استخدمها أي وقت أثناء الجلسة)

### تشيك سريع للتحليل الفني
> *"Run `get_ta_summary` for `BINANCE:BTCUSDT.P` on timeframes 5, 15, 60, 240. Then run `lookup_symbols` for the same symbol to get current price, volume, and daily change. Summarize the results and confirm whether 4H + 1H are aligned."*

### تشيك يدوي للشمعة الحالية
> *"Check the 5m candle now. Did any of our liquidity levels get swept? Evaluate volume and candle structure. If valid sweep → send entry report. If not → tell me current price and distance to each level."*

---

## 💡 نصائح مهمة

| النصيحة | التفاصيل |
|---------|----------|
| **لا تتداول بالعاطفة** | أكبر ميزة إن الآلة هي اللي تقرر الدخول بالأرقام (ATR + Volume) — أنت بس تنفذ |
| **احترم الستوب** | لو Claude قال "الستوب 1.2% — ألغِ الصفقة" لأنها تتعدى 0.8% → أطِعه فوراً |
| **بيانات حية** | تأكد إن Claude يحلل آخر شمعة، مو بيانات قديمة |
| **حد الخسارة اليومي** | خسرت صفقتين؟ وقّف — هذا قانون ما يتكسر |
| **حد الخسارة الأسبوعي** | خسرت 15% من رأس المال بالأسبوع؟ وقّف لنهاية الأسبوع |

---

## 📰 مصادر الأخبار الاقتصادية (تشيكها يومياً قبل التداول)

- 🔴 [Forex Factory Calendar](https://www.forexfactory.com/calendar) — خبر أحمر = لا تداول
- 🪙 [CoinMarketCal](https://coinmarketcal.com/en/) — أحداث خاصة بالكريبتو
- 📱 [Investing.com](https://www.investing.com/economic-calendar/) — تطبيق جوال مع تنبيهات

---

## ملخص الترتيب السريع

| الخطوة | الوصف | متى؟ |
|--------|-------|------|
| ① | تجهيز + تحميل الاستراتيجية | بداية اليوم |
| ② | ماكرو داشبورد (مود السوق) | قبل التداول |
| ③ | مسح الفريمات (4H→1H→15m→5m) | بداية الجلسة |
| ④ | تنبيهات + `/loop 3m` | بعد تحديد المستويات |
| ⑤ | تقييم الشمعة | لما التنبيه يدق |
| ⑥ | تنفيذ الصفقة على Binance | لما Claude يعطيك الضوء الأخضر |
