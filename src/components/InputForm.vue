<template>
  <form>
    <label>
      Имя
      <input v-model="newWorker.name" type="text" required />
    </label>
    <label>
      Номер телефона
      <input v-model="newWorker.tel" required />
    </label>
    <label>
      Начальник
      <select v-model="newWorker.manager" name="users" id="test">
        <option value=""></option>
        <option v-for="worker in workers" :key="worker.tel" :value="worker.name"
          >{{ worker.name }}
        </option>
      </select>
    </label>
    <button @click.prevent='sendNewWorker'>Submit</button>
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
        manager: "",
        isManager: false,
        subordinate: []
      }
    };
  },

  methods:{
      sendNewWorker() {
        const clone = Object.assign({}, this.newWorker)
        this.$emit('sendNewWorker', clone)
        this.newWorker.name = ""
        this.newWorker.tel = ""
      }

  },

  computed: {}
};
</script>
