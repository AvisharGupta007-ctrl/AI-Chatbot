index.html
Product manager AI assistance
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Assistant</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Mono:ital,wght@0,300;0,400;1,300&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0d0d0d;
    --surface: #161616;
    --surface2: #1e1e1e;
    --border: #2a2a2a;
    --accent: #c8ff00;
    --accent2: #00ffc8;
    --text: #f0f0f0;
    --muted: #666;
    --user-bg: #1a1a1a;
    --bot-bg: #111;
  }

  body {
    font-family: 'Syne', sans-serif;
    background: var(--bg);
    color: var(--text);
    height: 100vh;
    display: flex;
    flex-direction: column;
    overflow: hidden;
  }

  /* Animated background grid */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(200,255,0,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(200,255,0,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  /* Header */
  header {
    position: relative;
    z-index: 10;
    padding: 16px 24px;
    display: flex;
    align-items: center;
    gap: 12px;
    border-bottom: 1px solid var(--border);
    background: rgba(13,13,13,0.95);
    backdrop-filter: blur(10px);
  }

  .logo {
    width: 36px; height: 36px;
    background: var(--accent);
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
    flex-shrink: 0;
    animation: pulse-logo 3s ease-in-out infinite;
  }

  @keyframes pulse-logo {
    0%, 100% { box-shadow: 0 0 0 0 rgba(200,255,0,0.4); }
    50% { box-shadow: 0 0 0 8px rgba(200,255,0,0); }
  }

  .header-text h1 {
    font-size: 16px;
    font-weight: 800;
    letter-spacing: -0.02em;
    color: var(--text);
  }

  .status {
    display: flex; align-items: center; gap: 6px;
    font-family: 'DM Mono', monospace;
    font-size: 11px;
    color: var(--accent2);
  }

  .status-dot {
    width: 6px; height: 6px;
    background: var(--accent2);
    border-radius: 50%;
    animation: blink 2s ease-in-out infinite;
  }

  @keyframes blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.3; }
  }

  .header-tag {
    margin-left: auto;
    font-family: 'DM Mono', monospace;
    font-size: 10px;
    color: var(--muted);
    border: 1px solid var(--border);
    padding: 4px 8px;
    border-radius: 4px;
  }

  /* Chat area */
  #chat {
    flex: 1;
    overflow-y: auto;
    padding: 24px;
    display: flex;
    flex-direction: column;
    gap: 16px;
    position: relative;
    z-index: 1;
    scroll-behavior: smooth;
  }

  #chat::-webkit-scrollbar { width: 4px; }
  #chat::-webkit-scrollbar-track { background: transparent; }
  #chat::-webkit-scrollbar-thumb { background: var(--border); border-radius: 2px; }

  /* Welcome message */
  .welcome {
    text-align: center;
    padding: 40px 20px;
    animation: fadeUp 0.6s ease forwards;
  }

  .welcome-title {
    font-size: 32px;
    font-weight: 800;
    letter-spacing: -0.03em;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 8px;
  }

  .welcome-sub {
    font-family: 'DM Mono', monospace;
    font-size: 13px;
    color: var(--muted);
    margin-bottom: 32px;
  }

  .suggestions {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    justify-content: center;
    max-width: 520px;
    margin: 0 auto;
  }

  .suggestion-chip {
    background: var(--surface);
    border: 1px solid var(--border);
    color: var(--text);
    padding: 8px 14px;
    border-radius: 20px;
    font-family: 'DM Mono', monospace;
    font-size: 12px;
    cursor: pointer;
    transition: all 0.2s;
  }

  .suggestion-chip:hover {
    border-color: var(--accent);
    color: var(--accent);
    transform: translateY(-1px);
  }

  /* Messages */
  .message {
    display: flex;
    gap: 12px;
    max-width: 780px;
    width: 100%;
    animation: fadeUp 0.3s ease forwards;
    opacity: 0;
  }

  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .message.user { align-self: flex-end; flex-direction: row-reverse; }
  .message.bot { align-self: flex-start; }

  .avatar {
    width: 32px; height: 32px;
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    font-size: 14px;
    flex-shrink: 0;
    margin-top: 2px;
  }

  .message.bot .avatar { background: var(--accent); color: #000; }
  .message.user .avatar { background: var(--surface2); border: 1px solid var(--border); }

  .bubble {
    padding: 12px 16px;
    border-radius: 12px;
    font-size: 14px;
    line-height: 1.65;
    max-width: calc(100% - 44px);
  }

  .message.bot .bubble {
    background: var(--surface);
    border: 1px solid var(--border);
    border-top-left-radius: 2px;
    color: var(--text);
  }

  .message.user .bubble {
    background: var(--accent);
    color: #000;
    font-weight: 600;
    border-top-right-radius: 2px;
  }

  /* Typing indicator */
  .typing-indicator {
    display: flex;
    align-items: center;
    gap: 4px;
    padding: 14px 16px;
  }

  .dot {
    width: 6px; height: 6px;
    background: var(--muted);
    border-radius: 50%;
    animation: bounce 1.2s ease-in-out infinite;
  }

  .dot:nth-child(2) { animation-delay: 0.2s; }
  .dot:nth-child(3) { animation-delay: 0.4s; }

  @keyframes bounce {
    0%, 60%, 100% { transform: translateY(0); opacity: 0.4; }
    30% { transform: translateY(-6px); opacity: 1; }
  }

  /* Streaming text cursor */
  .streaming::after {
    content: '▋';
    animation: cursor-blink 0.7s step-end infinite;
    color: var(--accent);
    font-size: 12px;
  }

  @keyframes cursor-blink {
    0%, 100% { opacity: 1; }
    50% { opacity: 0; }
  }

  /* Input area */
  .input-area {
    position: relative;
    z-index: 10;
    padding: 16px 24px 20px;
    border-top: 1px solid var(--border);
    background: rgba(13,13,13,0.95);
    backdrop-filter: blur(10px);
  }

  .input-wrapper {
    display: flex;
    align-items: flex-end;
    gap: 10px;
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 10px 10px 10px 16px;
    transition: border-color 0.2s;
  }

  .input-wrapper:focus-within {
    border-color: var(--accent);
    box-shadow: 0 0 0 2px rgba(200,255,0,0.08);
  }

  #input {
    flex: 1;
    background: none;
    border: none;
    outline: none;
    color: var(--text);
    font-family: 'Syne', sans-serif;
    font-size: 14px;
    resize: none;
    max-height: 120px;
    line-height: 1.5;
    padding: 2px 0;
  }

  #input::placeholder { color: var(--muted); }

  #send-btn {
    width: 36px; height: 36px;
    background: var(--accent);
    border: none;
    border-radius: 8px;
    cursor: pointer;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
    transition: all 0.2s;
    font-size: 16px;
  }

  #send-btn:hover { background: var(--accent2); transform: scale(1.05); }
  #send-btn:active { transform: scale(0.95); }
  #send-btn:disabled { opacity: 0.4; cursor: not-allowed; transform: none; }

  .input-hint {
    text-align: center;
    font-family: 'DM Mono', monospace;
    font-size: 10px;
    color: var(--muted);
    margin-top: 8px;
  }

  /* Code blocks in responses */
  .bubble code {
    font-family: 'DM Mono', monospace;
    background: rgba(200,255,0,0.08);
    color: var(--accent);
    padding: 2px 5px;
    border-radius: 3px;
    font-size: 12px;
  }

  .bubble pre {
    background: #0a0a0a;
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 12px;
    overflow-x: auto;
    margin: 8px 0;
    font-family: 'DM Mono', monospace;
    font-size: 12px;
    line-height: 1.6;
  }

  /* Error state */
  .error-bubble {
    background: rgba(255, 80, 80, 0.08) !important;
    border-color: rgba(255,80,80,0.3) !important;
    color: #ff8080 !important;
  }
