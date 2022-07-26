<template>
  <v-data-table
    :headers="headers"
    :items="employees"
    hide-default-footer
    class="elevation-1"
  ></v-data-table>
</template>

<script>
export default {
  name: 'ListEmployee',
  data () {
    return {
      headers: [
        { text: 'Имя', value: 'firstname' },
        { text: 'Фамилия', value: 'surname' },
        { text: 'Отчество', value: 'patronymic' },
        { text: 'Должность', value: 'jobtitle' }
      ],
      employees: [
        {
          id: 1,
          firstname: 'Эстелла',
          surname: 'Щербакова',
          patronymic: 'Борисовна',
          jobtitle: 'Менеджер'
        },
        {
          id: 2,
          firstname: 'Иоанна',
          surname: 'Красильникова',
          patronymic: 'Христофоровна',
          jobtitle: 'Бухгалтер'
        },
        {
          id: 3,
          firstname: 'Марфа',
          surname: 'Мэлсовна',
          patronymic: 'Мельникова',
          jobtitle: 'Диспетчер'
        },
        {
          id: 4,
          firstname: 'Мэри',
          surname: 'Субботина',
          patronymic: 'Антониновна',
          jobtitle: 'Психолог'
        },
        {
          id: 5,
          firstname: 'Аким',
          surname: 'Марков',
          patronymic: 'Куприянович',
          jobtitle: 'Механик'
        },
        {
          id: 6,
          firstname: 'Лев',
          surname: 'Субботин',
          patronymic: 'Львович',
          jobtitle: 'Инженер'
        },
        {
          id: 7,
          firstname: 'Валерий',
          surname: 'Коновалов',
          patronymic: 'Геннадиевич',
          jobtitle: 'Брокер'
        },
        {
          id: 8,
          firstname: 'Артур',
          surname: 'Ситников',
          patronymic: 'Лукьевич',
          jobtitle: 'Аналитик'
        }
      ],
    }
  }
}
</script>

<style>
</style>