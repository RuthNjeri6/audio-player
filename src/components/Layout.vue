<template>
  <div id="__sdncjhbjcehbuhdfc">
      <div class="controls">
         <play-pause status="Pablo" />
         <button @click="submit">Submit</button>
      </div>
      <div class="text-area-container">
         <textarea ref="text" @input='update($event)' class="text-area" v-model="value"></textarea>
      </div>
   </div>
</template>

<script>

import PlayPause from "./PlayPause.vue";

export default {
   name: 'layout',

   components: {
      PlayPause
   },
   
   data() {
      return {
         seedLine: [],
         value: "on my hecatomithi reluctantly make friends to the increasingly tired analogy of canons at the chosen people three from bondage egypt and set on a long"
      }
   },

   mounted() {
      let vm = this
      this.$nextTick(() => {
         vm.update()
      })
   },

   methods: {
      submit() {

      },

      update() {
         let target = this.$refs.text
         target.style.height = "";
         target.style.height = target.scrollHeight + "px"
      }
   }
}
</script>

<style scoped>
   .controls {
      display: flex;
      flex-direction: row;
      justify-content:space-between;
      padding-bottom: 2em;
   }
   
   .off:focus {
      outline: none;
   }

   textarea {
      font-family: 'Ubuntu', sans-serif;
      font-size: 4.5em;
      border: none;
      overflow: auto;
      outline: none;

      -webkit-box-shadow: none;
      -moz-box-shadow: none;
      box-shadow: none;
      resize: none;

      width: 100%;
      min-height: 35px;
   }

   .text-area-container {  
      width:100%;
      height: 100%;
   }

</style>