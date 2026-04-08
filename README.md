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
  }

  h1 {
    text-align: center;
    font-size: 28px;
    color: #00e5ff;
    margin-bottom: 4px;
  }
  .sub {
    text-align: center;
    font-size: 12px;
    color: #4ade80;
    margin-bottom: 20px;
  }

  /* API Key box */
  .card {
    background: #1a1a1a;
    border: 1px solid #333;
    border-radius: 14px;
    padding: 16px;
    margin-bottom: 16px;
  }
  .card label {
    font-size: 12px;
    color: #aaa;
    display: block;
    margin-bottom: 8px;
  }
  .card input {
    width: 100%;
    background: #0a0a0a;
    border: 1px solid #444;
    color: #fff;
    padding: 12px;
    border-radius: 10px;
    font-size: 14px;
    margin-bottom: 10px;
  }
  .card input:focus { outline: none; border-color: #00e5ff; }
  .btn-green {
    width: 100%;
    background: #4ade80;
    color: #000;
    border: none;
    padding: 14px;
    border-radius: 10px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
  }
  .btn-green:active { opacity: 0.8; }

  /* Status */
  #status-box {
    background: #1a1a1a;
    border: 1px solid #333;
    border-radius: 14px;
    padding: 14px 16px;
    margin-bottom: 16px;
    font-size: 14px;
    color: #aaa;
    min-height: 52px;
    text-align: center;
  }
  #status-box.good { color: #4ade80; border-color: #4ade80; }
  #status-box.bad  { color: #ff4f6b; border-color: #ff4f6b; }
  #status-box.info { color: #00e5ff; border-color: #00e5ff; }

  /* What you said */
  #heard-box {
    background: #1a1a1a;
    border: 1px solid #333;
    border-radius: 14px;
    padding: 14px 16px;
    margin-bottom: 16px;
    display: none;
  }
  #heard-box .label { font-size: 11px; color: #666; margin-bottom: 6px; }
  #heard-box .text  { font-size: 15px; color: #fff; }

  /* Reply box */
  #reply-box {
    background: #1a1a1a;
    border: 1px solid #00e5ff;
    border-radius: 14px;
    padding: 16px;
    margin-bottom: 16px;
    display: none;
  }
  #reply-box .label { font-size: 11px; color: #00e5ff; margin-bottom: 8px; }
  #reply-box .text  { font-size: 15px; color: #fff; line-height: 1.5; }

  /* Action button */
  #action-btn {
    display: none;
    width: 100%;
    padding: 18px;
    border-radius: 14px;
    border: none;
    font-size: 18px;
    font-weight: bold;
    cursor: pointer;
    margin-bottom: 16px;
    text-decoration: none;
    text-align: center;
  }
  #action-btn.call      { background: #00ffb3; color: #000; }
  #action-btn.sms       { background: #00e5ff; color: #000; }
  #action-btn.whatsapp  { background: #25d366; color: #000; }
  #action-btn.openapp   { background: #7b4fff; color: #fff; }
  #action-btn:active { opacity: 0.8; }

  /* Mic button */
  .mic-wrap {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-top: auto;
    padding-top: 10px;
    gap: 10px;
  }
  #mic-btn {
    width: 90px;
    height: 90px;
    border-radius: 50%;
    background: linear-gradient(135deg, #00e5ff, #7b4fff);
    border: none;
    font-size: 36px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
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

  .examples {
    background: #1a1a1a;
    border: 1px solid #333;
    border-radius: 14px;
    padding: 14px;
    margin-bottom: 16px;
  }
  .examples .label { font-size: 11px; color: #666; margin-bottom: 8px; }
  .ex { font-size: 13px; color: #aaa; padding: 4px 0; }
  .ex span { color: #00e5ff; }
</style>
</head>
<body>

<h1>🎙️ ARIA</h1>
<div class="sub">Free AI Voice Assistant — Powered by Gemini</div>

<!-- API Key -->
<div class="card" id="key-card">
  <label>🔑 Paste your FREE Gemini API Key here</label>
  <input type="password" id="api-key" placeholder="AIza..." />
  <button class="btn-green" onclick="saveKey()">✅ Save & Connect</button>
</div>

<!-- Status -->
<div id="status-box">Not connected — paste your API key above</div>

<!-- What ARIA heard -->
<div id="heard-box">
  <div class="label">🎤 YOU SAID:</div>
  <div class="text" id="heard-text"></div>
</div>

<!-- ARIA reply -->
<div id="reply-box">
  <div class="label">🤖 ARIA SAYS:</div>
  <div class="text" id="reply-text"></div>
</div>

<!-- Big action button -->
<a id="action-btn">TAP TO OPEN</a>

<!-- Try these examples -->
<div class="examples">
  <div class="label">💡 TRY SAYING:</div>
  <div class="ex"><span>"Call Mom"</span></div>
  <div class="ex"><span>"Text John I am on my way"</span></div>
  <div class="ex"><span>"Open YouTube"</span></div>
  <div class="ex"><span>"What is the weather today"</span></div>
  <div class="ex"><span>"Open WhatsApp"</span></div>
</div>

<!-- Mic -->
<div class="mic-wrap">
  <button id="mic-btn" onclick="toggleMic()">🎤</button>
  <div id="mic-label">Tap mic to speak</div>
</div>

<script>
  // ── Setup ───────────────────────────────────────────
  let apiKey = localStorage.getItem('gemini_key') || '';
  let recognition = null;
  let listening = false;

  const APP_LINKS = {
    youtube:    'https://youtube.com',
    whatsapp:   'https://wa.me',
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
    setStatus('✅ Connected! Tap the mic and speak.', 'good');
    document.getElementById('key-card').style.display = 'none';
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

  // ── Speech Recognition ──────────────────────────────
  function setupMic() {
    const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (!SR) {
      setStatus('❌ Open this in Chrome browser for voice to work!', 'bad');
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
        setStatus('❌ Please allow microphone permission in Chrome!', 'bad');
      } else {
        setStatus('❌ Mic error: ' + e.error + '. Try again.', 'bad');
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
    try { recognition.start(); } catch(e) { recognition = null; setupMic(); recognition.start(); }
    document.getElementById('mic-btn').classList.add('listening');
    document.getElementById('mic-label').textContent = '🔴 Listening... tap to stop';
    setStatus('🎤 Listening... speak now!', 'info');
    // hide old results
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

  // ── Command Processing ──────────────────────────────
  function processCommand(text) {
    // Show what was heard
    document.getElementById('heard-text').textContent = text;
    document.getElementById('heard-box').style.display = 'block';

    const t = text.toLowerCase().trim();

    // CALL
    const callMatch = t.match(/(?:call|phone|ring|dial)\s+(.+)/);
    if (callMatch) {
      const name = callMatch[1].trim();
      showReply('📞 Calling ' + name + '! Tap the button below.');
      showAction('call', '📞  TAP TO CALL ' + name.toUpperCase(), 'tel:' + encodeURIComponent(name));
      speak('Calling ' + name);
      return;
    }

    // WHATSAPP
    if (t.includes('whatsapp')) {
      const msgMatch = t.match(/whatsapp\s+\w+\s+(.+)/);
      const msg = msgMatch ? msgMatch[1] : '';
      showReply('💚 Opening WhatsApp! Tap below.');
      showAction('whatsapp', '💚  TAP TO OPEN WHATSAPP', 'https://wa.me/?text=' + encodeURIComponent(msg));
      speak('Opening WhatsApp');
      return;
    }

    // SMS / TEXT
    const smsMatch = t.match(/(?:text|send (?:a )?(?:message|sms)(?: to)?|message)\s+(\w+)(?:\s+(.+))?/);
    if (smsMatch) {
      const name = smsMatch[1];
      const msg = smsMatch[2] || '';
      showReply('💬 Sending text to ' + name + '! Tap below.');
      showAction('sms', '💬  TAP TO TEXT ' + name.toUpperCase(), 'sms:' + encodeURIComponent(name) + '?body=' + encodeURIComponent(msg));
      speak('Texting ' + name);
      return;
    }

    // OPEN APP
    const openMatch = t.match(/(?:open|launch|go to|show)\s+(\w+)/);
    if (openMatch) {
      const app = openMatch[1].toLowerCase();
      if (APP_LINKS[app]) {
        showReply('📱 Opening ' + app + '! Tap below.');
        showAction('openapp', '📱  TAP TO OPEN ' + app.toUpperCase(), APP_LINKS[app]);
        speak('Opening ' + app);
        return;
      }
    }

    // AI Question — ask Gemini
    setStatus('🤖 Thinking...', 'info');
    askGemini(text);
  }

  function showReply(msg) {
    document.getElementById('reply-text').textContent = msg;
    document.getElementById('reply-box').style.display = 'block';
    setStatus('✅ Done! See below.', 'good');
  }

  function showAction(type, label, href) {
    const btn = document.getElementById('action-btn');
    btn.textContent = label;
    btn.href = href;
    btn.className = type;
    btn.style.display = 'block';
    // For external links open in new tab
    if (href.startsWith('http')) btn.target = '_blank';
    else btn.removeAttribute('target');
  }

  function speak(text) {
    window.speechSynthesis.cancel();
    const u = new SpeechSynthesisUtterance(text);
    u.rate = 1.0;
    window.speechSynthesis.speak(u);
  }

  // ── Gemini API ──────────────────────────────────────
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
        const msg = data.error.message || 'API error';
        showReply('❌ Error: ' + msg);
        setStatus('❌ ' + msg, 'bad');
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
