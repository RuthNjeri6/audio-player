<template>
  <div class="audio-player">
    <div>
      <audio id="audio-player" ref="player">
        <source
          src="https://file-examples-com.github.io/uploads/2017/11/file_example_MP3_5MG.mp3"
          type="audio/mpeg"
        />
        Your browser does not support the audio tag.
      </audio>
    </div>
    <div class="playpause">
      <img
        src="../assets/play-solid.svg"
        alt=""
        @click="toggleAudio()"
        v-show="!isPlaying"
        ref="play"
        id="play-button"
      />
      <img
        src="../assets/pause-solid.svg"
        alt=""
        @click="toggleAudio()"
        v-show="isPlaying"
        ref="pause"
        id="pause-button"
      />
      <!-- <input type="checkbox" value="None" id="playpause" name="check" />
      <label for="playpause" tabindex=1></label> -->
    </div>
    <div class="progress">
       <div style="position: relative;">
         <input
            v-model="playbackTime"
            type="range" 
            name="position" 
            id="position" 
            min="0"
            :max="audioDuration"/>
         <div class="">
            <span class="" style="color: #94bcec" v-html="elapsedTime()"> 00:00 </span>
            <span class="" style="color: #94bcec" v-html="totalTime()"> 00:00 </span>
         </div>
       </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "play-pause",

  data() {
    return {
      isPlaying: false,
      // audioLoaded: false,
      playbackTime: 0,
      audioDuration: 100
    };
  },

  methods: {
   initSlider(){
      var audio = this.$refs.player;
      if(audio){
         this.audioDuration = Math.round(audio.duration);
      }
   },
   toggleAudio() {
      var audio = this.$refs.player;

      if(audio.paused) {
         audio.play();
         this.isPlaying = true;
      }
      else {
         audio.pause();
         this.isPlaying = false;
      }
   },
   convertTime(seconds){
      const format = val => `0${Math.floor(val)}`.slice(-2);
      // var hours = seconds / 3600;
      var minutes = (seconds % 3600) / 60;
      return [minutes, seconds % 60].map(format).join(":");
   },
   totalTime() {
      var audio = this.$refs.player;
      if(audio){
      var seconds = audio.duration;
      return this.convertTime(seconds);
      }else {
         return '00:00';
      }
   },
   elapsedTime(){
      var audio = this.$refs.player;
      if(audio){
         var seconds = audio.currentTime;
         return this.convertTime(seconds);
      }
      else {
         return '00:00';
      }
   },
   playbackListener() {
      var audio = this.$refs.player;

      this.playbackTime = audio.currentTime;

      audio.addEventListener("ended", this.endListener);
      audio.addEventListener("pause", this.pauseListener);
   
   },
   pauseListener() {
      this.isPlaying = false;
      this.listenerActive = false;
      this.cleanupListener();
   },
   endListener() {
      this.isPlaying = false;
      this.listenerActive = false;
      this.cleanupListener();
   },
   cleanupListener() {
      var audio = this.$refs.player;
      audio.removeEventListener("freqtimeupdate",this.playbackListener);
      audio.removeEventListener("ended", this.endListener);
      audio.removeEventListener("pause", this.pauseListener);
   }
  },
      mounted: function() {
      // nextTick code will run only after the entire view has been rendered
      this.$nextTick(function() {
        
        var audio=this.$refs.player;
        //Wait for audio to load, then run initSlider() to get audio duration and set the max value of our slider 
        // "loademetadata" Event https://www.w3schools.com/tags/av_event_loadedmetadata.asp
        audio.addEventListener(
          "loadedmetadata",
          function() {
            this.initSlider();
          }.bind(this)
        );
        // "canplay" HTML Event lets us know audio is ready for play https://www.w3schools.com/tags/av_event_canplay.asp
        audio.addEventListener(
          "canplay",
          function() {
            this.audioLoaded=true;
          }.bind(this)
        );
        //Wait for audio to begin play, then start playback listener function
        this.$watch("isPlaying",function() {
          if(this.isPlaying) {
            var audio=this.$refs.player;
            this.initSlider();
            //console.log("Audio playback started.");
            //prevent starting multiple listeners at the same time
            if(!this.listenerActive) {
              this.listenerActive=true;
              audio.addEventListener("freqtimeupdate",this.playbackListener);
            }
          }
        });
        //Update current audio position when user drags progress slider
        this.$watch("playbackTime",function() {
            var audio=this.$refs.player;
        var diff=Math.abs(this.playbackTime-audio.currentTime);
        
          //Throttle synchronization to prevent infinite loop between playback listener and this watcher
          if(diff>0.01) {
            this.$refs.player.currentTime=this.playbackTime;
          }
        });
      });
    }
};
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
.playpause img {
  height: auto;
  min-width: 2em;
  cursor: pointer;
}
.progress {
  display: flex-grow;
  background-color: white;
  border: 4px solid #2b6cb0;
  padding: 8px;
  min-width: 30em;
  height: auto;
}
.audio-player {
  display: flex;
  align-items: center;
}
.audio-player div {
}
</style>