</style>
</head>
<body>

<header>
  <div class="logo">✦</div>
  <div class="header-text">
    <h1>AI Assistant</h1>
    <div class="status">
      <div class="status-dot"></div>
      <span>online</span>
    </div>
  </div>
  <div class="header-tag">powered by claude</div>
</header>

<div id="chat">
  <div class="welcome" id="welcome">
    <div class="welcome-title">Hello there 👋</div>
    <div class="welcome-sub">// ask me anything</div>
    <div class="suggestions">
      <div class="suggestion-chip" onclick="sendSuggestion(this)">What is a Product Manager?</div>
      <div class="suggestion-chip" onclick="sendSuggestion(this)">How do I prepare for a PM interview?</div>
      <div class="suggestion-chip" onclick="sendSuggestion(this)">Explain AI in simple terms</div>
      <div class="suggestion-chip" onclick="sendSuggestion(this)">Help me write a PRD</div>
      <div class="suggestion-chip" onclick="sendSuggestion(this)">What skills does an AI PM need?</div>
    </div>
  </div>
</div>

<div class="input-area">
  <div class="input-wrapper">
    <textarea id="input" rows="1" placeholder="Type a message..."></textarea>
    <button id="send-btn" onclick="sendMessage()">➤</button>
  </div>
  <div class="input-hint">press Enter to send · Shift+Enter for new line</div>
