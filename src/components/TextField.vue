<template>
  <div>
    <textarea disabled v-model="textJason" rows="20" cols="30"></textarea>
    <div id="editor-fild"
      contenteditable="true"
      @click="getSelectionText">
    <span  
    class="editing" >{{inishialText}}</span></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inishialText: "My lovely little Pony",
      textJason: ""
    };
  },

  methods: {
    getSelectionText: function(e) {
      let targetSpan = e.target.closest("span");
      let txt = "";

      if ((txt = window.getSelection)) {
        txt = window.getSelection().toString();
      } else {
        txt = document.selection.createRange().text;
      }

      let spanInfo = {};
      spanInfo.text = targetSpan.textContent;
      spanInfo.style = {
        color: targetSpan.style.color,
        fontSize: targetSpan.style.fontSize,
        backgroundColor: targetSpan.style.backgroundColor
      };
  
      this.textJason = JSON.stringify(spanInfo);

      this.$emit("textSpan", {
        textSpan: targetSpan
      });

      this.$emit("text", {
        text: txt
      });
    }
  }
};
</script>

<style scoped>
#editor-fild {
  border-left: 2px solid rgba(128, 128, 128, 0.4);
  background-color: rgba(126, 252, 0, 0.1);
  max-width: 550px;
}

div {
  display: flex;
  width: 100%;
  word-wrap: break-word;
}

span {
  text-shadow: rgba(0, 0, 0, 0.2) 2px 2px 0px;
  line-height: 1.5em;
  padding: 10px;
  height: max-content;
  width: max-content;
}

textarea {
  padding: 5px;
  margin-right: 10px;
  background-color: rgba(255, 105, 180, 0.1);
}
</style>