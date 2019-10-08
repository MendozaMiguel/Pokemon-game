<template>
  <div v-on:mousemove="updateCoordinates">
    <p>WORK COMPONENT</p>
    {{ msg }}
    <!-- <button v-on:click="increase(2)">Click ME !</button> |
    <button v-on:click="decrease(1)">Decrease</button>-->
    <button v-on:click="counter = counter + 2">Click ME !</button> |
    <button v-on:click="counter--">Decrease</button> |
    <button v-on:click="secondCounter++">Click ME !</button>
    <p>counter : {{counter}} | {{secondCounter}}</p>
    <!-- <p>result : {{this.result}}</p> -->
    <p>result : {{result()}} | {{output}}</p>
    <p>Coordinates: {{x}} / {{y}}</p>
    <span v-on:mousemove="dummy">DEAD SPOT</span>
    <input type="text" v-model="name" />
    <p>{{name}}</p>
    <hr />
    <button v-on:click="changeUrl">Clik to change Link</button>
    <p>{{test ===true ? myUrl1: myUrl2}}</p>
    <a v-bind:href="test ===true ? myUrl1: myUrl2">{{test ===true ? myUrl1: myUrl2}}</a>

    <hr />
    <div class="demo" @click="attachRed = ! attachRed" :class="{red: attachRed}"></div>
    <div class="demo" @click="attachRed = ! attachRed" :class="{yellow: !attachRed}"></div>
    <div class="demo" @click="attachRed = ! attachRed" :class="{green: attachRed}"></div>
  </div>
</template>
<script>
export default {
  name: 'ExoTestComponent',
  props: {
    msg: String
  },
  data: function () {
    return {
      counter: 0,
      secondCounter: 0,
      // result: "",
      x: 0,
      y: 0,
      name: '',
      myUrl1: 'http://www.google.com',
      myUrl2: 'http://www.apple.com',
      test: true,
      attachRed: false
    }
  },
  computed: {
    output: function () {
      console.log('computed')
      return this.counter > 10 ? 'Greather than 10' : 'Smaller than 10'
    }
  },
  watch: {
    counter: function (value) {}
  },
  methods: {
    changeUrl: function () {
      this.test = !this.test
      console.log('this.test : ', this.test)
    },
    // increase: function(step) {
    //   this.counter += step;
    //   console.log(this.counter);
    //   this.result = this.counter > 10 ? "Greater 10" : "Smaller 10";
    // },
    // decrease: function(step) {
    //   this.counter -= step;
    //   console.log(this.counter);
    //   this.result = this.counter >= 10 ? "Greater 10" : "Smaller 10";
    // },
    result: function () {
      console.log('method')
      if (this.counter === 37) {
        console.log('done !')
        setTimeout(() => {
          this.counter = 0
        }, 5000)
      }
      return this.counter > 10 ? 'Greater than 10' : 'Smaller than 10'
    },
    updateCoordinates: function (event) {
      this.x = event.clientX
      this.y = event.clientY
    },
    dummy: function (event) {
      event.stopPropagation()
    }
  }
}
</script>
<style scoped>
.demo {
  width: 100px;
  height: 100px;
  background-color: gray;
  display: inline-block;
  margin: 10px;
}

.red {
  background-color: red;
}
.green {
  background-color: green;
}
.yellow {
  background-color: yellow;
}
</style>
