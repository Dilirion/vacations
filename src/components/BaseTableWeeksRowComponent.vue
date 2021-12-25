<template>
<tbody>
    <tr v-if="allWeeks.length" class="d-flex justify-content-center">
        <td class="border-start border-end border-bottom" v-for="week in month.weeks" :key="week">
            <div class="rotate"> {{allWeeks[week-1].dates}} </div>
        </td>
    </tr>
    <tr class="d-flex justify-content-center">
        <td class="border-start border-end" v-for="week in month.weeks" :key="week" style="width:100%;">{{week}}</td>
    </tr>
</tbody>
</template>

<script>

export default {
    data() {
        return {
            allWeeks: []
        }
    },
    props:{
        "month": Object,
    },
    mounted() {
        // fills the array of weeks starting from first week which is 01.01-03.01
        this.allWeeks.push({id: 1, dates: "01.01-03.01"})
        for (let i = 2; i < 53+1; i++) {
            let lastDay = this.weekDates(3+(i-1)*7);
            let firstDay = this.weekDates(3+(i-1)*7-6)
            this.allWeeks.push({id: i, dates: `${firstDay}-${lastDay}`})            
        }
    },
    methods: {
        // returns string 'day.month' from passed number of the day of the year
         weekDates(number) {
            let oneJan = new Date("2021-01-01")
            let date = new Date(oneJan.getTime() + (number-1)*24 * 60 * 60 * 1000)
            let strmonth = date.getMonth()+1 < 10 ? `0${date.getMonth()+1}` : `${date.getMonth()+1}`
            let strday = date.getDate() < 10 ? `0${date.getDate()}` : `${date.getDate()}`
            return strday + '.' + strmonth
        },
    }
}
</script>

<style scoped>
 .rotate {
  transform: rotate(-90deg);
  -webkit-transform: rotate(-90deg);
  -moz-transform: rotate(-90deg);
  -ms-transform: rotate(-90deg);
  -o-transform: rotate(-90deg);
  filter: progid:DXImageTransform.Microsoft.BasicImage(rotation=3); 
}  
</style>