</div>

<script>
  const chat = document.getElementById('chat');
  const input = document.getElementById('input');
  const sendBtn = document.getElementById('send-btn');
  const welcome = document.getElementById('welcome');

  const history = [];

  // Auto-resize textarea
  input.addEventListener('input', () => {
    input.style.height = 'auto';
    input.style.height = Math.min(input.scrollHeight, 120) + 'px';
  });

  input.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      sendMessage();
    }
  });

  function sendSuggestion(el) {
    input.value = el.textContent;
    sendMessage();
  }

  function addMessage(role, text, isStreaming = false) {
    if (welcome) welcome.style.display = 'none';

    const wrap = document.createElement('div');
    wrap.className = `message ${role}`;

    const avatar = document.createElement('div');
    avatar.className = 'avatar';
    avatar.textContent = role === 'bot' ? '✦' : '👤';

    const bubble = document.createElement('div');
    bubble.className = 'bubble' + (isStreaming ? ' streaming' : '');
    bubble.innerHTML = formatText(text);

    wrap.appendChild(avatar);
    wrap.appendChild(bubble);
    chat.appendChild(wrap);
    chat.scrollTop = chat.scrollHeight;
    return bubble;
  }

  function addTyping() {
    const wrap = document.createElement('div');
    wrap.className = 'message bot';
    wrap.id = 'typing';

    const avatar = document.createElement('div');
    avatar.className = 'avatar';
    avatar.textContent = '✦';

    const bubble = document.createElement('div');
    bubble.className = 'bubble';
    bubble.innerHTML = `<div class="typing-indicator"><div class="dot"></div><div class="dot"></div><div class="dot"></div></div>`;

    wrap.appendChild(avatar);
    wrap.appendChild(bubble);
    chat.appendChild(wrap);
    chat.scrollTop = chat.scrollHeight;
    return wrap;
  }

  function formatText(text) {
    // Basic markdown-like formatting
    return text
      .replace(/```([\s\S]*?)```/g, '<pre><code>$1</code></pre>')
      .replace(/`([^`]+)`/g, '<code>$1</code>')
      .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
      .replace(/\*(.*?)\*/g, '<em>$1</em>')
      .replace(/\n/g, '<br>');
  }

  async function sendMessage() {
    const text = input.value.trim();
    if (!text || sendBtn.disabled) return;

    input.value = '';
    input.style.height = 'auto';
    sendBtn.disabled = true;

    addMessage('user', text);
    history.push({ role: 'user', content: text });

    const typing = addTyping();

    try {
      const response = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          model: 'claude-sonnet-4-20250514',
          max_tokens: 1000,
          system: "You are a helpful, smart, and friendly AI assistant. You are particularly knowledgeable about Product Management, AI, and technology. Keep responses concise and clear. Use markdown formatting when helpful.",
          messages: history
        })
      });

      const data = await response.json();
      typing.remove();

      if (data.error) throw new Error(data.error.message);

      const reply = data.content?.[0]?.text || 'Sorry, I could not generate a response.';
      history.push({ role: 'assistant', content: reply });
      addMessage('bot', reply);

    } catch (err) {
      typing.remove();
      const bubble = addMessage('bot', '⚠ ' + (err.message || 'Something went wrong. Please try again.'));
      bubble.classList.add('error-bubble');
      history.pop(); // remove failed user message from history
    }

    sendBtn.disabled = false;
    input.focus();
  }
</script>
</body>
</html>
