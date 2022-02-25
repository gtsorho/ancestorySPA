<template>
    <Navbar routeName="Home" @isauthenticated="$emit('isauthenticated')" @search="searchFx" />
    <div>
        <div id="category" class="container mt-5 pt-5">
            <div class="row justify-content-center row-cols-2 row-cols-lg-5 g-5 g-lg-5">
                <div class="card card-product col p-0 mx-3" 
                    style="width: 20rem;" 
                    v-for="item in items" :key="item.id"
                    >
                    <router-link class="text-secondary" :to="{path: 'Home', query:{ancestor: JSON.stringify(item)}}" replace>
                        <img :src="item.memories[1].images" class="card-img-top" alt="...">
                        <div class="card-body">
                            <h5 class="card-title fw-bold">{{item.firstname +' '+ item.lastname +' '+ item.othernames }}</h5>
                            <hr>
                                <h4 class="text">{{item.hometown}}</h4>
                                <p class="text text-muted mb-0" style="font-size:12px">{{'From '+item.dob.split(' ')[0] +' to '+ item.dod.split(' ')[0]}}</p>
                        </div>
                    </router-link>
                </div>
            </div>
            <div class="alert alert-primary w-50 p-0 mx-auto" role="alert">
                <p class="fw-bold text-secondary fs-3 mb-0 text-center">{{items.length}} results found</p>
            </div>            
            <!-- <nav aria-label="Page navigation example">
            <ul class="pagination justify-content-center mt-5">
                <li class="page-item disabled">
                <a class="page-link">Previous</a>   
                </li>
                <li class="page-item"><a class="page-link" href="#">1</a></li>
                <li class="page-item"><a class="page-link" href="#">2</a></li>
                <li class="page-item"><a class="page-link" href="#">3</a></li>
                <li class="page-item">
                <a class="page-link" href="#">Next</a>
                </li>
            </ul>
            </nav> -->
        </div>

    </div>
</template>
<script>
import axios from 'axios'
import Navbar from "@/components/Navbar.vue";

export default {
    data() {
        return {
            items:[]
        }
    },
    components:{
        Navbar
    },
    emits:[
        'isauthenticated'
    ],
    beforeCreate: function() {
        document.body.className = 'index';
    },
    created(){
            // call index............................................
            axios.get('https://ancestryapi.herokuapp.com/api/index'
            ).then(response => {
                this.items = response.data 
                console.log(this.items)
            }).catch(error => {
                console.log(error);
            })
            // end of call index.....................................

    },
    methods:{
        searchFx(event){
            if(event == null){
                axios.get('https://ancestryapi.herokuapp.com/api/index'
                ).then(response => {
                    this.items = response.data 
                }).catch(error => {
                    console.log(error);
                })
            }
            this.items = event
        }

    }
}
</script>
<style scoped>
    .card-product a{
        padding: 0px !important;
        
    }
    .card-product{
        transition: transform .2s; /* Animation */
    }
    .card-product:hover {
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
        transform: scale(1.05);
    }
    a{
        font-weight: 500;
        display: block;
        padding: .5rem 1rem;
        /* color:  #198754 !important; */
        text-decoration: none !important;
        transition: color .15s;
    }
</style>