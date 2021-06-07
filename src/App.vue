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
      let index; // to delete
      const managerName = newWOrker.manager;

      if (managerName) {
        this.checkNewManagers(this.workers, managerName, newWOrker);
        // index = this.newWOrkerIndex(newWOrker); // define index of new worker in worker array.
      }else{
        this.workers.push(newWOrker)
      }
    },

    checkNewManagers(workers, managerName, newWOrker) {
      workers.forEach((worker, index) => {
        if (worker.name == managerName) {
          workers[index].childs.push(newWOrker);
          return;
        } else if (worker.childs.length) {
          this.checkNewManagers(worker.childs, managerName, newWOrker);
        }
      });
    },
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
