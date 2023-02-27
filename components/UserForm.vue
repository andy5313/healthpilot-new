<template>
  <div class="div-container">
    <TopBar />
    <div class="form-container">
      <div class="container">
        <div class="form-fields">
          <label for="name-input">Name</label>
          <input id="name-input" v-model="name" type="text" placeholder="Enter your name" />
        </div>
        <div class="form-fields">
          <label for="zipcode-input">Zip Code</label>
          <input id="zipcode-input" v-model="zipcode" type="text" maxlength="5" placeholder="Enter your 5-digit zip code" />
          <div class='error' v-if="errorMessage">{{ errorMessage }}</div>
        </div>
        <div class="form-fields" v-if="counties.length > 0">
          <label>County</label>
          <select v-model="selectedCounty">
            <option :value="{}" disabled selected>Select County</option>
            <option v-for="(county, index) in counties" :key="index" :value="county">{{ county.countyName }}</option>
        </select>
        </div>
      </div>
      <div class="btn-div">
          <button class="next-button" :class="{ 'disabled-button': isDisabled }" :disabled='isDisabled' @click="submitForm">Next</button>
      </div>
    </div>
    
  </div>
  
</template>

<script>
export default {
  data(){
    return {
      name: '',
      zipcode: '',
      errorMessage: '',
      counties : [],
      selectedCounty : {},
    }
  },

  computed: {
    isDisabled() {
      return !(this.name && this.selectedCounty.stateName !== undefined && this.selectedCounty && Object.keys(this.selectedCounty).length > 0);
    },
  },

  watch:  {
    zipcode(zipcodeInput){
      if (zipcodeInput.length === 5){
        this.makeApiRequest()

      }

      if (zipcodeInput.length < 5){
        this.selectedCounty = {}
      }
    }
  },

  methods: {
    async makeApiRequest() {
      try {
        const response = await this.$axios.get(`https://api-connecture.healthpilot.com/api/Geo/counties/${this.zipcode}`)
        if (response.data.length === 0){
          this.errorMessage = 'Not a valid zipcode'
          this.counties = [];
          this.selectedCounty = {};
        } else if (response.data.length === 1){
          this.errorMessage = ''
          const county = response.data[0];
          this.counties = [county];
          this.selectedCounty = county;
        } else {
          this.errorMessage = ''
          this.selectedCounty = {};
          this.counties = response.data;
        }
    
      } catch (error) {
        this.counties = [];
        console.error(error)
        
      }
    },

    submitForm() {
      this.$router.push({
        name: 'completed',
        params: {
          name: this.name,
          countyName: this.selectedCounty.countyName,
          state: this.selectedCounty.stateName
        },
      });
    },
  }
}
</script>

<style scoped>
.container {
  width: 40vw;
  margin: 0 auto;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 1vw;
}

.form-container {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 85%;
}

.div-container{
  background-color: rgb(245, 245, 245);
  height: 100vh;
}


.head {
  font-size: 24px;
  font-weight: bold;
  text-align: center;

}

label {
  display: block;
  margin-bottom: 1.5vh;
  font-size: 1.5em;
}

input, select {
  padding: 2vh 1vw;
  border: 1px #d7d7d7 solid;
  border-radius: 5px;
  margin-bottom: 1vh;
}


.error {
  padding-top: 5px;
  color: red;
}

.form-fields {
  display: flex;
  flex-direction: column;
  border-radius: 1vw;
}

select:focus, input:focus {
  outline: none;
  border-color: rgb(255, 190, 26);
}

.next-button {
  background-color: darkblue;
  color: white;
  padding: 1vh 1vw;
  border-radius: 5px;
  font-size: 1.5em;
  width: 150px;
}

.disabled-button {
  background-color: rgb(219, 219, 219);
  color: gray;
  cursor: not-allowed;
  border: 0;
}

.btn-div {
  display: flex;
  justify-content: center;
}

@media only screen and (max-width: 375px) and (max-height: 667px) {

  .container {
    width: 90%;
    margin: 0;
  }
}

</style>


