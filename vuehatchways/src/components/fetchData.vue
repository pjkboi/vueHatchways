<template>
    <div id="fetchData">
        <input class="searchbar" v-model="search" type="text" placeholder="Search by name">
        <input class="searchbar" v-model="tagSearch" type="text" placeholder="Search by tag">
        <div v-for="students in searchResult" :key="students.index" >
            <div class="card">
                <img :src="students.pic" alt="Image" class="rounded-0" />
                <span id="cardtext">
                    <span id="title">
                        <h1>{{ students.firstName.toUpperCase() }} {{ students.lastName.toUpperCase() }}</h1>
                        <button type="button" class="dropdown" @click="students.hidden=!students.hidden"></button>
                    </span>
                    
                    <div id="student-info">
                        Email: {{ students.email }}
                        <br />Company: {{ students.company }}
                        <br />Skill: {{ students.skill }}
                        <br />Average: {{ findingAverage(students.grades) }}%   
                        <div class="collapsible" :hidden="students.hidden">
                            <div v-for="grade in students.grades" :key="grade.index">
                                Test {{ students.grades.indexOf(grade)+1 }}: {{ grade }} 
                            </div>
                        </div>
                        <div v-if="students.tagArray.length !== 0" >
                            <span v-for="tag in students.tagArray" :key="tag.index"> 
                                <button class="tag">{{ tag }}</button>
                            </span>
                        </div>
                        <form @submit.prevent="addTag(students.id)">
                            <input class="searchbar" type="text" placeholder="Add a tag" v-model="students.tag">
                        </form>
                    </div>
                </span>
            </div>
            <hr />
        </div>
    </div>
    
</template>

<script>
import axios from 'axios';
export default {
    name: 'fetchData',
    data(){
        return{
            students: [],
            average: 0,
            search: '',
            hidden: true,
            tag: '',
            tagSearch: '',
        }
    },
    methods:{
        findingAverage(grades){
            let sum = 0;
            for(let i=0; i <grades.length; i++){
                sum = sum + parseFloat(grades[i]);
            }
            return this.average = sum/grades.length;
        },
        addTag(id){
            this.students[id-1].tagArray.push(this.students[id-1].tag);
            this.students[id-1].tag = '';
            
        }   
    },
    computed: {
        searchResult() {
            let tempStudents = this.students;
            if (this.search != '' && this.search) {
                tempStudents = tempStudents.filter((item) => {
                    var firstName = item.firstName
                        .toUpperCase()
                        .includes(this.search.toUpperCase());
                    var lastName = item.lastName
                        .toUpperCase()
                        .includes(this.search.toUpperCase());
                    if(firstName){
                        return firstName;
                    }
                    if(lastName){
                        return lastName;
                    }
                
                });
            }
            if(this.tagSearch != '' && this.tagSearch){
                tempStudents = tempStudents.filter((item) => {
                    var tag = item.tagArray.includes(this.tagSearch);
                    if(tag){
                        return tag;
                    }
                });
            }
            return tempStudents;
        }
    },
    mounted() {
        axios
            .get("https://api.hatchways.io/assessment/students")
            .then(response => {
                this.students = response.data.students;
                for(var i=0; i<this.students.length; i++){
                    this.students[i].tag = '';
                    this.students[i].tagArray = [];
                    this.students[i].hidden = true;
                }
            });
        
        
    },
}
</script>

<style>
    ::-webkit-scrollbar {
        width: 0;  /* Remove scrollbar space */
        background: transparent;  /* Optional: just make scrollbar invisible */
    }
    #title{
        display: flex;
    }
    .tag{
        margin-top: 0.5em;
        background-color: lightgrey;
        padding: 1em;
        border: none;
        border-radius: 5px;

    }
    .dropdown{
        display: flex;
        margin-left: auto;
        margin-right: 1em;
        color: black;
        padding: 0px;
        width: 0%;
        border: none;
        text-align: right;
        outline: none;
        font-size: 30px;
    }
    .dropdown:after {
        content: '\002B';
        color: black;
        font-weight: bold;
    }
    
    .collapsible {
        overflow: hidden;
    }
    .searchbar{
        width: 90%;
        height: 40px;
        border: none;
        border-bottom: 2px solid #edeff2;
        outline: none;
    }
    #fetchData{
        border-radius: 5px;
        box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
        height:100%;
        max-height: 600px;
        width: 100%;
        overflow-y: scroll;
        background-color: white;
    }
    h1{
        font-family: 'Raleway-Black';
        padding-bottom: 0%;
        margin-top: 0%;
        margin-bottom: 0.1em;
    }
    .card {
        display: flex;
        padding-top: 1em;
        padding-left: 2em;
        padding: 1em;
    }
    img {
        display: block;
        width: 100px;
        height: 100px;
        border-radius: 50%;
        border: 2px solid #edeff2;
    }
    #cardtext {
        flex-direction: column;
        padding: 0;
        width:100%;
    }
    #student-info{
        text-align: left;
        padding-left: 2em;
        padding-top: 0.5em;
    }
    hr {
        border: 1px solid #edeff2;
    }
</style>