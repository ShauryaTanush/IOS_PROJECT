<template>
  <q-page class="bg-blue-1 column">
    <div class="row q -pa-sm bg-prima">
      <q-input
        v-model="newTask"
        @keyup.enter="add_task"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="New To-Do"
        dense
      >
        <template v-slot:append>
          <q-btn @click="add_task" round dense flat icon="add" />
        </template>
      </q-input>
    </div>

    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{ 'done bg-grey-3': task.done }"
        clickable
        v-ripple
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
            @click.stop="delete_task(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No To-Dos</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  data() {
    return {
      newTask: "",
      tasks: [
        //   {
        //   title: 'hey',
        //    done: false
        //  },
        //           {
        //  title: 'hi',
        // done: false
        //},
        //         {
        //   title: 'hello',
        //  done: false
        //}
      ],
    };
  },
  methods: {
    delete_task(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Are you sure you want to delete this to-do?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("To-Do Deleted");
        });
    },
    add_task() {
      this.tasks.push({
        title: this.newTask,
        done: false,
      });
      this.newTask = "";
    },
  },
});
</script>
<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;

    color: #bbb;
  }
}

.bg-brand {
  background: #1d212a !important;
}
.text-brand {
  color: #ffffff !important;
}
.no-tasks {
  opacity: 0.4;
}
</style>
