<template>
  <div class="container">
      <TableFilter v-model:client-name="filterName" v-model:client-id="filterId" v-model:client-payment="filterPayment"
        v-model:status-id="filterStatusId" />
      <div class="table-container">
        <table class="table table-striped main_table">
          <TableHead v-model:sort="sort" />
          <TableBody v-model:data="sortedData" />
        </table>
      </div>
  </div>
</template>

<script>
import jsonData from './data/data.json'
import TableBody from './components/TableBody.vue'
import TableHead from './components/TableHead.vue'
import TableFilter from './components/TableFilter.vue'

export default {
  components: { TableBody, TableHead, TableFilter },
  data() {
    return {
      data: jsonData,
      sort: {
        field: '',
        desc: false,
      },
      filterId: 0,
      filterName: 0,
      filterPayment: 0,
      filterStatusId: 0,
    }
  },

  methods: {
    dateValue(date1, date2) {
      const days = this.culcDate(date1, date2);
      if (days.beforeDays > 0) {
        return 1
      } else if (days.finishDays < -1) {
        return 2
      } else if (days.finishDays < 0) {
        return 3
      } else if (days.finishDays < 1) {
        return 4
      } else {
        return 5
      }
    },

    dietArr(dietArr) {
      if (this.sort.desc) {
        return Math.max.apply(null, dietArr)
      } else {
        return Math.min.apply(null, dietArr)
      }
    },

    dateIndex(dateArr) {
      let newArr = []

      for (let date of dateArr) {
        const days = this.culcDate(date.end_date, date.start_date);
        if (days.beforeDays > 0) {
          newArr.push(days.beforeDays * 10000)
        } else if (days.finishDays < -1) {
          newArr.push(Math.abs(days.finishDays) * -1000000)
        } else if (days.finishDays < 0) {
          newArr.push(Math.abs(days.finishDays) * -100)
        } else if (days.finishDays < 1) {
          newArr.push(Math.abs(days.finishDays) + -1000)
        } else {
          newArr.push(Math.abs(days.finishDays) * -10000)
        }
      }

      if (this.sort.desc) {
        return Math.max.apply(null, newArr)
      } else {
        return Math.min.apply(null, newArr)
      }
    },

    doSort(field) {
      if (field == this.sort.field) {
        this.sort.desc = !this.sort.desc;
      } else {
        this.sort.field = field;
        this.sort.desc = false;
      }
    },

    culcDate(date1, date2) {
      let today = new Date()
      let finishDate = new Date(date1.replace(new RegExp (/ - /), "/"))
      let beforeDate = new Date(date2.replace(new RegExp (/ - /), "/"))
      let finishDays = Math.floor((finishDate - today) / 86400000)
      let beforeDays = Math.floor((beforeDate - today) / 86400000)
      return { finishDays, beforeDays }
    },
  },

  computed: {
    filteredClients() {
      let filteredClients = this.data;
      if (this.filterId) {
        filteredClients = filteredClients.filter(client => String(client.o_id).includes(String(this.filterId)))
      }

      if (this.filterName) {
        filteredClients = filteredClients.filter(client => String(client.client_name).includes(String(this.filterName)))
      }

      if (this.filterPayment) {
        filteredClients = filteredClients.filter(client => {
          let payStatus = ''
          if (client.pay_status == "Оплачен") {
            payStatus = 1
          } else {
            payStatus = 2
          }
          if (payStatus == this.filterPayment) {
            return true
          }
        })
      }

      if (this.filterStatusId) {
        filteredClients = filteredClients.filter(client => {
          const newArr = [0]
          for (let date of client.dates) {
            const culcedDate = this.dateValue(date.end_date, date.start_date)
            newArr.push(culcedDate)
          }
          if (newArr.includes(this.filterStatusId)) {
            return true
          }
        })
      }
      return filteredClients;
    },

    sortedData() {
      let newData = this.filteredClients;
      if (!this.sort.field) {
        return newData;
      }
      if (this.sort.field == 'date_index') {
        for (let item of newData.concat()) {
          item.date_index = this.dateIndex(item.dates)
        }
      }
      if (this.sort.field == 'diet') {
        for (let item of newData.concat()) {
          item.diet = this.dietArr(item.diets)
        }
      }
      return newData.concat().sort((a, b) => {
        if (this.sort.desc) {
          return a[this.sort.field] > b[this.sort.field] ? -1 : 1;
        } else {
          return a[this.sort.field] < b[this.sort.field] ? -1 : 1;
        }
      })
    }
  }
}
</script>

<style></style>
