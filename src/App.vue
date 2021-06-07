<template>
  <div id="app">
    <Header> </Header>
    <div class="wrapper">
      <TableForm :workers="workers" v-on:formVisibility="setVisibility">
      </TableForm>
      <InputForm
        :workers="workers"
        v-if="formVisibility"
        @sendNewWorker="addToWorkers"
      ></InputForm>
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
      workers: [],
      formVisibility: false
    };
  },

  created() {
    // get data from LS if exists
    const workersData = localStorage.getItem("workers-list");
    if (!workersData) {
      return;
    }
    this.workers = JSON.parse(workersData);
  },

  methods: {
    setVisibility() {
      this.formVisibility = !this.formVisibility;
    },

    addToWorkers(newWOrker) {
      let index = null; 

      if(newWOrker.manager){
        this.checkNewManagers(newWOrker);
        index = this.newWOrkerIndex(newWOrker);
      };
      console.log(index)
      this.workers.splice(index+1, 0, newWOrker)
    },

    checkNewManagers(newWOrker){
      let managerSet = this.workers.find(
        workerData => workerData.name == newWOrker.manager
      );
      if(managerSet){
        managerSet.isManager = true;
      }
    },

    newWOrkerIndex(newWOrker){
      const manager = this.workers.find(manager => manager.name == newWOrker.manager)
      return this.workers.indexOf(manager)
    }


  },

  watch: {
    workers: {
      deep: true,
      handler() {
        localStorage.setItem("workers-list", JSON.stringify(this.workers));
      }
    }
  },

  computed: {}
};
</script>
