<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Assistente Domiciliar Inteligente</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; background-color: #121212; color: white; }
        .container { max-width: 600px; margin: auto; padding: 20px; }
        .voice-button {
            background-color: #1db954;
            border: none;
            color: white;
            padding: 15px 32px;
            text-align: center;
            font-size: 16px;
            border-radius: 50%;
            cursor: pointer;
            transition: 0.3s;
        }
        .voice-button:active {
            background-color: #17a74b;
        }
        .list-container {
            background: #1e1e1e;
            padding: 15px;
            border-radius: 10px;
            margin-top: 10px;
        }
        ul { list-style-type: none; padding: 0; }
        li { background: #333; margin: 5px; padding: 10px; border-radius: 5px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Assistente Domiciliar Inteligente</h1>
        <p id="resposta">Toque no botão para falar</p>
        <button class="voice-button" onclick="ativarReconhecimentoVoz()">🎤</button>

        <div class="list-container">
            <h2>Lista de Compras</h2>
            <ul id="listaCompras"></ul>
        </div>

        <div class="list-container">
            <h2>Lista de Tarefas</h2>
            <ul id="listaTarefas"></ul>
        </div>

        <div class="list-container">
            <h2>Planejador de Refeições</h2>
            <ul id="listaRefeicoes"></ul>
        </div>
    </div>

    <script>
        function adicionarCompra(item) {
            let lista = document.getElementById("listaCompras");
            let li = document.createElement("li");
            li.textContent = item;
            lista.appendChild(li);
            falar("Item adicionado à lista de compras.");
        }

        function adicionarTarefa(tarefa) {
            let lista = document.getElementById("listaTarefas");
            let li = document.createElement("li");
            li.textContent = tarefa;
            lista.appendChild(li);
            falar("Tarefa adicionada à lista.");
        }

        function adicionarRefeicao(refeicao) {
            let lista = document.getElementById("listaRefeicoes");
            let li = document.createElement("li");
            li.textContent = refeicao;
            lista.appendChild(li);
            falar("Refeição adicionada ao planejador.");
        }

        function ativarReconhecimentoVoz() {
            let reconhecimento = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
            reconhecimento.lang = "pt-BR";
            reconhecimento.start();
            document.getElementById("resposta").textContent = "Ouvindo...";
            
            reconhecimento.onresult = function(evento) {
                let comando = evento.results[0][0].transcript.toLowerCase();
                document.getElementById("resposta").textContent = "Você disse: " + comando;
                processarComando(comando);
            };
        }
        
        function processarComando(comando) {
            if (comando.includes("adicionar item")) {
                let item = comando.replace("adicionar item", "").trim();
                adicionarCompra(item);
            } else if (comando.includes("adicionar tarefa")) {
                let tarefa = comando.replace("adicionar tarefa", "").trim();
                adicionarTarefa(tarefa);
            } else if (comando.includes("adicionar refeição")) {
                let refeicao = comando.replace("adicionar refeição", "").trim();
                adicionarRefeicao(refeicao);
            } else {
                falar("Desculpe, não entendi o comando.");
            }
        }
        
        function falar(mensagem) {
            let sintese = new SpeechSynthesisUtterance(mensagem);
            sintese.lang = "pt-BR";
            window.speechSynthesis.speak(sintese);
        }
    </script>
</body>
</html>

