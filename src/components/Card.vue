<template>
  <div class="card" >
    <div class="card-body">
      <h4 class="card-title">Adder #: {{ number }}</h4>
      <a @click="increment" class="btn btn-primary">Sum + {{ counter }}</a>
      <a @click="incrementCounter" class="btn btn-info">+1</a>
      <a @click="decrementCounter" class="btn btn-info">-1</a>
    </div>
  </div>
</template>

<script>
  import { eventBus } from '../main'

export default {
    name: 'card',
    data () {
      return {
        counter: 1
      }
    },
    methods: {
      decrementCounter () {
        if (this.counter >= 2) {
          this.counter--
        }
      },
      incrementCounter () {
        this.counter++
      },
      increment () {
        this.emitCount(this.counter)
      },
      emitCount (counter) {
        eventBus.$emit('countIncreased', {
          increment: counter
        })
        this.$emit('countIncreased', {
          increment: counter
        })
      }
    },
    props: {
      number: Number
    },
    created () {
      eventBus.$on('resetCards', () => {
        this.counter = 0
      })
    }
}
</script>
