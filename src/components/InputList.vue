<template>
  <div class="wrapper">
    <div class="card">
      <!-- ~~~~~~~~~~~~~~~~~~~~~~~~~~~ input for maxCost variable ~~~~~~~~~~~~~~~~~~~~~~~~~~~ -->
      <h3>How much money can you spend?</h3>
      <input v-model.number="maxCost">
      <!-- ~~~~~~~~~~~~~~~~~~~~~~~ input for sales tax (sTax) variable ~~~~~~~~~~~~~~~~~~~~~~~ -->
      <div>
        <h3>Sales Tax Rate</h3>
        <input v-model.number="sTax"
          class="rattattoo" 
          placeholder="enter your sales tax rate">%
      </div>
      <!-- ~~~~~~~~~~~~~~~~~~~~~~~~~ main input for food name and cost ~~~~~~~~~~~~~~~~~~~~~~~~~  -->
      <input v-model="message" v-on:keyup.enter="run()" placeholder="name of food">  
      <input v-model.number="foods.cost" v-on:keyup.enter="run()" placeholder="cost of food">  
      <!-- button to call run function that creates the list and adds up the costs of everything -->
      <button v-on:click="run()" type="submit">Add to list</button>
    </div>
    <!-- <div class="seperator"></div> -->
    <div class="card">
       <!-- ~~~~~~~~~~~~~~~~~~~~ where everything cost related is printed out ~~~~~~~~~~~~~~~~~~~~   -->
      <h3>SubTotal - ${{ subTotal }}</h3>
      <h3>+ Sales Tax - ${{ salesTax }}</h3>
      <hr>
      <h3>= Total - ${{ newTotal }}</h3>
      <!-- ~~~~~~~ alert stating that you're spending more than you have e.g. maxCost < totalCost~~~~~~~~~~~~ -->
      <div v-if="costCheck() == true">
        <h4 class="danger">Uh oh..... You're spending too much! Maybe consider putting something back... </h4>
        <!-- TODO create a function to suggest putting back the most expensive item -->
      </div>
    </div>
    <!-- <div class="seperator"></div> -->
    <div class="card" id="list">
      <ul>
        <!-- ~~~~~~~~~~~~~~~~~~~~~~~~ main list ~~~~~~~~~~~~~~~~~~~~~~~~ -->
        <li v-for="(food, index) in foods" :key="food.id" :index="index" v-on:remove="foods.splice(index, 1)">
          {{ food.name }} ... ${{ food.cost / 100 }}
          <button v-on:click="dleet(index)">x</button>
        </li>
      </ul>
    </div>
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
      costArray: [],
      totalCost: 1577, //cost originally listed without decimal point to avoid float-math-hell
      sTax: 7.25, //variable to initialize sales tax
      maxCost: 35, //variable initializing the most we can spend
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
          console.log(numArr);
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
.wrapper {
  display: flex;
  flex-direction: row;
  // flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
}
.card {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  width: 322px;
  height: 300px;
  background-color: #888888;
  border-radius: 23px;
  padding: 12px;
  margin: 10px;
  #list {
    display: flex;
    flex-direction: column;
    align-self: flex-start;
    justify-content: flex-start;
    min-width: 200px;
    max-width: 400px;
    text-align: left;
  }
  hr {
    background-color: black;
    width: 100%;
  }
  .rattattoo {
    width: 35px;
    text-align: center;
    margin-bottom: 20px;
  }
}

@media only screen and (max-device-width: 1073px) { 
  .wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    .card{
      width: 75%;
      height: 100%;
    }
  }
}
// .seperator {
//   width: 10px;
//   height: 100%;
// }
.danger {
  color: red;
}
</style>
