<template>
   <section class="section">
    <div class="container">

      <h1 class="title">A Simple Translate Application Powered By Vue.js</h1>
            <br />
          <div class="field is-horizontal">
        <div class="field-label is-normal"></div>


        <div class="field-body">
          <div class="field">
            <div class="control has-icons-left">
              <div class="select is-fullwidth">
              <select v-model="endLanguage">
                <option v-for="(lang, index) in langs" :value="lang"> {{lang}} </option>

              </select>
              </div>
              <div class="icon is-small is-left">
                <i class="fa fa-globe"></i>
              </div>

            </div>
          </div>
          <div class="field">
            <p class="control is-expanded has-icons-left has-icons-right">
              <input class="input" v-model="text" type="text" placeholder="Enter Text">
              <span class="icon is-small is-left">
                <i class="fa fa-pencil"></i>
              </span>
              <span class="icon is-small is-right">
                <i class="fa fa-spin"></i>
              </span>
            </p>
          </div>
          <div class="field">
            <p class="control is-expanded has-icons-left has-icons-right">
            <a class="button is-primary" href="/translate" @click.prevent="translateText">Translate Text</a>
            </p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'Translate',
  props:['langs'],
  data () {
    return {
        endLanguage:'',
        text:'',
        encodedText:''
    }
  },
  created:function(){
    this.$emit('fetchLang');
  },
  methods:{
    translateText:function(){
      if(this.endLanguage=='' || this.text==''){
        alert('Please Fill In All Fields');
      }else{
        this.encodedText=encodeURI(this.text);
        this.text='';
        var $vm=this;
        Object.keys(this.langs)
        .forEach(function(elem){
          if($vm.langs[elem]===$vm.endLanguage){
            $vm.endLanguage=elem;
            $vm.$emit('translated',{language:$vm.endLanguage,text:$vm.encodedText});
          }
        });


      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


</style>
