<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Joyeux anniversaire !</title>
  <style>
    body {
      margin: 0;
      background: black;
      overflow: hidden;
      color: white;
      text-align: center;
      font-family: 'Segoe UI', sans-serif;
    }

    h1 {
      margin-top: 100px;
      font-size: 3em;
      color: #ff66b2;
    }

    canvas {
      position: absolute;
      top: 0;
      left: 0;
    }

    .bougies {
      margin-top: 40px;
    }

    .bougie {
      display: inline-block;
      margin: 0 10px;
      width: 20px;
      height: 50px;
      background: orange;
      position: relative;
      border-radius: 3px;
    }

    .flamme {
      width: 10px;
      height: 10px;
      background: yellow;
      border-radius: 50%;
      position: absolute;
      top: -10px;
      left: 5px;
      animation: flamme 1s infinite alternate;
    }

    @keyframes flamme {
      0% { transform: scale(1); opacity: 0.8; }
      100% { transform: scale(1.5); opacity: 0.2; }
    }
  </style>
</head>
<body>
  <h1>Happy birthday ma vie❤️</h1>
  <div class="bougies">
    <div class="bougie"><div class="flamme"></div></div>
    <div class="bougie"><div class="flamme"></div></div>
    <div class="bougie"><div class="flamme"></div></div>
  </div>

  <canvas id="fireworks"></canvas>

  <script>
    // Feux d'artifice inspirés d'un script open source
    const canvas = document.getElementById('fireworks');
    const ctx = canvas.getContext('2d');
    let cw, ch;

    function resizeCanvas() {
      cw = canvas.width = window.innerWidth;
      ch = canvas.height = window.innerHeight;
    }
    resizeCanvas();
    window.addEventListener('resize', resizeCanvas);

    class Firework {
      constructor() {
        this.reset();
      }

      reset() {
        this.x = Math.random() * cw;
        this.y = ch;
        this.targetY = Math.random() * ch / 2;
        this.speed = Math.random() * 3 + 2;
        this.color = `hsl(${Math.random() * 360}, 100%, 50%)`;
        this.radius = 2;
      }

      update() {
        this.y -= this.speed;
        if (this.y < this.targetY) {
          this.explode();
          this.reset();
        }
      }

      draw() {
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.radius, 0, 2 * Math.PI);
        ctx.fillStyle = this.color;
        ctx.fill();
      }

      explode() {
        for (let i = 0; i < 20; i++) {
          particles.push(new Particle(this.x, this.y, this.color));
        }
      }
    }

    class Particle {
      constructor(x, y, color) {
        this.x = x;
        this.y = y;
        this.color = color;
        this.radius = 2;
        this.angle = Math.random() * 2 * Math.PI;
        this.speed = Math.random() * 4 + 1;
        this.life = 60;
      }

      update() {
        this.x += Math.cos(this.angle) * this.speed;
        this.y += Math.sin(this.angle) * this.speed;
        this.life--;
      }

      draw() {
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.radius, 0, 2 * Math.PI);
        ctx.fillStyle = this.color;
        ctx.fill();
      }
    }

    const fireworks = [];
    const particles = [];

    for (let i = 0; i < 5; i++) fireworks.push(new Firework());

    function animate() {
      ctx.fillStyle = 'rgba(0, 0, 0, 0.2)';
      ctx.fillRect(0, 0, cw, ch);

      fireworks.forEach(f => { f.update(); f.draw(); });
      particles.forEach((p, i) => {
        p.update();
        p.draw();
        if (p.life <= 0) particles.splice(i, 1);
      });

      requestAnimationFrame(animate);
    }

    animate();
  </script>
</body>
</html>
