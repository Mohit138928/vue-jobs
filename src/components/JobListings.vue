<script setup>
import JobListing from "./JobListing.vue";
// import jobData from "@/jobs.json";
import { ref, defineProps , onMounted} from "vue";
import { RouterLink } from "vue-router";
import axios from "axios";

defineProps({
    limit: Number,
    showButton : {
        type: Boolean,
    }
})

const jobs = ref([]);
console.log(jobs.value);

onMounted(async()=>{
    try {
        const response = await axios.get("/api/jobs");
        jobs.value = response.data;
    } catch (error) {
        console.error(error);
        
    }
})

</script>

<template>
    <section class="bg-blue-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-center text-3xl font-bold text-green-500 mb-6"> Browse Jobs</h2>
            <div class="grid grid-cols-1 gap-6 md:grid-cols-3">
                <JobListing v-for="job in jobs.slice(0, limit || jobs.length)" :key="job.id" :job="job"/>
            </div>
        </div>
    </section>
    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
      <RouterLink
        to="/jobs"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
        >View All Jobs</RouterLink
      >
    </section>
    
</template>