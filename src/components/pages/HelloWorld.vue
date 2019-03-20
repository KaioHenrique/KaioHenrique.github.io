<template>
  <div class="cmd">      
    <a v-for="(tag, index) in tags" :key="index" :style="{'color': tag.color}">{{tag.value}}</a>
    <div id="pointer" :class="!isCursorActive?'animation':''">▍</div>	
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      tags: [],
      isCursorActive: false,
      default : {
        color : "rgb(0,255,0)",
        delay: 80
      }
    }
  }, 
  methods: {
    async writeWord(word) {
      this.isCursorActive = true;

      var letters = word.value.split('');
      
      for (var i = 0; i < letters.length; i++) {        
        await this.writeLetter(letters[i], word.data)
      }
      this.isCursorActive = false;
      return letters.length;
    }, 

    async writeLetter(valueToBeWritten, data) {
      await this.sleep(data && data.delay ? data.delay : this.default.delay);
      this.tags.push({value: valueToBeWritten, color : data && data.color ? data.color : this.default.color});
    },
    async removeWords(words, data) {      
      await this.sleep(data.waitBeforeAction || 0)
      this.isCursorActive = true;
      var countLetterInWords = words.reduce((total, actual) => total + actual);
      for (var i = 0; i < countLetterInWords; i++){
        await this.sleep(data && data.delay ? data.delay : this.default.delay);
        this.tags.pop();
      }    
      this.isCursorActive = false;    
    },

    sleep (sleepForMS) {
      return new Promise(resolve => setTimeout(resolve, sleepForMS))
    },

    async writeTest() {
      
      await this.removeWords(
        [
          await this.writeWord({value : "Olá."})
        ],
      { delay : 80, waitBeforeAction: 1000} );

      await this.sleep(1000);

      await this.removeWords(
        [
          await this.writeWord({value : "Eu me chamo "}),
          await this.writeWord({value : "Kaio Henrique", data: { delay: 80, color: 'red'}}),
          await this.writeWord({value : ". "}),                
          await this.writeWord({value : "Seja bem vindo ao meu currículo"})
        ],
      { delay : 35, waitBeforeAction: 1000 } );            
    },
  },
  
  created() {
    this.writeTest().then();
  },

}
</script>

<style scoped>

  @-webkit-keyframes pointer-animation {
    0% { opacity: 0; }
    50% { opacity: 1; }
  } 

  @keyframes pointer-animation {
    0% { opacity: 0; }
    50% { opacity: 1; }
  }

  * {
    outline: none !important;
  }

  .cmd {
    padding: 5px;
    font-family: 'Roboto Mono', monospace;
    font-size: 0;
  }

  .cmd > * {
    font-size: 1rem;
  }

  #pointer {
    padding: 0px;

  }

  #pointer.animation {
    -webkit-animation: pointer-animation step-end 1s infinite;
    animation: pointer-animation 1s step-end infinite;
  }

  #pointer {
    color: green;
    display: inline;
  }
</style>
