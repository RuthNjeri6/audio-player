<template>
   <div class="audio-player">
      <div >
         <audio 
            id="audio-player"
            ref="player"
            >
         <source src="https://file-examples-com.github.io/uploads/2017/11/file_example_MP3_5MG.mp3" type="audio/mpeg">
         Your browser does not support the audio tag.
      </audio>
      </div>
      <div class="playpause">
         <img 
            src="../assets/play-solid.svg" 
            alt="" 
            @click ="toggleAudio()"
            ref="play"
            id="play-button">
         <img 
            src="../assets/pause-solid.svg" 
            alt="" 
            @click ="toggleAudio()"
            ref="pause"
            id="pause-button">
      <!-- <input type="checkbox" value="None" id="playpause" name="check" />
      <label for="playpause" tabindex=1></label> -->
      </div>
      <div class="progress">

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
      }
   },

   methods: {
      toggleAudio (){
         var audio = this.$refs.player;
         var playbutton = document.getElementById("play-button");
         var pausebutton = document.getElementById("pause-button");
         playbutton.addEventListener("click", function(){
            audio.play();
         })
         pausebutton.addEventListener("click", function(){
            audio.pause();
         })
      }
   }
}
</script>

<style scoped>
   /* .playpause label {
      display: block;
      box-sizing: border-box;
      width: 0;
      height: 74px;
      border-color: transparent transparent transparent #202020;
      transition: 100ms all ease;
      cursor: pointer;
      border-style: double;
      border-width: 0px 0 0px 60px;
      /* border-radius: 25px; 
   }

   .playpause input[type=checkbox] {
      position: absolute;
      left: -9999px;
   }

   .playpause input[type=checkbox]:checked + label {
      border-style: solid;
      border-width: 37px 0 37px 60px;
   }

   .playpause input[type=checkbox]:focus + label {
      box-shadow: 0 0 35px lightblue;
   } */
   .playpause img{
      height: auto;
      min-width: 2em;
      cursor: pointer;
   }
   .progress {
      display: flex-grow;
      background-color: white;
      border: 4px solid #2b6cb0;
      padding: 8px;
      max-width: 30em;
      height: auto;
   }
   .audio-player  {
      display: flex;
      align-items: center;
   }
   .audio-player div{
     
   }
</style>