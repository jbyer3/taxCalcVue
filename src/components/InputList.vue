<template>
  <div class="inputer">
    <!-- ~~~~~~~~~~~~~~~~~~~~~~~~~~~ input for maxCost variable ~~~~~~~~~~~~~~~~~~~~~~~~~~~ -->
    <div>
      <h1>How much money can you spend?</h1>
      <input v-model.number="maxCost">
    </div>
    <!-- ~~~~~~~ alert stating that you're spending more than you have e.g. maxCost < totalCost~~~~~~~~~~~~ -->
    <div v-if="costCheck() == true">
      <h4>Uh oh..... You're spending too much! Maybe consider putting something back...</h4>
      <!-- TODO create a function to suggest putting back the most expensive item -->
    </div>
    <!-- ~~~~~~~~~~~~~~~~~~~~~~~ input for sales tax (sTax) variable ~~~~~~~~~~~~~~~~~~~~~~~ -->
    <div>
      <h1>Sales Tax Rate - %<input v-model.number="sTax" placeholder="enter your sales tax rate"></h1>
    </div>
    <!-- ~~~~~~~~~~~~~~~~~~~~~~~~~ main input for food name and cost ~~~~~~~~~~~~~~~~~~~~~~~~~  -->
    <input v-model="message" v-on:keyup.enter="run()" placeholder="name of food">  
    <input v-model.number="foods.cost" v-on:keyup.enter="run()" placeholder="cost of food">  
    <!-- button to call run function that creates the list and adds up the costs of everything -->
    <button v-on:click="run()" type="submit">Add to list</button>
    <!-- ~~~~~~~~~~~~~~~~~~~~ where everything cost related is printed out ~~~~~~~~~~~~~~~~~~~~   -->
    <h2>SubTotal - ${{ subTotal }}</h2>
    <h2>CLT Sales Tax - ${{ salesTax }}</h2>
    <h2>Total - ${{ newTotal }}</h2>
    <!-- ~~~~~~~~~~~~~~~~~~~~~~~~ main list ~~~~~~~~~~~~~~~~~~~~~~~~ -->
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
      totalCost: 1577, //cost originally listed without decimal point to avoid float-math-hell
      sTax: 0, //variable to initialize sales tax
      maxCost: 0, //variable initializing the most we can spend
      message: ""
    };
  },
  methods: {
    addItem: function() {
      //create new food object
      let newFood = {
        name: this.message,
        cost: this.foods.cost * 100 // multiply the cost input by 100 to avoid float-math-hell
      };
      // validations for the submission of a new food item
      //broken section
      let numArr = this.foods.cost;
      let digits = ("" + numArr).split("");
      if (this.message != "") {
        if (digits[digits.length - 3] == ".") {
          this.foods.push(newFood);
        } else {
          alert("cost must be entered in x.xx format");
          console.log(numArr)
        }
      } else {
        alert("Please enter a food");
      }
      // clear inputs after submission pass/fail
      this.message = "";
      this.foods.cost = "";
    },
    subtCalc: function() {
      // clear total cost and loop over every item in foods array/list to push them into cost array
      this.totalCost = 0;
      let costArray = [];
      for (let i = 0; i < this.foods.length; i++) {
        costArray.push(this.foods[i].cost);
      }

      // use .reduce to loop over the newly created costArray and assign the value to this.totalCost that we previously cleared, and obtain the sum of all the food costs
      this.totalCost = costArray.reduce((total, amount) => {
        total += amount;
        return total;
      });
    },
    run: function() {
      // "dry" up the template by mashing addItem and subtCalc together
      this.addItem();
      this.subtCalc();
    },
    dleet: function(index) {
      // allows for deletion of items from the list, running a new total calculation
      this.foods.splice(index, 1);
      this.subtCalc();
    },
    costCheck: function() {
      //evaluates if we are spending more than we meant to
      let num1 = this.maxCost;
      let num2 = this.newTotal;
      return num1 < num2;
    }
  },
  computed: {
    // makes the totalCost into a "float" that looks like a typical money amount
    subTotal: function() {
      return this.totalCost / 100;
    },
    salesTax: function() {
      //multiply subtotal by the sales tax, sTax has to be multiplied by 100 to get rid of the decimal from the input, avoiding float-math-hell
      let bigTax = this.subTotal * (this.sTax * 100);
      // take the bigTax and decrease it by four decimal places to make it look like money
      let smallTax = bigTax / 10000;
      // realTax takes the smallTax and chops off everything but two decimal places.
      let realTax = Math.round(smallTax * 100) / 100;
      return realTax;
    },
    newTotal: function() {
      //adds the subtotal and sales tax, same decimal place chop off as above just in case
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
