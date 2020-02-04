<template>
  <v-card>
    <v-card-title>
      <div>
        Student List
      </div>
      <v-spacer />
      <v-btn
        color="blue-grey darken-2"
        class="white--text"
        depressed
        @click="state.openUserForm = true"
      >
        Create New
      </v-btn>
    </v-card-title>
    <v-divider class="mt-0" />
    <v-data-table :items="state.userDetails" :headers="headers">
      <template slot="item" slot-scope="props">
        <tr class="">
          <td class="py-3">
            <div>{{ props.item.name }}</div>
          </td>
          <td class="py-3">
            {{ props.item.batch }}
          </td>
          <td class="py-3">
            {{ props.item.course }}
          </td>
          <td class="py-3">
            {{ props.item.role }}
          </td>
        </tr>
      </template>
    </v-data-table>
    <v-dialog
      v-model="state.openUserForm"
      scrollable
      @keypress.esc="state.openUserForm = false"
    >
      <add-new-user @close="getUserDetails(), (state.openUserForm = false)" />
    </v-dialog>
  </v-card>
</template>
<script>
import { reactive, onMounted } from "@vue/composition-api"
import AddNewUser from "../../../components/Users/AddNewUser"
export default {
  components: { AddNewUser },
  setup(_, { root: { $axios } }) {
    const headers = [
      { text: "Name", value: "name", sortable: false },
      { text: "Batch", sortable: false },
      { text: "Courses", sortable: false },
      { text: "Role", sortable: false }
    ]
    const state = reactive({
      userDetails: [],
      openUserForm: false
    })
    const getUserDetails = () => {
      $axios.$get("api/v1/users").then(response => {
        state.userDetails = [...response]
      })
    }
    onMounted(() => {
      getUserDetails()
    })
    return {
      getUserDetails,
      state,
      headers
    }
  }
}
</script>