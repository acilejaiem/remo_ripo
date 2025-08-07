<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Chat avec ChatGPT</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 600px; margin: 2em auto; }
    #chat { border: 1px solid #ccc; padding: 1em; height: 300px; overflow-y: auto; }
    .bubble { margin: 0.5em 0; padding: 0.5em; border-radius: 10px; }
    .user { background: #daf8cb; text-align: right; }
    .assistant { background: #f1f0f0; text-align: left; }
  </style>
</head>
<body>
  <h1>Chat avec ChatGPT</h1>
  <div id="chat"></div>
  <input type="text" id="message" placeholder="Tapez votre message…" style="width: 80%;" />
  <button id="send">Envoyer</button>

  <script>
    const API_KEY = 'VOTRE_CLE_API';
    const chatDiv = document.getElementById('chat');
    const input = document.getElementById('message');
    const btn = document.getElementById('send');

    async function envoyerMessage() {
      const texte = input.value;
      if (!texte) return;

      // Affiche le message utilisateur
      const userBubble = document.createElement('div');
      userBubble.className = 'bubble user';
      userBubble.textContent = texte;
      chatDiv.appendChild(userBubble);
      input.value = '';

      try {
        const response = await fetch('https://api.openai.com/v1/chat/completions', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
            'Authorization': 'Bearer ' + API_KEY
          },
          body: JSON.stringify({
            model: 'gpt-3.5-turbo',
            messages: [{ role: 'user', content: texte }]
          })
        });

        const data = await response.json();
        const reply = data.choices[0].message.content;

        const assistBubble = document.createElement('div');
        assistBubble.className = 'bubble assistant';
        assistBubble.textContent = reply;
        chatDiv.appendChild(assistBubble);
      } catch (err) {
        console.error(err);
      }
    }

    btn.addEventListener('click', envoyerMessage);
    input.addEventListener('keydown', e => {
      if (e.key === 'Enter') envoyerMessage();
    });
  </script>
</body>
</html>
