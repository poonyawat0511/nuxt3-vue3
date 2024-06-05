<script setup lang="ts">
const { name, status } = defineProps({
  name: { type: String, required: true },
  status: [String, Boolean, Number],
});

const emit = defineEmits(["wow"]);

function onActionClick() {
  emit("wow", name, status);
}
</script>


<template>
  <div>
    <div class="card">
      <div class="card-header">
        <div class="card-avatar"></div>
        <div class="card-author">
          <div class="card-author-name">{{ name }}</div>
          <div class="card-author-status">{{ status }}</div>
        </div>
      </div>
      <div class="card-content"><slot></slot></div>
      <div class="card-action">
        <button class="button" v-on="{ click: onActionClick }">Action</button>
      </div>
    </div>
  </div>
</template>
<style>
.card {
  background-color: white;
  width: 100%;
  height: min-content;

  display: flex;
  flex-direction: column;

  border: 1px solid #d9d9d9;
  border-radius: 10px;

  overflow: hidden;
}

.card-header {
  display: flex;
  flex-direction: row;

  align-items: start;

  justify-content: start;

  height: min-content;

  padding: 10px 10px;
}

.card-avatar {
  clip-path: circle();
  background-color: #d9d9d9;
  width: 30px;
  height: 30px;
}

.card-author {
  margin-left: 10px;
  display: flex;
  flex-direction: column;
}

.card-author-name {
  font-size: 12px;
  font-weight: 500;
}

.card-author-status {
  font-size: 8px;
  font-weight: lighter;
}

.card-content {
  padding: 10px;
  height: 100px;
  background-color: #d9d9d9;
}

.card-action {
  height: min-content;
  padding: 10px;
  display: flex;
  flex-direction: row;
  justify-content: end;
  background-color: white;
}

.button {
  align-items: center;
  appearance: button;
  background-color: #0276ff;
  border-radius: 8px;
  border-style: none;
  box-shadow: rgba(255, 255, 255, 0.26) 0 1px 2px inset;
  box-sizing: border-box;
  color: #fff;
  cursor: pointer;
  display: flex;
  flex-direction: row;
  flex-shrink: 0;
  font-family: "RM Neue", sans-serif;
  font-size: 100%;
  line-height: 1.15;
  margin: 0;
  padding: 10px 21px;
  text-align: center;
  text-transform: none;
  transition: color 0.13s ease-in-out, background 0.13s ease-in-out,
    opacity 0.13s ease-in-out, box-shadow 0.13s ease-in-out;
  -webkit-user-select: none;
  user-select: none;
  touch-action: manipulation;
}

.button:active {
  background-color: #006ae8;
}

.button:hover {
  background-color: #1c84ff;
}
</style>

  