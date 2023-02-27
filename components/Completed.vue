<template>
    <div class="container">
        <TopBar />
        <div class="completed-form">
            <h3>Hello {{ name }}!</h3>
            <p>You selected {{ countyName }} County, {{ state }}</p>
        </div>
    </div>
    
</template>
  
<script>
export default {
  name: 'Completed',
  props: {
    name: {
      type: String,
      required: true
    },
    countyName: {
      type: String,
      required: true
    },
    state: {
      type: String,
      required: true
    }
  }
}
</script>


<style scoped>
.container{
    background-color: rgb(231, 231, 231);
    width: 100vw;
    height: 100vh;
}

.completed-form {
    display: flex;
    flex-direction: column;
    width: 50%;
    min-width: 320px;
    padding: 15px;
    background-color: white;
    border-radius: 5px;
    margin: 1vw auto;
    box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
}

h3 {
    margin: 0;
    font-size: x-large;
    font-weight: 200;
    font-size: 1.5em;

}

p {
    margin: .5vw 0;
    font-size: 1em;
}

@media only screen and (max-width: 375px) and (max-height: 667px) {

  .completed-form {
    width: 320px;
    margin: 3vw auto;
  }
}


</style>
