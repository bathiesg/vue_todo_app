<template>
  <button class="btn btn-primary" v-if="!editing" @click="toggleEditing(true)">+Add item</button>
  <button class="btn btn-cancel" v-if="editing" @click="toggleEditing(false)">Cancel</button>
  <div>
    <form v-if="editing" class="add-item-form" @submit.prevent="saveItem">
      <div class="form-input">
        <input type="text" v-model="newItem" placeholder="Add new item" />
        <label for="set-priority">
          Is prioritary?
          <input type="checkbox" id="set-priority" v-model="isPrioritary" />
          <span class="checkmark"></span>
        </label>
      </div>
      <button class="btn btn-primary">Add item</button>
    </form>
  </div>

  <div>
    <ul class="list">
      <li
        class="item-list"
        v-for="item in reverseItems"
        :key="item.id"
        :class="{ strikeout: item.completed, priority: item.isPrioritary }"
      >
        <label :for="'item-' + item.id">
          {{ item.title }}
          <input type="checkbox" :id="'item-' + item.id" v-model="item.completed" />
          <span class="checkmark"></span>
        </label>
      </li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from 'vue'

const editing = ref(false)

const toggleEditing = (status: boolean) => {
  editing.value = status
  newItem.value = ''
  isPrioritary.value = false
}

const newItem = ref('')
const isPrioritary = ref(false)

const saveItem = () => {
  items.push({
    id: items.length + 1,
    title: newItem.value,
    completed: false,
    isPrioritary: isPrioritary.value
  })

  newItem.value = ''
  isPrioritary.value = false
}

const reverseItems = computed(() => [...items].reverse())

const items = reactive([
  {
    id: 1,
    title: 'delectus aut autem',
    completed: false,
    isPrioritary: false
  },
  {
    id: 2,
    title: 'quis ut nam facilis et officia qui',
    completed: true,
    isPrioritary: false
  },
  {
    id: 3,
    title: 'fugiat veniam minus',
    completed: false,
    isPrioritary: true
  },
  {
    id: 4,
    title: 'et porro tempora',
    completed: true,
    isPrioritary: false
  },
  {
    id: 5,
    title: 'laboriosam mollitia et enim quasi adipisci quia provident illum',
    completed: false,
    isPrioritary: false
  }
])
</script>

<style scoped lang="scss">
.add-item-form {
  .form-input {
    display: flex;
    justify-content: space-between;
    flex-direction: column;

    input[type='text'] {
      color: var(--vt-c-indigo);
      // width: ;
    }
  }
}

.list {
  margin-top: 80px;

  .item-list {
    color: var(--vt-c-white);
    box-shadow: 0px 2px 1px -1px var(--vt-c-text-light-2);
    &.strikeout {
      text-decoration: line-through;
      color: #b8c2cc;
      &:hover {
        color: #8795a1;
      }
    }

    &.priority {
      color: #de751f;
    }
  }
}

label {
  display: block;
  position: relative;
  padding: 15px 0 15px;
  // margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  padding-left: 35px;

  /*Hide the browser input*/
  input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }

  /* Create a custom checkbox */
  .checkmark {
    position: absolute;
    top: 0;
    left: 0;
    height: 25px;
    width: 25px;
    background-color: var(--vt-c-white);
    margin-top: 22px;
    &:after {
      content: '';
      position: absolute;
      display: none;

      left: 8px;
      top: 2px;
      width: 10px;
      height: 15px;
      border: solid var(--vt-c-divider-dark-1);
      border-width: 0 3px 3px 0;
      -webkit-transform: rotate(45deg);
      -ms-transform: rotate(45deg);
      transform: rotate(45deg);
    }
  }

  /* On mouse-over, add a grey background color */
  &:hover input ~ .checkmark {
    background-color: #ccc;
  }

  /* When the checkbox is checked, add a blue background */
  input:checked ~ .checkmark {
    background-color: var(--vt-c-white-mute);
  }
  /* Show the checkmark when checked */
  input:checked ~ .checkmark:after {
    display: block;
  }
  /* Style the checkmark/indicator */
}
</style>
