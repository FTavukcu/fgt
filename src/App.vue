<template>
  <div id="app">
    <label><input type="checkbox" v-model="allexpanded">Force expand all</label>
    <ul class="root">
      <li class="head">
        <div v-for="(attribute, index) in attributes" :key="'attribut-' + index" :class="['attr', attribute]"><input type="text" :placeholder="attribute" v-model="filters[attribute]"></div>
      </li>
      <tree :job="job" :filters="filters" :allexpanded="allexpanded" />
    </ul>
  </div>
</template>

<style>
body {
  font-family: 'Courier New', Courier, monospace;
  background: #e8e8e8;
}

input, body {
    font-size: .9vw;
}

.head {
  padding-bottom: .4em;
}

.root {
  padding: 0 0 0 1em;
  list-style-type: none;
}

input[type='text'] {
  width: 100%;
  border: 0;
  padding: 2px 4px;
}

.attr { position: absolute; height: 1em }

.attr.Objektname { position: relative; height: 0; width: 30% }
.attr.Runid { left: 32%; width: 7% }
.attr.Startzeit { left: 40%; width: 11% }
.attr.Endezeit { left: 52%; width: 11% }
.attr.Laufzeit { left: 64%; width: 5% }
.attr.RC { left: 70%; width: 2% }
.attr.Status { left: 73%; width: 25% }
</style>

<script>
import Tree from './components/Tree';

export default {
  name: 'App',
  components: {
    Tree: Tree
  },
  data: function(){
    return {
      attributes: ['Objektname', 'Runid', 'Startzeit', 'Endezeit', 'Laufzeit', 'RC', 'Status'],
      filters: {
        'Objektname':'',
        'Runid':'',
        'Startzeit':'',
        'Endezeit':'',
        'Laufzeit':'',
        'RC':'',
        'Status':''
      },
      job: {},
      allexpanded: false
    }
  },
  mounted() {
    fetch('/test.json').then(response => response.json()).then(json => this.job = json);
  }
}
</script>