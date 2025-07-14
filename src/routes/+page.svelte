<script lang="ts">
  let fileContent = '';

  // o objeto com conteúdo do .txt
  let objeto : { [key:string]: string } = {
    conteudo: ''
  }



  // vai conter o JSON string
  let parajson:string = '';



  // pego o conteúdo do .txt pelo input
  function handleFileChange(event:Event) {
    const input = event.target as HTMLInputElement
    const file = input.files?.[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        fileContent = (e.target as FileReader).result as string;
        objeto.conteudo = fileContent;
        parajson = JSON.stringify(objeto)
    };
      reader.readAsText(file);
    }
  }


  // baixa o arquivo agora em JSON
  function downloadJson() {
    const blob = new Blob([parajson], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'convertido.json';
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
            <input id="the-input" type="file" on:change={handleFileChange}/>
        </button>
        <div class="info">
            <h2>Whats .txt?</h2>
            <p>
                A ".text archive" typically refers to a collection of text-based
                files that have been stored together for later access or
                preservation. This could be a collection of text messages, log
                files, or other types of documents where the primary content is
                textual information. It's essentially a way to organize and
                store these files in a single location, often for the purpose of
                long-term retention or for easier retrieval.
            </p>
        </div>
    </div>
    <div id="right-div">
        <p>baixe o seu arquivo convertido para json</p>
        <a href="#right-div" on:click={downloadJson}>aqui</a>
        <div class="info">
            <h2>Whats .json?</h2>
            <p>
                JSON (JavaScript Object Notation) is an open standard file format and data
                interchange format that uses human-readable text to store and
                transmit data objects consisting of name–value pairs and arrays
                (or other serializable values). It is a commonly used data
                format with diverse uses in electronic data interchange,
                including that of web applications with servers.
            </p>
        </div>
    </div>

    <footer>
        <hr />
        <hr style="opacity: 0;">
        <div id="contact">
            <img src="github-icon.png" alt="" />
            <a href="#right-div">repositório</a>
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

    .info{
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
