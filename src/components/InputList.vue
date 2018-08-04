<template>
  <div class="inputer">
    <input v-model="message" v-on:keyup.enter="addItem(), taxCalc()" placeholder="name of food">  
    <input v-model.number="foods.cost" v-on:keyup.enter="addItem(), taxCalc()" placeholder="cost of food">  
    <button v-on:click="addItem(), taxCalc()" type="submit">Add to list</button>

    <h2>{{ totalCost / 100 }}</h2>
    

    <ul>
      <li v-for="food in foods" :key="food.id">
        {{ food.name }} ${{ food.cost / 100 }}
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
        { name: "apples", cost: 399 },
        { name: "carrots", cost: 279 },
        { name: "onions", cost: 899 }
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
        cost: this.foods.cost * 100
      };
      
      let numArr = this.foods.cost;
      let digits = (""+numArr).split("");
      
      console.log(digits);

      if (this.message != ""){
        if ((digits[digits.length - 3] == ".") && (this.message != "")) {
          this.foods.push(newFood);
        } else {
          alert("cost must be entered in x.xx format")
        }
      } else {
        alert("Please enter a food")
      }
        

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
