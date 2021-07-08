<template>
  <v-container class="grey darken-4">
    <v-row class="align-center">
      <v-col cols="4" dark class="d-flex justify-center">Name</v-col>
      <v-col cols="7" class="d-flex justify-center">Desciption</v-col>
      <v-col cols="1" class="d-flex justify-center">
        <v-btn @click.stop="createDialog = true"
          ><v-icon>mdi-note-plus</v-icon></v-btn
        >
        <v-dialog v-model="createDialog" max-width="500">
          <v-card>
            <v-card-title>Create todo</v-card-title>
            <v-card-actions>
              <v-text-field
                v-model="createTodoProps.name"
                label="Todo name"
                counter
                maxlength="20"
              ></v-text-field>
              <v-spacer></v-spacer>
              <v-text-field
                v-model="createTodoProps.description"
                label="Todo description"
                counter
                maxlength="40"
              ></v-text-field>
              <v-spacer></v-spacer>
              <v-btn v-on:click="$emit('create-todo', createTodoProps)"
                >SUBMIT</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-col>
    </v-row>
    <v-row class="grey darken-3" v-for="i in todolist" :key="i.id">
      <v-col cols="4"
        ><p dark class="d-flex justify-center">{{ i.name }}</p></v-col
      >
      <v-divider vertical></v-divider>
      <v-col cols="6" class="d-flex justify-center">{{ i.description }}</v-col>
      <v-col cols="1" class="d-flex justify-center">
        <v-btn icon class="red darken-2" v-on:click.stop="editDialog = true"
          ><v-icon>mdi-pencil</v-icon>
        </v-btn>
        <v-dialog v-model="editDialog" max-width="500">
          <v-card>
            <v-card-title>Edit todo</v-card-title>
            <v-card-actions>
              <v-text-field
                v-model="editTodoProps.name"
                counter
                maxlength="20"
                label="Todo name"
              ></v-text-field>
              <v-spacer></v-spacer>
              <v-text-field
                v-model="editTodoProps.description"
                label="Todo description"
                counter
                maxlength="40"
              ></v-text-field>
              <v-spacer></v-spacer>
              <v-btn
                v-on:click="
                  $emit('edit-todo', {
                    id: i.id,
                    name: editTodoProps.name,
                    description: editTodoProps.description,
                  })
                "
                >SUBMIT</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-col>
      <v-col cols="1" class="d-flex justify-center">
        <v-btn icon class="red darken-2" v-on:click="$emit('delete-todo', i.id)"
          ><v-icon>mdi-trash-can</v-icon>
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "",
  data() {
    return {
      createTodoProps: {
        name: null,
        description: null,
      },
      editTodoProps: {
        name: null,
        description: null,
      },
      createDialog: false,
      editDialog: false,
      rules: {
        counter20: (value) => value.length() <= 20 || "Max of 20 characters",
        counter40: (value) => value.length() <= 40 || "Max of 40 characters",
      },
    };
  },
  components: {},
  props: ["todolist"],
  mounted() {
    console.log(this.todolist);
  },
};
</script>

<style>
</style>