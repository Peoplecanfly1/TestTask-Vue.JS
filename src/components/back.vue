<template>
    <form v-on:submit.prevent="addNewEmployee">
      <label>
        Имя
        <input v-model="newEmployee.name" type="text" required />
      </label>
      <label>
        Номер телефона
        <input
          v-model="newEmployee.tel"
          type="tel"
          pattern="(\+?\d[- .]*){7,13}"
          required
        />
      </label>
      <label>
        Начальник
        <select v-model="newEmployee.manager" name="users" id="test">
          <option value=""></option>
          <option
            v-for="employee in employees"
            v-bind:key="employee.tel"
            v-bind:value="employee.name"
            >{{ employee.name }}
          </option>
        </select>
      </label>
      <button>Submit</button>
    </form>
</template>