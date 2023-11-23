<template>
  <form action="" class="form-container" @submit.prevent="submit">
    <div class="form-group">
      <label for="formControlInput2">ID клиента</label>
      <input id="formControlInput2" type="number" class="form-control" placeholder="ID" v-model.number="currentId">
    </div>
    <div class="form-group">
      <label for="formControlInput1">Фамилия, Имя, Отчество</label>
      <input id="formControlInput1" type="text" class="form-control" placeholder="ФИО" v-model="currentName">
    </div>
    <div class="form-group">
      <label for="formControlSelect1">Статус оплаты</label>
      <select class="form-control" id="formControlSelect1" v-model.number="currentPayment">
        <option value="0">Не выбрано</option>
        <option value="1">Оплачен</option>
        <option value="2">Неоплачен</option>
      </select>
    </div>
    <div class="form-group">
      <label for="formControlSelect2">Статус заказа</label>
      <select class="form-control" id="formControlSelect2" v-model.number="currentStatusId">
        <option value="0">Не выбрано</option>
        <option value="1">Начинается через Х дней</option>
        <option value="3">Заканчивается сегодня</option>
        <option value="4">Заканчивается завтра</option>
        <option value="5">Заканчивается через Х дней</option>
        <option value="2">Закончилось Х дней назад</option>
      </select>
    </div>
    <button type="submit" class="btn btn-primary btn-search">Поиск</button>
    <button type="button" @click.prevent="reset" class="btn btn-primary">Сброс</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      currentId: '',
      currentName: '',
      currentPayment: 0,
      currentStatusId: 0,
    }
  },
  props: ['clientId', 'clientName', 'clientPayment', 'statusId'],

  watch: {
    clientId(value) {
      this.currentId = value;
    },
    clientName(value) {
      this.currentName = value;
    },
    clientPayment(value) {
      this.currentPayment = value;
    },
    statusId(value) {
      this.currentStatusId = value;
    }
  },
  methods: {
    submit() {
      this.$emit('update:clientId', this.currentId)
      this.$emit('update:clientName', this.currentName)
      this.$emit('update:clientPayment', this.currentPayment)
      this.$emit('update:statusId', this.currentStatusId)
    },

    reset() {
      this.$emit('update:clientId', '')
      this.$emit('update:clientName', '')
      this.$emit('update:clientPayment', 0)
      this.$emit('update:statusId', 0)
    }
  }
}
</script>