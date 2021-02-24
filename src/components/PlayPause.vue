<template>
  <div class="audio-player">
    <div>
      <audio id="audio-player" ref="player">
        <source
          src="../assets/Sho Madjozi - John CenaA COLORS SHOW.mp3"
          type="audio/mp3"
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
    </div>
    <div class="progress">
      <div v-show="audioLoaded">
        <div class="progress-bar">
          <input
            v-model="playbackTime"
            type="range"
            name="position"
            id="position"
            min="0"
            :max="audioDuration"
          />
        </div>
        <div class="time-update">
          <span class="" style="color: #94bcec" v-html="elapsedTime()">
            00:00
          </span>
          <span class="" style="color: #94bcec" v-html="totalTime()">
            00:00
          </span>
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
      audioLoaded: false,
      playbackTime: 0,
      audioDuration: 100,
    };
  },

  methods: {
    initSlider() {
      var audio = this.$refs.player;
      if (audio) {
        this.audioDuration = Math.round(audio.duration);
      }
    },
    toggleAudio() {
      var audio = this.$refs.player;

      if (audio.paused) {
        audio.play();
        this.isPlaying = true;
      } else {
        audio.pause();
        this.isPlaying = false;
      }
    },
    convertTime(seconds) {
      const format = (val) => `0${Math.floor(val)}`.slice(-2);
      // var hours = seconds / 3600;
      var minutes = (seconds % 3600) / 60;
      return [minutes, seconds % 60].map(format).join(":");
    },
    totalTime() {
      var audio = this.$refs.player;
      if (audio) {
        var seconds = audio.duration;
        return this.convertTime(seconds);
      } else {
        return "00:00";
      }
    },
    elapsedTime() {
      var audio = this.$refs.player;
      if (audio) {
        var seconds = audio.currentTime;
        return this.convertTime(seconds);
      } else {
        return "00:00";
      }
    },
    //Playback listener function runs every 100ms while audio is playing
    playbackListener() {
      var audio = this.$refs.player;

      //Sync local 'playbackTime' var to audio.currentTime and update global state
      this.playbackTime = audio.currentTime;

      //Add listeners for audio pause and audio end events
      audio.addEventListener("ended", this.endListener);
      audio.addEventListener("pause", this.pauseListener);
    },
    pauseListener() {
      this.isPlaying = false;
      this.listenerActive = false;
      this.cleanupListeners();
    },
    endListener() {
      this.isPlaying = false;
      this.listenerActive = false;
      this.cleanupListeners();
    },
    cleanupListeners() {
      var audio = this.$refs.player;
      audio.removeEventListener("freqtimeupdate", this.playbackListener);
      audio.removeEventListener("ended", this.endListener);
      audio.removeEventListener("pause", this.pauseListener);

      console.log("All cleaned up!");
    },
  },
  mounted: function () {
    // nextTick code will run only after the entire view has been rendered
    this.$nextTick(function () {
      var audio = this.$refs.player;
      //Wait for audio to load, then run initSlider() function to get audio duration and set the max value of our slider
      audio.addEventListener(
        "loadedmetadata",
        function () {
          this.initSlider();
        }.bind(this)
      );
      // "canplay" HTML Event lets us know audio is ready for play
      audio.addEventListener(
        "canplay",
        function () {
          this.audioLoaded = true;
        }.bind(this)
      );
      //Wait for audio to begin play, then start playback listener function
      this.$watch("isPlaying", function () {
        if (this.isPlaying) {
          // var audio = this.$refs.player;

          this.initSlider();
          //console.log("Audio playback started.");
          //prevent starting multiple listeners at the same time
          if (!this.listenerActive) {
            this.listenerActive = true;

            audio.addEventListener("freqtimeupdate", this.playbackListener);
          }
        }
      });
      //Update current audio position when user drags progress slider
      this.$watch("playbackTime", function () {
        var audio = this.$refs.player;
        var diff = Math.abs(this.playbackTime - audio.currentTime);

        //Throttle synchronization to prevent infinite loop between playback listener and this watcher
        if (diff > 0.01) {
          this.$refs.player.currentTime = this.playbackTime;
        }
      });
    });
  },
};
</script>

<style scoped>
.playpause {
  height: auto;
  min-width: 2em;
  cursor: pointer;
}
.progress {
  flex-grow: 1;
  background-color: white;
  padding: 8px;
  min-width: 30em;
  height: auto;
  position: relative;
}
.progress-bar {
  display: flex;
  flex-direction: column;
}
input {
  cursor: pointer;
  width: 100%;
}
.time-update {
  display: flex;
  justify-content: space-between;
}
.audio-player {
  display: flex;
  align-items: center;
  width: 100%;
  margin: 0em 3em;
  padding-top: 1em;
  box-shadow: 0 4px 16px 0 rgba(0, 0, 0, 0.07);
}
</style>
