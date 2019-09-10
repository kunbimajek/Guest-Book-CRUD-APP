<template>
<v-app>

  <!-- Decalring Visitors Guest Book properties for rows and columns on data table-->
  <v-data-table :headers="headers" :items="information" :search="search" sort-by="name" class="elevation-1">

    <template v-slot:top>
      <v-toolbar flat color="white">
        <v-toolbar-title>Visitors Guest Book</v-toolbar-title>
        <div class="flex-grow-1"></div>

         <!-- search table -->
        <v-text-field  class="mr-10" v-model="search" append-icon="mdi-account-search" label="Search" single-line hide-details></v-text-field>

        <!-- Modal for adding and editing Visitor information -->
        <v-dialog v-model="dialog" max-width="500px">
          <template v-slot:activator="{ on }">
            <v-btn color="primary" dark class="mb-2" v-on="on">New Visitor</v-btn>
          </template>
          <v-card>
            <!-- Modal title -->
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>
            <!-- Modal body -->
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.name" label="Name"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.number" label="Phone Number"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.address" label="Address"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.purposeOfVisit" label="Purpose of Visit"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.timeIn" label="Time In"></v-text-field>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="editedItem.timeOut" label="Time Out"></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <!-- Modal actions -->
            <v-card-actions>
              <div class="flex-grow-1"></div>
              <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
              <v-btn color="blue darken-1" text @click="save">Save</v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>

    <!-- Table actions (edit and delete) -->
    <template v-slot:item.action="{ item }">
      <v-icon small class="mr-2" @click="editItem(item)" color="green">
        mdi-account-edit
      </v-icon>
      <v-icon small @click="deleteItem(item)" color="red">
        mdi-delete
      </v-icon>
    </template>
 
     <!-- Reset information-->
    <template v-slot:no-data>
      <v-btn color="primary" @click="initialize">Reset</v-btn>
    </template>
  </v-data-table>

</v-app>
</template>



<script>
import Home from './components/Home';

export default {
    data: () => ({
      dialog: false,
      search: '',
      //assigning names and values to table rows
      headers: [
        {
          text: 'Name',
          align: 'left',
          sortable: false,
          value: 'name',
        },
        { text: 'Phone Number', value: 'number' },
        { text: 'Address', value: 'address' },
        { text: 'Purpose of Visit', value: 'purposeOfVisit' },
        { text: 'Time In', value: 'timeIn' },
        { text: 'Time Out', value: 'timeOut' },
        { text: 'Actions', value: 'action', sortable: false },
      ],
      information: [],
      editedIndex: -1,
      editedItem: {
        name: '',
        number: '',
        address: '',
        purposeOfVisit: '',
        timeIn: '',
        timeOut: '',
      },
      defaultItem: {
        name: '',
        number: '',
        address: '',
        purposeOfVisit: '',
        timeIn: '',
        timeOut: '',
      },
    }),

    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'New Visitor' : 'Edit Visitor'
      },
    },

    watch: {
      dialog (val) {
        val || this.close()
      },
    },

    created () {
      this.initialize()
    },

    methods: {
      //dummy visitor information for table columns
      initialize () {
        this.information = [
          {
            name: 'Olumide Adeyemo',
            number: '07034567356',
            address: '3 Fatai Irawo, Ajao Estate, Lagos',
            purposeOfVisit: 'Work',
            timeIn: '8:30',
            timeOut: '11:30',
          },
          {
            name: 'Damilola Majek',
            number: '07031812960',
            address: '7 Etop Okoko Street, isolo, Lagos',
            purposeOfVisit: 'Assessment',
            timeIn: '9:13',
            timeOut: '12:00',
          },
          {
            name: 'Linda Mathew',
            number: '08147638932',
            address: '4 Adenola Street Ketu, Lagos',
            purposeOfVisit: 'Interview',
            timeIn: '9:00',
            timeOut: '12:15',
          },
        ]
      },
      //edit visitor
      editItem (item) {
        this.editedIndex = this.information.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },
      //delete visitor
      deleteItem (item) {
        const index = this.information.indexOf(item)
        confirm('Are you sure you want to delete this Visitor?') && this.information.splice(index, 1)
      },
      //close modal
      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },
      //save visitor information
      save () {
        if (this.editedIndex > -1) {
          Object.assign(this.information[this.editedIndex], this.editedItem)
        } else {
          this.information.push(this.editedItem)
        }
        this.close()
      },
    },
  }
</script>

<style>
.v-data-table {
  padding-left: 10px;
  padding-right: 10px;
}
header {
  margin-top: 20px;
  margin-bottom: 40px;
}
.v-data-table th {
  font-size: 16px !important;
  font-weight: 500;
}
.v-toolbar__title {
  font-size: 24px !important;
  font-weight: 700;
}
</style>
