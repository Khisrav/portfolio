<template>
  <header-view></header-view>
  <div class="hero">
    <div class="aligner">
      <div class="container">
        <div class="row">
          <div class="col-md-6">
            <h1>{{ $t("Hi") }}, I'm <span class="golden-text">{{ currentText }}<span v-show="isTyping" class="cursor">|</span></span></h1>
            <h2></h2>
          </div>
          <div class="col-md-6"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import HeaderView from '@/components/HeaderView.vue';
export default {
  components: {
    HeaderView
  },
  data() {
    return {
      winHeight: window.innerHeight,
      words: ["Fullstack Developer", "UI/UX designer", "Pythonist", "Khisrav"],
      currentIndex: 0,
      currentWord: "",
      currentText: "",
      typingSpeed: 75, // milliseconds per character
      delayBetweenWords: 1500, // milliseconds
      isTyping: false,
      showCursor: false
    }
  },
  methods: {
    startTyping() {
      if (this.currentIndex === this.words.length) {
        // If all words have been typed, stop the typing simulation
        return;
      }
      this.isTyping = true;
      this.showCursor = true;
      this.currentWord = this.words[this.currentIndex];
      this.simulateTyping(this.currentWord, () => {
        setTimeout(() => {
          this.simulateBackspace(this.currentWord, () => {
            this.currentIndex = (this.currentIndex + 1) % this.words.length;
            this.startTyping();
          });
        }, this.delayBetweenWords);
      });
    },
    simulateTyping(targetText, callback) {
      let index = 0;
      this.currentText = ""; // Reset the current text
      const interval = setInterval(() => {
        if (index < targetText.length) {
          this.currentText += targetText[index];
          index++;
        } else {
          clearInterval(interval);
          callback();
        }
      }, this.typingSpeed);
    },
    simulateBackspace(targetText, callback) {
      let index = targetText.length;
      const interval = setInterval(() => {
        if (index > 0) {
          this.currentText = targetText.substring(0, index - 1);
          index--;
        } else {
          clearInterval(interval);
          this.showCursor = false;
          callback();
        }
      }, this.typingSpeed / 2); // Backspace faster than typing
    },
  },
  watch: {
    currentIndex(newIndex) {
      if (newIndex === this.words.length) {
        // If the last word has been typed, stop the typing simulation
        this.isTyping = false;
      }
    },
  },
  mounted() {
    // Start automatic typing simulation when the component is mounted
    this.startTyping();
    console.log(this.$i18n)
  }
};
</script>

<style>
.hero {
  background-image: url('../assets/background.jpg');
  background-position: center;
  background-size: cover;
  background-attachment: fixed;
  height:100vh;
}
.aligner {
  padding-top: 256px;
}
.golden-text {
  color:#bfa75d;
}

.cursor {
  margin-left: 3px;
  animation: blink 0.8s steps(1) infinite;
}

@keyframes blink {
  0%, 100% {
    display:inline
  }
  50% {
    display: none;
  }
}
</style>