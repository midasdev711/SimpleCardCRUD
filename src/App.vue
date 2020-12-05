<template>
  <div id="app">
    <div class="container">
      <div class="page-header">
        <button type="button" @click="addNewTaskCard">Add new card</button>
      </div>
      <div class="page-content">
        <div class="taskcard-list">
          <TaskCard
            :content="content"
            :index="index"
            v-for="(content, index) in cards"
            :key="index"
            @set-status="setStatus"
            @new-task="newTask"
            @delete-card="deleteCard"
            @update-task="updateTask"
            @delete-task="deleteTask"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TaskCard from "./components/TaskCard";
export default {
  name: "App",
  components: {
    TaskCard,
  },
  data: () => {
    return {
      cards: [
        {
          title: "12/01/2020",
          tasks: [
            {
              description: "Build home page"
            },
            {
              description: "Integrate API"
            },
            {
              description: "Test page"
            },
          ],
          status: 2
        },
        {
          title: "12/02/2020",
          tasks: [
            {
              description: "Build dashboard page"
            },
            {
              description: "Integrate API"
            },
            {
              description: "Test page"
            },
          ],
          status: 0
        },
        {
          title: "Contact page",
          tasks: [
            {
              description: "Build contact page"
            },
            {
              description: "Integrate API"
            },
            {
              description: "Test page"
            },
          ],
          status: 0
        },
      ],
    };
  },
  methods: {
    setStatus(index, val) {
      this.cards[index].status = val;
    },
    newTask(index) {
      this.cards[index].tasks.push({
        description: 'Untitled task'
      });
    },
    updateTask(index, editIndex, editContent) {
      let tmp = Object.assign([], this.cards);
      tmp[index].tasks[editIndex].description = editContent;
      this.cards = Object.assign([], tmp);
    },
    deleteCard(index) {
      let tmp = Object.assign([], this.cards);
      tmp.splice(index, 1);
      this.cards = Object.assign([], tmp);
    },
    addNewTaskCard() {
      this.cards.push({
        title: 'Untitled',
        tasks: [{
          description: 'Untitled task'
        }],
        status: 0
      })
    },
    deleteTask(cardIndex, taskIndex) {
      let tmp = Object.assign([], this.cards);
      if (tmp[cardIndex].status > taskIndex) {
        tmp[cardIndex].status --;
      }
      tmp[cardIndex].tasks.splice(taskIndex, 1);
      this.cards = Object.assign([], tmp);
    },
  }
};
</script>
<style lang="scss">
* {
  box-sizing: border-box;
}
</style>
<style scoped lang="scss">
.container {
  width: 900px;
  margin: 0 auto;
}
.taskcard-list {
  display: flex;
  flex-wrap: wrap;
}
@media (max-width: 992px) {
  .taskcard-list {
    justify-content: space-between;
  }
  .container {
    width: 600px;
  }
}
@media (max-width: 680px) {
  .taskcard-list {
    justify-content: center;
  }
  .container {
    width: 90%;
  }
}
</style>
