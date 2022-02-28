<template>
<form ref="form" class="row g-3">
    <div class="personal-info row" v-show="formStage1">
    <div class="col-md-4">
        <label for="Firstname" class="form-label">First Name</label>
        <input type="text" class="form-control form-control-sm " placeholder="John" v-model="bio.firstname" id="Firstname" />
    </div>
    <div class="col-md-4">
        <label for="lastname" class="form-label">Last Name</label>
        <input type="text" class="form-control form-control-sm" placeholder="Doe" v-model="bio.lastname" id="lastname" />
    </div>
    <div class="col-md-4">
        <label for="othernames" class="form-label">Other Names</label>
        <input type="text" class="form-control form-control-sm" placeholder="Mensah" v-model="bio.othernames" id="othernames" />
    </div>
    <div class="col-md-4">
        <label for="inputPassword4" class="form-label text-success">DoB</label>
        <input type="date" class="form-control form-control-sm" v-model="bio.dob" id="inputPassword2" />
    </div>
    <div class="col-md-4">
        <label for="inputPassword4" class="form-label text-danger">DoD</label>
        <input type="date" class="form-control form-control-sm" v-model="bio.dod" id="inputPassword4" />
    </div>
    <div class="col-12">
        <label for="inputAddress" class="form-label" >Place of Bith</label>
        <input
        v-model="bio.placeofBirth"
        type="text"
        class="form-control form-control-sm"
        id="inputAddress"
        placeholder="Assin"
        />
    </div>
    <div class="col-12">
        <label for="inputAddress2" class="form-label">Final Residence</label>
        <input
        v-model="bio.finalResidence"
        type="text"
        class="form-control form-control-sm"
        id="inputAddress2"
        placeholder="Apartment, studio, or floor"
        />
    </div>
    <div class="col-md-6">
        <label for="inputCity" class="form-label">HomeTown</label>
        <input type="text" class="form-control form-control-sm" v-model="bio.hometown" id="inputCity" placeholder="Assin-Fosu" />
    </div>
    <div class="col-md-6">
        <label for="FamilyName" class="form-label">Family Name</label>
        <input type="text" class="form-control form-control-sm" v-model="bio.FamilyName" id="FamilyName" placeholder="Doe" />
    </div>
    
    <div class="col-12 mt-4">
        <button class="btn btn-sm btn-primary float-end" @click="next">next</button>
        <button class="btn btn-sm btn-dark float-end mx-2" @click="previous">previous</button>
    </div>
    </div>
    <!-- occupation and terretries -->
    <span class="row" v-show="formStage2">
    <div class="terretires col-md-6">
        <h2>Territories</h2>
        <div
        class="input-group mb-3"
        v-for="(input, k) in territories"
        :key="k"
        >
        <input
            type="text"
            v-model="input.place"
            class="form-control form-control-sm"
            placeholder="Kasoa, Brekum etc."
            aria-label="Kasoa, Brekum etc."
            aria-describedby="basic-addon2"
        />
        <span
            class="input-group-text bg-danger text-light"
            @click="removeterritories(k)"
            v-show="k || (!k && territories.length > 1)"
            >-</span
        >
        <span
            class="input-group-text bg-success text-light"
            id="basic-addon2"
            @click="addterritories(k)"
            v-show="k == territories.length - 1"
            ><i class="bi bi-plus-lg"></i></span
        >
        </div>
    </div>
    <div class="terretires col-md-6">
        <h2>Occupation</h2>
        <div
        class="input-group mb-3"
        v-for="(input, k) in occupation"
        :key="k"
        >
        <input
            type="text"
            v-model="input.institution"
            class="form-control form-control-sm"
            placeholder="Sales Manager"
            aria-label="Sales Manager"
            aria-describedby="basic-addon2"
        />
        <span
            class="input-group-text bg-danger text-light"
            @click="removeoccupation(k)"
            v-show="k || (!k && occupation.length > 1)"
            >-</span
        >
        <span
            class="input-group-text bg-success text-light"
            id="basic-addon2"
            @click="addoccupation(k)"
            v-show="k == occupation.length - 1"
            ><i class="bi bi-plus-lg"></i></span
        >
        </div>
        <div class="col-12">
        <button class="btn btn-sm btn-primary float-end" @click="next">next</button>
        <button class="btn btn-sm btn-dark float-end mx-2" @click="previous">
            previous
        </button>
        </div>
    </div>
    </span>

    <span v-show="formStage3">
    <div class="form-floating">
        <textarea
        v-model="bio.biography"
        class="form-control form-control-sm"
        placeholder="Leave a Biography here"
        id="floatingTextarea2"
        style="height: 100px"
        ></textarea>
        <label for="floatingTextarea2">Biography</label>
        <div class="mt-2 col-md-6">
            <label for="cod" class="form-label">Cause of Death</label>
            <input type="text" class="form-control form-control-sm" v-model="bio.causeofDeath" id="cod" placeholder="sickness, accident etc" />
        </div>
    </div>
    <div class="mt-2">
        <button class="btn btn-sm btn-primary float-end" @click="next">next</button>
        <button class="btn btn-sm btn-dark float-end mx-2" @click="previous">previous</button>
    </div>
    </span>

    <!-- Relations -->
    <span class="row"  v-show="formStage4">
        <div class="terretires col-md-12">
        <h2>Reletives and Relations</h2>
        <div class="input-group mb-3" v-for="(input, k) in relations" :key="k">
        <input
            type="text"
            v-model="input.relativeName"
            class="form-control form-control-sm me-2"
            placeholder="Full Name"
            aria-label="Recipient's username"
            aria-describedby="basic-addon2"
        />
        <select class="form-select ms-3" aria-label="Default select example" v-model="input.relativeRelation">
            <option value="" disabled selected>Open this select menu</option>
            <option value="Father">Father</option>
            <option value="Mother">Mother</option>
            <option value="Sister">Sister</option>
            <option value="Uncle">Uncle</option>
            <option value="Brother">Brother</option>
            <option value="Aunty">Aunty</option>
            <option value="Spouse">Spouse</option>
            <option value="Child">Child</option>
            <option value="Grandparent">Grandparent</option>
        </select>
        <span class="input-group-text bg-danger text-light" @click="removeRelation(k)" v-show="k || (!k && relations.length > 1)" >-</span>
        <span class="input-group-text bg-success text-light" id="basic-addon2" @click="addRelation(k)" v-show="k == relations.length - 1"><i class="bi bi-plus-lg"></i></span>
        </div>
    </div>
        <div class="mt-2">
        <button class="btn btn-sm btn-primary float-end" @click="next">next</button>
        <button class="btn btn-sm btn-dark float-end mx-2" @click="previous">previous</button>
    </div>
    </span>

    <span v-show="formStage5">
        <div class="mb-3">
            <h3 class="">Profile and Memories</h3>
            <i class="bi text-warning fs-5 bi-exclamation"></i>
            <label for="formFileSm" class=" form-label" style="color: #90610e;">First Image will be applyed to profile image</label>
            <input
                class="form-control form-control-sm form-control form-control-sm-sm"
                @change="onFileChange"
                id="formFileSm"
                ref="importfile"
                type="file"
                multiple
            />
        </div>
            <div class="">
                <div class="input-group mb-3">
                    <input type="text" v-model="imageId" style="max-width:10%" class="form-control form-control-sm" disabled placeholder="2" aria-label="No#">
                    <span class="input-group-text">@</span>
                    <input type="text" class="form-control form-control-sm" v-model="imageDescription"  placeholder="please select an image and add description" aria-label="description">
                    <span class="input-group-text bg-success" @click="addDescription "><i class="bi bi-plus-lg"></i></span>
                </div>
            </div>
            <div class="">
                <div class="input-group flex-nowrap">
                    <input type="url" class="form-control form-control-sm" v-model="videoLink" placeholder="video link" aria-label="video link" aria-describedby="addon-wrapping">
                    <span class="input-group-text bg-secondary" @click="bio.links.push(videoLink); videoLink = '' " id="addon-wrapping"><i class="bi bi-link-45deg"></i></span>
                </div>
            </div>

            <div class="btn btn-sm link-btn me-1 mb-3" style="width:fit-content" role="alert"  v-for="(link, i) in bio.links" :key="i">
                <a class="me-2 text-decoration-none" :href="link">{{link}}</a>
                <p class="bi bi-x-circle text-dark" @click="bio.links.splice(i,1);" ></p>
            </div>
        <div class="row row-cols-4" v-if="bio.url">
            <div class="card col mb-2 me-3" style="width: 18rem; padding-right: 0px; padding-left: 0px; height: fit-content;" v-for="(image, i) in bio.url" :key="i" @click="imageId = i+1">
                <img :src="image.image" width="400" height="300" />
                <div class="card-body" v-if="image.description">
                    <h5 class="card-title" >Description</h5>
                    <hr class="w-25">
                    <p class="card-text">{{image.description}}</p>
                </div>
            </div>
        </div>

        <!-- <div class="row row-cols-4" v-if="bio.url">
            <div class="col mb-2 hover-shadow" v-for="(image, i) in bio.url" :key="i" @click="imageId = i+1">
                <img :src="image.image" width="400" height="300" />
                <p>{{image.description}}</p>
            </div>
        </div> -->
        <div class="mt-2">
            <p class="text-success fs-6 float-start mx-auto" v-show="responseMsg">Your process was Successful</p>
            <p class="text-danger fs-6 float-start mx-auto" v-show="errorMsg">{{error}}</p>
            <button class="btn btn-sm btn-success float-end" v-show="!dataToEdit" @click="addBio">submit</button>
            <button class="btn btn-sm btn-success float-end" v-show="dataToEdit" @click.prevent="addBio">Update</button>
            <button class="btn btn-sm btn-danger float-end mx-2" @click="previous">previous</button>
        </div>
    </span>
