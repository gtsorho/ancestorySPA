<template>
<div class="col-lg-4 col-12">
    <div>
        <img :src="memories[0].images" alt="" width="400" height="300">
    </div>
    
    <div class="row ms-2 mt-3" >
        <div class="col">
            <h2 class="d-inline ">{{new Date(this.dob).toDateString()}}</h2>
            <p class="text-muted mt-3 text-center">{{placeofBirth}}</p>
        </div>
        <hr class="col mt-5" > 
        <div class="col">
            <h2 class="d-inline ">{{new Date(this.dod).toDateString()}}</h2>
            <p class="text-muted mt-3 text-center">{{finalResidence}}</p>
        </div>
    </div>
</div>
</template>

<script>
export default {
    name: 'leftcol',
    props:{
        memories:Array,
        dod:String,
        dob:String,
        placeofBirth:String,
        finalResidence:String
    },
}

</script>

<style scoped>
img{
    width: 100%;
    height: 6.5in;
    object-fit: cover;
    border-bottom-right-radius: 10%;
    transition: font-size .25s linear; 
    -o-transition: font-size .25s linear; 
    -moz-transition: font-size .25s linear; 
    -webkit-transition: font-size .25s linear; 
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
hr:not([size]) {
    height: 2px;
}

@media screen and (max-width: 1050px) {
    img{
        height: 4.5in;
    }
} 
@media screen and (max-width: 993px) {
    img{
        height: auto;
    }
} 

</style>