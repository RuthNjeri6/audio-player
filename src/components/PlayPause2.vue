<template>
   <div class="audio-player">
      <div class="audio">
         <audio 
            id="audio-player"
            ref= "player">
         <source src="https://file-examples-com.github.io/uploads/2017/11/file_example_MP3_5MG.mp3" type="audio/mpeg">
         Your browser does not support the audio tag.
         </audio>
      </div>
      <div 
         class="playpause"
         @click = "toggleAudio()"
         >
         <input type="checkbox" value="None" id="playpause" name="check" />
         <label for="playpause" tabindex=1></label>
      </div>
      <div class="progress">
         <!--progress bar goes here -->
      </div>
   </div>
</template>

<script>
export default {
   name: 'play-pause',

   data() {
      return {
         isPlaying: false,
         audioLoaded: false
      };
   },
   methods: {
      toggleAudio() {
         var audio = this.$refs.player; 
         this.isPlaying ? audio.pause(): audio.play();     
      }
   },
   mounted: function() {
      var audio = this.$refs.player;
      console.log(audio.paused)
   }
}
</script>

<style scoped>
   .playpause label {
      display: block;
      box-sizing: border-box;
      width: 0;
      height: 74px;
      border-color: transparent transparent transparent #202020;
      transition: 100ms all ease;
      cursor: pointer;
      border-style: solid;
      border-width: 37px 0 37px 60px;
      /* border-style: double;
      border-width: 0px 0 0px 60px; */
      /* border-radius: 25px; */
   }

   .playpause input[type=checkbox] {
      position: absolute;
      left: -9999px;
   }

   .playpause input[type=checkbox]:checked + label {
      border-style: double;
      border-width: 0px 0 0px 60px;
      /* border-style: solid;
      border-width: 37px 0 37px 60px; */
   }

   .playpause input[type=checkbox]:focus + label {
      box-shadow: 0 0 35px lightblue;
   }
   .progress {
      background-color: purple;
      min-width: 15em;
      height: 2em;
   }
</style>