<template>

   <div>
        <table  class="table table-dark">
            <tr v-for="item in students"  v-bind:key="item._id"> 
                <td><router-link v-bind:to="'/student-info/'+item._id"> {{item.name}}</router-link></td><td><input type="checkbox" v-model="item.isDonePr"></td><td>{{item.group}}</td><td>{{ item.mark}}</td>
                <td><a href = "#" v-on:click.prevent = "removeStud(item._id)">Удалить</a></td>
                <td v-if = "item.name != search"><a href = "#" @click="changeVal(item.name)">Изменить</a></td>
                <td v-if = "item.name == search">
                    <a href = "#" @click="changeStud(item._id)">Изменино</a>
                </td>
                
            </tr>
            
        </table>
        
        
        <div class = "search">
            <input type="text" v-model = "student.name">
            <select v-model="student.group">
                 <option>Group</option>
                <option value="RPZ 17 1/9">РПЗ 17 1/9</option>
                <option value="RPZ 17 2/9">РПЗ 17 2/9</option>
            </select>
            <input type="number" v-model = "student.mark">
            MARK:<input type = "checkbox" v-model = "student.isDonePr"> 
            <button v-on:click="studAdd()">Add</button>
        </div> 
         
   </div>
</template>

<script>
    import Vue from 'vue'
    import axios from 'axios'
    import VueAxios from 'vue-axios'
 
    export default {
          props: {
            id:'',
        },
       data: function() {
           return {
                search:"",
                students: [],
                student: {name:"", group:"", mark:"", isDonePr:""},
           };
        },

        mounted: async function(){      
           
            let response = await Vue.axios.get("http://46.101.212.195:3000/students");
            this.students = response.data;
 
        },
        
                        computed: {
                },

        methods: {
            removeStud: function(id){
                Vue.axios.delete("http://46.101.212.195:3000/students/" + id)
                .then((response) => {
                    console.log(response.data)
                    this.resp();
                })
            },
            changeVal: function(name){
                this.student = this.students.find(elem => {
                    if(elem.name == name){
                        return elem
                    }
                });
                this.search = name
            },
           changeStud: function(id){
                Vue.axios.put("http://46.101.212.195:3000/students/" + id,{
                    name: this.student.name,
                    group: this.student.group,
                    mark: this.student.mark,
                    isDonePr: this.student.isDonePr
                })
                .then((response) => {
                    console.log(response.data)
                    this.resp();
                })
            },
            studAdd: function(){
                Vue.axios.post("http://46.101.212.195:3000/students",{
                    name: this.student.name,
                    group: this.student.group,
                    mark: this.student.mark,
                    isDonePr: this.student.isDonePr
                })
                
                .then((response) => {
                    console.log(response.data)
                    this.resp();
                })
            },
            resp: function(){
                Vue.axios.get("http://46.101.212.195:3000/students").then((response) => {
                console.log(response.data)
                this.students = response.data;

                })
            }
        }
    }
    
</script>
<style scoped>

</style>


