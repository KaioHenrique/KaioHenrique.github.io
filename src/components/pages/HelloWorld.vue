<template>
  <div class="cmd">      
    <a v-for="(tag, index) in tags" :key="index" :style="{'color': tag.color}">{{tag.value}}</a>
    <div id="pointer" :class="!isWriting?'animation':''">▍</div>	
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      tags: [],
      isWriting: false,
      default : {
        color : "rgb(0,255,0)"
      }
    }
  }, 
  methods: {
    async writeWord(word) {
      this.isWriting = true;
      var letters = word.value.split('');
      
      for (var i = 0; i < letters.length; i++) {        
        await this.writeLetter(letters[i], word.data)
      }
      this.isWriting = false;
    }, 

    async writeLetter(valueToBeWritten, data) {
      await this.sleep(data.speed || 0);
      this.tags.push({value: valueToBeWritten, color : data && data.color ? data.color : this.default.color});
    },

    sleep (sleepForMS) {
      return new Promise(resolve => setTimeout(resolve, sleepForMS))
    },
    async writeTest() {
      await this.writeWord({value : "Olá.", data: { speed: 80}});
      await this.writeWord({value : "Eu me chamo ", data: { speed: 80}});
      await this.writeWord({value : "Kaio Henrique", data: { speed: 80, color: 'red'}});
      await this.writeWord({value : ". ", data: { speed: 80}});
      await this.writeWord({value : "Seja bem vindo ao meu currículo", data: { speed: 80}});   
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

  .letter {
    color: rgb(0, 255, 0)
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
