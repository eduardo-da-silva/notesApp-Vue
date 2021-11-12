<template>
  <div class="card" v-for="(note, i) of notes" :key="i">
    <div class="card-title">{{ note.title }}</div>
    <div class="card-content">
      <div v-if="note.edit" class="edit-marked">
        <textarea
          v-model="note.markedText"
          @blur="note.edit = false"
        ></textarea>
      </div>
      <div
        v-else
        class="show-marked"
        v-html="convertToHTML(note.markedText)"
        @dblclick="note.edit = true"
      ></div>
    </div>
  </div>
</template>

<script>
import { marked } from "marked";
// import { hljs } from "highlight.js";
export default {
  name: "NotesApp",
  data() {
    return {
      notes: [
        {
          title: "Text 1",
          markedText: "# Marked in browser\n\nRendered by **marked**.",
          edit: true,
        },
        {
          title: "Text 2",
          markedText: "# Marked in browser 2\n\nRendered by **marked**.",
          edit: false,
        },
        {
          title: "Text 2",
          markedText: "# Marked in browser 2\n\nRendered by **marked**.",
          edit: false,
        },
      ],
    };
  },
  methods: {
    convertToHTML(markedText) {
      marked.setOptions({
        renderer: new marked.Renderer(),
        highlight: function (code, lang) {
          const hljs = require("highlight.js");
          const language = hljs.getLanguage(lang) ? lang : "plaintext";
          console.log(lang);
          return hljs.highlight(code, { language }).value;
        },
      });
      return marked.parse(markedText);
    },
  },
};
</script>

<style scoped>
.card {
  border-radius: 10px;
  box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.75);
  -webkit-box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 10px 10px 5px 0px rgba(0, 0, 0, 0.75);
  margin: 20px auto;

  width: 60%;
  padding: 0;
}

.card-title {
  border: 0;
  text-align: center;
  line-height: 50px;
  height: 50px;
  padding: 0;
  border-top-right-radius: 10px;
  border-top-left-radius: 10px;
  background-color: #0093e9;
  background-image: linear-gradient(160deg, #0093e9 0%, #80d0c7 100%);
  color: white;
  font-size: 30px;
}

.card-content {
  display: inline-block;
  width: 100%;
  min-height: 200px;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  background-image: radial-gradient(
    circle 321px at 8.3% 75.7%,
    rgba(209, 247, 241, 1) 0%,
    rgba(249, 213, 213, 1) 81%
  );
}

.edit-marked {
  /* padding: 0; */
}

.show-marked {
  display: flex;
  flex-direction: column;
  align-items: start;
  align-content: start;
  padding-left: 20px;
}

.edit-marked textarea {
  min-height: 160px;
  width: 95%;
  margin: 10px;
  max-width: 98%;
  overflow-y: auto;
  word-wrap: break-word;
  background: transparent;
  border: 0;
}
</style>
