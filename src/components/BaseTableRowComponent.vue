<template>
    <tr scope="row" class="d-flex justify-content-center">
        <td scope="col" v-for="week in month.weeks" :key="week" class="d-flex align-items-center justify-content-center" 
            :class="{'colored': weeksOfVacations.weeks.includes(week), 'border-start border-end' : month.isSelected}" 
            style="width:100%; height: 40px;">
            <!-- this element will be showed only in vacations cells provided this month is selected -->
            <div class="text-white" v-if="month.isSelected && weeksOfVacations.weeks.includes(week)">
                {{findVacation(week)}}
            </div>
        </td> 
    </tr>
</template>

<script>

export default {
    data() {
        return {
        }
    },
    props: {
        "weeksOfVacations": Object,
        "month": Object,
        "vacations": Array
    },
    methods: {
        // gets the week number and returns vacation's start and end days in string
        findVacation(number) {
            let vacation_id = -1
            for (let i = 0; i < this.weeksOfVacations.vacations.length; i++) {
                // check is the passed week in each vacation's list of weeks, breaks when true
                if (this.weeksOfVacations.vacations[i].includes(number)) {
                    vacation_id = i
                    break
                }
            }
            if (vacation_id >= 0) return `${this.vacations[vacation_id].startDate.substring(8)}-${this.vacations[vacation_id].endDate.substring(8)}`
        }
    }
}
</script>

<style scoped>
.colored {
    background-color: rgb(51, 119, 207);
}
</style>
