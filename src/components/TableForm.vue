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
      <tr v-for="(worker, index) in workers" :key="index">
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
//  [] Подход к решению задачи в общем. Норм?
//  [] К
//  []Если за место готового массива в цикле v-for: делать функцию, которая бы возвращала нужный массив. То она возвращает результат 2жды почемуто.  при чем не важно куда положить функцию в методы в ватчер или еще куда.
//  []176 строка , почему-то не работает верно логика постановки курсора.
//  [] Сейчас я делаю модификацию инлайн стилей, как лучше это все запихнуть просто в стили ?
//  [] Что нужно вынести в computed, по идее то что вычесляется прям в тимлейте, но у меня ничего такого нет, проблема с архитектурой?

export default {
  name: "TableForm",
  props: ["workers"],
  data() {
    return {
      
    };
  },
  watch:{
   
  }


};
//   created() {
//     const employeesData = localStorage.getItem("workers-list");
//     if (!employeesData) {
//       return;
//     }

//     this.employees = JSON.parse(employeesData);
//   },

//   methods: {
//     addNewEmployee() {
//       const employeeCopied = Object.assign({}, this.newEmployee);

//       let isTelNumberUniq = !this.employees.find(
//         item => item.tel == employeeCopied.tel
//       );

//       if (!isTelNumberUniq) {
//         alert("This tel number already exists");
//         return;
//       }

//       this.employees.splice(this.hierarchyIndex, 0, employeeCopied);

//       // clean a form
//       this.newEmployee.name = "";
//       this.newEmployee.tel = "";
//       this.newEmployee.manager = "";
//       this.newEmployee.isManager = false;
//       this.newEmployee.level = 1;
//     },

//     collapse(item) {
//       if (!item.isManager) {
//         return;
//       }

//       const emplIndex = this.employees.indexOf(item);
//       console.log(emplIndex);
//       for (let index = emplIndex + 1; index < this.employees.length; index++) {
//         let employee = this.employees[index];

//         // one lvl below show/hide
//         //   Самая главная проблема тут.
//         if (employee.level == item.level + 1) {
//           console.log("hiding this elem:", employee);
//           switch (employee.style.display) {
//             case "table-row":
//               employee.style.display = "none";
//               break;
//             case "none":
//               employee.style.display = "table-row";
//               break;
//           }
//           // 2+ lvl below set as hide on click
//         } else if (employee.level > item.level + 1) {
//           console.log("imhere down");
//           switch (employee.style.display) {
//             case "none":
//               break;
//             case "table-row":
//               employee.style.display = "none";
//               break;
//           }
//         } else if (employee.level == 1) {
//           console.log("imhere break");
//           break;
//         }
//       }

//       localStorage.setItem("workers-list", JSON.stringify(this.employees));
//     }
//   },

//   computed: {},

//   watch: {
//     newEmployee: {
//       deep: true,
//       handler() {
//         // to define who is manager of new employee to set right level
//         let manager = null;
//         if (!this.employees.length) {
//           return;
//         }

//         if (this.newEmployee.manager) {
//           manager = this.employees.filter(
//             worker => worker.name == this.newEmployee.manager
//           );
//           this.newEmployee.level = manager[0].level + 1;

//           this.hierarchyIndex = this.employees.indexOf(manager[0]) + 1;
//         } else {
//           this.hierarchyIndex = this.employees.length;
//         }
//       }
//     },

//     employees() {
//       // Слишком много раз выполняется. Лишних 2 раза.
//       console.log("im done");

//       const arrayOfManagers = this.employees.filter(worker =>
//         worker.manager ? true : false
//       );
//       let managerNames = [];
//       if (arrayOfManagers.length) {
//         arrayOfManagers.forEach(item => managerNames.push(item.manager));
//       }

//       //uniq managers names
//       managerNames = Array.from(new Set(managerNames));

//       // set manageger status as true
//       this.employees.forEach(worker => {
//         if (managerNames.includes(worker.name)) {
//           worker.isManager = true;
//         }
//       });

//       // при добавлении элемента на 2рой уровень делает его кусор курсор поинтер
//       this.employees.forEach(worker =>
//         worker.isManager ? (worker.style.cursor = "pointer") : false
//       );

//       // add to local storage in case change of workers array
//       localStorage.setItem("workers-list", JSON.stringify(this.employees));
//     }
//   }
// };
//
</script>
