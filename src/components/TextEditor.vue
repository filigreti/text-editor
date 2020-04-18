<template>
  <main class="container">
    <div class="text">Text - Editor</div>

    <div class="text-editor-container">
      <div class="button-container">
        <Background @changeBackground="changeBackground" />
        <Color @changeColor="changeColor" />
        <FontSize @changefont="changeFont" />
      </div>
      <div class="text-editor">
        <span
          v-for="(value, index) in content"
          :id="`content-${index}`"
          :key="index"
          contenteditable="true"
          @input="event => onInput(event, index)"
          @keyup.space="next(index)"
          @keyup.delete="onRemove(index)"
          @click="getcurrent(index)"
          @mouseup="getcurrent(index)"
          @keyup.enter="enter"
        ></span>
      </div>
      <pre style="position:absolute;right:0; top:20px">
        {{content}}
      </pre>
    </div>
  </main>
</template>

<script>
import Background from "@/components/Background";
import Color from "@/components/Color";
import FontSize from "@/components/FontSize";

export default {
  components: {
    Background,
    Color,
    FontSize
  },
  data() {
    return {
      content: [
        {
          value: "",
          fontSize: "16px",
          color: "#000000",
          background: ""
        }
      ],
      highlightedValue: null,
      newcolor: null,
      newbackground: null,
      newfont: null,
      currentIndex: 0
    };
  },
  watch: {
    content: {
      deep: true,
      handler(x) {}
    },
    highlightedValue(x) {},
    newcolor: {
      immediate: true,
      handler(x) {
        if (x) {
          this.$nextTick(() => {
            let el = document.getElementById(`content-${this.currentIndex}`);
            el.style.color = x;
            this.content[this.currentIndex].color = x;
          });
        }
      }
    },
    newbackground(x) {
      if (x) {
        this.$nextTick(() => {
          let el = document.getElementById(`content-${this.currentIndex}`);
          el.style.background = x;
          el.style.borderRadius = "4px";
          this.content[this.currentIndex].background = x;
        });
      }
    },
    newfont(x) {
      this.$nextTick(() => {
        let el = document.getElementById(`content-${this.currentIndex}`);
        el.style.fontSize = `${x}px`;
        this.content[this.currentIndex].fontSize = `${x}px`;
      });
    }
  },
  methods: {
    onInput(event, index) {
      const value = event.target.innerText;
      const fontSize = event.target.style.fontSize;
      const color = event.target.style.color;
      const background = event.target.style.background;
      this.content[index].value = value;
      this.content[index].fontSize = fontSize;
      this.content[index].color = color;
      this.content[index].background = background;
    },
    enter() {
      let el = document.getElementById(`content-${this.currentIndex}`);
      el.appendChild(document.createElement("br"));
    },
    next(x) {
      this.content.push({
        value: "",
        fontSize: "16px",
        background: "#ffffff",
        color: "#000000"
      });
      this.$nextTick(() => {
        this.initContent();
      });
    },
    changeColor(x) {
      this.newcolor = x;
    },
    changeFont(x) {
      this.newfont = x;
    },
    changeBackground(x) {
      this.newbackground = x;
    },
    onRemove(index) {
      if (this.content.length > 1 && this.content[index].value.length === 0) {
        this.$delete(this.content, index);
        this.initContent();
      }
    },
    placeCaretAtEnd(el) {
      el.focus();
      if (
        typeof window.getSelection != "undefined" &&
        typeof document.createRange != "undefined"
      ) {
        var range = document.createRange();
        range.selectNodeContents(el);
        range.collapse(false);
        var sel = window.getSelection();
        sel.removeAllRanges();
        sel.addRange(range);
      } else if (typeof document.body.createTextRange != "undefined") {
        var textRange = document.body.createTextRange();
        textRange.moveToElementText(el);
        textRange.collapse(false);
        textRange.select();
      }
    },
    getcurrent(x) {
      console.log(x);
      this.currentIndex = x;
    },
    initContent() {
      this.content.forEach((c, index) => {
        let el = document.getElementById(`content-${index}`);
        el.focus();
        el.innerText = c.value;
        el.style.fontSize = c.fontSize;
        el.style.color = c.color;
        el.style.background = c.background;
      });
    }
  },

  mounted() {
    this.initContent();
    document.getElementById("content-0").focus();
    document.addEventListener("mouseup", e => {
      this.highlightedValue = window.getSelection().toString();
    });
  }
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  flex-direction: column;
}
.text-editor {
  border: solid 1px #ccc;
  padding: 20px;
  height: 300px;
  outline: none;
}

.text-editor-container {
  width: 50%;
}
.text {
  margin: 2rem 0;
}
.button-container {
  width: 80%;
  display: flex;
  justify-content: space-between;
}
span {
  margin: 0px !important;
  line-height: 16px !important;
  display: inline-flex;
  align-items: center;
  padding: 0 3px;
  outline: none;
  margin: 0;
}
span br {
}
</style>