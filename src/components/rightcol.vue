<template>
    <div class="col ms-5">
        <div class="container" :style="{height:biographyH}">
            <h1 class="mb-4">{{firstname +' '+lastname +' '+ othernames }}</h1>
                <p class=" d-inline">
                        {{firsthalf}}
                    <span class="text-dark" :style="dots">...</span>
                    <span class="text-dark" :style="{display: more}" >
                        {{secondhalf}}
                    </span>
                </p>
                <a href="#" class="d-inline" @click="showMore">{{btnTxt}}</a>
        </div>

        <div class="row ps-4" >
            <div class="col">
                <h3>Territories</h3>
                <ul class="list-inline">
                    <li class="list-inline-item" v-for="(location, i) in Territories" :key="i">{{location}}</li>
                </ul>
                <!-- <li v-for="(location, i) in Territories" :key="i">{{location}}</li> -->
            </div>
        </div>

        <h3 class="mt-3  ms-4" >Memories</h3>
        <Memoryslider :memories="memories" />
    </div>
</template>

<script>
import Memoryslider from "@/components/memoryslider.vue"

export default {
    name: "rightcol",
    components:{
        Memoryslider
    },
    props:{
        firstname:String,
        lastname:String,
        othernames:String,
        territories:String,
        biography:String,
        memories:Array
    },
    data() {
        return {
            Territories: JSON.parse(this.territories),
            firsthalf:null,
            secondhalf:null,
            dots: {
                display: ""
            },
            more: 'none',
            btnTxt: 'Read more',
            biographyH: "40%"
        }
    },
    created(){
        var newBio = this.biography.match(/[^.?!]+[.!?]+[\])'"`’”]*/g);
        var midile = Math.ceil(newBio.length/2)
        var arr = []
        arr = newBio.splice(0, midile);

        this.firsthalf = arr.join(" ");
        this.secondhalf = newBio.join(" ");

        console.log(arr.join(" "), newBio.join(" "))
    },
    methods: {
        showMore(){
            if(this.dots.display === 'none'){
                this.dots.display = "inline"
                this.btnTxt = "Read more"; 
                this.more = "none";
                this.biographyH = '40%'
            }else{
                this.dots.display = "none";
                this.btnTxt = "Read less"; 
                this.more = "inline";
                this.biographyH = ''
            }
        }
    },
    
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@400&display=swap');

p, h1, h2, h3, li, a{
    font-family: 'Raleway', sans-serif;
}
p{
    font-size: 130%;
    color: #000;
    transition: ease all .5s;
}

li{
    font-size: 120%;
}
a{
    text-decoration: none;
}
@media only screen and (max-width: 400px) {
    .row {
            padding-top: 20rem!important;
        }
}
@media only screen and (min-width: 400px) {
    .row {
            padding-top: 18rem!important;
        }
}
 @media only screen and (min-width: 500px) {
    .row {
            padding-top: 12rem!important;
        }
}
 @media only screen and (min-width: 600px) {
    .row {
            padding-top: 10rem!important;
        }
}
@media only screen and (min-width: 770px) {
    .row {
            padding-top: 5rem !important;
        }
}
@media only screen and (min-width: 992px) {
    .row {
            padding-top: 7rem!important;
        }
}  

</style>