<template>
    <div id="fetchData">
        <input class="searchbar" v-model="search" type="text" placeholder="Search by name">
        <div v-for="students in searchResult" :key="students.index" >
            <div class="card">
                <img :src="students.pic" alt="Image" class="rounded-0" />
                <span id="cardtext">
                    <h1>{{ students.firstName.toUpperCase() }} {{ students.lastName.toUpperCase() }}</h1>
                    <button type="button" class="dropdown" v-on:click="hidden=!hidden">+</button>
                    <div id="student-info">
                        Email: {{ students.email }}
                        <br />Company: {{ students.company }}
                        <br />Skill: {{ students.skill }}
                        <br />Average: {{ findingAverage(students.grades) }}%   
                        <div class="collapsible" :hidden="hidden">
                            fddsfdsafds
                        </div>
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
            hidden: true
        }
    },
    methods:{
        findingAverage(grades){
            let sum = 0;
            for(let i=0; i <grades.length; i++){
                sum = sum + parseFloat(grades[i]);
            }
            return this.average = sum/grades.length;
        }
    },
    computed: {
        searchResult() {
            let tempStudents = this.students
            
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
                    
                    })
                }
                return tempStudents
            }
    },
    mounted() {
        axios
            .get("https://api.hatchways.io/assessment/students")
            .then(response => {
                this.students = response.data.students;
            });
    },
}
</script>

<style>
    ::-webkit-scrollbar {
        width: 0;  /* Remove scrollbar space */
        background: transparent;  /* Optional: just make scrollbar invisible */
    }
    .dropdown{
        background-color: #fff;
        font-size: 16px;
        height: 2.5em;
        width: 2.5em;
        position: relative;
        border: none;
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