<template>
  <thead class="table__head">
    <tr>
      <th>
        <a href="#" @click.prevent="doSort('o_id')">ID</a>
      </th>
      <th>
        <a href="#" @click.prevent="doSort('client_name')">ФИО</a>
      </th>
      <th>
        <a href="#" @click.prevent="doSort('diets')">
          Название диеты
        </a>
      </th>
      <th>
        <p>
          Тариф
        </p>
      </th>
      <th>
        <p>
          Адрес
        </p>
      </th>
      <th>
        <p>
          Телефон
        </p>
      </th>
      <th>
        <p>
          Дата
        </p>
      </th>
      <th>
        <p>
          Финансовые данные
        </p>
      </th>
      <th>
        <p>
          Комментарий для курьера
        </p>
      </th>
      <th>
        <p>
          Другие комментарии
        </p>
      </th>
      <th>
        <a href="#" @click="doSort('date_index')">
          Статус
        </a>
      </th>
    </tr>
  </thead>
</template>

<script>
export default {
  props: ['sort'],
  data() {
    return {
      currentSort: {
        field: '',
        desc: false,
      }
    }
  },

  watch: {
    sort(value) {
      this.currentSort = value;
    }
  },

  methods: {
    doSort(field) {
      if (field == this.currentSort.field) {
        this.currentSort.desc = !this.currentSort.desc;
      } else {
        this.currentSort.field = field;
        this.currentSort.desc = false;
      }

      this.$emit('update:sort', this.currentSort)
    },
  },
}
</script>