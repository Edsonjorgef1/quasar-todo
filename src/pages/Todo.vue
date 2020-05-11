<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        bg-color="white"
        filled
        class="col"
        placeholder="Adiciona Tarefa"
        dense
      >
        <template v-slot:append>
          <q-btn
            @click="addTask"
            @keyup.enter="addTask"
            round
            dense
            flat
            icon="add"
          />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <!--
        Rendering a <label> tag (notice tag="label")
        so QCheckboxes will respond to clicks on QItems to
        change Toggle state.
      -->

      <q-item
        v-for="(task, index) in tasks"
        v-bind:key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        v-ripple
        clickable
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            dense
            rounded
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">
        Sem tarefas!
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: [
        // {
        //   title: "Watch the Extraction ",
        //   done: false
        // },
        // {
        //   title: "Ver a Playlist",
        //   done: true
        // },
        // {
        //   title: "Terminar o job",
        //   done: false
        // }
      ]
    };
  },
  methods: {
    addTask() {
      // console.log("Adicionado");
      this.tasks.push({
        title: this.newTask,
        done: false
      });
      this.newTask = "";
    },
    deleteTask(index) {
      this.$q
        .dialog({
          dark: true,
          title: "Confirmacao",
          message: "Desejar apagar?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Tarefa apagada");
        });
    }
  }
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
