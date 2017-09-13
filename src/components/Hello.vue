<template>
  <div class="container">
    <div class="jumbotron">
      <h1 class="display-3">Big Sum: {{ bigSum }}</h1>
      <p class="lead">{{ msg }}</p>
      <hr class="my-4">
      <p>Increased by: {{ increasedBy }}</p>
      <p class="lead">
        <a @click="removeAdder" class="btn btn-info btn-lg" role="button">Remove Adders</a>
        <a @click="addAdder" class="btn btn-info btn-lg" role="button">Add Adders</a>
        <a @click="bigSum = 0; increasedBy = 0" class="btn btn-primary btn-lg" role="button">Reset Sum</a>
        <a @click="resetCards" class="btn btn-danger btn-lg" role="button">Reset Adders</a>
      </p>
    </div>

    <div class="row">
      <div v-for="(n, i) in cardArray" class="card-container col-md-3">
        <card @countIncreased="addToBigSum($event)" :number="i+1"></card>
      </div>
    </div>


  </div>
</template>

<script>
import Card from './Card'
import { eventBus } from '../main'

export default {
  name: 'hello',
  data () {
    return {
      cardArray: [1, 1, 1, 1, 1],
      bigSum: 0,
      msg: 'Welcome to Ridgeway\'s adding machine!',
      increasedBy: 0
    }
  },
  components: {
    card: Card
  },
  methods: {
    addAdder () {
      this.cardArray.push(1)
    },
    removeAdder () {
      if (this.cardArray.length >= 1) {
        this.cardArray.pop()
      }
    },
    addToBigSum (event) {
      this.bigSum += event.increment
    },
    resetCards () {
      eventBus.$emit('resetCards')
    }
  },
  created () {
    eventBus.$on('countIncreased', (data) => {
      this.increasedBy = data.increment
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.card-container {
  margin-bottom: 1em
}
</style>
