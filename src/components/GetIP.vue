<template>
  <div id="app">

    <form @submit.prevent="checkIP">
      <button type="submit" name="button"> Check </button>
    </form>

    <div class="meta-response">
      <div v-if="show">
        <h2>IP-Status: {{this.APIanswer.verbose_msg}}</h2>
        <p><b>Netzwerk-IP: </b> {{this.APIanswer.network}} </p>
        <p><b>Kontinent: </b> {{this.APIanswer.continent}} </p>
        <button type='button' @click= "details">Details</button>
      </div>

      <div v-if="moreDetails" >
        <p><b>Netzwerk: </b> {{this.APIanswer.last_https_certificate.subject.CN}} </p>
        <p><b>Land: </b> {{this.APIanswer.last_https_certificate.subject.C}} </p>
        <p><b>Standort: </b> {{this.APIanswer.last_https_certificate.subject.L}} </p>
        <p><b>Organisation: </b> {{this.APIanswer.last_https_certificate.subject.O}} </p>
        <p><b>State: </b> {{this.APIanswer.last_https_certificate.subject.ST}} </p>
        <a v-bind:href="link">Mehr Informationen</a>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name:"GetIP",
  props: ['InputIP'],
  data () {
      return{
          APIanswer: [],
          IPString: '',
          show: false,
          moreDetails: false,
          link: ''
      }
  },

  methods: {
    checkIP(e) {
        e.preventDefault();     
        this.IPString = 'https://cors-anywhere.herokuapp.com/https://www.virustotal.com/vtapi/v2/ip-address/report?apikey=5e28b7891c2d6e9cedef2895728789071e59769820f5f8110293cb699697cc52&ip='+this.InputIP, 
        console.log(this.IPString);
        axios.get(this.IPString)
        .then(res => this.APIanswer = res.data)
        .catch(err => console.log(err));
        this.show = true
    },

    details(){
      this.moreDetails = true,
      this.link = 'https://www.virustotal.com/gui/ip-address/'+this.InputIP+'/detection'
    },
   
  }
}
</script>

<style scoped>

.meta-response {
  background: rgba(98, 80, 110, 0.5);;
  width: 800px;
  margin-left: auto;
  margin-right: auto;
  margin-bottom: 15px;
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
  margin-bottom: 30px;
  Padding: 15px;
  width: 250px;
}
</style>