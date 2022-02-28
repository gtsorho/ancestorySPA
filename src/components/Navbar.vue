<template>
<nav class="navbar navbar-light bg-light">
    <a class="navbar-brand" href="#">
        <img :src="logo" alt="" width="30" height="30" class="d-inline-block ms-2 mb-1">
        <router-link to="/"><h4 class="d-inline-block ms-2 mt-2 text-secondary"  id="navel3">{{brandName}}</h4> </router-link>
    </a>
    <form class="form-inline d-flex me-3">
        <div class="input-group flex-nowrap" id="navel1">
            <button class="btn btn-sm btn-success " v-show="page" type="button"  data-bs-toggle="modal" data-bs-target="#exampleModal" @click="addAncestor">Add/Edit</button>
            <input type="text" class="form-control form-control-sm" id="navel2" v-show="searchOption" v-model="searchValue" placeholder="Family/Lastname | Hometown" @keyup="search" aria-label="Username" aria-describedby="addon-wrapping">

            <i class="bi bi-search input-group-text " id="addon-wrapping" v-show="searchOption" ></i>
            <i class=" btn btn-sm bi bi-search input-group-text rounded-circle" id="addon-wrapping"  @click="toast" v-show="!searchOption"></i>
            <button class="btn btn-sm btn-danger rounded-circle ms-2" v-show="token"  @click.prevent="logout" > <i class="bi bi-door-open"></i></button>
            
            <!-- <i class="bi bi-door-open input-group-text ms-2 bg-warning rounded-circle" v-show="token" id="addon-wrapping"></i> -->
        </div>
    </form>
</nav>

<div class="position-fixed top-0 start-50 translate-middle-x p-3" style="z-index: 11">
    <div id="liveToast" class="toast" role="alert"  style="width:450px" aria-live="assertive" aria-atomic="true" data-bs-autohide="false">
        <div class="toast-header">
            <input type="text" class="form-control form-control-sm " v-model="searchValue" placeholder="Family/Lastname | Hometown" @keyup="adminsearch">
        <button type="button" class="btn-close text-danger me-1" @click="toast_bool=false" data-bs-dismiss="toast" aria-label="Close"></button>
        </div>
        <div class="toast-body">
            <ul class="list-group list-group-flush">
                <li class="list-group-item bg-transparent fw-bold"  
                v-for="(value, i) in adminsearchvalue"
                :key="i">{{value.firstname +' '+ value.lastname}}
                <a href="#" @click="$emit('editdata',value); toastclose()" ><i class=" bi bi-pencil-square text-primary  fs-5 float-end"></i></a>
                <a href="#" @click="deleteData(value.id, i)"><i class="bi bi-trash-fill fs-5 me-2 text-danger float-end"></i></a>
                </li>
                <p class="text-muted float-end">{{adminsearchvalue ? adminsearchvalue.length : ''}} results found</p>
            </ul>
        </div>
    </div>
</div>

<div id="overlay" v-show="toast_bool" ></div>
</template>

<script>
import {Toast} from 'bootstrap'
import axios from 'axios';
import { reactive } from '@vue/reactivity';

