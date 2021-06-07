<template>
  <form @submit.prevent="sendNewWorker">
    <label>
      Имя
      <input v-model="newWorker.name" type="text" required />
    </label>
    <label>
      Номер телефона
      <input
        v-model="newWorker.tel"
        type="tel"
        required
      />
    </label>
    <label>
      Начальник
      <select v-model="newWorker.manager" name="users" id="test">
        <option value=""></option>
        <option v-for="(worker,index) in getNames(this.workers)" :key="index" :value="worker"
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

 
  methods: {
    
    sendNewWorker() {
      const clone = Object.assign({}, this.newWorker);
      if(this.workers.length && clone.manager){

        clone.lvl = this.workers.find(item => item.name == clone.manager).lvl +1
      }
    
      this.$emit("sendNewWorker", clone);
      this.newWorker.name = "";
      this.newWorker.tel = "";
      this.newWorker.manager = "";
    },


    getNames(workers){
      let namesArray  =  []
      workers.forEach((element,index) => {
        namesArray.push(element.name)
        if(element.childs){
          this.getNames(element.childs)
        }else{
          return
        }
      });
      console.log(namesArray)
      return namesArray
    }
  },

};
</script>
