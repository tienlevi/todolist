<script setup>
import { onMounted, ref } from "vue";
import { Icon } from "@iconify/vue/dist/iconify.js";
import Button from "./components/Button/Button.vue";
import Input from "./components/Input/Input.vue";

const name = ref("");
const nameEdit = ref("");
const select = ref(null);
const lists = ref([]);

const handleAdd = () => {
  const data = { name: name.value, status: "todo" };
  const add = [...lists.value, data];
  lists.value = add;
  localStorage.setItem("Todolists", JSON.stringify(add));
};

const handleDelete = (id) => {
  const remove = lists.value.filter((_, index) => index !== id);
  lists.value = remove;
  localStorage.setItem("Todolists", JSON.stringify(remove));
};

const handleConfirm = (id) => {
  const edit = lists.value.map((item, index) =>
    index === id ? { name: nameEdit.value, status: "todo" } : item,
  );
  lists.value = edit;
  select.value = null;
  localStorage.setItem("Todolists", JSON.stringify(edit));
};

const handleSelect = (id) => {
  select.value = id;
  nameEdit.value = lists.value.find((_, index) => index === id).name;
};

const doneTodo = (id, status) => {
  if (status === "todo") {
    const done = lists.value.map((item, index) =>
      index === id ? { ...item, status: "done" } : item,
    );
    lists.value = done;
    localStorage.setItem("Todolists", JSON.stringify(done));
  } else {
    const todo = lists.value.map((item, index) =>
      index === id ? { ...item, status: "todo" } : item,
    );
    lists.value = todo;
    localStorage.setItem("Todolists", JSON.stringify(todo));
  }
};

onMounted(() => {
  lists.value = JSON.parse(localStorage.getItem("Todolists")) || [];
});
</script>

<template>
  <div class="container">
    <form id="itemForm">
      <h3>to do list</h3>
      <div class="input-group">
        <Input v-model="name" :value="name" placeholder="Add todo" />
        <div class="input-group-append">
          <Button children="Add item" @click="handleAdd()" />
        </div>
      </div>
    </form>
    <div class="item-container">
      <div v-for="(list, index) in lists" class="item-list">
        <div v-if="select === index" class="input-group">
          <Input v-model="nameEdit" :value="nameEdit" placeholder="Edit todo" />
          <div class="input-group-append" style="display: flex; gap: 20px">
            <Button
              children="Edit todo"
              style="width: 100%"
              @click="() => handleConfirm(index)"
            />
            <Button
              children="Cancel"
              style="width: 100%"
              @click="
                () => {
                  select = null;
                }
              "
            />
          </div>
        </div>
        <div v-else class="item">
          <div class="item-info">
            <input
              type="checkbox"
              value="done"
              @click="doneTodo(index, list.status)"
              :checked="list.status === 'done'"
            />
            <h6 class="item-index">{{ index + 1 }}</h6>
            <del v-if="list.status === 'done'" class="item-name">
              {{ list.name }}
            </del>
            <p v-if="list.status === 'todo'" class="item-name">
              {{ list.name }}
            </p>
          </div>
          <div class="item-icons" v-if="list.status === 'todo'">
            <div @click="handleDelete(index)">
              <Icon
                icon="material-symbols:close-small-outline-rounded"
                width="24"
                height="24"
              />
            </div>
            <div @click="handleSelect(index)">
              <Icon
                icon="material-symbols:ink-pen-outline"
                width="24"
                height="24"
              />
            </div>
          </div>
        </div>
      </div>
      <Button children="Clear Lists" />
    </div>
  </div>
</template>

<style scoped></style>
