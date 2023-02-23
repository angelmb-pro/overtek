<script setup>
import axios from 'axios';
import CustomTable from './components/CustomTable.vue';
 import FilterElement from './components/FilterElement.vue';
</script>
<template>
  <main>
    <button @click="getEmployees">recuperar los datos de forma manual</button>
    <div v-if="employees!==null" >
      <filter-element 
        filterName="Edad" 
        :options="ageFilter" 
        @new-filter="(filter) => currentAgeFilterSelection = filter">
      </filter-element>
      <filter-element 
        filterName="Salario" 
        :options="salaryFilter" 
        @new-filter="(filter) => currentSalaryFilterSelection = filter">
      </filter-element>
      <custom-table :content="filteredEmployees"/>
    </div>
    <div v-else>
    {{ error }}
    </div>
  </main>
</template>

<script>
export default {
  async mounted(){
    await this.getEmployees();
  },
  computed:{
    filteredEmployees(){
      if(this.employees.data){
        let list = [...this.employees.data];
        if(this.currentAgeFilterSelection > 0){
          list = [...this.filterEmployeesByAge(list, 45, this.currentAgeFilterSelection === "1")];
        }
        if(this.currentSalaryFilterSelection > 0){
          list = [...this.filterEmployeesBySalary(list, 200000, this.currentSalaryFilterSelection === "1")];
        }
        return list;
      }
      return null;      
    }
  },
  methods:{
    getEmployees(){
      axios
        .get('https://dummy.restapiexample.com/api/v1/employees')
        .then(response => (this.employees = response.data))
        .catch(error => (this.error = error.message));
    },
    filterEmployeesByAge(list,limit,lessThan){
      if(list){
        if(lessThan){
          return list.filter(employee => employee.employee_age < limit);
        }
        else{
          return list.filter(employee => employee.employee_age >= limit);
        }
      }
      return null;
    },
    filterEmployeesBySalary(list,limit,lessThan){
      if(list){
        if(lessThan){
          return list.filter(employee => employee.employee_salary < limit);
        }
        else{
          return list.filter(employee => employee.employee_salary >= limit);
        }
      }
      return null;
    },

  },
  data(){
    return{
      employees: null,
      error: "",
      ageFilter:[
        {id:0, label: 'sin filtro'},
        {id:1, label: 'menos de 45'},
        {id:2, label: 'mayor o igual de 45'}
      ],
      currentAgeFilterSelection: 0,
      salaryFilter:[
        {id:0, label: 'sin filtro'},
        {id:1, label: 'menos de 200.000'},
        {id:2, label: 'mayor o igual de 200.000'}
      ],
      currentSalaryFilterSelection:0
    }
  }
}
</script>
