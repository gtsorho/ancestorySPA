<template>
<Navbar routeName="Home" @isauthenticated="$emit('isauthenticated')" />
<div class="row">
<Leftcol v-bind="ancestor" />
<Rightcol v-bind="ancestor" />
</div>
</template>

<script>
// @ is an alias to /src
import Leftcol from "@/components/leftcol.vue";
import Rightcol from "@/components/rightcol.vue";
import Footer from "@/components/Footer.vue";
import Navbar from "@/components/Navbar.vue";


export default {
  name: 'Home',
  components: {
    Leftcol,
    Rightcol,
    Footer,
    Navbar,
  },
  emits:[
        'isauthenticated'
    ],
    data() {
        return {
            ancestor:JSON.parse(this.$route.query.ancestor)
        }
    },
    created(){
    },
  beforeCreate: function() {
    document.body.className = 'home';
  }

}
</script>

<style scoped>
</style>
