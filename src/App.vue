<template>
  <div id="app">
    <TranslateForm @fetchLang="fetchLanguage()" @translated="translateNow" :langs="langs"/>
    <TranslateResult :text="translation"/>
  </div>
</template>

<script>
import TranslateForm from './components/TranslateForm'
import TranslateResult from './components/TranslateResult'

export default {
  name: 'app',
  components: {
    TranslateForm,
    TranslateResult
  },
  data:function(){
    return{
      langs:{},
      error:'',
      translation:''
    }
  },
  methods:{
    fetchLanguage:function(){
      this.$http.get("https://translate.yandex.net/api/v1.5/tr.json/getLangs?ui=en&key=trnsl.1.1.20171127T011435Z.a515bea39170c596.c15aa1dc10172be31e1bf4e46e06044a1cfb09a5")
      .then(function(response){
        this.langs=response.body.langs;
      })
      .catch(function(err){
          console.log(err);
      });
    },

    translateNow:function(ev){
      this.$http.get('https://translate.yandex.net/api/v1.5/tr.json/translate?key=trnsl.1.1.20171127T011435Z.a515bea39170c596.c15aa1dc10172be31e1bf4e46e06044a1cfb09a5&text='+ev.text+'&lang='+ev.language+'&format=plain')
      .then(function(response){
        this.validateResponse(response);
      })
      .catch(function(err){
        console.log(err);
      });
    },

    validateResponse:function(response){
        if(response.status===422){
            this.error='Could Not Translate Text Please Try Again Later';
        }else if(response.status===413){
          this.error="Please Enter A Shorter Text to translate ";
        }else if(response.status===200){
            this.translation=JSON.parse(response.bodyText).text[0];
        }else{
          this.error='Cannot translate at this moment please try again later';
        }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

