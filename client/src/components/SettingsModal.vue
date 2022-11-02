<template>
  <div class="modal-container">
    <div class="modal">
      <div class="input-container">
        <button class="enter-button" type="submit" @click="saveSettings" aria-label="Save">Save</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">

  const emit = defineEmits(['saveSettings'])

  function saveSettings() {
    emit('saveSettings');
  }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.modal-container {
  width: 100%;
  height: 100%;
  //background: pink;
  display: flex;
  justify-content: center;
  position: absolute;
}

.modal {
  width: 400px;
  height: 220px;
  top: 35%;
  position: absolute;
  border-radius: 15px;
  box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
    6px 6px 10px rgba(0, 0, 0, 0.2);
  color: #161b1f;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  .input-container {
    display: flex;
  }

  .enter-button {
    background: #54e8dd;
    border: none;
    cursor: pointer;
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px;
    width: 50px;
  }

  input {
    box-sizing: border-box;
    padding: 8px;
    width: 295px;
    height: 55px;
    border: none;
    outline: none;
    font-size: 26px;
    border-top-left-radius: 10px;
    border-bottom-left-radius: 10px;
    color: #161b1f;
    padding-right: 50px;
  }
}

span {
  line-height: 80px;
  font-family: "Montserrat", sans-serif;
  font-size: 26px;
  font-weight: semibold;
}
</style>
