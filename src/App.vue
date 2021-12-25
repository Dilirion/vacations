<template>
  <div class="table-responsive mx-5">
    <table class="table table-striped table-bordered">
      <thead>
        <tr>
          <th scope="col"> Employees </th>
          <th scope="col" class="head" v-for="month in months" :key="month.id" @click="selectMonth(month.id)" style="min-width: 85px;">{{month.title}}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-if="isAnySelected">
          <th scope="row"></th>
          <td v-for="month in months" :key="month.id" class="p-0" > 
            <base-weeks-row v-if="month.isSelected" :month="month"></base-weeks-row>
          </td>
        </tr>
        <tr v-for="employee in myData" :key="employee.id">
          <th scope="row">{{employee.person.name}}</th>
          <td scope="row" class="p-0" v-for="month in months" :key="month.id">
            <base-table-row :month="month" :weeksOfVacations="getWeekNumbers(employee)" :vacations="employee.vacations"></base-table-row>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import BaseTableRow from './components/BaseTableRowComponent.vue'
import BaseWeeksRow from './components/BaseTableWeeksRowComponent.vue'

export default {
  name: 'App',
  components: {
    BaseTableRow, BaseWeeksRow
  },
  data() {
    return {      
      myData: [],
      isAnySelected: false,
      months: [{id: 1, title: "January", isSelected: false, weeks: [1,2,3,4,5]}, {id: 2, title: "February", isSelected: false, weeks: [6, 7, 8, 9]}, {id: 3, title: "March", isSelected: false, weeks: [10,11,12,13,14]}, {id: 4, title: "April", isSelected: false, weeks: [14,15,16,17,18]},
        {id: 5, title: "May", isSelected: false, weeks: [18,19,20,21,22,23]}, {id: 6, title: "June", isSelected: false, weeks: [23,24,25,26,27]}, {id: 7, title: "July", isSelected: false, weeks: [27,28,29,30,31]}, {id: 8, title: "August", isSelected: false, weeks: [31,32,33,34,35,36]},
        {id: 9, title: "September", isSelected: false, weeks: [36,37,38,39,40]}, {id: 10, title: "October", isSelected: false, weeks: [40,41,42,43,44]}, {id: 11, title: "November", isSelected: false, weeks: [45,46,47,48,49]},
        {id: 12, title: "December", isSelected: false, weeks: [49,50,51,52,53]}],
    }
  },
  mounted() {
    fetch('http://localhost:3000/data')
    .then(res => res.json())
    .then(data => {this.myData = data})    
    .catch(err => console.log(err.message))
  },
  methods: {
    // returns two arrays: first for all weeks which are included in all employee's vacations 
    // and second for weeks which are included in each employee's vacation
    getWeekNumbers(emp) {
      let weekNumbers = [], weeksOfVacations = []
      for (let i = 0; i < emp.vacations.length; i++) {
        weeksOfVacations.push([])
        let startWeek = this.weekNumber(new Date(emp.vacations[i].startDate))
        let endWeek = this.weekNumber(new Date(emp.vacations[i].endDate))
        for (let j = startWeek; j <= endWeek; j++) {
          weekNumbers.push(j);
          weeksOfVacations[i].push(j)
        }
      }
      return {weeks: weekNumbers, vacations: weeksOfVacations}
    },
    // returns number of the week of the year corresponding to the passed date
    weekNumber (date) {
      let oneJan = new Date("2021-01-01")
      let numberOfDays = Math.floor((date - oneJan) / (24 * 60 * 60 * 1000)) + 1
      let result = Math.ceil((numberOfDays - 3) / 7 + 1)
      return result
    },
    // checks was the clicked month selected before or not, 
    // sets the true value for isAnySelected and for this particular month.isSelected
    selectMonth(month_id) {
      if(this.isAnySelected && this.months[month_id-1].isSelected) this.isAnySelected = false
      else this.isAnySelected = true
      this.months.forEach(month => (month.id == month_id) ? month.isSelected = !month.isSelected : month.isSelected = false)
      console.log(month_id, this.months[month_id-1].id, this.months[month_id-1].isSelected)
    }
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
.head {
  cursor: pointer;
}
</style>
