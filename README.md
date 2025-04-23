<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Carta Interativa</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      overflow: hidden;
      background: radial-gradient(circle, #ff69b4 0%, #ff0000 100%);
      font-family: 'Segoe UI', sans-serif;
      color: white;
      height: 100vh;
      position: relative;
    }

    .carta {
      width: 220px;
      height: 130px;
      background: linear-gradient(145deg, #ff8ac3, #ff3366);
      border: 3px solid white;
      border-radius: 15px;
      position: absolute;
      top: 20px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
      font-size: 1.2rem;
      cursor: pointer;
      box-shadow: 0 0 25px #ff69b4, 0 0 10px #fff inset;
      text-align: center;
      z-index: 5;
      padding: 10px;
      animation: pulse 2s infinite ease-in-out, girar 10s linear infinite;
      transition: left 2s ease;
    }

    @keyframes pulse {
      0% { transform: translateX(-50%) scale(1); }
      50% { transform: translateX(-50%) scale(1.05); }
      100% { transform: translateX(-50%) scale(1); }
    }

    @keyframes girar {
      0% { transform: translateX(-50%) rotate(0deg); }
      100% { transform: translateX(-50%) rotate(360deg); }
    }

    .mensagem {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      text-align: center;
      display: none;
      font-size: 1.4rem;
      max-width: 90%;
      line-height: 1.6;
      z-index: 10;
      background: rgba(255, 255, 255, 0.1);
      padding: 20px;
      border-radius: 20px;
      box-shadow: 0 0 30px #fff;
      backdrop-filter: blur(8px);
    }

    canvas {
      position: absolute;
      top: 0;
      left: 0;
      pointer-events: none;
      z-index: 1;
    }

    .coraçao {
      position: absolute;
      width: 20px;
      height: 20px;
      font-size: 20px;
      opacity: 0.8;
      animation: subir 5s linear infinite;
    }

    @keyframes subir {
      0% {
        transform: translateY(0) scale(1);
        opacity: 1;
      }
      100% {
        transform: translateY(-100vh) scale(1.5);
        opacity: 0;
      }
    }

    button:hover {
      background-color: #ff3366;
    }

  </style>
</head>
<body>

  <div class="carta" id="carta" onclick="mostrarMensagem()">💌 Clique aqui 💌</div>

  <div class="mensagem" id="mensagem">
    Querido Leonardo,<br><br>
    Você tem uma bunda tão linda que eu poderia dormir dias inteiros nas suas bochechas arrebitadas!<br>
    Queria viver com você e usar sua bunda como chapéu para sempre!<br><br>
    <strong>PS: Amo você</strong>
    <br><br>
    <button onclick="enviarEmail()" style="
      padding: 10px 20px;
      background-color: #ff69b4;
      color: white;
      border: none;
      border-radius: 20px;
      font-size: 1rem;
      cursor: pointer;
      box-shadow: 0 0 10px #fff;
      transition: 0.3s;
    ">
      Enviar um email de satisfação 💖
    </button>
  </div>

  <!-- Música (tocando em segundo plano) -->
  <audio autoplay loop>
    <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mp3">
    Seu navegador não suporta a tag de áudio.
  </audio>

  <canvas id="fogos"></canvas>

  <script>
    const carta = document.getElementById('carta');
    const mensagem = document.getElementById('mensagem');
    const canvas = document.getElementById('fogos');
    const ctx = canvas.getContext('2d');
    let particulas = [];

    function resizeCanvas() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }

    function mostrarMensagem() {
      carta.style.display = 'none';
      mensagem.style.display = 'block';
      startFireworks();
    }

    function startFireworks() {
      resizeCanvas();
      for (let i = 0; i < 100; i++) {
        particulas.push({
          x: canvas.width / 2,
          y: canvas.height / 2,
          radius: Math.random() * 2 + 1,
          color: `hsl(${Math.random() * 360}, 100%, 60%)`,
          angle: Math.random() * 2 * Math.PI,
          speed: Math.random() * 6 + 2
        });
      }
      animate();
    }

    function animate() {
      ctx.fillStyle = "rgba(0, 0, 0, 0.15)";
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      particulas.forEach(p => {
        p.x += Math.cos(p.angle) * p.speed;
        p.y += Math.sin(p.angle) * p.speed;
        p.radius *= 0.96;

        ctx.beginPath();
        ctx.arc(p.x, p.y, p.radius, 0, Math.PI * 2);
        ctx.fillStyle = p.color;
        ctx.fill();
      });

      particulas = particulas.filter(p => p.radius > 0.5);
      if (particulas.length > 0) {
        requestAnimationFrame(animate);
      }
    }

    // Mover carta continuamente na tela
    function moverCarta() {
      if (carta.style.display === 'none') return;

      const larguraMax = window.innerWidth - carta.offsetWidth;
      const alturaMax = window.innerHeight - carta.offsetHeight;

      // Movimentar carta aleatoriamente, dentro dos limites da tela
      const novoLeft = Math.random() * larguraMax;
      const novoTop = Math.random() * alturaMax;

      carta.style.left = `${novoLeft}px`;
      carta.style.top = `${novoTop}px`;
    }

    function criarCoracao() {
      const coracao = document.createElement("div");
      coracao.classList.add("coraçao");
      coracao.innerText = "❤️";
      coracao.style.left = Math.random() * window.innerWidth + "px";
      coracao.style.top = "100vh";
      document.body.appendChild(coracao);

      setTimeout(() => {
        coracao.remove();
      }, 5000);
    }

    function enviarEmail() {
      const email = "oliveiratayssa729@gmail.com";
      const assunto = encodeURIComponent("Você me deixou muito feliz! 💘");
      const corpo = encodeURIComponent("Recebi sua linda carta e fiquei emocionado(a)!\n\nMuito obrigado por esse carinho.\n\nCom amor ❤️,\nLeonardo");
      window.location.href = `mailto:${email}?subject=${assunto}&body=${corpo}`;
    }

   
}
<audio autoplay loop>
  <source src="https://www.youtube.com/watch?v=lJBcZHzgD7s&pp=0gcJCfcAhR29_xXO" type="audio/mp3">
  Seu navegador não suporta a tag de áudio.
</audio>
    }

window.onload = () => {
      resizeCanvas();
      setInterval(moverCarta, 2000); // move a carta a cada 2 segundos
      setInterval(criarCoracao, 300); // corações subindo
    };

    window.addEventListener('resize', resizeCanvas);
  </script>

</body>
</html>
