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
        }
      ],
      employees: [],
      editedIndex: -1,
      editedEmployer: {
        firstname: '',
        surname: '',
        patronymic: '',
        jobtitle: ''
      },
      defaultEmployer: {
        firstname: '',
        surname: '',
        patronymic: '',
        jobtitle: ''
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
      ]
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