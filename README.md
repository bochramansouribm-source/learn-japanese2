<!doctype html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>تعلم اللغة اليابانية — صوتي</title>
  <style>
    :root{
      --accent:#e60033; --muted:#6b7280; --card:#ffffff;
    }
    *{box-sizing:border-box}
    body{margin:0;font-family:Tahoma,Arial,sans-serif;background:#f5f7fb;color:#0f172a}
    header{background:linear-gradient(120deg,var(--accent),#ff7b7b);color:#fff;padding:20px;text-align:center}
    header h1{margin:0;font-size:20px}
    nav{display:flex;gap:12px;justify-content:center;background:#fff;padding:10px;box-shadow:0 2px 8px rgba(2,6,23,0.06)}
    nav a{color:var(--accent);text-decoration:none;font-weight:700}
    .container{max-width:1100px;margin:20px auto;padding:0 16px}
    h2{color:var(--accent);margin:0 0 12px 0;border-bottom:2px solid var(--accent);display:inline-block;padding-bottom:6px}
    .grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(170px,1fr));gap:12px}
    .card{background:var(--card);padding:14px;border-radius:12px;box-shadow:0 6px 18px rgba(2,6,23,0.05)}
    .jp{font-weight:800;font-size:1.3rem;direction:ltr}
    .romaji{color:var(--muted);font-size:0.95rem;margin-top:6px}
    .btn{display:inline-flex;align-items:center;gap:8px;padding:8px 10px;border-radius:9px;border:0;background:var(--accent);color:#fff;cursor:pointer;font-weight:700}
    .btn-ghost{background:transparent;border:1px solid #eee;padding:6px 8px;border-radius:8px;cursor:pointer}
    footer{margin:30px 0;text-align:center;color:var(--muted);font-size:13px}
    .note{font-size:13px;color:var(--muted)}
    @media (max-width:720px){ .jp{font-size:1.05rem} }
  </style>
</head>
<body>
  <header>
    <h1>✨ تعلم اليابانية — حروف وجمل صوتية (بالعربية)</h1>
    <div class="note">تشغيل من خادم صوتي محلي إن وُجد، وإلا يستعمل نطق المتصفح (SpeechSynthesis).</div>
  </header>

  <nav class="container" aria-label="روابط">
    <a href="#hiragana">الهيراغانا (الحروف)</a>
    <a href="#phrases">العبارات الأساسية</a>
    <a href="#usage">كيفية الإعداد</a>
  </nav>

  <main class="container">

    <!-- HIRAGANA -->
    <section id="hiragana" style="margin-top:18px">
      <h2>الهيراغانا — الحروف الأساسية (一覧)</h2>
      <p class="note">اضغط على 🔊 لتشغيل ملف صوتي محلي (من الخادم). إن لم يوجد الملف سيتم النطق عبر المتصفح.</p>

      <div class="grid" id="hiraganaGrid" style="margin-top:12px">
        <!-- سنملأ هذا بالـ JS (أجعل HTML أنظف) -->
      </div>
    </section>

    <!-- PHRASES -->
    <section id="phrases" style="margin-top:22px">
      <h2>العبارات الأساسية — جمل يومية</h2>
      <div class="grid" id="phrasesGrid" style="margin-top:12px"></div>
    </section>

    <!-- USAGE / INSTRUCTIONS -->
    <section id="usage" style="margin-top:20px">
      <h2>كيفية تشغيل الخوادم الصوتية محليًا</h2>
      <ol>
        <li>ضع هذا الملف <code>index.html</code> داخل مجلّد <code>web/</code> أو افتحه مباشرة.</li>
        <li>ضع الخادوم Node/Express في مجلّد <code>server/</code>، وشغّل سكربت Python <code>generate_tts.py</code> لتوليد ملفات MP3 (سأعطيك السكربت أدناه).</li>
        <liشغّل الخادوم: <code>node server.js</code>. ملفات الصوت ستكون متاحة عند <code>http://localhost:4000/audio/...</code>.</li>
      </ol>
      <p class="note">إذا فتحت الملف عبر <code>file://</code> فلن تعمل الشبكة المحلية إلا إذا شغّلت الخادوم؛ لذلك الأفضل فتحه من الخادوم (index.html يمكن أن يُخدم أيضاً من الخادوم).</p>
    </section>
  </main>

  <footer>
    © 2025 — تعلم اليابانية بالعربية — يدعم الصوت (ملفات MP3 محلية أو نطق المتصفح)
  </footer>

  <script>
  /********** إعدادات مسارات الصوت **********/
  // AUDIO_BASE: المسار الأساسي لملفات الصوت (يبحث أولاً في origin إذا الصفحة مخدومة عبر http)
  const AUDIO_BASE = (function(){
    try {
      if (location.protocol.startsWith('http')) return location.origin + '/audio';
    } catch(e){}
    return 'http://localhost:4000/audio'; // fallback عندما تفتح الملف مباشرة
  })();

  /********** بيانات الحروف والجمل (قوائم) **********/
  // قائمة الهيراغانا (مفتاح الملف: romaji)
  const HIRAGANA = [
    ['a','あ','a'], ['i','い','i'], ['u','う','u'], ['e','え','e'], ['o','お','o'],
    ['ka','か','ka'], ['ki','き','ki'], ['ku','く','ku'], ['ke','け','ke'], ['ko','こ','ko'],
    ['sa','さ','sa'], ['shi','し','shi'], ['su','す','su'], ['se','せ','se'], ['so','そ','so'],
    ['ta','た','ta'], ['chi','ち','chi'], ['tsu','つ','tsu'], ['te','て','te'], ['to','と','to'],
    ['na','な','na'], ['ni','に','ni'], ['nu','ぬ','nu'], ['ne','ね','ne'], ['no','の','no'],
    ['ha','は','ha'], ['hi','ひ','hi'], ['fu','ふ','fu'], ['he','へ','he'], ['ho','ほ','ho'],
    ['ma','ま','ma'], ['mi','み','mi'], ['mu','む','mu'], ['me','め','me'], ['mo','も','mo'],
    ['ya','や','ya'], ['yu','ゆ','yu'], ['yo','よ','yo'],
    ['ra','ら','ra'], ['ri','り','ri'], ['ru','る','ru'], ['re','れ','re'], ['ro','ろ','ro'],
    ['wa','わ','wa'], ['wo','を','wo'], ['n','ん','n']
  ];

  // قائمة العبارات الأساسية (مفتاح الملف = romaji_key)
  const PHRASES = [
    ['konnichiwa','こんにちは','مرحباً / مساء الخير'],
    ['ohayou_gozaimasu','おはようございます','صباح الخير (رسمي)'],
    ['arigatou_gozaimasu','ありがとうございます','شكراً جزيلاً'],
    ['arigatou','ありがとう','شكراً (غير رسمي)'],
    ['sumimasen','すみません','عذراً / لو سمحت'],
    ['hai','はい','نعم'],
    ['iie','いいえ','لا'],
    ['sayounara','さようなら','إلى اللقاء'],
    ['watashi_wa_gakusei_desu','私は学生です','أنا طالب/طالبة'],
    ['toire_wa_doko_desu_ka','トイレはどこですか','أين الحمام؟'],
    ['onegai_shimasu','お願いします','من فضلك (لطلب رسمي)'],
    ['yoroshiku_onegaishimasu','よろしくお願いします','سعدت بلقائك / أرجو تعاونك'],
    ['o_genki_desu_ka','お元気ですか','هل أنت بخير؟']
  ];

  /********** رسم الواجهة (الحروف والجمل) **********/
  const hiraganaGrid = document.getElementById('hiraganaGrid');
  const phrasesGrid = document.getElementById('phrasesGrid');

  function buildCard(titleJP, romaji, note, onclick){
    const div = document.createElement('div');
    div.className = 'card';
    div.innerHTML = `
      <div class="jp" style="direction:ltr">${titleJP}</div>
      <div class="romaji">${romaji} ${note ? '• ' + note : ''}</div>
      <div style="margin-top:10px">
        <button class="btn" type="button">🔊 تشغيل</button>
      </div>
    `;
    div.querySelector('button').addEventListener('click', onclick);
    return div;
  }

  // fill hiragana
  HIRAGANA.forEach(([key,kana,romaji])=>{
    const card = buildCard(kana, romaji, '', ()=> playAudio('hiragana', key, kana));
    hiraganaGrid.appendChild(card);
  });

  // fill phrases
  PHRASES.forEach(([key,jp,ar])=>{
    const card = buildCard(jp, key.replace(/_/g,' '), ar, ()=> playAudio('phrases', key, jp));
    phrasesGrid.appendChild(card);
  });

  /********** تشغيل الصوت: يحاول تشغيل mp3 من الخادم وإلا يستخدم SpeechSynthesis كبديل **********/
  async function playAudio(type, key, jpText){
    const url = `${AUDIO_BASE}/${type}/${key}.mp3`;
    // انشئ عنصر صوت وحاول تشغيله
    try {
      const audio = new Audio(url);
      audio.volume = 1.0;
      // محاولة تشغيل — بعض المتصفحات قد تمنع التشغيل الآلي حتى يتفاعل المستخدم (لكن هنا استجابة للنقرة)
      await audio.play();
      return;
    } catch (err) {
      // فشل تشغيل mp3، سنستخدم SpeechSynthesis كبديل
      try {
        const ut = new SpeechSynthesisUtterance(jpText || key);
        ut.lang = 'ja-JP';
        // اختيار صوت ياباني إذا موجود
        const voices = speechSynthesis.getVoices();
        const jpVoice = voices.find(v => /ja|japanese/i.test(v.lang) || /japan/i.test(v.name));
        if (jpVoice) ut.voice = jpVoice;
        speechSynthesis.cancel();
        speechSynthesis.speak(ut);
      } catch(e2){
        alert('تعذر تشغيل الصوت (لا توجد ملفات صوت محلية ولا يدعم المتصفح النطق).');
      }
    }
  }

  /********** تحسين: تحميل أصوات الـ SpeechSynthesis على بعض المتصفحات (تحفيز) **********/
  // استدعاء getVoices مرةً واحدة لتحفيز المتصفح على تحميل الأصوات
  if ('speechSynthesis' in window) {
    window.speechSynthesis.getVoices();
  }
  </script>
</body>
</html>
