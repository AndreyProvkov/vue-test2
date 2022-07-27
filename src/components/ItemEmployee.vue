<template>
    <div>
        <v-app-bar
            color="white"
            elevation="2"
        >
            <v-btn icon to="/">
                <v-icon>mdi-arrow-left-bold</v-icon>
            </v-btn>

            <v-toolbar-title>Информация о сотруднике</v-toolbar-title>
        </v-app-bar>
        <v-card class="mt-1">
            <v-card-title>
                <span>{{ fullName }}</span>
            </v-card-title>
            <v-container>
                <v-row>
                    <v-col
                        cols="2"
                    >
                        <span>Должность:</span>
                    </v-col>
                    <v-col
                        cols="10"
                    >
                        {{ employee.jobtitle }}
                    </v-col>
                </v-row>
                <v-row :align="'center'">
                    <v-col
                        cols="2"
                    >
                        <span>Трудовая книжка сдана:</span>
                    </v-col>
                    <v-col
                        cols="10"
                    >
                        <v-icon v-if="employee.workBookSubmitted">mdi-checkbox-marked-outline</v-icon>
                        <v-icon v-else>mdi-close-box-outline</v-icon>
                    </v-col>
                </v-row>
                <v-row>
                    <v-col
                        cols="2"
                    >
                        <span>Оклад:</span>
                    </v-col>
                    <v-col
                        cols="10"
                    >
                        {{ employee.salary.toLocaleString() }} руб.
                    </v-col>
                </v-row>
                <v-row :align="'center'">
                    <v-col
                        cols="2"
                    >
                        <span>Дата выхода на работу:</span>
                    </v-col>
                    <v-col
                        cols="10"
                    >
                        {{ new Date(employee.startDate).toLocaleString('ru-RU', {
                            dateStyle: 'short'
                        }) }}
                    </v-col>
                </v-row>
                <v-row>
                    <v-col
                        cols="2"
                    >
                        <span>Ставка:</span>
                    </v-col>
                    <v-col
                        cols="10"
                    >
                        {{ employee.time }}
                    </v-col>
                </v-row>
            </v-container>
        </v-card>
        
    </div>
</template>

<script>
export default {
  name: 'ItemEmployee',
  data() {
    return {
        employee: JSON.parse(this.item)
    }
  },
  props: {
    item: String
  },
  computed: {
    fullName() {
        return `${this.employee.firstname} 
                ${this.employee.surname} 
                ${this.employee.patronymic}`
    }
  }
}
</script>