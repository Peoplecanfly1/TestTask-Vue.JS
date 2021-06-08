<template>
  <form @submit.prevent="sendNewWorker">
    <label>
      Имя
      <input v-model="newWorker.name" type="text" required />
    </label>
    <label>
      Номер телефона
      <input v-model="newWorker.tel" type="tel" required />
    </label>
    <label>
      Начальник
      <select v-model="newWorker.manager" name="users" id="test">
        <option value=""></option>
        <option
          v-for="(worker, index) in allWorkersNames"
          :key="index"
          :value="worker"
          >{{ worker }}
        </option>
      </select>
    </label>
    <button>Submit</button>
  </form>
</template>

<script>
export default {
  name: "InpitForm",
  props: ["workers"],
  data() {
    return {
      newWorker: {
        name: "",
        tel: "",
        lvl: 1,
        manager: "",
        isManager: false,
        childs: [],
        show: true,
        style: { cursor: "default", display: "table-row" }
      },
      allWorkersNames: []
    };
  },

  mounted() {
    this.workerNamesListDef(this.workers);
  },

  methods: {
    sendNewWorker() {
      let clone = Object.assign({}, this.newWorker);
      this.allWorkersNames.push(clone.name);
      this.$emit("sendNewWorker", clone);
      // сlean form and data
      this.cleanForm();
    },

    workerNamesListDef(workers) {
      workers.forEach(element => {
        this.allWorkersNames.push(element.name);

        if (element.childs) {
          this.workerNamesListDef(element.childs);
        } else {
          return;
        }
      });
    },

    cleanForm() {
      this.newWorker.name = "";
      this.newWorker.tel = "";
      this.newWorker.manager = "";
      this.newWorker.childs = [];
      this.newWorker.style.cursor = "default";
    }
  }
};
</script>
