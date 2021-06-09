<template>
  <div class="table-wrapper">
    <button v-on:click="$emit('formVisibility')" class="add">
      Add new employee
    </button>
    <table class="treetable">
      <thead>
        <tr>
          <th>Name</th>
          <th>Phone</th>
        </tr>
      </thead>
      <tr
        v-for="(worker, index) in renderList"
        v-bind:key="index"
        v-bind:class="'level-' + worker.lvl.toString()"
        v-bind:style="worker.style"
        @click="collapse(worker)"
      >
        <td class="name">
          {{ worker.name }}
        </td>
        <td>
          {{ worker.tel }}
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "TableForm",
  props: ["renderList"],
  data() {
    return {};
  },
  methods: {
    collapse(worker) {
      this.$emit("collapse", worker);
    }
  },

  watch: {}
};
</script>
