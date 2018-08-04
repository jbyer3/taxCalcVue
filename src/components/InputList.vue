<template>
  <div class="inputer">
    <input v-model="message" v-on:keyup.enter="addItem(), taxCalc()" placeholder="name of food">  
    <input v-model="foods.cost" v-on:keyup.enter="addItem(), taxCalc()" placeholder="cost of food">  
    <button v-on:click="addItem()" type="submit">Add to list</button>

    <h2>{{ totalCost }}</h2>
    

    <ul>
      <li v-for="food in foods" :key="food.id">
        {{ food.name }} ${{ food.cost }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "InputList",
  data() {
    return {
      foods: [
        { name: "apples", cost: 3.99 },
        { name: "carrots", cost: 2.79 },
        { name: "onions", cost: 8.99 }
      ],
      totalCost: 0,
      message: ""
    };
  },
  methods: {
    taxCalc: function() {
      this.totalCost = 0;
      let costArray = [];
      for (let i = 0; i < this.foods.length; i++) {
        costArray.push(this.foods[i].cost);
      }
      console.log(costArray);
      this.totalCost = costArray.reduce((total, amount) => {
        total += amount;
        return total;
      });
    },
    addItem: function() {
      let newFood = {
        name: this.message,
        cost: Number(this.foods.cost)
      };
      this.foods.push(newFood);
      this.message = "";
      this.foods.cost = "";
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
* {
  background-color: chartreuse;
}
</style>
