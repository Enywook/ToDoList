<template>
  <Page>
  <StackLayout>
    <Button class="ajout" text="Ajouter une tâche" @tap="addTodo" />
    <Button class="ajout" text="Tester ajout Todo" @tap="save" />
    <SegmentedBar v-model="filter">
      <SegmentedBarItem title="Tous" />
      <SegmentedBarItem title="Fait" />
      <SegmentedBarItem title="Pas encore fait" />
    </SegmentedBar>
<!--    <ListView for="todo in filter_todos" @itemTap="onTap" class="listView">-->
      <ListView for="todo in $store.state.data" @itemTap="onTap" class="listView">
      <v-template class="itemView">
        <StackLayout>
          <Label>
            <Span :text="todo.content" fontSize="18" :class="colorDone(todo.done)" />
          </Label>
<!--          <StackLayout v-if="todo.done">-->
<!--            <Button class="ajout" text="Supprimer" @tap="deleteTodo(todo.content)" fontSize="20" />-->
<!--          </StackLayout>-->
        </StackLayout>
      </v-template>
    </ListView>
  </StackLayout>
  </Page>
</template>


<script>
import Detail from "./Detail";

export default {
  name: "List",
  components: {
    Detail
  },
  data() {
    return {
      arrayTodo: [
        {
          content: " Faire la To do List",
          done: true
        },
        {
          content: " Faire du vueJs",
          done: false
        },
        {
          content: " Faire à manger",
          done: false
        },
        {
          content: " Faire du PHP",
          done: false
        },
        {
          content: " Faire un portfolio en anglais",
          done: false
        },
        {
          content: " FF 15 PLS",
          done: true
        },
        {
          content: " Go next",
          done: true
        }
      ],
      input: {
        content: ""
      },
      filter: 0
    };
  },
  methods: {
    save(){
      this.$store.dispatch("insertTest", this.input);
    },
    load(){
      this.$store.dispatch("queryTest")
    },
    colorDone(done) {
      return done === 0 ? "text-green" : "text-red";
    },
    onTap(event) {
      //console.log(event.item);
      event.item.done = event.item.done === 0 ;
      this.$navigateTo(Detail, { props: { item: event.item } });
    },
    addTodo() {
      const dialogs = require("tns-core-modules/ui/dialogs");
      prompt({
        title: "Ajout d'un toDo",
        message: "Intitulé de l'item",
        okButtonText: "OK",
        cancelButtonText: "Cancel",
        inputType: dialogs.inputType.text
      }).then(result => {
        this.$store.dispatch("insertNewToDo", result.text);
      });
    },
  },
  mounted(){
    this.load();
    this.$store.state.data.forEach(element=>
    console.log(element.id))
  },
  computed: {
    filter_todos: function() {
      if (this.filter == 1) {
        return this.arrayTodo.filter(todo => todo.done);
      } else if (this.filter == 0) {
        return this.arrayTodo;
      } else {
        return this.arrayTodo.filter(todo => !todo.done);
      }
    }
  }
};
</script>

<style scoped lang="scss">
.sort {
  background-color: #263859;
}
.listView {
  height: auto;
  background-color: #263859;
}
.text-red {
  color: #c9485b;
}
.text-green {
  color: #21bf73;
}
  .ajout{
    background-color: #282a36;
    color: #f8f8f2;
  }
</style>
