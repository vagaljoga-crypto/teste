<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Plataforma de Prova</title>
    <!-- Tailwind CSS para um design rápido e moderno -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Estilos personalizados para o corpo quando em modo de prova */
        body.prova-ativa {
            -webkit-user-select: none; /* Safari */
            -ms-user-select: none; /* IE 10+ */
            user-select: none; /* Padrão */
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center h-screen font-sans">

    <!-- Tela Inicial -->
    <div id="tela-inicial" class="text-center p-8 bg-white rounded-lg shadow-2xl max-w-lg w-full">
        <h1 class="text-3xl font-bold text-gray-800 mb-4">Bem-vindo à Prova</h1>
        <p class="text-gray-600 mb-6">
            A avaliação começará pelo módulo de Matemática. Leia as instruções com atenção:
        </p>
        <ul class="text-left list-disc list-inside bg-gray-50 p-4 rounded-md mb-8">
            <li class="mb-2">A prova será executada em <strong>tela cheia</strong>.</li>
            <li class="mb-2"><strong>Não saia</strong> da tela cheia, troque de aba ou atualize a página.</li>
            <li class="mb-2">Qualquer tentativa de sair <strong>anulará sua prova e emitirá um alerta sonoro</strong>.</li>
        </ul>
        <div class="flex space-x-4">
             <button id="iniciar-prova" class="w-full bg-blue-600 text-white font-bold py-3 px-6 rounded-lg hover:bg-blue-700 transition-transform transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-blue-300">
                Iniciar Prova
            </button>
        </div>
    </div>

    <!-- Tela da Prova (escondida por padrão) -->
    <div id="tela-prova" class="hidden w-full h-full p-8 bg-white overflow-y-auto">
        <div class="max-w-4xl mx-auto">
            <div class="flex justify-between items-center mb-6 pb-4 border-b sticky top-0 bg-white pt-4">
                <h2 id="titulo-modulo" class="text-2xl font-bold text-gray-800">Questões</h2>
                <div id="cronometro" class="hidden text-xl font-mono bg-red-500 text-white px-4 py-2 rounded-md shadow-lg">00:00</div>
            </div>

            <!-- Container das Questões -->
            <div id="container-questoes">
                <!-- As questões serão injetadas aqui pelo JavaScript -->
            </div>
            
            <div class="mt-8 flex justify-end pb-8">
                <button id="finalizar-prova" class="bg-purple-600 text-white font-bold py-2 px-8 rounded-lg hover:bg-purple-700 transition-transform transform hover:scale-105 focus:outline-none focus:ring-4 focus:ring-purple-300">
                    Finalizar Módulo
                </button>
            </div>
        </div>
    </div>
    
    <!-- Tela de Violação (escondida por padrão) -->
    <div id="tela-violacao" class="hidden text-center p-8 bg-white rounded-lg shadow-2xl max-w-md w-full">
         <svg class="mx-auto mb-4 w-16 h-16 text-red-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path></svg>
        <h1 class="text-2xl font-bold text-red-700 mb-4">ATENÇÃO! PROVA ANULADA!</h1>
        <p class="text-gray-600">
            Foi detectada uma violação das regras. Sua tentativa foi invalidada.
        </p>
    </div>

    <!-- Tela de Resultados/Transição (escondida por padrão) -->
    <div id="tela-resultado" class="hidden text-center p-8 bg-white rounded-lg shadow-2xl max-w-md w-full">
        <h1 id="titulo-resultado" class="text-3xl font-bold text-gray-800 mb-4">Módulo Finalizado</h1>
        <p id="paragrafo-resultado" class="text-gray-700 mb-6 text-lg">Clique para continuar.</p>
        <button id="btn-resultado" class="bg-gray-500 text-white font-bold py-2 px-6 rounded-lg hover:bg-gray-600 transition">
            Continuar
        </button>
    </div>


<script>
    // --- ELEMENTOS DO DOM ---
    const telaInicial = document.getElementById('tela-inicial');
    const telaProva = document.getElementById('tela-prova');
    const telaViolacao = document.getElementById('tela-violacao');
    const telaResultado = document.getElementById('tela-resultado');
    const btnIniciarProva = document.getElementById('iniciar-prova');
    const btnFinalizar = document.getElementById('finalizar-prova');
    const containerQuestoes = document.getElementById('container-questoes');
    const tituloModuloDisplay = document.getElementById('titulo-modulo');

    // --- BANCO DE QUESTÕES ---
    const questoesMatematica = [
        {
            pergunta: "Observe o gráfico a seguir, que apresenta os dez primeiros Clubes brasileiros no Ranking Mundial de Clubes. De acordo com os dados, é correto afirmar que:",
            imagem: "https://i.ibb.co/TMrbWV7n/imagem1.jpg",
            opcoes: ["São Paulo ocupa a quinta posição no Ranking Mundial de Clubes.", "Considerando que o Brasil só classifica para próxima fase os dois clubes mais bem pontuados, esses clubes são Corinthians e Santos.", "Curitiba está na décima posição com 124 (cento e vinte e quatro) pontos.", "Vasco da Gama está na sétima posição com 166 pontos.", "A soma dos pontos do primeiro e do décimo colocado é 360 pontos."],
            resposta: "Vasco da Gama está na sétima posição com 166 pontos."
        },
        {
            pergunta: "Para esvaziar um reservatório que tinha 4 000 litros de água, João usou ininterruptamente um instrumento de sucção que suga 250 litros de água a cada 5 minutos. Quantos minutos foram necessários para esvaziar completamente esse reservatório?",
            opcoes: ["16", "50", "80", "800", "850"],
            resposta: "80"
        },
        {
            pergunta: "O preço da passagem de ônibus em uma cidade era de R$ 1,80. Esse preço sofreu um aumento de 25%. Qual é o preço dessa passagem após o aumento?",
            opcoes: ["R$ 2,25", "R$ 1,80", "R$ 1,35", "R$ 0,45", "R$ 0,30"],
            resposta: "R$ 2,25"
        },
        {
            pergunta: "Uma empresa automobilística tinha 8 mil veículos estocados para serem vendidos em 2006. Durante o ano, a empresa registrou venda de 85,6% do estoque. Quantos veículos a empresa deixou de vender?",
            opcoes: ["856", "1 152", "6 848", "7 144", "7 915"],
            resposta: "1 152"
        },
        {
            pergunta: "O gráfico mostra a quantidade de livros vendidos em uma livraria durante 4 meses. Qual foi o total de livros vendidos nesses meses?",
            imagem: "https://quickchart.io/chart?c={type:'bar',data:{labels:['Janeiro','Fevereiro','Março','Abril'],datasets:[{label:'Livros Vendidos',data:[120,150,180,210]}]},options:{plugins:{datalabels:{anchor:'end',align:'top',color:'black'}}}}",
            opcoes: ["640", "650", "660", "670"],
            resposta: "660"
        },
        {
            pergunta: "Em uma pesquisa, 25% dos 1.200 alunos de uma escola disseram gostar de química. Quantos alunos foram?",
            opcoes: ["290", "295", "300", "305"],
            resposta: "300"
        },
        {
            pergunta: "Uma loja vendeu 480 produtos em 12 dias. Mantendo o mesmo ritmo, quantos produtos venderá em 20 dias?",
            opcoes: ["780", "790", "800", "810"],
            resposta: "800"
        },
        {
            pergunta: "O gráfico mostra a quantidade de visitantes em um museu durante 3 dias. Qual foi a média de visitantes por dia?",
            imagem: "https://placehold.co/500x300/e2e8f0/334155?text=Gráfico+de+Visitantes%0A%0ADia+1:+200%0ADia+2:+250%0ADia+3:+300",
            opcoes: ["240", "245", "250", "255"],
            resposta: "250"
        },
        {
            pergunta: "O gráfico mostra a produção de uma fábrica em 4 dias. Qual foi a produção total?",
            imagem: "https://quickchart.io/chart?c={type:'bar',data:{labels:['Dia 1','Dia 2','Dia 3','Dia 4'],datasets:[{label:'Peças Produzidas',data:[100,120,140,160]}]},options:{plugins:{datalabels:{anchor:'end',align:'top',color:'black'}}}}",
            opcoes: ["510", "520", "530", "540"],
            resposta: "520"
        },
        {
            pergunta: "Em um jardim, um canteiro tem formato circular e 10 metros de diâmetro. Considere π = 3,14. Qual é a medida aproximada, em metros, do perímetro desse canteiro? (Comprimento = 2.π.r ou Comprimento = π.d)",
            opcoes: ["31,4", "62,8", "100", "314", "628"],
            resposta: "31,4"
        },
        {
            pergunta: "Uma pesquisa sobre a preferência de desportos entre 200 estudantes mostrou o seguinte resultado. Qual a percentagem de alunos que prefere Futebol?",
            imagem: "https://quickchart.io/chart?c={type:'pie',data:{labels:['Futebol (50%)','Vólei (25%)','Basquete (15%)','Outros (10%)'],datasets:[{data:[50,25,15,10]}]},options:{plugins:{datalabels:{color:'white',formatter:(val,ctx)=>ctx.chart.data.labels[ctx.dataIndex].split('(')[0]}}}}",
            opcoes: ["25%", "40%", "50%", "60%"],
            resposta: "50%"
        },
        {
            pergunta: "O gráfico de pizza representa o orçamento mensal de uma família. Qual categoria representa a maior despesa?",
            imagem: "https://quickchart.io/chart?c={type:'pie',data:{labels:['Moradia (35%)','Alimentação (30%)','Transporte (20%)','Lazer (15%)'],datasets:[{data:[35,30,20,15]}]},options:{plugins:{datalabels:{color:'white',formatter:(val,ctx)=>ctx.chart.data.labels[ctx.dataIndex].split('(')[0]}}}}",
            opcoes: ["Lazer", "Transporte", "Alimentação", "Moradia"],
            resposta: "Moradia"
        },
        {
            pergunta: "Numa eleição escolar com 800 votantes, o resultado foi o apresentado no gráfico. Quantos votos recebeu o Candidato B?",
            imagem: "https://quickchart.io/chart?c={type:'pie',data:{labels:['Candidato A (45%)','Candidato B (30%)','Candidato C (25%)'],datasets:[{data:[45,30,25]}]},options:{plugins:{datalabels:{color:'white',formatter:(val,ctx)=>ctx.chart.data.labels[ctx.dataIndex].split('(')[0]}}}}",
            opcoes: ["200 votos", "240 votos", "300 votos", "360 votos"],
            resposta: "240 votos"
        },
        {
            pergunta: "A preferência musical de um grupo de pessoas está no gráfico. Qual a soma das percentagens dos que preferem Rock e Pop?",
            imagem: "https://quickchart.io/chart?c={type:'pie',data:{labels:['Rock (35%)','Pop (40%)','Sertanejo (15%)','Outros (10%)'],datasets:[{data:[35,40,15,10]}]},options:{plugins:{datalabels:{color:'white',formatter:(val,ctx)=>ctx.chart.data.labels[ctx.dataIndex].split('(')[0]}}}}",
            opcoes: ["50%", "65%", "75%", "85%"],
            resposta: "75%"
        }
    ];
    const questoesPortugues = [
        {
            pergunta: "O meme gera humor pela reação do personagem, que se mostra",
            imagem: "https://i.imgur.com/94PbeQT.png",
            opcoes: ["chateado com a cobrança do frete.", "confuso com a promoção apresentada.", "satisfeito com a ilusão do desconto no frete.", "furioso com o aumento no preço do produto.", "indiferente com os dois valores apresentados."],
            resposta: "satisfeito com a ilusão do desconto no frete."
        },
        {
            pergunta: "Nesse meme, por meio da associação do texto verbal e do texto visual, infere-se que a palavra “atrai” transmite a ideia de",
            imagem: "https://i.imgur.com/Xdr1PWA.png",
            opcoes: ["receber valores sem esforço.", "provocar bons acontecimentos.", "atingir um estado de bom humor.", "fascinar as pessoas pela beleza.", "alcançar conquistas profissionais."],
            resposta: "provocar bons acontecimentos."
        },
        {
            pergunta: "A ideia principal desse texto é o(a)",
            imagem: "https://i.imgur.com/AR6P47b.png",
            opcoes: ["facilidade de acesso à informação.", "avanço dos meios de comunicação.", "importância da família para os jovens.", "medo da solidão entre os mais velhos.", "crítica aos hábitos atuais das pessoas."],
            resposta: "crítica aos hábitos atuais das pessoas."
        },
        {
            pergunta: "No texto desse meme, a palavra “lá” se refere",
            imagem: "https://i.imgur.com/LezZ0Iq.png",
            opcoes: ["ao celular.", "às redes sociais.", "à agência bancária.", "ao aplicativo do banco.", "à conversa com um amigo."],
            resposta: "ao aplicativo do banco."
        },
        {
            pergunta: "Esses comentários foram postados em uma rede social a respeito de um texto sobre o uso de tablets por crianças. Quanto ao posicionamento dos usuários, infere-se que eles",
            imagem: "https://i.imgur.com/xAzuIP6.png",
            opcoes: ["discordam do autor e o ofendem.", "concordam em partes com o texto.", "estão de acordo com o conteúdo lido.", "empregam termos agressivos na fala.", "recorrem ao argumento de autoridade."],
            resposta: "estão de acordo com o conteúdo lido."
        },
        {
            pergunta: "Biólogo descobre espécie de inseto – e coloca nome da esposa nela. [...] A principal ideia apresentada nesse texto é o(a)",
            texto: "Uma nova espécie de mariposa foi descoberta por um entomólogo (especialista em insetos) que vive na Áustria. E ele não precisou sequer ir visitar uma floresta para fazer isso. Afinal, a nova espécie, que ganhou o nome científico de Carcina ingridmariae, em homenagem à esposa do cientista, Ingrid Maria, estava guardada no Museu Estatal del Tirol, onde o especialista trabalha. Mas, durante décadas, foi confundida com outra espécie.",
            opcoes: ["lugar onde ocorreu a descoberta.", "histórico profissional do entomólogo.", "descoberta de uma espécie de inseto.", "homenagem à esposa feita pelo cientista.", "artigo científico publicado pelo especialista."],
            resposta: "descoberta de uma espécie de inseto."
        },
        {
            pergunta: "Os parênteses em 'foi descoberta por um entomólogo (especialista em insetos) que vive na Áustria' têm como finalidade",
            opcoes: ["revelar um juízo de valor da autora.", "expressar um trecho irônico no texto.", "isolar uma expressão de caráter explicativo.", "indicar a origem da informação apresentada.", "levar o leitor a refletir sobre a ocupação do biólogo."],
            resposta: "isolar uma expressão de caráter explicativo."
        },
        {
            pergunta: "Apesar de tratar do mesmo tema do texto I, identifica-se que o texto II difere na abordagem, pois",
            texto: "TEXTO I: Terra gira mais rápido e dia mais curto do ano acontece nesta terça-feira, 5. Com uma rotação mais rápida, o dia de 5 de agosto de 2025 será 1,25 milissegundo mais curto que o habitual.\n\nTEXTO II: Terra está girando mais rápido! Hoje é o dia mais curto do ano. Se você tem a sensação de que 2025 está passando rápido demais, tem toda razão – embora a diferença 'no relógio' seja praticamente imperceptível.",
            opcoes: ["expõe dados precisos sobre o fato.", "apresenta caráter sensacionalista.", "emprega uma linguagem formal.", "demonstra mais confiabilidade.", "informa conceitos científicos."],
            resposta: "apresenta caráter sensacionalista."
        },
        {
            pergunta: "Nesse podcast, a inteligência artificial é apresentada como uma",
            texto: "Bem-vindos a mais um episódio do Tech em Foco... Hoje vamos falar sobre como a inteligência artificial está transformando setores inteiros... Claro, essa revolução vem acompanhada de desafios... Mas, como em toda grande mudança, a chave está no equilíbrio: usar a tecnologia como aliada, sem perder de vista o fator humano.",
            opcoes: ["ameaça que deve ser evitada a todo custo.", "tecnologia com pouca utilidade no dia a dia.", "solução limitada à área da saúde e medicina.", "invenção voltada para entretenimento e lazer.", "ferramenta que deve ser usada com equilíbrio."],
            resposta: "ferramenta que deve ser usada com equilíbrio."
        },
        {
            pergunta: "No trecho 'Já parou para pensar' (1º parágrafo), o locutor busca",
            opcoes: ["estabelecer um diálogo com o ouvinte.", "conversar com um especialista no tema.", "pedir ao ouvinte que responda suas dúvidas.", "convencer o ouvinte a abandonar a tecnologia.", "fazer uma crítica ao uso da inteligência artificial."],
            resposta: "estabelecer um diálogo com o ouvinte."
        },
        {
            pergunta: "Destaque abaixo a frase que mais expressa os fatos sobre o novo parque:",
            texto: "O novo parque da cidade foi inaugurado neste sábado. Ele ocupa cerca de 20 mil metros quadrados e conta com brinquedos, pistas de caminhada e áreas de lazer. Moradores afirmam que o espaço é o melhor da região e que finalmente a prefeitura fez algo de valor.",
            opcoes: ["O espaço é o melhor da região.", "A prefeitura fez algo de valor.", "Parque inaugurado no sábado, com 20 mil m².", "Moradores estão muito satisfeitos."],
            resposta: "Parque inaugurado no sábado, com 20 mil m²."
        },
        {
            pergunta: "Com base no trecho sobre o novo parque, qual das frases abaixo expressa uma opinião dos moradores?",
            opcoes: ["O parque foi inaugurado no sábado.", "Ele tem 20 mil metros quadrados.", "O espaço é o melhor da região.", "O parque possui pistas de caminhada."],
            resposta: "O espaço é o melhor da região."
        },
        {
            pergunta: "Ao ler o trecho, qual informação podemos inferir (deduzir) sobre o estado de João?",
            texto: "João chegou em casa, jogou a mochila no chão e foi direto para a geladeira. Ele não disse 'Oi' para ninguém e a mãe dele apenas suspirou, balançando a cabeça.",
            opcoes: ["João é um estudante dedicado.", "João está com muita fome.", "João chegou em casa cansado ou irritado.", "A geladeira está vazia."],
            resposta: "João chegou em casa cansado ou irritado."
        },
        {
            pergunta: "O termo 'membranas', presente no texto, se refere às",
            texto: "A queda do Império Romano não aconteceu da noite para o dia... As membranas do Império, mais do que se romper bruscamente foram se permeabilizando. Na Península Ibérica, o período que se estendeu dos anos 400 até os anos 700 de nossa era foi uma fase de transição entre o domínio coeso e centralizado de Roma...",
            opcoes: ["tradições culturais romanas.", "fronteiras do Império Romano.", "defesas militares dos romanos.", "relações diplomáticas entre romanos e bárbaros.", "influências exercidas por Roma sobre outros povos."],
            resposta: "fronteiras do Império Romano."
        }
    ];

    let questoesAtuais = [];
    let respostasUsuario = {};

    // --- ESTADO DA PROVA ---
    let provaAnulada = false;
    let telaCheiaAtiva = false;
    let moduloAtual = '';

    // --- FUNÇÕES DA PROVA ---

    function carregarQuestoes(numeroInicial = 1) {
        containerQuestoes.innerHTML = '';
        questoesAtuais.forEach((q, index) => {
            const questaoDiv = document.createElement('div');
            questaoDiv.className = 'mb-8 p-6 bg-gray-50 rounded-lg shadow-sm';
            
            const numeroQuestao = numeroInicial + index;
            const perguntaP = document.createElement('p');
            perguntaP.className = 'text-lg font-semibold text-gray-700 mb-4';
            perguntaP.textContent = `${numeroQuestao}. ${q.pergunta}`;
            questaoDiv.appendChild(perguntaP);
            
            if (q.texto) {
                const textoP = document.createElement('p');
                textoP.className = 'text-sm text-gray-600 mb-4 p-3 bg-gray-100 border-l-4 border-gray-300 italic';
                textoP.innerText = q.texto;
                questaoDiv.appendChild(textoP);
            }

            if (q.imagem) {
                const img = document.createElement('img');
                img.src = q.imagem;
                img.alt = `Imagem para a questão ${numeroQuestao}`;
                img.className = 'my-4 rounded-lg max-w-sm mx-auto shadow-md';
                questaoDiv.appendChild(img);
            }
            
            const opcoesDiv = document.createElement('div');
            opcoesDiv.className = 'space-y-3';
            
            q.opcoes.forEach((opcao, opcaoIndex) => {
                const label = document.createElement('label');
                label.className = 'flex items-center p-3 bg-white border rounded-md hover:bg-blue-50 cursor-pointer';
                
                const input = document.createElement('input');
                input.type = 'radio';
                input.name = `questao-${index}`;
                input.value = opcao;
                input.className = 'mr-3 h-4 w-4 text-blue-600 focus:ring-blue-500 border-gray-300';
                input.onchange = () => {
                    respostasUsuario[index] = opcao;
                };

                label.appendChild(input);
                const letra = String.fromCharCode(65 + opcaoIndex); // Gera A, B, C...
                label.appendChild(document.createTextNode(`${letra}) ${opcao}`));
                opcoesDiv.appendChild(label);
            });
            
            questaoDiv.appendChild(opcoesDiv);
            containerQuestoes.appendChild(questaoDiv);
        });
    }

    function iniciarProva(modulo, questoesModulo, numeroInicial) {
        telaProva.scrollTop = 0;
        
        moduloAtual = modulo;
        questoesAtuais = questoesModulo;
        respostasUsuario = {};
        
        tituloModuloDisplay.textContent = `Módulo: ${modulo}`;
        
        telaInicial.classList.add('hidden');
        telaResultado.classList.add('hidden');
        telaProva.classList.remove('hidden');
        document.body.classList.add('prova-ativa');
        
        entrarTelaCheia();
        carregarQuestoes(numeroInicial);
    }
    
    function finalizarProva() {
        if (provaAnulada) return;
        
        telaProva.classList.add('hidden');
        telaResultado.classList.remove('hidden');
        
        const tituloResultado = document.getElementById('titulo-resultado');
        const pResultado = document.getElementById('paragrafo-resultado');
        const btnResultado = document.getElementById('btn-resultado');

        if (moduloAtual === 'Matemática') {
            tituloResultado.textContent = 'Módulo de Matemática Finalizado';
            pResultado.textContent = 'Clique abaixo para continuar para a prova de Português.';
            btnResultado.textContent = 'Continuar para Português';
            
            const novoBtn = btnResultado.cloneNode(true);
            btnResultado.parentNode.replaceChild(novoBtn, btnResultado);

            novoBtn.addEventListener('click', () => {
                iniciarProva('Português', questoesPortugues, 15);
            });
        } else if (moduloAtual === 'Português') {
            document.body.classList.remove('prova-ativa');
            sairTelaCheia();
            tituloResultado.textContent = 'Prova Concluída';
            pResultado.textContent = 'Você completou a avaliação. Entregue sua folha de respostas.';
            btnResultado.textContent = 'Voltar ao Início';
            
            const novoBtn = btnResultado.cloneNode(true);
            btnResultado.parentNode.replaceChild(novoBtn, btnResultado);
            novoBtn.addEventListener('click', () => {
                window.location.reload();
            });
        }
    }

    // --- FUNÇÕES DE SEGURANÇA ---

    function tocarAlertaSonoro() {
        try {
            const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            const oscillator = audioCtx.createOscillator();
            const gainNode = audioCtx.createGain();
            
            oscillator.connect(gainNode);
            gainNode.connect(audioCtx.destination);
            
            oscillator.type = 'square';
            oscillator.frequency.setValueAtTime(1200, audioCtx.currentTime); // Tom agudo
            gainNode.gain.setValueAtTime(1, audioCtx.currentTime); // Volume máximo
            
            oscillator.start();
            setTimeout(() => {
                oscillator.stop();
            }, 1500); // Toca por 1.5 segundos
        } catch(e) {
            console.error("Não foi possível tocar o som de alerta.", e);
        }
    }
    
    function entrarTelaCheia() {
        const elem = document.documentElement;
        if (elem.requestFullscreen) {
            elem.requestFullscreen()
                .then(() => { telaCheiaAtiva = true; })
                .catch(err => {
                    telaCheiaAtiva = false;
                    console.error(`Erro ao entrar em tela cheia: ${err.message}. A prova continuará.`);
                });
        }
    }
    
    function sairTelaCheia() {
        if (document.fullscreenElement && document.exitFullscreen) {
            document.exitFullscreen();
        }
    }
    
    function anularProva() {
        if (provaAnulada) return;
        provaAnulada = true;
        
        tocarAlertaSonoro();
        
        telaInicial.classList.add('hidden');
        telaProva.classList.add('hidden');
        telaResultado.classList.add('hidden');
        telaViolacao.classList.remove('hidden');
        document.body.classList.remove('prova-ativa');
        
        sairTelaCheia();
    }
    
    // Listener para ALT+TAB e mudança de janela
    window.addEventListener('blur', () => {
        if (document.body.classList.contains('prova-ativa')) {
            anularProva();
        }
    });
    
    document.addEventListener('visibilitychange', () => {
        if (document.hidden && document.body.classList.contains('prova-ativa')) {
            anularProva();
        }
    });

    document.addEventListener('fullscreenchange', () => {
        if (!document.fullscreenElement && document.body.classList.contains('prova-ativa') && telaCheiaAtiva) {
            anularProva();
        }
    });

    document.addEventListener('contextmenu', event => {
        if(document.body.classList.contains('prova-ativa')) {
            event.preventDefault();
        }
    });

    document.addEventListener('copy', event => {
        if(document.body.classList.contains('prova-ativa')) {
            event.preventDefault();
        }
    });
    
    window.addEventListener('beforeunload', (event) => {
        if (document.body.classList.contains('prova-ativa')) {
            anularProva();
            event.preventDefault(); 
            event.returnValue = '';
        }
    });

    // --- EVENT LISTENERS ---
    btnIniciarProva.addEventListener('click', () => iniciarProva('Matemática', questoesMatematica, 1));
    btnFinalizar.addEventListener('click', finalizarProva);

</script>

</body>
</html>

