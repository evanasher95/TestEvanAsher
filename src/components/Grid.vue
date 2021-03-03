<template>
    <div>
        <ul>
            <li v-for="item in categories" :key="item.id">
                {{ item.name }}
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Grid',
  data() {
      return {
          items: {},
          categories: [],
          mappedItem: new Map(),
          category: []
      }
  },
  methods: {
      fetch:  function(){
        axios.get('https://api.mocki.io/v1/47ed1580').then(r => {
            this.items = r;
            this.categories = [...this.items.data.categories];
            for (let i=0; i < this.categories.length; i++){
              if(!this.categories[i].parent_id){
                this.category.push(this.categories[i]);
              }
              else {
                  if(this.mappedItem.has(this.categories[i].parent_id)){
                      const temp = [...this.mappedItem.get(this.categories[i].parent_id)];
                      temp.push(this.categories[i]);
                      this.mappedItem.set(this.categories[i].parent_id, temp);
                  }
                  else {
                      this.mappedItem.set(this.categories[i].parent_id, [this.categories[i]]);
                  }
              }
          }
        }
        );
      }
  },
  created: function(){
      this.fetch();
  }
}
</script>
