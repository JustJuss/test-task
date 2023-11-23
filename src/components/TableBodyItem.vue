<template>
  <td class="table-success table__td table__id">
    <p>
      {{ item.o_id }}
    </p>
  </td>
  <td class="table__td table__name">
    <p>
      {{ item.client_name }}
    </p>
  </td>
  <td class="table__td table__diet">
    <table class="table_in_table">
      <tbody>
        <tr v-for="diet in item.diets" :key="diet">
          <p>
            {{ diet }}
          </p>
        </tr>
      </tbody>
    </table>
  </td>
  <td class="table__td table__tariff">
    <table class="table_in_table">
      <tbody>
        <tr v-for="tariff in item.tariff" :key="tariff">
          <p>
            {{ tariff }}
          </p>
        </tr>
      </tbody>
    </table>
  </td>
  <td class="table__td table__address">
    <p>
      {{ item.address }}
    </p>
  </td>
  <td class="table__td table__phone">
    <p>
      {{ item.phone }}
    </p>
  </td>
  <td class="table__td table__date">
    <table class="table_in_table">
      <tbody>
        <tr v-for="date in item.dates" :key="date">
          <p>
            {{ normalizeDate(date.start_date) }} - <br> {{ normalizeDate(date.end_date) }}
          </p>
        </tr>
      </tbody>
    </table>
  </td>
  <td class="table__td table__sum" :class="debtStatus(item.pay_status)">
    <table class="table_in_table">
      <tr>
        <td>
          <p>
            Стоим.: {{ item.order_sum }}
          </p>
        </td>
      </tr>
      <tr>
        <td>
          <p>
            {{ item.pay_status }}
          </p>
        </td>
      </tr>
      <tr v-if="item.discount">
        <td>
          <p>
            Скидка: {{ item.discount }}%
          </p>
        </td>
      </tr>
      <tr v-if="item.order_sum - item.order_payed != 0">
        <td>
          <p>
            {{ culcDebt(item.order_sum, item.order_payed) }}
          </p>
        </td>
      </tr>
    </table>
  </td>
  <td class="table__td table__comment">
    <div class="table__comment-deco">
      <p>
        {{ item.courier_comment }}
      </p>
    </div>
  </td>
  <td class="table__td table__comment">
    <div class="table__comment-deco">
      <p>
        {{ item.inner_comment }}
      </p>
    </div>
  </td>
  <td class="table__td table__finish table-info">
    <table class="table_in_table">
      <tbody>
        <tr v-for="date in item.dates" :key="date">
          <p>
            {{ culcFinish(date.end_date, date.start_date) }}
          </p>
        </tr>
      </tbody>
    </table>
  </td>
</template>

<script>
export default {
  props: ['data'],
  data() {
    return {
      item: this.data,
      months: ['янв', 'фев', 'мар', 'апр', 'май', 'июн', 'июл', 'авг', 'сент', 'окт', 'ноя', 'дек'],
    }
  },

  methods: {
    formatDate(date) {
      return new Date(date.replace(new RegExp (/ - /), "/"))
    },

    normalizeDate(date) {
      const newDate = this.formatDate(date)
      let normalizedMonth = newDate.getMonth();
      for (const [index, value] of this.months.entries()) {
        if (normalizedMonth == index) {
          normalizedMonth = value
        }
      }
      const normalized = `${newDate.getDate()} ${normalizedMonth} (${newDate.getFullYear()})`
      return normalized
    },

    culcDebt(sum1, sum2) {
      let debt = sum1 - sum2;
      if (debt < 0) {
        return `Переплата: ${Math.abs(debt.toFixed(2))}`
      } else {
        return `Долг: ${Math.abs(debt.toFixed(2))}`
      }
    },

    culcFinish(date1, date2) {
      let today = new Date()
      let finishDate = this.formatDate(date1)
      let beforeDate = this.formatDate(date2)
      let finishDays = Math.floor((finishDate - today) / 86400000)
      let beforeDays = Math.floor((beforeDate - today) / 86400000)
      if (beforeDays > 0) {
        return `Начинается через ${Math.abs(beforeDays)} дней`
      } else if (finishDays < -1) {
        return `Завершилось ${Math.abs(finishDays)} дней назад`
      } else if (finishDays < 0) {
        return `Заканчивается сегодня`
      } else if (finishDays < 1) {
        return `Заканчивается завтра`
      } else {
        return `Заканчивается через ${Math.abs(finishDays)} дней`
      }
    },

    debtStatus(status) {
      if (status === "Неоплачен ч.") {
        return "table-danger"
      } else {
        return "table-success"
      }
    },
  },
}
</script>