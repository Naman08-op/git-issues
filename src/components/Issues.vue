<template>
    <div> <button class="button is-link" @click="issue">Open Issues</button>
        Open Issues: {{opencount}}
        <p>Closed Issues: {{closedcount}}</p>
        <p>Week Issues: {{weekcount}}</p>
        <p>{{issue.number}}</p>
        <div class="container">
            <div class="table-container">
                <table class="table is-bordered is-striped is-hoverable is-fullwidth">
                    <thead>
                        <tr>
                            <th>Issue no.</th>
                            <th>Title</th>
                            <th>State</th>
                            <th>Creation Date</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="issue in issues" v-bind:key="issue.id">
                            <td>{{issue.number}}</td>
                            <td><a v-bind:href=issue.html_url v-bind:title=issue.title>{{issue.title}}</a></td>
                            <td>{{issue.state}}</td>
                            <td>{{issue.created_at | formatDate}}</td>

                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import moment from 'moment';
export default {
    name:"Issues",
    data(){
        return{
            issues: [],
            opencount:[],
            open:null,
            closedcount:[],
            weekcount:[],
            
        }
       
    },
    methods: 
        {openissues(){
        //  axios.get("https://api.github.com/repos/ccgus/fmdb/issues")
        //  .then((res)=>
        //  this.opencount=res.data.filter(element=>element.state=="open").length,
        //  //this.closedcount=res.data.filter(element=>element.state=="closed").length,
        //  console.log(this.opencount)
        //  )
        axios.get('https://api.github.com/search/issues?q=is:issue+is:open+repo:'+'khronosgroup'+'/'+'webgl')
        .then((res)=>{
            console.log(res)
            this.opencount=res.data.total_count
        })
     },
     closedissues(){
        //  axios.get("https://api.github.com/repos/ccgus/fmdb/issues")
        //  .then((res)=>{
        //     this.closedcount=res.data.filter(element=>element.state=="closed").length,
        //  console.log(this.closedcount)
        //  })
        axios.get('https://api.github.com/search/issues?q=is:issue+is:closed+repo:'+'khronosgroup'+'/'+'webgl')
        .then((res)=>{
            console.log(res)
            this.closedcount=res.data.total_count
        })
     },
     weeklycount(){
         axios.get('https://api.github.com/search/issues?q=is:issue+repo:'+'khronosgroup'+'/'+'webgl'+"+created:" + moment().subtract(7, "days").format('YYYY-MM-DD') + ".." + moment().subtract(24, "hours").format('YYYY-MM-DD'))
         .then((res)=>{
             this.weekcount=res.data.total_count
         })
     },
     issuetable(){
         axios.get('https://api.github.com/search/issues?q=is:issue+repo:'+'khronosgroup'+'/'+'webgl').
         then((res)=>{
             this.issues=res.data.items
         })
     },
     
     async issue(){
         
         this.openissues(),
         this.closedissues(),
         this.weeklycount(),
         this.issuetable()
     }
     }
}
</script>

<style>
  #list {
  margin: auto;
  margin-top: 3rem;
  max-width: 700px;
}
.icon {
  cursor: pointer;
}
</style>