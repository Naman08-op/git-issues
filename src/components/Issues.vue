<template>
    <div> <button class="button is-link" @click="issue">Open Issues</button>
        Open Issues: {{opencount}}
        <p>Closed Issues: {{closedcount}}</p>
        <p>Week Issues: {{weekcount}}</p>
    </div>
</template>

<script>
import axios from "axios";
import moment from 'moment'
export default {
    name:"Issues",
    data(){
        return{
            issues: null,
            opencount:[],
            open:null,
            closedcount:[],
            weekcount:[]
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
        axios.get('https://api.github.com/search/issues?q=is:issue+is:open+repo:'+'octocat'+'/'+'hello-world')
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
     async issue(){
         
         this.openissues(),
         this.closedissues(),
         this.weeklycount()
     }
     }
}
</script>