<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>ARIA Voice Assistant</title>
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    background: #0a0a0a;
    color: #fff;
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    padding: 16px;
    gap: 14px;
  }
  h1 { text-align: center; font-size: 28px; color: #00e5ff; }
  .sub { text-align: center; font-size: 12px; color: #4ade80; margin-top: 2px; }

  .card {
    background: #1a1a1a;
    border: 1px solid #333;
    border-radius: 14px;
    padding: 16px;
  }
  .card label { font-size: 12px; color: #aaa; display: block; margin-bottom: 8px; }
  .card input {
    width: 100%; background: #0a0a0a; border: 1px solid #444;
    color: #fff; padding: 12px; border-radius: 10px; font-size: 14px; margin-bottom: 10px;
  }
  .card input:focus { outline: none; border-color: #00e5ff; }
  .btn-green {
    width: 100%; background: #4ade80; color: #000;
    border: none; padding: 14px; border-radius: 10px;
    font-size: 16px; font-weight: bold; cursor: pointer;
  }

  #status-box {
    background: #1a1a1a; border: 1px solid #333; border-radius: 14px;
    padding: 14px 16px; font-size: 14px; color: #aaa; text-align: center;
  }
  #status-box.good { color: #4ade80; border-color: #4ade80; }
  #status-box.bad  { color: #ff4f6b; border-color: #ff4f6b; }
  #status-box.info { color: #00e5ff; border-color: #00e5ff; }

  #heard-box { background: #1a1a1a; border: 1px solid #333; border-radius: 14px; padding: 14px 16px; display: none; }
  #heard-box .label { font-size: 11px; color: #666; margin-bottom: 6px; }
  #heard-box .text  { font-size: 15px; color: #fff; }

  #reply-box { background: #1a1a1a; border: 1px solid #00e5ff; border-radius: 14px; padding: 16px; display: none; }
  #reply-box .label { font-size: 11px; color: #00e5ff; margin-bottom: 8px; }
  #reply-box .text  { font-size: 15px; color: #fff; line-height: 1.5; }

  /* Backup button in case auto-open is blocked */
  #action-btn {
    display: none; width: 100%; padding: 18px; border-radius: 14px;
    border: none; font-size: 17px; font-weight: bold; cursor: pointer;
    text-decoration: none; text-align: center;
  }
  #action-btn.call     { background: #00ffb3; color: #000; }
  #action-btn.sms      { background: #00e5ff; color: #000; }
  #action-btn.whatsapp { background: #25d366; color: #000; }
  #action-btn.openapp  { background: #7b4fff; color: #fff; }
  #action-btn:active { opacity: 0.8; }

  .mic-wrap {
    display: flex; flex-direction: column; align-items: center;
    margin-top: auto; padding-top: 10px; gap: 10px;
  }
  #mic-btn {
    width: 90px; height: 90px; border-radius: 50%;
    background: linear-gradient(135deg, #00e5ff, #7b4fff);
    border: none; font-size: 36px; cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    transition: transform 0.15s;
  }
  #mic-btn:active { transform: scale(0.92); }
  #mic-btn.listening {
    background: linear-gradient(135deg, #ff4f6b, #ff8f00);
    animation: pulse 1.2s infinite;
  }
  @keyframes pulse {
    0%,100% { box-shadow: 0 0 0 0 rgba(255,79,107,0.5); }
    50%      { box-shadow: 0 0 0 20px rgba(255,79,107,0); }
  }
  #mic-label { font-size: 13px; color: #666; }

  .examples { background: #1a1a1a; border: 1px solid #333; border-radius: 14px; padding: 14px; }
  .examples .label { font-size: 11px; color: #666; margin-bottom: 8px; }
  .ex { font-size: 13px; color: #aaa; padding: 3px 0; }
  .ex span { color: #00e5ff; }
</style>
</head>
<body>

<h1>🎙️ ARIA</h1>
<div class="sub">Free AI Voice Assistant — Powered by Gemini</div>

<div class="card" id="key-card">
  <label>🔑 Paste your FREE Gemini API Key here</label>
  <input type="password" id="api-key" placeholder="AIza..." />
  <button class="btn-green" onclick="saveKey()">✅ Save & Connect</button>
</div>

<div id="status-box">Not connected — paste your API key above</div>

<div id="heard-box">
  <div class="label">🎤 YOU SAID:</div>
  <div class="text" id="heard-text"></div>
</div>

<div id="reply-box">
  <div class="label">🤖 ARIA SAYS:</div>
  <div class="text" id="reply-text"></div>
</div>

<!-- Backup button if auto-open is blocked by browser -->
<a id="action-btn">TAP TO OPEN</a>

<div class="examples">
  <div class="label">💡 TRY SAYING:</div>
  <div class="ex"><span>"Call Mom"</span></div>
  <div class="ex"><span>"Text John I am on my way"</span></div>
  <div class="ex"><span>"Open YouTube"</span></div>
  <div class="ex"><span>"Open WhatsApp"</span></div>
  <div class="ex"><span>"What is the weather today"</span></div>
</div>

<div class="mic-wrap">
  <button id="mic-btn" onclick="toggleMic()">🎤</button>
  <div id="mic-label">Tap mic to speak</div>
</div>

<script>
  let apiKey = localStorage.getItem('gemini_key') || '';
  let recognition = null;
  let listening = false;

  const APP_LINKS = {
    youtube:    'https://youtube.com',
    whatsapp:   'https://api.whatsapp.com/send',
    instagram:  'https://instagram.com',
    twitter:    'https://twitter.com',
    x:          'https://twitter.com',
    facebook:   'https://facebook.com',
    tiktok:     'https://tiktok.com',
    maps:       'https://maps.google.com',
    gmail:      'https://mail.google.com',
    spotify:    'https://open.spotify.com',
    netflix:    'https://netflix.com',
    google:     'https://google.com',
    news:       'https://news.google.com',
    weather:    'https://weather.com',
    calculator: 'https://www.google.com/search?q=calculator',
  };

  if (apiKey) {
    document.getElementById('api-key').value = apiKey;
    document.getElementById('key-card').style.display = 'none';
    setStatus('✅ Connected! Tap the mic and speak.', 'good');
  }

  function saveKey() {
    const k = document.getElementById('api-key').value.trim();
    if (!k || k.length < 10) { alert('Please paste your Gemini API key first!'); return; }
    apiKey = k;
    localStorage.setItem('gemini_key', k);
    document.getElementById('key-card').style.display = 'none';
    setStatus('✅ Connected! Tap the mic and speak.', 'good');
  }

  function setStatus(msg, type) {
    const el = document.getElementById('status-box');
    el.textContent = msg;
    el.className = type || '';
  }

  // ── Auto open a URL ─────────────────────────────────
  // On Android Chrome, window.location works for tel: sms: and http links
  function autoOpen(url, btnType, btnLabel) {
    // Try auto open
    window.location.href = url;

    // Show backup button in case browser blocks it
    setTimeout(() => {
      showBackupButton(btnType, btnLabel, url);
    }, 800);
  }

  function showBackupButton(type, label, href) {
    const btn = document.getElementById('action-btn');
    btn.textContent = label;
    btn.href = href;
    btn.className = type;
    btn.style.display = 'block';
    if (href.startsWith('http')) btn.target = '_blank';
    else btn.removeAttribute('target');
  }

  // ── Mic ─────────────────────────────────────────────
  function setupMic() {
    const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (!SR) {
      setStatus('❌ Open this page in Chrome browser!', 'bad');
      return false;
    }
    recognition = new SR();
    recognition.lang = 'en-US';
    recognition.interimResults = false;
    recognition.maxAlternatives = 1;

    recognition.onresult = function(e) {
      const said = e.results[0][0].transcript;
      stopMic();
      processCommand(said);
    };
    recognition.onerror = function(e) {
      stopMic();
      if (e.error === 'not-allowed') {
        setStatus('❌ Tap the 🔒 icon in address bar → allow Microphone → refresh', 'bad');
      } else {
        setStatus('❌ Mic error: ' + e.error + '. Tap mic to try again.', 'bad');
      }
    };
    recognition.onend = function() { stopMic(); };
    return true;
  }

  function toggleMic() {
    if (!recognition) { if (!setupMic()) return; }
    if (listening) { stopMic(); } else { startMic(); }
  }

  function startMic() {
    listening = true;
    try { recognition.start(); } catch(e) { recognition = null; if (setupMic()) recognition.start(); }
    document.getElementById('mic-btn').classList.add('listening');
    document.getElementById('mic-label').textContent = '🔴 Listening... tap to stop';
    setStatus('🎤 Listening... speak now!', 'info');
    document.getElementById('heard-box').style.display = 'none';
    document.getElementById('reply-box').style.display = 'none';
    document.getElementById('action-btn').style.display = 'none';
    window.speechSynthesis.cancel();
  }

  function stopMic() {
    listening = false;
    try { recognition.stop(); } catch(e) {}
    document.getElementById('mic-btn').classList.remove('listening');
    document.getElementById('mic-label').textContent = 'Tap mic to speak';
  }

  // ── Commands ─────────────────────────────────────────
  function processCommand(text) {
    document.getElementById('heard-text').textContent = text;
    document.getElementById('heard-box').style.display = 'block';

    const t = text.toLowerCase().trim();

    // CALL
    const callMatch = t.match(/(?:call|phone|ring|dial)\s+(.+)/);
    if (callMatch) {
      const name = callMatch[1].trim();
      showReply('📞 Calling ' + name + '...');
      speak('Calling ' + name);
      autoOpen('tel:' + encodeURIComponent(name), 'call', '📞  TAP TO CALL ' + name.toUpperCase());
      return;
    }

    // WHATSAPP — fixed URL
    if (t.includes('whatsapp')) {
      const msgMatch = t.match(/whatsapp\s+(?:\w+\s+)?(.+)/);
      const msg = msgMatch ? msgMatch[1] : '';
      showReply('💚 Opening WhatsApp...');
      speak('Opening WhatsApp');
      // Use the correct WhatsApp URL that works on Android
      const waUrl = msg
        ? 'https://api.whatsapp.com/send?text=' + encodeURIComponent(msg)
        : 'whatsapp://';
      autoOpen(waUrl, 'whatsapp', '💚  TAP TO OPEN WHATSAPP');
      return;
    }

    // SMS
    const smsMatch = t.match(/(?:text|send (?:a )?(?:message|sms)(?: to)?|message)\s+(\w+)(?:\s+(.+))?/);
    if (smsMatch) {
      const name = smsMatch[1];
      const msg = smsMatch[2] || '';
      showReply('💬 Opening SMS to ' + name + '...');
      speak('Texting ' + name);
      autoOpen('sms:' + encodeURIComponent(name) + '?body=' + encodeURIComponent(msg), 'sms', '💬  TAP TO TEXT ' + name.toUpperCase());
      return;
    }

    // OPEN APP
    const openMatch = t.match(/(?:open|launch|go to|show)\s+(\w+)/);
    if (openMatch) {
      const app = openMatch[1].toLowerCase();
      if (APP_LINKS[app]) {
        showReply('📱 Opening ' + app + '...');
        speak('Opening ' + app);
        autoOpen(APP_LINKS[app], 'openapp', '📱  TAP TO OPEN ' + app.toUpperCase());
        return;
      }
    }

    // AI question
    setStatus('🤖 Thinking...', 'info');
    askGemini(text);
  }

  function showReply(msg) {
    document.getElementById('reply-text').textContent = msg;
    document.getElementById('reply-box').style.display = 'block';
    setStatus('✅ Done!', 'good');
  }

  function speak(text) {
    window.speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    u.rate = 1.0;
    window.speechSynthesis.speak(u);
  }

  // ── Gemini ───────────────────────────────────────────
  async function askGemini(question) {
    if (!apiKey) {
      showReply('Please connect your Gemini API key first!');
      setStatus('❌ No API key!', 'bad');
      return;
    }
    try {
      const res = await fetch(
        'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=' + apiKey,
        {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({
            system_instruction: {
              parts: [{ text: 'You are ARIA, a helpful voice assistant. Give very short answers, 1 to 2 sentences only, because your answer will be spoken out loud. No bullet points, no markdown, just plain simple text.' }]
            },
            contents: [{ parts: [{ text: question }] }],
            generationConfig: { maxOutputTokens: 150 }
          })
        }
      );
      const data = await res.json();
      if (data.error) {
        showReply('❌ Error: ' + (data.error.message || 'Check your API key.'));
        setStatus('❌ API Error', 'bad');
        return;
      }
      const answer = data.candidates?.[0]?.content?.parts?.[0]?.text || 'Sorry, I could not find an answer.';
      document.getElementById('reply-text').textContent = answer;
      document.getElementById('reply-box').style.display = 'block';
      document.getElementById('action-btn').style.display = 'none';
      setStatus('✅ ARIA answered!', 'good');
      speak(answer);
    } catch(err) {
      showReply('❌ No internet or wrong API key. Please check and try again.');
      setStatus('❌ Connection failed.', 'bad');
    }
  }
</script>
</body>
</html>
