<template>
  <div class="taskcard">
    <textarea class="taskcard__title" v-model="content.title"></textarea>
    <button type="button" class="taskcard__delete" @click="deleteCard">X</button>
    <div class="taskcard__body">
      <input
        type="range"
        min="0"
        :max="content.tasks.length"
        v-model="currentStatus"
        class="slider"
        :style="sliderStyle"
        :disabled="editIndex"
      />
      <ul class="taskcard__body__tasks">
        <li
          v-for="(task, taskIndex) in content.tasks"
          :key="taskIndex"
          :class="
            content.status > taskIndex
              ? 'taskcard__body__task--done'
              : 'taskcard__body__task--pending'
          "
        >
          <template v-if="editIndex == taskIndex">
            <div class="taskcard__body__tasks__input">
              <input type="text" maxlength="20" v-model="editContent" :key="index + '' + taskIndex" autofocus />
              <button type="button" @click="saveTask">Save</button>
            </div>
          </template>
          <template v-else>
            <span>{{ taskIndex + 1 + '.' }}</span>&nbsp;<span class="description" :title="task.description" @click="enableEdit(taskIndex)">{{ task.description }}</span>
            <button type="button" class="taskcard__body__task__delete" @click="deleteTask(taskIndex)">X</button>
          </template>
        </li>
        <li>
          <button type="button" @click="addNewTask">Add</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "TaskCard",
  props: {
    content: {
      title: String,
      tasks: Array,
      status: Number
    },
    index: Number
  },
  data: () => {
    return {
      editIndex: null,
      editContent: null,
      isTitleEdit: false,
    }
  },
  computed: {
    sliderStyle() {
      return `width: ${this.content.tasks.length * 40 + 10}px; 
              left: -${this.content.tasks.length * 20 + 10}px;
              top: ${this.content.tasks.length * 20 + 15}px;`;
    },
    currentStatus: {
      get() {
        return this.content.status;
      },
      set(val) {
        this.$emit('set-status', this.index, val);
      },
    },
  },
  methods: {
    addNewTask() {
      this.$emit('new-task', this.index);
    },
    deleteCard() {
      this.$emit('delete-card', this.index);
    },
    saveTask() {
      this.$emit('update-task', this.index, this.editIndex, this.editContent);
      this.disableEdit();
    },
    deleteTask(taskIndex) {
      this.$emit('delete-task', this.index, taskIndex);
    },
    enableEdit(taskIndex) {
      if (this.content.status > taskIndex) {
        return;
      }
      this.editContent = this.content.tasks[taskIndex].description;
      this.editIndex = taskIndex;
    },
    disableEdit() {
      this.editIndex = null;
      this.editContent = null;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.taskcard {
  width: 260px;
  max-width: 260px;
  min-width: 260px;
  margin: 20px;
  background: #e0e0e0;
  padding: 12px;
  position: relative;
  .taskcard__title {
    font-size: 24px;
    font-weight: bold;
    background: transparent;
    border: none;
    width: 100%;
    resize: none;
    margin-top: 20px;
  }
  .taskcard__delete {
    position: absolute;
    top: 10px;
    right: 10px;
  }
  .taskcard__body {
    display: flex;
    position: relative;
    .taskcard__body__tasks {
      list-style-type: none;
      padding-left: 10px;
      margin: 40px 0 0 25px;
      li {
        padding: 0.5em 0;
        font-size: 18px;
        line-height: 22px;
        width: 200px;
        display: flex;
        position: relative;
        span.description {
          display: inline-block;
          white-space: nowrap;
          overflow: hidden;
          text-overflow: ellipsis;
          width: 150px;
          height: 22px;
        }
        .taskcard__body__task__delete {
          opacity: 0;
        }
        &:hover .taskcard__body__task__delete {
          opacity: 1;
        }
        &.taskcard__body__task--done {
          font-weight: bold;
          span.description {
            text-decoration: line-through;
          }
        }
        &.taskcard__body__task--pending {
          cursor: pointer;
          .taskcard__body__tasks__input {
            position: relative;
            input {
              font-size: 17px;
              width: 180px;
            }
            button {
              position: absolute;
              bottom: -25px;
              right: 0;
            }
          }
        }
      }
    }
    .slider {
      transform: rotate(90deg);
      margin: 0 20px;
      position: absolute;
    }
  }
}
</style>
