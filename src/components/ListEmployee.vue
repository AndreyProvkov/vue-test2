<template>
  <v-data-table
    :headers="headers"
    :items="employees"
    :search="search"
    sort-by="firstname"
    hide-default-footer
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-text-field
          v-model="search"
          append-icon="mdi-magnify"
          placeholder="Поиск"
          single-line
          hide-details
        ></v-text-field>

        <v-spacer></v-spacer>

        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              Добавить сотрудника
            </v-btn>
          </template>

          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedEmployer.firstname"
                      label="Имя"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedEmployer.surname"
                      label="Фамилия"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedEmployer.patronymic"
                      label="Отчество"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedEmployer.jobtitle"
                      label="Должность"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-checkbox
                      v-model="editedEmployer.workBookSubmitted"
                      :label="`Трудовая книжка сдана`"
                    ></v-checkbox>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model.number="editedEmployer.salary"
                      label="Оклад"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-menu
                      v-model="menuDate"
                      :close-on-content-click="false"
                      :nudge-right="40"
                      transition="scale-transition"
                      offset-y
                      min-width="auto"
                    >
                      <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                          v-model="editedEmployer.startDate"
                          label="Дата выхода на работу"
                          prepend-icon="mdi-calendar"
                          readonly
                          v-bind="attrs"
                          v-on="on"
                        ></v-text-field>
                      </template>
                      <v-date-picker
                        v-model="editedEmployer.startDate"
                        @input="menuDate = false"
                      ></v-date-picker>
                    </v-menu>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-select
                      v-model.number="editedEmployer.time"
                      :items="times"
                      label="Ставка"
                    ></v-select>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Отмена
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Сохранить
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">Удалить сотрудника?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Отмена</v-btn>
              <v-btn color="blue darken-1" text @click="deleteEmployerConfirm">Да</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>

    <template v-slot:[`item.actions`]="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editEmployer(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        small
        @click="deleteEmployer(item)"
      >
        mdi-delete
      </v-icon>
    </template>

    <template v-slot:[`item.showAll`]="{ item }">
      <v-icon
        small
        @click="showAllInfo(item)"
      >
        mdi-open-in-new
      </v-icon>
    </template>

    <template v-slot:no-data>
      <h2>Сотрудники отсутствуют</h2>
    </template>

    <template v-slot:no-results>
      <p>Нет совпадений</p>
    </template>
  </v-data-table>
</template>

<script>
export default {
  name: 'ListEmployee',
  data () {
    return {
      search: '',
      dialog: false,
      dialogDelete: false,
      menuDate: false,
      times: ['Полная', 'Половина'],
      headers: [
        { text: 'Имя', value: 'firstname' },
        { text: 'Фамилия', value: 'surname' },
        { text: 'Отчество', value: 'patronymic' },
        { text: 'Должность', value: 'jobtitle' },
        { 
          text: 'Действие',
          value: 'actions',
          sortable: false,
          filterable: false
        },
        { 
          text: '',
          value: 'showAll',
          sortable: false,
          filterable: false
        }
      ],
      employees: [],
      editedIndex: -1,
      editedEmployer: {
        firstname: '',
        surname: '',
        patronymic: '',
        jobtitle: '',
        workBookSubmitted: false,
        salary: 0,
        startDate: "",
        time: ''
      },
      defaultEmployer: {
        firstname: '',
        surname: '',
        patronymic: '',
        jobtitle: '',
        workBookSubmitted: false,
        salary: 0,
        startDate: "",
        time: ''
      },
    }
  },
  computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'Добавление сотрудника' : 'Изменение сотрудника'
      },
    },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },
  },

  created () {
    this.initialize()
  },

  methods: {
    initialize () {
      this.employees = [
        {
          id: 0,
          firstname: 'Эстелла',
          surname: 'Щербакова',
          patronymic: 'Борисовна',
          jobtitle: 'Менеджер',
          workBookSubmitted: true,
          salary: 20000,
          startDate: "2022-09-10",
          time: 'Полная'
        },
        {
          id: 1,
          firstname: 'Иоанна',
          surname: 'Красильникова',
          patronymic: 'Христофоровна',
          jobtitle: 'Бухгалтер',
          workBookSubmitted: true,
          salary: 25000,
          startDate: "2022-08-10",
          time: 'Половина'
        },
        {
          id: 2,
          firstname: 'Марфа',
          surname: 'Мэлсовна',
          patronymic: 'Мельникова',
          jobtitle: 'Диспетчер',
          workBookSubmitted: false,
          salary: 15000,
          startDate: "2022-09-15",
          time: 'Половина'
        },
        {
          id: 3,
          firstname: 'Мэри',
          surname: 'Субботина',
          patronymic: 'Антониновна',
          jobtitle: 'Психолог',
          workBookSubmitted: false,
          salary: 23000,
          startDate: "2022-08-01",
          time: 'Полная'
        },
        {
          id: 4,
          firstname: 'Аким',
          surname: 'Марков',
          patronymic: 'Куприянович',
          jobtitle: 'Механик',
          workBookSubmitted: true,
          salary: 16000,
          startDate: "2022-07-03",
          time: 'Половина'
        },
        {
          id: 5,
          firstname: 'Лев',
          surname: 'Субботин',
          patronymic: 'Львович',
          jobtitle: 'Инженер',
          workBookSubmitted: true,
          salary: 40000,
          startDate: "2022-10-10",
          time: 'Полная'
        },
        {
          id: 6,
          firstname: 'Валерий',
          surname: 'Коновалов',
          patronymic: 'Геннадиевич',
          jobtitle: 'Брокер',
          workBookSubmitted: false,
          salary: 35000,
          startDate: "2022-06-20",
          time: 'Полная'
        },
        {
          id: 7,
          firstname: 'Артур',
          surname: 'Ситников',
          patronymic: 'Лукьевич',
          jobtitle: 'Аналитик',
          workBookSubmitted: false,
          salary: 20000,
          startDate: "2022-09-10",
          time: 'Половина'
        }
      ]
    },

    showAllInfo (item) {
      this.$router.push({
        path: `/${item.id}`,
        query: { item: JSON.stringify(item) }
      })
    },

    editEmployer (item) {
      this.editedIndex = this.employees.indexOf(item)
      this.editedEmployer = Object.assign({}, item)
      this.dialog = true
    },

    deleteEmployer (item) {
      this.editedIndex = this.employees.indexOf(item)
      this.editedEmployer = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteEmployerConfirm () {
      this.employees.splice(this.editedIndex, 1)
      this.closeDelete()
    },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedEmployer = Object.assign({}, this.defaultEmployer)
        this.editedIndex = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedEmployer = Object.assign({}, this.defaultEmployer)
        this.editedIndex = -1
      })
    },

    save () {
      if (this.editedIndex > -1) {
        Object.assign(this.employees[this.editedIndex], this.editedEmployer)
      } else {
        this.employees.push(this.editedEmployer)
      }
      this.close()
    }
  }
}
</script>

<style>
</style>