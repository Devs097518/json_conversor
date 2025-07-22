<script lang="ts">
    let fileContent = "";
    let mostrar: number = 0;

    // o objeto com conteúdo do .txt
    let objeto: { [key: string]: string } = {
        conteudo: "",
    };

    // vai conter o JSON string
    let parajson: string = "";

    // pego o conteúdo do .txt pelo input
    function handleFileChange(event: Event) {
        const input = event.target as HTMLInputElement;
        const file = input.files?.[0];
        if (file) {
            if (file.name.endsWith(".txt")) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    fileContent = (e.target as FileReader).result as string;

                    objeto.conteudo = fileContent; // <--

                    console.log(objeto); // <-- para ver o objeto no console

                    let linhas: string[] = objeto.conteudo.split("\r\n");
                    let tratado: string[][] = []; // é uma array de array de strings [['a'],['b'],['c']]

                    /*cada linha é colocada uma lista ordenada*/
                    for (let i = 0; i < linhas.length; i++) {
                        let get = linhas[i].split(",");
                        tratado.push(get);
                    }

                    type Pessoa = {
                        nome: string;
                        idade: number;
                        id: string;
                    };

                    //contém o objeto formatado
                    let conjunto: Pessoa[] = [];

                    /*para cada linha, se é colocado num objeto*/
                    for (let i_b = 0; i_b < tratado.length; i_b++) {
                        construir(tratado[i_b]);
                    }

                    /*pega a linha e a bota num objeto anônimo*/
                    function construir(vetor: (string | number)[]): void {
                        const obj: Pessoa = {
                            nome: vetor[0] as string,
                            idade: Number(vetor[1]) as number,
                            id: vetor[2] as string,
                        };
                        conjunto.push(obj);
                    }

                    parajson = JSON.stringify(conjunto, null, 2); // <-- conteúdo do JSON formatado
                    mostrar = 1;
                };

                reader.readAsText(file);
            } else {
                alert("Por favor, insira um arquivo tipo .txt");
            }
        }
    }

    // baixa o arquivo agora em JSON
    function downloadJson() {
        const blob = new Blob([parajson], { type: "application/json" });
        const url = URL.createObjectURL(blob);
        const a = document.createElement("a");
        a.href = url;
        a.download = "convertido.json";
        a.click();
        URL.revokeObjectURL(url);
    }
</script>

<div id="container">
    <header>
        <h1>arquive conversor</h1>
        <hr />
    </header>
    <div id="left-div">
        <button id="input-txt">
            <label for="">drop your .txt file here</label>
            <input id="the-input" type="file" on:change={handleFileChange} />
        </button>
        <div class="info">
            <h2>Whats .txt?</h2>
            <p class="text-align">
               O formato TXT refere-se a arquivos de texto simples, que são documentos de texto com formatação mínima ou nenhuma. Esses arquivos podem ser abertos e lidos pela maioria dos editores de texto e sistemas operacionais. 
            </p>
        </div>
    </div>
    <div id="right-div">
        <div id="baixar" style="opacity: {mostrar};">
            <p>baixe o seu arquivo convertido para json</p>
            <a href="#right-div" on:click={downloadJson}>aqui</a>
        </div>

        <div class="info">
            <h2>O que é .json?</h2>
            <p class="text-align">
                JSON é um formato de dados comumente usado por desenvolvedores web para transferir dados entre um servidor e uma aplicação da web. Os desenvolvedores geralmente preferem o JSON porque ele simplifica a troca de dados entre diferentes tecnologias.
            </p>
        </div>
    </div>
    <div id="formato">
        <h1>Qual a formatação usada?</h1>
        <p id="info-formato"> atual formatação pega o arquivo de texto em que cada linha cada informação está separada por vírgulas, o programa identifica cada linha e cada item de sua linha e o coloca em um objeto anônimo com sua chave e respectivo valor. No caso atual cada objeto possui 3 chaves(nome, idade e id). No final, esse conteúdo é colocado no arquivo .json já formatado.</p>
        <img src="exemplo_formato.png" id="exemplo_formato" alt="">
    </div>

    <footer>
        <hr />
        <hr style="opacity: 0;" />
        <div id="contact">
            <img src="github-icon.png" alt="" />
            <a href="https://github.com/Devs097518/json_conversor"
                >repositório</a
            >
        </div>
    </footer>
</div>

<style>
    :global(:root) {
        background-image: radial-gradient(#ccc 1px, transparent 1px);
        background-size: 10px 10px;
        font-family: monospace;
    }

    #container {
        width: 50em;
        height: auto;
        border: 1px solid black;
        margin: auto;
        margin-top: 5em;
        background-color: rgb(255, 242, 227);
        display: flex;
        flex-wrap: wrap;
    }

    header {
        text-align: center;
        padding: 0 2em 0 2em;
        width: 100%;
    }

    .info {
        margin: 15px;
    }

    #left-div {
        width: 45%;
        padding: 1em;
        background-color: rgba(255, 0, 0, 0);
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
    }

    #input-txt {
        width: 70%;
        margin: auto;
        background-color: rgba(128, 128, 128, 0.267);
        border: 3px dotted black;
    }

    .text-align{
        text-align: center;
    }

    #the-input {
        opacity: 0;
        padding: 3em;
    }

    #right-div {
        width: 45%;
        padding: 1em;
        background-color: rgba(0, 0, 255, 0);
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        align-items: center;
    }

    #baixar {
        text-align: center;
    }

    #formato{
        display: flex;
        flex-direction: column;
        width: 100%;
        text-align: center;
    }

    #exemplo_formato{
        padding-top: 4em;
        width: 80%;
        margin: auto;
    }

    #info-formato{
        width: 90%;
        text-align: center;
        margin: auto;
    }

    footer {
        width: 100%;
        padding: 3em;
    }

    img {
        width: 25px;
        height: auto;
        margin: 1em;
    }

    #contact {
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
    }
</style>
