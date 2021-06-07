<template>
  <div id="app">
    <Header> </Header>
    <div class="wrapper">
      <TableForm :workers="workers" v-on:formVisibility="setVisibility" > </TableForm>
      <InputForm :workers="workers"   v-if='formVisibility'></InputForm >
    </div>
  </div>
</template>

<script>
import TableForm from "./components/TableForm.vue";
import Header from "./components/Header.vue";
import InputForm from "./components/InputForm.vue";

export default {
  name: "App",
  components: {
    TableForm,
    Header,
    InputForm
  },
  data() {
    return {
      workers: [
        { name: "Vasya", tel: '8999999999' },
      ],

      formVisibility: false
    };
  },
methods: {
  setVisibility(){
     this.formVisibility  = !this.formVisibility
    }
},

  computed: {
    
  }
};
</script>
