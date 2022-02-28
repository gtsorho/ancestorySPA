<template>
<Navbar routeName="Registration" @editdata="edit" />


  <div class="card" :style="{border:borderValue}">
    <div class="card-header">
      <div class="progress">
        <div
          class="progress-bar progress-bar-striped progress-bar-animated " 
          :class='color'
          role="progressbar"
          aria-valuenow="75"
          aria-valuemin="0"
          aria-valuemax="100"
          :style="{width:progressVal + '%'}"
        >{{progressVal}}</div>
      </div>
    </div>
    <div class="card-body">      
      <Form @progress="progress($event)" @borderstatus="setbordervalue" :dataToEdit="editable" />
    </div>
  </div>

</template>

<script>
import Form from '@/components/form.vue'
import Navbar from "@/components/Navbar.vue";

export default {
  name: 'registration',
  components:{
    Form,
    Navbar
  },
  emits:[
        'isauthenticated'
    ],
  data() {
    return {
      progressVal:25,
      color:'bg-danger',
      borderValue:null,
      editable:null
      
    }
  },
  methods:{
    edit(data){
      this.editable = data
    },
    progress(data){
      switch(data) {
        case 1:
          this.progressVal = 20
          this.color = 'bg-danger'
          break;
        case 2:
          this.progressVal = 40
          this.color = 'bg-info'
          break;
        case 3:
          this.progressVal = 60
          this.color = 'bg-warning'
          break;
        case 4:
          this.progressVal = 80
          this.color = 'bg-secondary'
          break;
        case 5:
          this.progressVal = 100
          this.color = 'bg-success'
          break;
        default:
          // code block
      }
    },
    setbordervalue(){
      this.borderValue = "6px solid #198754 !important";
      setTimeout(() => {
          this.borderValue = null;
          window.location.reload()
      }, 3000);
    }
  },
  beforeMount(){
          if(!localStorage.getItem('token')){
              this.$router.push('/')
          }
  },
  beforeCreate: function() {
    document.body.className = 'about';
    // document.getElementById("navel1").classList.add("about");
    // document.getElementById("navel2").classList.add("about");
    // document.getElementById("navel3").classList.remove("text-dark");
    // document.getElementById("navel3").classList.add("text-light");    
  }
};
</script>

<style scoped>
.card{
  width:50%;
  margin-top: 5% !important;
  margin: 0 auto; /* Added */
  float: none; /* Added */
  margin-bottom: 10px; /* Added */
  background-color: #ffffffdb !important;
}

</style>