<template>
<div style="padding:2%" >
    <vueper-slides
        :infinite="true"
        :bullets="false"
        autoplay
        class="no-shadow"
        :visible-slides="4"
        :gap="2"
        :slide-ratio="1 / 4"
        :dragging-distance="70">
    <vueper-slide v-for="(slide, i) in slides" :key="i"  :image="slide.images" /></vueper-slides>
</div>
</template>

<script>
import { VueperSlides, VueperSlide } from 'vueperslides'
import 'vueperslides/dist/vueperslides.css'

export default {
    name: "Memoryslider",
    components: { 
        VueperSlides,
        VueperSlide 
    },
    props:[
        'memories'    
    ],
    data() {
        return {
            pauseOnHover: true,
            autoPlaying: true,
            internalAutoPlaying: true,
            slides: this.memories
        }
    },
    created(){
        console.log(this.memories)
    }
}
</script>
<style scoped>
img{
    border-radius: 5%;
}
.vueperslide{
    border-radius: 10px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);

}

</style>
