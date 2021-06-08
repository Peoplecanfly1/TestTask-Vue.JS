<template>
  <div id="app">
    <Header> </Header>
    <div class="wrapper">
      <TableForm :renderList="renderList" v-on:formVisibility="setVisibility">
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
      formVisibility: false,
      renderList: [],
      newWOrker: null
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
      const managerName = newWOrker.manager;

      if (managerName) {
        this.newWOrker = newWOrker;
        this.checkNewManagers(this.workers, managerName, newWOrker);
        this.pushChildRenderQueue(newWOrker);
      } else {
        this.workers.push(newWOrker);
      }

      localStorage.setItem("workers-list", JSON.stringify(this.workers));
    },

    checkNewManagers(workers, managerName, newWOrker) {
      workers.forEach((worker, index) => {
        if (worker.name == managerName) {
          newWOrker.lvl = worker.lvl + 1; // increase lvl of worker
          worker.isManager = true; // set managerStatus
          workers[index].childs.push(newWOrker); // push worker as a child
          return;
        } else if (worker.childs.length) {
          this.checkNewManagers(worker.childs, managerName, newWOrker);
        }
      });
    },

    pushChildRenderQueue(newWOrker) {
      const index =
        this.renderList.findIndex(item => item.name == newWOrker.manager) + 1;
      this.renderList.splice(index, 0, newWOrker);
    }
  },

  watch: {
    workers: {
      handler() {
        this.workers.forEach(item => {
          if (!this.renderList.includes(item)) {
            this.renderList.push(item);
          }
        });
      }
    }
  }
};
</script>
