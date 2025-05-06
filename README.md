<html>
    <head>

        <title>YESHUA</title>
    </head>

    <body>
    

        <h1>TESTE SITE WANDER</h1>
        <img src="">
    </body>

    <!DOCTYPE html>
    <html lang="pt-BR">
    <head>
      <meta charset="UTF-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
      <title>Conversor Binário Hacker</title>
      <style>
        html, body {
          margin: 0;
          padding: 0;
          min-height: 100vh;
          background: black;
          color: #00ff00;
          font-family: 'Courier New', Courier, monospace;
          overflow-x: hidden;
        }
    
        canvas {
          position: fixed;
          top: 0;
          left: 0;
          z-index: -1;
        }
    
        .container {
          position: relative;
          padding: 2rem;
          z-index: 1;
        }
    
        textarea, input, button, select {
          width: 100%;
          margin-top: 1rem;
          padding: 1rem;
          font-size: 1rem;
          background-color: #111;
          border: 1px solid #0f0;
          color: #0f0;
        }
    
        button {
          cursor: pointer;
          font-weight: bold;
        }
    
        .resultado {
          margin-top: 1rem;
          font-weight: bold;
          background-color: #000;
          padding: 1rem;
          border: 1px solid #0f0;
          border-radius: 8px;
        }
    
        h1, h2 {
          color: #00ff00;
        }
      </style>
    </head>
    <body>
      <canvas id="matrix"></canvas>
    
      <div class="container">
        <h1>Conversor Binário + Criptografia</h1>
    
        <h2>Texto → Binário</h2>
        <textarea id="texto" rows="3" placeholder="Digite um texto aqui..."></textarea>
        <button onclick="converterParaBinario()">Converter para Binário</button>
        <button onclick="limparTextoParaBinario()">Limpar</button>
        <div class="resultado" id="resultadoBinario"></div>
    
        <h2>Criptografar / Descriptografar Binário com Porta Lógica</h2>
        <textarea id="mensagemBinaria" rows="3" placeholder="Digite a mensagem binária aqui (ex: 01010101...)"></textarea>
        <select id="portaLogica">
          <option value="AND">AND</option>
          <option value="OR">OR</option>
          <option value="XOR">XOR</option>
        </select>
        <input id="chave" placeholder="Digite a chave binária (ex: 101010...)">
        <button onclick="criptografar()">Criptografar</button>
        <button onclick="descriptografar()">Descriptografar</button>
        <button onclick="limparCriptografia()">Limpar Criptografia</button>
        <div class="resultado" id="resultadoCriptografia"></div>
    
        <h2>Binário → Texto</h2>
        <textarea id="binario" rows="3" placeholder="Digite código binário (ex: 01001000 01101001)"></textarea>
        <button onclick="converterParaTexto()">Converter para Texto</button>
        <button onclick="limparBinarioParaTexto()">Limpar</button>
        <div class="resultado" id="resultadoTexto"></div>
      </div>
    
      <script>
        function converterParaBinario() {
          const texto = document.getElementById('texto').value;
          let binario = '';
    
          for (let i = 0; i < texto.length; i++) {
            const bin = texto[i].charCodeAt(0).toString(2).padStart(8, '0');
            binario += bin + ' ';
          }
    
          document.getElementById('resultadoBinario').innerText = 'Binário: ' + binario.trim();
        }
    
        function limparTextoParaBinario() {
          document.getElementById('texto').value = '';
          document.getElementById('resultadoBinario').innerText = '';
        }
    
        function aplicarPortaLogica(bit1, bit2, tipo) {
          switch(tipo) {
            case 'AND': return (bit1 & bit2).toString();
            case 'OR': return (bit1 | bit2).toString();
            case 'XOR': return (bit1 ^ bit2).toString();
            default: return '0';
          }
        }
    
        function criptografar() {
          document.getElementById('resultadoCriptografia').innerText = '';
    
          const mensagem = document.getElementById('mensagemBinaria').value.trim().replace(/ /g, '');
          const chave = document.getElementById('chave').value.trim().replace(/ /g, '');
          const porta = document.getElementById('portaLogica').value;
    
          if (mensagem === '' || chave === '') {
            alert('Preencha a mensagem binária e a chave.');
            return;
          }
    
          let chaveEstendida = chave;
          while (chaveEstendida.length < mensagem.length) {
            chaveEstendida += chave;
          }
          chaveEstendida = chaveEstendida.slice(0, mensagem.length);
    
          let resultado = '';
          for (let i = 0; i < mensagem.length; i++) {
            resultado += aplicarPortaLogica(mensagem[i], chaveEstendida[i], porta);
          }
    
          let resultadoFormatado = resultado.match(/.{1,8}/g)?.join(' ') || resultado;
          document.getElementById('resultadoCriptografia').innerText = `Criptografado (${porta}): ${resultadoFormatado}`;
        }
    
        function descriptografar() {
          document.getElementById('resultadoCriptografia').innerText = '';
    
          const mensagem = document.getElementById('mensagemBinaria').value.trim().replace(/ /g, '');
          const chave = document.getElementById('chave').value.trim().replace(/ /g, '');
          const porta = document.getElementById('portaLogica').value;
    
          if (mensagem === '' || chave === '') {
            alert('Preencha a mensagem binária e a chave.');
            return;
          }
    
          let chaveEstendida = chave;
          while (chaveEstendida.length < mensagem.length) {
            chaveEstendida += chave;
          }
          chaveEstendida = chaveEstendida.slice(0, mensagem.length);
    
          let resultado = '';
          for (let i = 0; i < mensagem.length; i++) {
            if (porta === 'XOR') {
              resultado += aplicarPortaLogica(mensagem[i], chaveEstendida[i], 'XOR');
            } else {
              resultado += '?'; // Não é possível descriptografar AND/OR de forma confiável
            }
          }
    
          let resultadoFormatado = resultado.match(/.{1,8}/g)?.join(' ') || resultado;
          document.getElementById('resultadoCriptografia').innerText = `Descriptografado (${porta}): ${resultadoFormatado}`;
        }
    
        function limparCriptografia() {
          document.getElementById('mensagemBinaria').value = '';
          document.getElementById('chave').value = '';
          document.getElementById('resultadoCriptografia').innerText = '';
        }
    
        function converterParaTexto() {
          const input = document.getElementById('binario').value.trim();
          const binarios = input.split(' ');
          let texto = '';
    
          for (let bin of binarios) {
            if (/^[01]{8}$/.test(bin)) {
              texto += String.fromCharCode(parseInt(bin, 2));
            } else {
              texto += '�';
            }
          }
    
          document.getElementById('resultadoTexto').innerText = 'Texto: ' + texto;
        }
    
        function limparBinarioParaTexto() {
          document.getElementById('binario').value = '';
          document.getElementById('resultadoTexto').innerText = '';
        }
    
        const canvas = document.getElementById("matrix");
        const ctx = canvas.getContext("2d");
    
        canvas.height = window.innerHeight;
        canvas.width = window.innerWidth;
    
        const letters = "01";
        const fontSize = 14;
        const columns = canvas.width / fontSize;
        const drops = Array.from({ length: columns }).fill(1);
    
        function draw() {
          ctx.fillStyle = "rgba(0, 0, 0, 0.05)";
          ctx.fillRect(0, 0, canvas.width, canvas.height);
    
          ctx.fillStyle = "#0F0";
          ctx.font = fontSize + "px monospace";
    
          for (let i = 0; i < drops.length; i++) {
            const text = letters.charAt(Math.floor(Math.random() * letters.length));
            ctx.fillText(text, i * fontSize, drops[i] * fontSize);
    
            if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
              drops[i] = 0;
            }
    
            drops[i]++;
          }
        }
    
        setInterval(draw, 33);
      </script>
    </body>
    </html>
