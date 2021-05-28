<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperiences"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading....</p>
      <p v-if="NoDataFound">No Data Found! Start Adding some survey results first!!!</p>
      <ul v-else>
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
        <p v-if="isError">{{ error }}</p>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  // props: ['results'],
  mounted() {
    this.loadExperiences();
  },
  components: {
    SurveyResult,
  },
  data() {
    return {
      results:[],
      isLoading:false,
      NoDataFound:false,
      error:null,
      isError:false
    }
  },
  methods: {
    loadExperiences(){
      this.isLoading= true
      this.NoDataFound=false
          fetch('https://vue-http-demo2021-default-rtdb.firebaseio.com/surveys.json').then((res)=>{
            if(res.ok){
              return res.json();
            }
          }).then((data)=>{
            // console.log(data);
            const results=[];
            for(const id in data){
              results.push({ id: id, name: data[id].name, rating: data[id].rating})
            }
            this.results=results;
            console.log( this.results);
            if(!this.results.length){
              this.NoDataFound=true
            }
            else{
              this.NoDataFound=false
            }
            this.isLoading=false
          }).catch((err)=>{
            this.isLoading=false
            this.NoDataFound=false
             console.log(err);
             alert(err)
             this.isError=true
            this.error="Faild to fetch data, Try Again!!"
          })
         
       //get req doesnt require header/body/and method
    }
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
