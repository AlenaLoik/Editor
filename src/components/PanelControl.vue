<template>
  <div class="main">
    <section>
      <label>
        bg color:
        <input @change="changeBgColor" value="bgColor" type="color" name="color" />
      </label>
      <label>
        text color:
        <input @change="changeTextColor" value="textColor" type="color" name="color" />
      </label>
      <label>
        font-size
        <select @change="changeFontSize" v-model="fontSize">
          <option v-for="option in options" :key="option" :value="option">{{ option }}</option>
        </select>
      </label>
      <label>
        reset
        <button @click="handleReset" value="RESET">x</button>
      </label>
    </section>
    <p>JSON (click on element...)</p>
    <TextField @text="onText" @textSpan="addClass" />
  </div>
</template>

<script>
import TextField from "@/components/TextField";

export default {
  data() {
    return {
      textColor: "textColor",
      fontSize: "12px",
      bgColor: "#ffffff",
      options: ["8px", "12px", "16px", "20px", "24px", "30px", "40px"],
      textForEdit: "",
      selectedSpan: "",
      highlightDiv: ""
    };
  },
  methods: {
    changeTextColor(event) {
      this.textColor = event.target.value;
      const editParams = "TC";
      this.handleEditing(this.textForEdit, editParams);
    },

    changeFontSize(event) {
      this.fontSize = event.target.value;
      const editParams = "FS";
      this.handleEditing(this.textForEdit, editParams);
    },

    changeBgColor(event) {
      this.bgColor = event.target.value;
      const editParams = "BC";
      this.handleEditing(this.textForEdit, editParams);
    },

    sharedSwitch(param) {
      switch (param) {
        case "TC":
          this.highlightDiv.style.color = this.textColor;
          break;
        case "FS":
          this.highlightDiv.style.fontSize = this.fontSize;
          break;
        case "BC":
          this.highlightDiv.style.backgroundColor = this.bgColor;
          break;
        default:
          alert("try agane");
      }
    },

    handleEditing(text, editParams) {
      this.selectedSpan.className = "editing";
      const elentForEdit = document.querySelector(".editing");
      const root = elentForEdit.firstChild;
      const content = root.nodeValue;

      if (content === text) {
        this.highlightDiv = elentForEdit;
        elentForEdit.className = "";
        return this.sharedSwitch(editParams);
      }
      if (~content.indexOf(text)) {
        if (document.createRange) {
          const rng = document.createRange();
          rng.setStart(root, content.indexOf(text));
          rng.setEnd(root, content.indexOf(text) + text.length);
          this.highlightDiv = document.createElement("span");
          this.sharedSwitch(editParams);
          rng.surroundContents(this.highlightDiv);

          const elentForEditString = elentForEdit.innerHTML;
          const elentForEditAddSpan1 = elentForEditString.replace(
            "</span>",
            " </span><span>"
          );
          const elentForEditAddSpan2 = elentForEditAddSpan1.replace(
            "<span ",
            "</span><span "
          );
          elentForEdit.innerHTML = elentForEditAddSpan2;
        } else {
          alert("Sorry, but you have IE8-, editor is not working...");
        }
      } else {
        alert(
          "Sorry, but you can`t edit this element, because it has a diferends styles! We working on this problem..."
        );
      }
      console.log(elentForEdit.innerHTML)

      elentForEdit.className = "";
    },

    addClass({ textSpan }) {
      this.selectedSpan = textSpan;
    },

    onText({ text }) {
      this.textForEdit = text;
    },

    handleReset() {
      const textField = document.getElementById("editor-fild");
      textField.innerHTML = `<span class="editing">My lovely little Pony</span>`;
    }
  },
  components: {
    TextField
  }
};
</script>

<style scoped>
.main {
  display: flex;
  flex-direction: column;
  width: 100%;
  max-width: 800px;
}
section {
  display: flex;
  align-items: center;
  justify-content: space-around;
  background-color: rgba(65, 105, 225, 0.397);
  margin-bottom: 20px;
  padding: 20px 0;
  width: 100%;
}

select {
  margin-top: 10px;
}

label {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-transform: uppercase;
  font-weight: bold;
  margin-bottom: 10px;
}

button {
  width: 22px;
  height: 22px;
  font-size: 12px;
  text-align: center;
  padding: 0;
  color: rgba(255, 0, 0, 0.507);
  margin-top: 10px;
}

input {
  margin-top: 10px;
}

p {
  text-align: start;
  margin: 0;
  color: rgba(0, 0, 0, 0.404);
}
</style>