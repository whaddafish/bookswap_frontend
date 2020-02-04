<template>
  <v-card>
    <v-card-title>
      Add New User
    </v-card-title>
    <v-divider class="my-0" />
    <v-card-text>
      <v-row>
        <v-col cols="6">
          <v-text-field v-model="state.formValues.name" label="Name" />
        </v-col>
        <v-col cols="6">
          <v-text-field
            v-model="state.formValues.email"
            type="email"
            label="Name"
          />
        </v-col>
        <v-col cols="6">
          <v-text-field v-model="state.formValues.phone" label="Phone" />
        </v-col>
        <v-col cols="6">
          <v-text-field v-model="state.formValues.password" label="Password" type="password"/>
        </v-col>
        <v-col cols="6">
          <div>Gender</div>
          <v-row>
            <v-col
              v-for="(gender, index) in genderChoices"
              :key="index"
              cols="auto"
            >
              <v-btn
                :outlined="state.formValues.gender !== gender"
                rounded
                depressed
                color="blue-grey darken-2"
                class="white--text"
                @click="state.formValues.gender = gender"
              >
                {{ gender }}
              </v-btn>
            </v-col>
          </v-row>
        </v-col>
        <v-col cols="6">
          <v-select
            v-model="state.formValues.address"
            label="Address"
          />
        </v-col>
        <v-col cols="6">
          <v-menu
            v-model="state.datePicker"
            :close-on-content-click="false"
            :nudge-right="40"
            transition="scale-transition"
            offset-y
            min-width="290px"
          >
            <template v-slot:activator="{ on }">
              <v-text-field
                v-model="state.formValues.dob"
                label="Date of birth"
                prepend-icon="mdi-calendar"
                readonly
                v-on="on"
              />
            </template>
            <v-date-picker
              v-model="state.formValues.dob"
              @input="state.datePicker = false"
            />
          </v-menu>
        </v-col>
      </v-row>
    </v-card-text>
    <v-divider class="my-0" />
    <v-card-actions>
      <v-spacer />
      <v-btn color="blue-grey" class="white--text" @click="createUser">
        Save
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  name: "AddNewUser",
  setup(_, { root: { $axios } }) {
    // const variables
    const genderChoices = ["Male", "Female", "Others"]
    // reactive component
    const state = reactive({
      datePicker: false,
      userCourseChoices: [],
      userBatchChoices: [],
      formValues: {
        gender: "Male"
      }
    })
    const createUser = () => {
      const dataToPost = new FormData()
      dataToPost.append("name", state.formValues.name)
      dataToPost.append("email", state.formValues.email)
      dataToPost.append("phone", state.formValues.phone)
      dataToPost.append("password", state.formValues.password)
      dataToPost.append("gender", state.formValues.gender)
      dataToPost.append("address", state.formValues.address)
      dataToPost.append("dob", state.formValues.dob)
      $axios
        .$post("/users/add", dataToPost)
        .then(() => {
          // setNotify(Successfully created ${formValues.role}.)
          this.$emit("close")
        })
        .catch(() => {
          // this.setNotify(Successfully created ${formValues.role}.)
        })
    }
    // mounted(() => {
    //   getCourse()
    //   getBatch()
    // })
    // const getCourse = () => {
    //   $axios.$get("api/v1/course").then(response => {
    //     state.userCourseChoices = response.result
    //   })
    // }
    // // batch details
    // const getBatch = () => {
    //   $axios.$get("api/v1/batch").then(response => {
    //     state.userBatchChoices = response
    //   })
    // }
    return {
      createUser,
      // userRoleChoices,
      genderChoices,
      // state,
      // getCourse,
      // getBatch
    }
  }
}
</script>

<style scoped></style>