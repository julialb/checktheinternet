<template>
  <div id="app">

    <!-- Einbinden des Buttons "Check" der die Funktion "checkURL" für die Get-Anfrage auslöst --> 
    <form @submit.prevent="checkURL">
      <button type="submit" name="button"> Check </button>
    </form>
     
    <!-- Ausgabe eines Teils der API-Antwort mittels Interpolation -->
    <div v-if="show"  class="meta-response">
      <h2>URL-Status: {{this.APIanswer.verbose_msg}}</h2>
      <p><b>Ressource:</b> {{this.APIanswer.resource}} </p>
      <p><b>Zuletzt geändert: </b>{{this.APIanswer.scan_date}} UTC </p>
      <p><b>Positive Funde:</b>{{this.APIanswer.positives}}</p>
      <p><b>Geprüfte Seiten:</b>{{this.APIanswer.total}} </p>
      
      <!-- Einbindung des Buttons "Mehr Info" der die Funktion "buttonShow" auslöst -->
      <form @submit.prevent="buttonShow">
        <button type="submit" name="button">Details</button>
      </form>

      <!-- Wenn "malware" durch "buttonShow" auf true gesetzt wird, werden nachträglich weitere Ergebnisse der Get-Anfrage ausgegeben -->
      <div v-if="malware">
        <h2><b>Scans</b></h2> 
        <p>Die Seite wurde von verschiedenen Dienstleistern auf Viren Überprüft:</p>
        <p><b>MalwarePatrol</b><br> Detected: {{MaPaDec}} <br> Result: {{MaPaRe}} <br></p> 
        <p> <b>BitDefender</b><br> Detected: {{BiDeDec}} <br> Result: {{BiDeRe}} <br></p>
        <p> <b>Kaspersky</b><br> Detected: {{KaDec}} <br> Result: {{KaRe}} <br></p>
        <p> <b>CyberCrime</b><br> Detected: {{CyCrDec}} <br> Result: {{CyCrRe}} <br></p>
        <a v-bind:href="link">Mehr Informationen</a>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name:"GetUrl",
  props: ['InputUrl'],  //Übergabe des eigegebenen Strings von "InsertTxt"

  data () {
      return{
          APIanswer: [],
          UrlString: '',
          MaPaDec: '',
          MaPaRe: '',
          BiDeDec: '',
          BiDeRe: '',
          KaDec: '',
          KaRe: '',
          CyCrDec:'',
          CyCrRe: '',
          show: false,
          malware: false,
          link: ''
      }
  },

  methods: {
  // API-Key: 5e28b7891c2d6e9cedef2895728789071e59769820f5f8110293cb699697cc52 

    checkURL(e) {
        e.preventDefault();     
        //Erstellen des Anfragelinks mittels Proxy, Link zur API-Dienstleistung, dem API-Key und der Usereingabe InputUrl
        this.UrlString = 'https://cors-anywhere.herokuapp.com/https://www.virustotal.com/vtapi/v2/url/report?apikey=5e28b7891c2d6e9cedef2895728789071e59769820f5f8110293cb699697cc52&resource='+this.InputUrl, 
        console.log(this.UrlString);
        axios.get(this.UrlString)  //GET-Anfrage an den API-Rechner
        .then(res => this.APIanswer = res.data) //Speichern der Antwort des API-Rechners
        .catch(err => console.log(err));
        this.show = true  
    },

 buttonShow(){
      this.MaPaDec = this.APIanswer.scans.MalwarePatrol.detected,
      this.MaPaRe = this.APIanswer.scans.MalwarePatrol.result,
      this.BiDeDec = this.APIanswer.scans.BitDefender.detected,
      this.BiDeRe = this.APIanswer.scans.BitDefender.result,
      this.KaDec = this.APIanswer.scans.Kaspersky.detected,
      this.KaRe = this.APIanswer.scans.Kaspersky.result,
      this.CyCrDec = this.APIanswer.scans.CyberCrime.detected,
      this.CyCrRe = this.APIanswer.scans.CyberCrime.result,
      this.malware = true,
      this.link = this.APIanswer.permalink
    }
  }
}
</script>

<style scoped>

.meta-response {
  background: rgba(98, 80, 110, 0.5);;
  padding: 1px;
  width: 800px;
  margin-left: auto;
  margin-right: auto;
  margin-top: 30px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25); 
  border-radius: 16px;
}

.meta-response h2{
  text-align: center;
  color: white;
  font-size: 25px;
  font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  margin-top: 30px;
  margin-bottom: 30px; 
}  

.meta-response p{
  text-align: left;
  color: white;
  font-size: 20px;
  font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  margin: 30px;
  margin-left: 5%;
}  

.meta-response a{
  text-align: Center;
  color: white;
  font-size: 20px;
  font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
}

button {
  margin: 20px;
  margin-bottom: 10px;
  Padding: 15px;
  width: 250px;
}
</style>