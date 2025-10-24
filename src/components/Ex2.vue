<script>
    export default { 
        // Add code in appropriate places
        data() {
            return {
                items: ['keyboard', 'mouse', 'iPhone', 'macbook', 'adapter'],
            newItem: ''
    }
  },
  methods: {
    addItem() {
      if (this.newItem.trim() !== '') {
        this.items.push(this.newItem.trim());
        this.newItem = ''; // clear input after adding
      }
    },
    deleteItem(index) {
      this.items.splice(index, 1); // remove item at given index
    }
  }
}
</script>

<template>
  <h2>Shopping Cart</h2>

  <!-- Input and Add button -->
  <input v-model="newItem" type="text" placeholder="Enter new item">
  <button @click="addItem">Add!</button>

  <!-- List of items -->
  <ul>
    <li v-for="(item, index) in items" :key="index">
      {{ item }}
      <button @click="deleteItem(index)">Delete</button>
    </li>
  </ul>
</template>
