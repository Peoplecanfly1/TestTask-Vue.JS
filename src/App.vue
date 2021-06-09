<template>
  <div id="app">
    <Header> </Header>
    <div class="wrapper">
      <TableForm
        :renderList="renderList"
        v-on:formVisibility="setVisibility"
        @collapse="nameSort"
      >
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

    // make a renderlist
    this.RenderQueueOnLoadFilling(this.workers);
  },

  methods: {
    setVisibility() {
      this.formVisibility = !this.formVisibility;
    },

    nameSort(worker) {
      if (!worker.isManager) {
        return;
      }
      console.log("worker click", worker);
      worker.show = !worker.show;
      const status = worker.show;
      this.setShowStatus(worker.childs, status);
    },

    setShowStatus(workers, status) {
      console.log(workers);
      workers.forEach(item => {
        item.show = status;
        if (status) {
          console.log("item to display", item);
          item.style.display = "table-row";
        } else if (!status) {
          console.log("item to hide", item);
          item.style.display = "none";
        }
        if (item.childs) {
          this.setShowStatus(item.childs, status);
        }
      });
    },

    addToWorkers(newWOrker) {
      const managerName = newWOrker.manager;

      if (managerName) {
        this.newWOrker = newWOrker;
        this.checkNewManagers(this.workers, managerName, newWOrker);
      } else {
        this.workers.push(newWOrker);
      }

      this.pushChildInRenderQueue(newWOrker);

      localStorage.setItem("workers-list", JSON.stringify(this.workers));
    },

    checkNewManagers(workers, managerName, newWOrker) {
      workers.forEach((worker, index) => {
        if (worker.name == managerName) {
          newWOrker.lvl = worker.lvl + 1; // increase lvl of worker
          worker.isManager = true; // set managerStatus
          worker.style = { cursor: "pointer" }; // set cursor pointer for manager
          workers[index].childs.push(newWOrker); // push worker as a child
          newWOrker.show = worker.show;
          return;
        } else if (worker.childs.length) {
          this.checkNewManagers(worker.childs, managerName, newWOrker);
        }
      });
    },

    pushChildInRenderQueue(newWOrker) {
      if (!newWOrker.manager) {
        this.renderList.push(newWOrker);
      } else {
        const index =
          this.renderList.findIndex(item => item.name == newWOrker.manager) + 1;
        this.renderList.splice(index, 0, newWOrker);
      }
    },

    RenderQueueOnLoadFilling(workers) {
      workers.forEach(worker => {
        this.renderList.push(worker);
        if (worker.childs) {
          this.RenderQueueOnLoadFilling(worker.childs);
        }
      });
    }
  }
};
</script>
