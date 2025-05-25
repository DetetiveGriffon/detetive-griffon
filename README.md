<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Livro-jogo do Detetive Griffon</title>
  <style>
    body {
      background-color: #fdf6e3;
      font-family: Arial, sans-serif;
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
      </select>

      <div id="perguntas" style="margin-top: 20px;">
        <label id="pergunta1Label"></label>
        <input type="text" id="resposta1">

        <label id="pergunta2Label"></label>
        <input type="text" id="resposta2">

        <button onclick="verificar()">Enviar</button>
      </div>

      <div id="resultado"></div>
    </div>

    <div id="rodape">
      <p id="perguntaRodape">Você gostaria de mais edições?</p>
      <button id="botaoMais" onclick="contarClique()">Quero!</button>
      <div id="contador">0 votos</div>
    </div>
  </div>

  <script>
    let lang = "pt";

    const textos = {
      pt: {
        titulo: "Livro-jogo de colorir do Detetive Griffon",
        dropdownOption: "Um Mistério em Londres",
        pergunta1: "Qual o objeto perdido?",
        pergunta2: "Em qual local ele estava?",
        correta1: ["lanterna"],
        correta2: ["a lanchonete da monique", "lanchonete"],
        acerto: "✅ Respostas corretas!",
        erro: "❌ Uma ou ambas as respostas estão incorretas. Tente novamente.",
        rodapePergunta: "Você gostaria de mais edições?",
        rodapeBotao: "Quero!",
        rodapeVotos: "votos"
      },
      en: {
        titulo: "Detective Griffon’s Coloring Gamebook",
        dropdownOption: "A Mystery in London",
        pergunta1: "What was the missing object?",
        pergunta2: "Where was it found?",
        correta1: ["flashlight"],
        correta2: ["monique’s diner", "diner"],
        acerto: "✅ Correct answers!",
        erro: "❌ One or both answers are incorrect. Try again.",
        rodapePergunta: "Would you like more editions?",
        rodapeBotao: "I do!",
        rodapeVotos: "votes"
      }
    };

    function setLang(novaLang) {
      lang = novaLang;
      const t = textos[lang];
      document.getElementById("tituloPrincipal").textContent = t.titulo;
      document.getElementById("opcaoMisterio").textContent = t.dropdownOption;
      document.getElementById("pergunta1Label").textContent = t.pergunta1;
      document.getElementById("pergunta2Label").textContent = t.pergunta2;
      document.getElementById("perguntaRodape").textContent = t.rodapePergunta;
      document.getElementById("botaoMais").textContent = t.rodapeBotao;
      atualizarContador();
      document.getElementById("jogo").style.display = "block";
    }

    function verificar() {
      const r1 = document.getElementById("resposta1").value.trim().toLowerCase();
      const r2 = document.getElementById("resposta2").value.trim().toLowerCase();
      const t = textos[lang];

      const correta1 = t.correta1;
      const correta2 = t.correta2;

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

    // --- CONTADOR ---

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

    window.onload = () => {
      atualizarContador();
    };
  </script>

</body>
</html>
