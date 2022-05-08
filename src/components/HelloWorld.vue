<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
  <component :is="componentFile"></component>
  <component :is="fileList('testComponent2')"></component>
  <div v-for="item in items" :key="item">
    <component :is="fileList(item.message)"></component>
  </div>

  <div v-for="file in files" :key="file">
    <component :is="fileList(file.name)"></component>
  </div>

</template>

<script>
import {defineAsyncComponent, reactive, toRef} from "vue";
import LoadingComponent from "@/components/LoadingComponent";
import ErrorComponent from "@/components/ErrorComponent";

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  setup(){
    const state = reactive({

    });
    const files = reactive([{name:'testComponent2'},{name:'testComponent3'}])
    const fileList =  (file) => defineAsyncComponent(() =>{
      return import(`./${file}.vue`);
    })
    const componentFile = defineAsyncComponent({
      loader: () => import('./testComponent1'),
      delay: 200,
      timeout: 300,
      errorComponent: ErrorComponent,
      loadingComponent: LoadingComponent
    });

    return {
      ...toRef(state),
      files,
      fileList,
      componentFile,

    }
  },
  data(){
    return {
      items: [{message: 'testComponent2'}, {message:'testComponent3'}]
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
