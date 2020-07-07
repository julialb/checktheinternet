<template>
  <div id="app">

    <div id="headerpic">
      <img src="./media/80head.png" alt="Internet der 80er">
    </div>

    <audio id="audio_with_controls" 
           controls
           src="./media/Timecop1983_On-the-run.mp3" 
           type="audio/mp3" 
    >
    </audio>

    <div class="head">
      <h2> Wir machen das Internet nostalgischer und sicherer! <br> Unsere Dienstleistungen für Sie: </h2>
      <p> Herausfinden von Standort und Subdomains einer Domain </p>
      <p> Überprüfung von URL's auf Viren</p>
      <p> Aufdecken zu wem eine IP-Adresse gehört </p>
    </div>

    <div>
      <!-- Einbinden der Komponente InsertTxt und setzen der Auswahl der Get-Komponenten "URL", Domain" und "IP-Adresse" mittels  true -->
      <InsertTxt @buttonClickedDOM="checkDom" 
                 @buttonClickedURL="checkUrl" 
                 @buttonClickedIP="checkIP"
                 :retdom="setdom"
                 :retip="setip"
                 :returl="seturl"
                 :retplace="placeh"/>
    </div>
  </div>
</template>

<script>
import InsertTxt from './components/InsertTxt.vue'

export default {
  name: 'App',

  components: {
    InsertTxt,
  },

  data(){
  return{
     // Boolean zur überprüfung, welche Komponente vom User ausgewählt wurde (Domain für Startzustand auf true)
     seturl: false,
     setdom: true,
     setip: false,
     placeh: 'Domain einfügen...'
  }
},

methods : {
   
    checkDom(dataDom){
      this.setdom = dataDom.dom,
      this.seturl = dataDom.url,
      this.setip = dataDom.ip
      this.placeh = 'Domain einfügen...'
      
    }, 
    checkUrl(dataUrl){
      this.setdom = dataUrl.dom,
      this.seturl = dataUrl.url,
      this.setip = dataUrl.ip,
      this.placeh = 'Url einfügen...'
     },
    checkIP(dataIP){
      this.setdom = dataIP.dom,
      this.seturl = dataIP.url,
      this.setip = dataIP.ip,
      this.placeh = 'IP-Adresse einfügen...'
    }
 } 
}
</script>

<style>
#headerpic img {
  height: 223px;
  width: 555px;
  margin: 50px;
}

.head {
  background: rgba(98, 80, 110, 0.5);;
  padding: 1px;
  width: 800px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25); 
  border-radius: 16px;
  margin-left: auto;
  margin-right: auto;
}

.head h2{
  color: white;
  font-size: 25px;
  font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  margin: 30px;
}

.head p{
  color: white;
  font-size: 20px;
  font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
  margin: 10px;
  text-align: center;
}

#app {
  text-align: center;
  color: #2c3e50;
  margin: auto;
  width: 100%;
  background-color: rgb(250, 249, 215);
  display: inline-block;
  margin-right: auto;
  margin-left: auto;

  background: url("./media/80body.jpg");
  background-position: bottom center; 
	background-repeat: no-repeat; 
	background-size:cover; 
	background-attachment:fixed;
}

audio {
  margin-bottom: 20px;
}
</style>