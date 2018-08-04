<template>
  <div class="inputer">
    <input v-model="message" v-on:keyup.enter="add()" placeholder="edit me">  
    <button v-on:click="add()" type="submit">Add to list</button>
    <ul>
      <li v-for="food in foods" :key="food.id">
        {{ food.name }}
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
        { name: "apples" }, 
        { name: "carrots" }, 
        { name: "onions" }
        ],
      cost: 0,
      message: ''
    };
  },
  methods: {
    add: function() {
      let newFood = { name: this.message }
      this.foods.push(newFood)
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
