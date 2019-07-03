<template>
  <v-app id="inspire">
    <v-toolbar color="indigo" dark fixed app>
      <v-toolbar-title>Collector: your debt listing</v-toolbar-title>
    </v-toolbar>

    <v-content>

    <v-layout>
      <v-flex xs12 sm6 offset-sm3>
        
        <v-card v-for="debt in debts" class="my-2">
          <v-card-text xs6>
              <p class="grey--text text-left pa-0 ma-0">{{ debt.title }}</p>
              <p class="text-xs-right pa-0 ma-0">{{ debt.amount }} zł</p>
              <p class="text-sm-left pa-0 ma-0">{{ debt.who }}</p>
              <v-btn fab absolute bottom right small color="red" @click="deleteing">
                <v-icon>remove</v-icon>
              </v-btn>
          </v-card-text>
        </v-card>
        

      </v-flex>
    </v-layout>
    </v-content>

    <v-footer
      dark
      height="auto"
    >
    <v-card
      class="flex "
    >
      <v-card-title class="teal">
        <v-form v-model="newDebt.valid">
          <v-container>
            <v-layout>
              <v-flex
                xs12
              >
                <v-text-field
                  v-model="newDebt.title"
                  :counter="15"
                  label="Nazwa"
                  required
                ></v-text-field>
              </v-flex>

              <v-flex
                xs12
              >
                <v-text-field
                  v-model="newDebt.who"
                  :counter="15"
                  label="Dłuznik"
                  required
                ></v-text-field>
              </v-flex>

              <v-flex
                xs12
              >
                <v-text-field
                  v-model="newDebt.amount"
                  label="Kwota"
                  required
                ></v-text-field>
              </v-flex>
              <v-btn 
                color="success"
                fab
                @click="submit"
              >
                <v-icon dark>add</v-icon>
              </v-btn>
            </v-layout>
          </v-container>
        </v-form>

        <v-spacer></v-spacer>

        
      </v-card-title>

      <v-card-actions class="darken-3 justify-center">
        <span class="white--text">Collector by Robert &copy; {{ currentYear }}</span>
      </v-card-actions>
    </v-card>
  </v-footer>

  </v-app>
</template>

<script>
  import axios from "axios";
  export default {
    data: () => ({
      debts: [
        // {
        //   title: 'Nazwa długu',
        //   who: 'Nazwa dłuznika',
        //   amount: 'Kwota długu',
        // },
        // {
        //   title: 'Nazwa długu 2',
        //   who: 'Nazwa dłuznika 2',
        //   amount: 'Kwota długu 2',
        // },
      ],
      drawer: null,
      newDebt: 
      {
        valid: true,
        title: '',
        who: '',
        amount: '',
        },
    }),
    props: {
      source: String
    },
    computed: {
      currentYear: function () {
        return new Date().getFullYear()
      }
    },
    async mounted() {
      this.getDebts()
    },
    methods: {
      async getDebts() {
        const response = await axios.get('http://localhost:3000/debts');
        console.log('debts', response.data);
        this.debts = response.data
      },
      async createDebt() {
        await axios.post('http://localhost:3000/debts', this.newDebt);
        this.newDebt = {
          valid: true,
          title: '',
          who: '',
          amount: '',
        }
      },
      async submit() {
        console.log('in submit', this.newDebt.title);
        await this.createDebt();
        await this.getDebts();
      
      }
    }
  }
</script>

<style>

</style>
