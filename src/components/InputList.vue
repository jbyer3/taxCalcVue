<template>
  <div class="inputer">
    <div>
      <h1>Sales Tax Rate - %<input v-model.number="sTax" placeholder="enter your sales tax rate"></h1>
    </div>
    <input v-model="message" v-on:keyup.enter="run()" placeholder="name of food">  
    <input v-model.number="foods.cost" v-on:keyup.enter="run()" placeholder="cost of food">  
    <button v-on:click="run()" type="submit">Add to list</button>

    <h2>SubTotal - ${{ subTotal }}</h2>
    <h2>CLT Sales Tax - ${{ salesTax }}</h2>
    <h2>Total - ${{ newTotal }}</h2>

    <ul class="liszt">
      <li v-for="(food, index) in foods" :key="food.id" :index="index" v-on:remove="foods.splice(index, 1)">
        {{ food.name }} ... ${{ food.cost / 100 }}
        <button v-on:click="dleet(index)">x</button>
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
      totalCost: 1577,
      sTax: 0,
      message: ""
    };
  },
  methods: {
    addItem: function() {
      let newFood = {
        name: this.message,
        cost: this.foods.cost * 100
      };
      let numArr = this.foods.cost;
      let digits = ("" + numArr).split("");
      if (this.message != "") {
        if (digits[digits.length - 3] == "." && this.message != "") {
          this.foods.push(newFood);
        } else {
          alert("cost must be entered in x.xx format");
        }
      } else {
        alert("Please enter a food");
      }

      this.message = "";
      this.foods.cost = "";
    },
    subtCalc: function() {
      this.totalCost = 0;
      let costArray = [];
      for (let i = 0; i < this.foods.length; i++) {
        costArray.push(this.foods[i].cost);
      }
      this.totalCost = costArray.reduce((total, amount) => {
        total += amount;
        return total;
      });
    },
    run: function() {
      this.addItem();
      this.subtCalc();
    },
    dleet: function(index) {
      this.foods.splice(index, 1);
      this.subtCalc();
    }
  },
  computed: {
    subTotal: function() {
      return this.totalCost / 100;
    },
    salesTax: function() {
      let bigTax = this.subTotal * (this.sTax * 100);
      let smallTax = bigTax / 10000;
      let realTax = Math.round(smallTax * 100) / 100;
      return realTax;
    },
    newTotal: function() {
      let totes = this.subTotal + this.salesTax;
      return Math.round(totes * 100) / 100;
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
