 <template>
  <div id="app">

    <form @submit.prevent="checkDOM">
      <button type="submit" name="button"> Check </button> 
    </form>

    <div class="meta-response">
      <div v-if="show">
        <h2>Domain-Status: {{this.APIanswer.verbose_msg}}</h2>
        <p><b>Kategorie: </b> {{this.APIanswer.categories}} </p>
        <button type='button' @click= "showsub">Show Subdomains</button>
        <button type='button' @click= "details">Details</button>
      </div>

      <div v-if="showmore">
        <p><b>Land: </b> {{this.APIanswer.last_https_certificate.subject.C}} </p>
        <p><b>Netzwerk: </b> {{this.APIanswer.last_https_certificate.subject.CN}} </p>
        <p><b>Standort: </b> {{this.APIanswer.last_https_certificate.subject.L}} </p>
        <p><b>Organisation: </b> {{this.APIanswer.last_https_certificate.subject.O}} </p>
        <p><b>State: </b> {{this.APIanswer.last_https_certificate.subject.ST}} </p>
        <a v-bind:href="link">Mehr Informationen</a>
      </div>

      <div v-if="showdom" id="subdomains">
       <div v-bind:key="sub.id" v-for="sub in subdom"> 
        <p>{{sub}}</p>
       </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name:"GetDom",
  props: ['InputDom'],

  data () {
      return{
          APIanswer: [],
          DomString: '',
          show: false,
          subdom: [],
          showdom: false,
          showmore: false,
          link: ''
      }
  },

  methods: {

    checkDOM() {
        this.DomString = 'https://cors-anywhere.herokuapp.com/https://www.virustotal.com/vtapi/v2/domain/report?apikey=5e28b7891c2d6e9cedef2895728789071e59769820f5f8110293cb699697cc52&domain='+this.InputDom, 
        console.log(this.DomString);
        axios.get(this.DomString)
        .then(res => this.APIanswer = res.data)
        .catch(err => console.log(err));
       this.showdom = false
       this.show = true
    },

    showsub(){
    this.subdom = this.APIanswer.subdomains 
    this.showdom = true
    this.showmore = false 
    },

    details(){
    this.showmore = true,
    this.showdom = false,
    this.link = 'https://www.virustotal.com/gui/domain/'+this.InputDom+'/detection'
    }
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

#subdomains p{
  text-align: center;
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