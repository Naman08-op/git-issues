<template>
    <div id="issue"> 
        <div id="inputRepo">
            <div class="columns is-vcentered ">
            <div class="column is 1">
                <input  class="input is-link" type="text" placeholder="Owner's Name" required v-model="owner"/>
            </div>
            <div >
        <span class="tag is-info is-light is-large  ">/</span>
    </div>
    <div class="column is 1">
                <input  class="input is-link" type="text" placeholder="Repository Name" required v-model="repo"/>
            </div>
            <div>
                <button class="button is-link" @click="issue">Open Issues</button>
            </div>
    </div>
        </div> 
        
            
            {{weekcount}}
            <br><br><br>
        
    <div class="columns is-vcentered ">
    <div class="column is-5">
        <span class="tag is-primary is-large  ">Open {{opencount}}</span>
    </div>
    <div class="column is-5">
        <span class="tag is-warning is-large  ">This week {{weekcount[0]}}</span>
    </div>
    <div>
        <span class="tag is-danger is-large is-light">Closed {{closedcount}}</span>
    </div>
    </div>
        
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
            owner:null,
            repo:null,
            issues: [],
            opencount:null,
            open:null,
            closedcount:null,
            weekcount:[],
            startDate:0,
            endDate:7
            
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
        axios.get('https://api.github.com/search/issues?q=is:issue+is:closed+repo:'+this.owner+'/'+this.repo)
        .then((res)=>{
            console.log(res)
            this.closedcount=res.data.total_count
        })
     },
     weeklycount(){
         for(let i=0;i<=10;i++){
             
         axios.get('https://api.github.com/search/issues?q=is:issue+repo:'+this.owner+'/'+this.repo+"+created:" + moment().subtract(this.endDate, "days").format('YYYY-MM-DD') + ".." + moment().subtract(this.startDate, "days").format('YYYY-MM-DD'))
         .then((res)=>{
            this.weekcount.push(res.data.total_count)
            console.log("haha")
         })
         console.log("aaao")
         this.startDate+=7,
         this.endDate+=7
         console.log(this.startDate)
         console.log(this.endDate)
         console.log("pppp")
         }
     },
     issuetable(){
         axios.get('https://api.github.com/search/issues?q=is:issue+repo:'+this.owner+'/'+this.repo).
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
  #issue {
  margin: auto;
  margin-top: 3rem;
  max-width: 1250px;
}
#inputRepo{
    max-width: 1240px;
}
.icon {
  cursor: pointer;
}
</style>