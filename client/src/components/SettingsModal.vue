<template>
  <div class="modal-container">
    <div class="modal">
      <div class="settings-container">
        <div class="settings-heading">Settings</div> 
        <div class="settings-subheading">Units</div> 
        <button  class="units-button no-hover">
        <span>Fibonacci</span>
      </button>

        <button  class="units-button no-hover">
        <span>Modified Fibonacci</span>
      </button>

        <button  class="units-button no-hover">
        <span>Fibonacci</span>
      </button>

        <button  class="units-button no-hover">
        <span>T-Shirt</span>
      </button>

        <button  class="units-button no-hover">
        <span>Powers of 2</span>
      </button>

        <button  class="units-button no-hover">
        <span>Five</span>
      </button>

        <button class="save-button" type="submit" @click="saveSettings" aria-label="Save">Save</button>
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
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
}

.modal {
  width: 400px;
  height: 450px;
  position: absolute;
  border-radius: 15px;
  box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
    6px 6px 10px rgba(0, 0, 0, 0.2);
  color: #161b1f;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  .settings-container {
  }

  .settings-heading {
    font-size: 26px;
    opacity: 0.5;
    margin-bottom: 5px;
  }

    .settings-subheading {
    font-size: 20px;
    opacity: 0.5;
    margin-bottom: 5px;
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
  font-size: 19px;
  font-weight: semibold;
 }

.units-button {
  display: flex;
  width: 200px;
  height: 40px;
  background: #f3f0f1;
  border-radius: 32px;
  justify-content: center;
  align-items: center;
  margin-bottom: 9px;
  border: none;
  cursor: pointer;
  transition: all 0.1s ease-in-out;
  box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
    6px 6px 10px rgba(0, 0, 0, 0.2);
  color: #161b1f;
  &:hover {
    opacity: 0.3;
    box-shadow: -6px -6px 10px rgba(255, 255, 255, 0.8),
      6px 6px 10px rgba(0, 0, 0, 0.2);
  }
  &:active {
    opacity: 1;
    box-shadow: inset -4px -4px 8px rgba(255, 255, 255, 0.5),
      inset 8px 8px 16px rgba(0, 0, 0, 0.1);
  }
}

.save-button {
    background: #54e8dd;
    border: none;
    cursor: pointer;
    border-radius: 10px;
    width: 60px;
    font-size: 15px;
}
</style>