</form>
</template>

<script>
import axios from 'axios'
import { parseJSON } from 'jquery';

export default {
emits:[
    'progress',
    'borderstatus'
],
props:[
    'dataToEdit'
],
data() {
    return {
        bio:
            {
                firstname: null,
                lastname: null,
                othernames: null,
                dod: null,
                dob: null,
                placeofBirth: null,
                hometown: null,
                finalResidence:null,
                FamilyName:null,
                url: [],
                territories: [],
                occupation: [],
                bioRelation: {},
                biography: null,
                causeofDeath:null,
                links:[]
            }
        ,
        errorMsg:false,
        error:null,
        responseMsg:false,
        postFormData: new FormData(),
        files: [],
        imageId:'',
        imageDescription:'',
        videoLink:null,
        progress: 0,
        formStage: 1,
        formStage1: false,
        formStage2: false,
        formStage3: false,
        formStage4: false,
        formStage5: false,

        territories: [
            {
                place: "",
            },
        ],
        occupation: [
            {
                institution: "",
            },
        ],
        relations: [
            {
                relativeName: null,
                relativeRelation:null
            },
        ],
    };
},
    mounted() {
        this.formStage1 = true;

    },
    watch:{
        dataToEdit(newVal, oldVal){
            // console.log('Prop changed: ', newVal, ' | was: ', oldVal)
            this.bio = 
                {
                    firstname: newVal.firstname,
                    lastname: newVal.lastname,
                    othernames: newVal.othernames,
                    dod: newVal.dod.split(' ')[0],
                    dob: newVal.dob.split(' ')[0],
                    placeofBirth: newVal.placeofBirth,
                    url:[],
                    territories: [],
                    occupation: [],
                    bioRelation: {},
                    links:[],   
                    hometown: newVal.hometown,
                    finalResidence:newVal.finalResidence,
                    FamilyName:newVal.FamilyName,
                    biography: newVal.biography,
                    causeofDeath:newVal.causeofDeath,
                }
            if(newVal.links){
                this.links = []
                JSON.parse(newVal.links).forEach(item => {
                this.links.push(item);
            });
            }
            this.territories =  []
            JSON.parse(newVal.territories).forEach(item => {
                this.territories.push({'place':item});
            });
            this.occupation =  []
            JSON.parse(newVal.occupation).forEach(item => {
                this.occupation.push({'institution':item});
            });
            this.relations =  []
            newVal.relations.forEach(item => {
                this.relations.push({'relativeName':item.relativeName, 'relativeRelation':item.relation });
            });
            newVal.memories.forEach(item => {
                this.bio.url.push({'image':item.images, 'description':item.imageDescription });
            });
        }
    },
    methods: {
        addterritories() {
        this.territories.push({});
        },

        removeterritories(index) {
        this.territories.splice(index, 1);
        },

        addoccupation() {
        this.occupation.push({});
        },

        removeoccupation(index) {
        this.occupation.splice(index, 1);
        },

        addRelation() {
        this.relations.push({});
        },

        removeRelation(index) {
        this.relations.splice(index, 1);
        },

        addDescription(){
            var size = this.bio.url.length;
            for(var i=0; i < size; i++){
                if(i == this.imageId-1){
                    this.bio.url[i].description = this.imageDescription 
                }
            }
            this.imageId = null
            this.imageDescription = null
        },

        addBio (e) {
            e.preventDefault();
            this.territories.forEach(item => {
                this.bio.territories.push(item.place)
            });
            this.occupation.forEach(item => {
                this.bio.occupation.push(item.institution)
            });
            this.relations.forEach(item => {
                this.bio.bioRelation[item.relativeName] = item.relativeRelation;
            });
            var token = localStorage.getItem('token');
            console.log(this.dataToEdit)
            if(this.dataToEdit){
                var urloption = `http://127.0.0.1:8000/api/update/${this.dataToEdit.id}`
            }else{
                var urloption = `http://127.0.0.1:8000/api/create`
            }
                console.log(urloption)

            axios.post(urloption, this.bio,
            {
                headers:{'Authorization': `Bearer ${token}`}
            }).then(response =>  {
                console.log(response.data)
                this.postFormData.append('newUser', response.data.ancestor_id);
                if(!this.files.length == 0){
                    console.log(this.files)
                    axios.post('http://127.0.0.1:8000/api/storeimg', this.postFormData,
                    {
                        headers:{'Authorization': `Bearer ${token}`}
                    })
                    .then(response =>  {
                        console.log(response)
                        this.$emit('borderstatus')   
                        // this.bio =   
                        //     {
                        //         firstname: null,
                        //         lastname: null,
                        //         othernames: null,
                        //         dod: null,
                        //         dob: null,
                        //         placeofBirth: null,
                        //         hometown: null,
                        //         finalResidence:null,
                        //         FamilyName:null,
                        //         url: [],
                        //         territories: [],
                        //         occupation: [],
                        //         bioRelation: {},
                        //         biography: null,
                        //         causeofDeath:null,
                        //         links:[]
                        //     }
                        //     this.territories =  [
                        //             {
                        //                 place: "",
                        //             },
                        //         ]
                        //     this.occupation = [
                        //             {
                        //                 institution: "",
                        //             },
                        //         ]
                        //     this.relations= [
                        //             {
                        //                 relativeName: null,
                        //                 relativeRelation:null
                        //             },
                        //         ]
                        //     this.responseMsg = true
                        //     this.postFormData = new FormData(),
                        //     this.$refs.importfile.type = 'text'
                        //     this.$refs.importfile.type = 'file'

                            // this.formStage1 = true;
                            // this.formStage2 = false;
                            // this.formStage3 = false;
                            // this.formStage4 = false;
                            // this.formStage5 = false;
                    }).catch(error => {
                        console.log(error.response.data.message);
                        this.errorMsg = true
                        this.error = error.response.data.message
                    })
                }
                else{
                    this.$emit('borderstatus')   
                }
            }).catch(error => {
                    console.log(error.response.data.message);
                    this.errorMsg = true
                    this.error = error.response.data.message
                                            this.territories =  [
                                    {
                                        place: "",
                                    },
                                ]
                            this.occupation = [
                                    {
                                        institution: "",
                                    },
                                ]
                            this.relations= [
                                    {
                                        relativeName: null,
                                        relativeRelation:null
                                    },
                                ]
                            this.responseMsg = true
                            this.postFormData = new FormData(),
                            this.$refs.importfile.type = 'text'
                            this.$refs.importfile.type = 'file'
                })

            }, 
        next(e) {
        e.preventDefault();
        this.formStage++;
        if (typeof eval(`this.formStage${this.formStage}`) !== "undefined") {
            this.formStage1 = false;
            this.formStage2 = false;
            this.formStage3 = false;
            this.formStage4 = false;
            eval(`this.formStage${this.formStage} = true`);
            this.$emit("progress", this.formStage);
        } else {
            this.formStage--;
        }
        },
        previous(e) {
        e.preventDefault();
        if (this.formStage > 1) {
            this.formStage--;
            if (typeof eval(`this.formStage${this.formStage}`) !== "undefined") {
            this.formStage1 = false;
            this.formStage2 = false;
            this.formStage3 = false;
            this.formStage4 = false;
            this.formStage5 = false;
            eval(`this.formStage${this.formStage} = true`);
            this.$emit("progress", this.formStage);
            }
        }
        },
        // *****************************************************************************
        onFileChange(e) {
            this.bio.url = []
            this.files = []
            const file = e.target.files;
            for (var i = 0; i < file.length; i++) {
                this.files.push(file[i]);
                this.bio.url.push({image : URL.createObjectURL(file[i])});
                this.postFormData.append('images[]',file[i]);
            }
        },
    },
};
</script>

<style scoped>
    @import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.7.2/font/bootstrap-icons.css");
        .input-group-text {
    cursor: pointer;
    }
    label{
        font-size: smaller;
    }
    .cursor {
        cursor: pointer;
    }
    img {
        width: 100%;
        /* height: 4.5in; */
        object-fit: cover;
        transition: font-size 0.25s linear;
        -o-transition: font-size 0.25s linear;
        -moz-transition: font-size 0.25s linear;
        -webkit-transition: font-size 0.25s linear;
    }
    .link-btn {
    color: #fff !important;
    background-color: #cdcdcdb0 !important;
    border-color: #bababa !important;
    }
    .link-btn:hover {
        color: #fff !important;
        background-color: #b5b5b5 !important;
        border-color: #878787 !important;
    }
    .bi-x-circle{
        transition: transform .2s; /* Animation */
        display: inherit;
        margin-bottom: 0px;
    }
    .bi-x-circle:hover{
        color: red !important;
          transform: scale(1.5); /* (150% zoom - Note: if the zoom is too large, it will go outside of the viewport) */
    }
    .card:hover {
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
    }
    .input-group-text:hover{
        opacity: .9;
    }
</style>