<script setup>
import JobLIstCard from "./JobListCard.vue";
import { RouterLink } from "vue-router";
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'
// import jobData from "@/jobs.json";//
import { ref,defineProps ,onMounted, reactive} from "vue";
import axios from 'axios'

defineProps({
   limit:Number,
   showButton:{
      type:Boolean,
      default:false
   }
})

const state=reactive({
  jobs:[],
  isLoading:true
})

const jobs = ref([]);

onMounted(async()=>{
  try{
    const response=await axios.get('http://localhost:5000/jobs')
    state.jobs=response.data

  }catch(error){
    console.error('error fetching jobs',error)
  }finally{
    state.isLoading=false
  }
})


// console.log('titles',jobss)
console.log("jobs",jobs.value);
</script>
<template>
  <div>
    <section class="bg-blue-50 px-4 py-10">
      <div class="contianer-xl lg:container m-auto">
        <h2 class="text-3xl font-extrabold text-green-500 mb-8 text-center">
          Browse Jobs
        </h2>

        <!-- Show loading spinner while loading is ture -->

        <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
          <PulseLoader/>
        </div>

        <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
          <JobLIstCard v-for="job in state.jobs.slice(0,limit || state.jobs.length)" :key="job.id" :job="job"/>
          
        </div>
      </div>
    </section>
    <section v-if="showButton" 
    class="m-auto max-w-lg my-10 px-6 flex justify-center pb-3">
      <RouterLink to="/jobs" class="bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700">
         View All jobs
      </RouterLink>
    </section>

    <!-- <div>{{ jobs[0].title }}</div> -->
  </div>
  <!-- <div>{{ jobs[0].title }}</div> -->
</template>
