<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Livro-jogo do Detetive Griffon</title>
  <link href="https://fonts.googleapis.com/css2?family=Winky+Sans&display=swap" rel="stylesheet">
  <style>
    body {
      background-image: url('fundo.png');
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      background-attachment: fixed;
      font-family: 'Winky Sans', sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 40px;
    }
    .container {
      max-width: 600px;
      width: 100%;
      text-align: center;
    }
    .flags {
      display: flex;
      align-items: center;
      gap: 15px;
      font-size: 18px;
      justify-content: center;
      margin-bottom: 20px;
    }
    .flags img {
      width: 40px;
      height: auto;
      cursor: pointer;
    }
    select, input[type="text"], button {
      padding: 10px;
      margin-top: 10px;
      width: 100%;
      box-sizing: border-box;
      font-size: 16px;
      font-family: inherit;
    }
    #resultado {
      margin-top: 20px;
      font-weight: bold;
    }
    label {
      display: block;
      margin-top: 15px;
      font-weight: bold;
    }
    #rodape {
      margin-top: 40px;
      padding-top: 20px;
      border-top: 1px solid #ccc;
      display: none;
    }
    #botaoMais {
      margin-top: 10px;
      font-size: 16px;
      cursor: pointer;
    }
    #contador {
      margin-top: 5px;
      font-weight: bold;
      font-size: 14px;
    }
    #mascoteDireito {
      position: fixed;
      bottom: 20px;
      right: 20px;
      width: 195px;
      max-width: 32.5%;
      z-index: 0;
      pointer-events: none;
      opacity: 0.9;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="flags">
      <span><strong>Selecione o idioma:</strong></span>
      <img src="https://flagcdn.com/w40/br.png" alt="Português" onclick="setLang('pt')">
      <img src="https://flagcdn.com/w40/gb.png" alt="English" onclick="setLang('en')">
    </div>

    <h1 id="tituloPrincipal">Selecione um idioma</h1>

    <div id="jogo" style="display: none;">
      <select id="menu">
        <option id="opcaoMisterio" value="misterio1">Um Mistério em Londres</option>
        <option value="misterio2">Um Mistério de Paris</option>
        <option value="misterio3">Um Mistério de Roma</option>
      </select>

      <div id="perguntas" style="margin-top: 20px;">
        <label id="pergunta1Label"></label>
        <input type="text" id="resposta1">

        <label id="pergunta2Label"></label>
        <input type="text" id="resposta2">

        <button id="botaoVerificar" onclick="verificar()">Enviar</button>
      </div>

      <div id="resultado"></div>
    </div>

    <div id="rodape">
      <p id="perguntaRodape">Você gostaria de mais edições?</p>
      <button id="botaoMais" onclick="contarClique()">Quero!</button>
      <div id="contador">0 votos</div>
    </div>
  </div>

  <img src="mascote.png" id="mascoteDireito" alt="Mascote do Detetive Griffon">

  <script>
    let lang = "pt";

    const textos = {
      pt: {
        titulo: "Livro-jogo de colorir do Detetive Griffon",
        dropdownOption: "Um Mistério em Londres",
        pergunta1: "Qual o objeto perdido?",
        pergunta2: "Em qual local ele estava?",
        botaoVerificar: "Descubra se desvendou o mistério!",
        rodapePergunta: "Você gostaria de mais edições?",
        rodapeBotao: "Quero!",
        rodapeVotos: "votos",
        edicoes: {
          misterio1: {
            correta1: ["lanterna"],
            correta2: ["a lanchonete da monique", "lanchonete"]
          },
          misterio2: {
            correta1: ["a máquina de escrever", "máquina de escrever", "uma máquina de escrever"],
            correta2: ["no quarto da lilou", "quarto da lilou"]
          },
          misterio3: {
            correta1: ["a balança", "balança", "uma balança"],
            correta2: ["na sala de costura da camille", "sala de costura da camile"]
          }
        },
        acerto: "✅ Respostas corretas!",
        erro: "❌ Uma ou ambas as respostas estão incorretas. Tente novamente."
      },
      en: {
        titulo: "Detective Griffon’s Coloring Gamebook",
        dropdownOption: "A Mystery in London",
        pergunta1: "What was the missing object?",
        pergunta2: "Where was it found?",
        botaoVerificar: "Find out if you solved the mystery!",
        rodapePergunta: "Would you like more editions?",
        rodapeBotao: "I do!",
        rodapeVotos: "votes",
        edicoes: {
          misterio1: {
            correta1: ["flashlight"],
            correta2: ["monique’s diner", "diner"]
          },
          misterio2: {
            correta1: ["typewriter", "a typewriter"],
            correta2: ["lilou’s room", "in lilou’s room"]
          },
          misterio3: {
            correta1: ["scale", "a scale"],
            correta2: ["camille’s sewing room", "in camille’s sewing room"]
          }
        },
        acerto: "✅ Correct answers!",
        erro: "❌ One or both answers are incorrect. Try again."
      }
    };

    function setLang(novaLang) {
      lang = novaLang;
      const t = textos[lang];

      document.getElementById("tituloPrincipal").textContent = t.titulo;
      document.getElementById("opcaoMisterio").textContent = t.dropdownOption;
      document.getElementById("pergunta1Label").textContent = t.pergunta1;
      document.getElementById("pergunta2Label").textContent = t.pergunta2;
      document.getElementById("botaoVerificar").textContent = t.botaoVerificar;
      document.getElementById("perguntaRodape").textContent = t.rodapePergunta;
      document.getElementById("botaoMais").textContent = t.rodapeBotao;
      document.getElementById("jogo").style.display = "block";
      document.getElementById("rodape").style.display = "block";
      atualizarContador();
    }

    function verificar() {
      const r1 = document.getElementById("resposta1").value.trim().toLowerCase();
      const r2 = document.getElementById("resposta2").value.trim().toLowerCase();
      const t = textos[lang];
      const edicaoSelecionada = document.getElementById("menu").value;

      const correta1 = t.edicoes[edicaoSelecionada].correta1;
      const correta2 = t.edicoes[edicaoSelecionada].correta2;

      const resultado = document.getElementById("resultado");

      const r1ok = correta1.includes(r1);
      const r2ok = correta2.includes(r2);

      if (r1ok && r2ok) {
        resultado.textContent = t.acerto;
        resultado.style.color = "green";
      } else {
        resultado.textContent = t.erro;
        resultado.style.color = "red";
      }
    }

    let clicado = false;

    function contarClique() {
      if (clicado) return;
      clicado = true;

      const total = Number(localStorage.getItem("contadorEdicoes") || "0") + 1;
      localStorage.setItem("contadorEdicoes", total);
      atualizarContador();
    }

    function atualizarContador() {
      const t = textos[lang];
      const total = localStorage.getItem("contadorEdicoes") || "0";
      document.getElementById("contador").textContent = `${total} ${t.rodapeVotos}`;
    }
  </script>
</body>
</html>