export default {
    name: "Navbar",
    props:[
        'routeName'
    ],
    emits:[
        'isauthenticated',
        'search',
        'editdata'
    ],
    data() {
        return {
            logo: require('@/assets/logo2.png'), 
            brandName: 'AncestryGH',
            page: true,
            searchValue:null,
            adminsearchvalue:null,
            searchOption:true,
            toast_bool:false
        }
    },
    computed:{
            token(){
                return localStorage.getItem('token');
            }
    },
    // beforeMount(){
    //         console.log(localStorage.getItem('token'))
    //         if(!localStorage.getItem('token')){
    //             this.$router.push('/')
    //         }
    // },
    created(){
            if(this.routeName == 'Registration'){
                this.page = false
                setTimeout(() => {
                    this.searchOption = false
                    document.getElementById("navel1").classList.add("about");
                    document.getElementById("navel2").classList.add("about");
                    document.getElementById("navel3").classList.remove("text-dark");
                    document.getElementById("navel3").classList.add("text-light"); 
                }, 500); 
            }else{
                this.page = true
            }
            if(this.routeName == 'index'){
                setTimeout(() => {
                    document.getElementById("navel1").classList.add("about");
                    document.getElementById("navel2").classList.add("about");
                    document.getElementById("navel3").classList.remove("text-dark");
                    document.getElementById("navel3").classList.add("text-light"); 
                }, 500); 
            }
    },
    methods:{
        search(){
            console.log(this.searchValue)
            if(this.searchValue){
                axios.post(`http://127.0.0.1:8000/api/search/${this.searchValue}`
                ).then(response => {
                    console.log(response.data)
                    this.$emit('search', response.data)
                    this.$router.push({ name: 'index', params: { searchData: response.data }})
                }).catch(error => {
                    console.log(error);
                })
            }else{
                this.$emit('search', null)
            }
        },
        adminsearch(){
            console.log(this.searchValue)
            var token = localStorage.getItem('token');
            axios.get(`http://127.0.0.1:8000/api/adminlookup/${this.searchValue}`,
            {
                headers:{'Authorization': `Bearer ${token}`}
            }).then(response => {
                console.log(response);
                this.adminsearchvalue = response.data
            }).catch(error => {
                console.log(error);
            })
        },
        addAncestor(){
            var token = localStorage.getItem('token');
            if(token){
                this.$emit('isauthenticated')
                this.$router.push('/registration')
            }
        },
        logout(){
            console.log('response');
            var token = localStorage.getItem('token');
            axios.get('http://127.0.0.1:8000/api/logout',
            {
                headers:{'Authorization': `Bearer ${token}`}
            }
            ).then(response => {
                console.log(response);
                localStorage.removeItem('token');
                this.$router.push('/')
            }).catch(error => {
                console.log(error);
            })
        },
        deleteData(id,i){
            // console.log(id,i)
            var token = localStorage.getItem('token');
            if(this.searchValue){
                axios.get(`http://127.0.0.1:8000/api/delete/${id}`,
                {
                headers:{'Authorization': `Bearer ${token}`}
                }).then(response => {
                    console.log(response.data)
                    this.adminsearchvalue.splice(i, 1);
                }).catch(error => {
                    console.log(error);
                })
            }else{
                this.$emit('search', null)
            }
        },
        toast(){
            this.toast_bool = true
            var toastLiveExample = document.getElementById('liveToast')
            var toast = new Toast(toastLiveExample)
            toast.show()
        },
        toastclose(){
            this.toast_bool = false
            var toastLiveExample = document.getElementById('liveToast')
            var toast = new Toast(toastLiveExample)
            toast.hide()
        }   
    }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

#overlay {
  position: fixed; /* Sit on top of the page content */
  width: 100%; /* Full width (cover the whole page) */
  height: 100%; /* Full height (cover the whole page) */
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0,0,0,0.5); /* Black background with opacity */
  z-index: 2; /* Specify a stack order in case you're using a different order for other elements */
  cursor: pointer; /* Add a pointer on hover */
}

@import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.7.2/font/bootstrap-icons.css");

.bg-light {
    --bs-bg-opacity: .0 !important;
}
.form-control{
    background-color: #ffffff9c;
    transition: 4ms linear; 
}
.input-group-text{
    background-color: #ffffff9c;
    border: 0px #ced4da;
}
.form-control:focus{
    /* color: #212529; */
    background-color: #ffffffb7; 
    border-color:unset;
    outline: 0;
    
    box-shadow: 0 0 0 0.15rem rgb(13 110 253 / 25%);
}
.list-group-item:hover{
    background-color: #9191913b !important;
}
.bi:hover{
    opacity: .6;
}

</style>
