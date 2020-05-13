<template>
  <div id="app" class="small-container">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <h3>I'm editable!</h3> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <h1>Employees</h1>
    <employee-form v-on:add:employee="addEmployee" />
    <employee-table
      v-bind:employees="employees"
      v-on:delete-employee="deleteEmployee"
      v-on:edit-employee="editEmployee"
    />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import EmployeeTable from '@/components/EmployeeTable'
import EmployeeForm from '@/components/EmployeeForm'
export default {
  name: 'App',
  components: {
    EmployeeTable,
    EmployeeForm
  },
  data() {
    return {
      employees: []
    }
  },
  methods: {
    async getEmployees() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users')
        const data = await response.json()
        this.employees = data
      } catch (error) {
        console.error(error)
      }
    },
    async addEmployee(employee) {
      // const size = this.employees.length;
      // if ( size > 0) {
      //   var lastId = this.employees[size - 1].id;
      // } else {
      //   lastId = 0;
      // }
      // let id = lastId + 1;
      // const newEmployee = [...employee, id];
      // this.employees = [...this.employees, newEmployee]
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users', {
          method: 'POST',
          body: JSON.stringify(employee),
          headers: { 'Content-type': 'application/json; charset=UTF-8'}
        })
        const data = await response.json()
        this.employees = [...this.employees, data]
      } catch (error) {
        console.error(error)
      }
    },
    async deleteEmployee(id) {
      // this.employees = this.employees.filter(
      //   employee => employee.id !== id
      // )
      try {
        await fetch(`https://jsonplaceholder.typicode.com/users/${id}`), {
          method: "DELETE"
        };
        this.employees = this.employees.filter(employee => employee.id !== id);
      } catch (error) {
        console.error(error)
      }
    },
    async editEmployee(id, updatedEmployee) {
      // this.employees = this.employees.map(
      //   employee => employee.id === id ? updatedEmployee : employee
      // )
      try {
        const response = await fetch(`https://jsonplaceholder.typicode.com/users/{id}`, {
          method: 'PUT',
          body: JSON.stringify(updatedEmployee),
          headers: { 'Content-type': 'application/json; charset=UTF-8'}
        })
        const data = await response.json()
        this.employees = this.employees.map(employee => (employee.id === id ? data : employee))
      } catch (error) {
        console.error(error)
      }
    }
  },
  mounted() {
    this.getEmployees()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
  button {
    background: #009435;
    border: 1px solid #009435;
  }

  .small-container {
    max-width: 680px;
  }
</style>
