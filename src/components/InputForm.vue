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
        childs: []
      },
      allWorkersNames: []
    };
  },

  mounted() {
    this.getNameList(this.workers);
  },

  methods: {
    sendNewWorker() {
      let clone = Object.assign({}, this.newWorker);

      this.$emit("sendNewWorker", clone);
      this.allWorkersNames.push(clone.name); //!!!!!!!!!!!!

      this.newWorker.name = "";
      this.newWorker.tel = "";
      this.newWorker.manager = "";
    },

    getNameList(workers) {
      workers.forEach(element => {
        this.allWorkersNames.push(element.name);

        if (element.childs) {
          this.getNameList(element.childs);
        } else {
          return;
        }
      });
    }
  }
};
</script>
