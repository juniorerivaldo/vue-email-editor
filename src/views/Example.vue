<template>
  <div id="example">
    <div class="container">
      <div id="bar">
        <h1>Vue Email Editor (Demo)</h1>

        <button v-on:click="saveDesign">Save Design</button>
        <button v-on:click="exportHtml">Export HTML</button>
      </div>

      <EmailEditor ref="emailEditor" v-on:load="editorLoaded" v-on:ready="editorReady" />
    </div>
  </div>
</template>

<script>
import { EmailEditor } from '../components'
import sample from '../data/sample.json';

export default {
  name: 'exampleView',
  components: {
    EmailEditor
  },
  methods: {
    // called when the editor is created
    editorLoaded() {
      console.log('editorLoaded');
      this.$refs.emailEditor.editor.loadDesign(sample);
    },
    // called when the editor has finished loading
    editorReady() {
      console.log('editorReady');
    },
    saveDesign() {
  this.$refs.emailEditor.editor.saveDesign((design) => {
    try {
      // Parse o conteúdo do design para garantir que seja um objeto JSON válido
      const newDesign = design

      // Cria um blob com o conteúdo JSON
      const blob = new Blob([JSON.stringify(newDesign, null, 2)], { type: 'application/json' });

      // Cria um objeto URL a partir do blob
      const url = window.URL.createObjectURL(blob);

      // Cria um link temporário e configura o URL
      const link = document.createElement('a');
      link.href = url;
      link.download = 'novo-design.json'; // Nome do arquivo para download

      // Adiciona o link ao documento
      document.body.appendChild(link);

      // Simula um clique no link para iniciar o download
      link.click();

      // Remove o link do documento
      document.body.removeChild(link);

      // Revoga o URL para liberar os recursos
      window.URL.revokeObjectURL(url);

      console.log('Download do novo arquivo JSON concluído com sucesso.');
    } catch (error) {
      console.error('Erro ao criar e baixar o novo arquivo JSON:', error);
    }
  });
}
,
    exportHtml() {
      this.$refs.emailEditor.editor.exportHtml(
        (data) => {
          const content = data.html;

      // Cria um blob com o conteúdo HTML.
      const blob = new Blob([content], { type: 'text/html' });

      // Cria um objeto URL a partir do blob.
      const url = window.URL.createObjectURL(blob);

      // Cria um link temporário e configura o URL.
      const link = document.createElement('a');
      link.href = url;
      link.download = 'export.handlebars';

      // Adiciona o link ao documento.
      document.body.appendChild(link);

      // Simula um clique no link para iniciar o download.
      link.click();

      // Remove o link do documento.
      document.body.removeChild(link);

      // Revoga o URL para liberar os recursos.
      window.URL.revokeObjectURL(url);
        }
      )
    }
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  font-family: Arial, "Helvetica Neue", Helvetica, sans-serif;
}

#app, #example {
  height: 100%;
}

#example .container {
  display: flex;
  flex-direction: column;
  position: relative;
  height: 100%;
}

#bar {
  flex: 1;
  background-color: #40B883;
  color: #FFF;
  padding: 10px;
  display: flex;
  max-height: 40px;
}

#bar h1 {
  flex: 1;
  font-size: 16px;
  text-align: left;
}

#bar button {
  flex: 1;
  padding: 10px;
  margin-left: 10px;
  font-size: 14px;
  font-weight: bold;
  background-color: #000;
  color: #FFF;
  border: 0px;
  max-width: 150px;
  cursor: pointer;
}
</style>
