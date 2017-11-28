<template>
 <section class="section">
    <div class="columns">
        <article v-if="text" class="message column is-8 container">
          <div class="message-body" v-text="text"></div>
          <input class="butt js--triggerAnimation" :click="speakNow()" type='button' value='🔊 Play' />


        </article>
    </div>
  </section>
</template>

<script>

export default {
  props:{text:{
    type:String,
    required:true
  }},
    data:function()  {
      return{

      }
    },
    name:'TranslateResult',
    methods:{
        speakNow:function(){
          responsiveVoice.speak(this.text);
        }
    }
}
</script>

<style scoped>


</style>
