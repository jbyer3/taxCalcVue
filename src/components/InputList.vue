<template>
  <div class="inputer">
    <input v-model="message" v-on:keyup.enter="run()" placeholder="name of food">  
    <input v-model.number="foods.cost" v-on:keyup.enter="run()" placeholder="cost of food">  
    <button v-on:click="run()" type="submit">Add to list</button>

    <h2>SubTotal - ${{ subTotal }}</h2>
    <h2>CLT Sales Tax - ${{ salesTax }}</h2>
    <h2>Total - ${{ newTotal }}</h2>

    
    
    <ul class="listicle">
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
      totalCost: 1577,
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
    }
  },
  computed: {
    subTotal: function() {
      return this.totalCost / 100;
    },
    salesTax: function() {
      let bigTax = this.subTotal * 725;
      let smallTax = bigTax / 10000;
      let realTax = Math.round(smallTax * 100) / 100;
      // check to see if number is x.xx or x.x, and if it is x.x add a 0 to the end of it
      let digits = ("" + realTax).split("");
      // if number is x.xx return the number
      if (digits[digits.length - 3] == ".") {
        return realTax;
        // if number is x.x return x.x0
      } else if (digits[digits.length - 2] == ".") {
        let fixDigits = digits.push("0");
        // rejoin new array
        return parseFloat(fixDigits);
        // if number is x return x.00
      } else {
        let fixDigitsArray1 = digits.push("0");
        let fixDigitsArray = fixDigitsArray1.push("0");
        //rejoin new array
        return parseFloat(fixDigitsArray);
      }
    },
    newTotal: function() {
      return this.subTotal + this.salesTax;
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
