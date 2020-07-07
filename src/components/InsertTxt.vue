<template>
  <main>
    <div>
      <!-- Einbinden der Buttons mit den jeweiligen Events "sendClick" zur Auswahl der Kindkomponente-->
      <button type="button" @click.prevent= "sendClickDOM"> Domain </button>
      <button type="button" @click.prevent= "sendClickURL"> URL </button>
      <button type="button" @click.prevent= "sendClickIP"> IP-Adresse </button> 
  
        <form>
          <!-- Einbinden des Textfeldes  -->
          <input type="text" v-model="InputString" name="InputString" :placeholder= "retplace" >       
        </form>

      <!-- Auswahl welche der Komponenten aufgerufen wird und übergabe des eingegebenen Strings -->
      <GetDom  v-if="retdom" :InputDom= "InputString" />
      <GetUrl v-if="returl" :InputUrl= "InputString" />
      <GetIP v-if="retip" :InputIP= "InputString" />
    </div>
  </main>
</template>

<script>
import GetDom from './GetDom.vue'
import GetUrl from './GetUrl.vue'
import GetIP from './GetIP.vue'
export default {
    name: 'InsertTxt',
    props:['retdom','returl','retip','retplace'],
    
    components: {
        GetDom,
        GetUrl,
        GetIP
        
    },
    data() {
        return {
            InputString: ''
    
        }
    },
    methods : {

  //Auswahl der jeweiligen Kindkomponente durch setzen von True und Löschung der Texteingabe beim erneuten Drücken eines Buttons

 sendClickDOM() {
      this.$emit('buttonClickedDOM',{
        url: false, ip: false, dom: true
      });
      this.InputString = ''
 },
 sendClickURL() {
      this.$emit('buttonClickedURL',{
        url: true, ip: false, dom: false
      });
      this.InputString = ''
}, 
  sendClickIP() {
      this.$emit('buttonClickedIP',{
        url: false, ip: true, dom: false
      });
      this.InputString = ''
  }
    }
}
</script>

<style>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

main {
  min-height: 100vh;
  padding: 25px;  
}

button {
  margin: 5px;
  margin-bottom: 20px;
  Padding: 15px;
  width: 120px;
}

input {
  display: inline-block;
  width: 500px;
  padding: 15px;

  appearance: none; 
  border: none; 
  outline: none; 
  background: none; 

  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.25); 
}

input:focus,
input:hover {
  background-color: rgba(255, 255, 255, 0.75); 
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25); 
  border-radius: 16px 0px 16px 0px;
}

</style>