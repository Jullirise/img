<template>
  <div class="status-bar">
    <div class="scale-control">
      <label for="scale" style="color: white;">Масштаб</label>
      <input
        type="range"
        class="scale"
        id="scale"
        name="scale"
        min="12"
        max="300"
        step="1"
        :value="scale"
        @input="updateScale"
      />
    </div>
    <div v-if="imgLoaded && imageWidth !== null && imageHeight !== null && imageWidth !== 300 && imageHeight !== 150"> <!-- Изменено условие отображения -->
      Ширина: {{ imageWidth }} Высота: {{ imageHeight }}
    </div>
    <div v-if="state === 'pipette' && pickedColor">
      Цвет: {{ pickedColor }}
    </div>
    <div
      v-if="state === 'pipette' && pickedColor"
      class="pipette-color"
      :style="{
        background: pickedColor,
      }"
    ></div>
    <div v-if="state === 'pipette' && pickedColor && xMouse !== null && yMouse !== null">
      Координаты: {{ xMouse }}:{{ yMouse }}
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "StatusBar",
  props: {
    state: String,
    imageHeight: Number,
    imageWidth: Number,
    pickedColor: String,
    xMouse: Number,
    yMouse: Number,
    scale: Number,
    imgLoaded: Boolean, // Добавили этот параметр
  },
  methods: {
    updateScale(event) {
      const newScale = +event.target.value;
      this.$emit("updateScale", newScale);
    },
  },
});
</script>

<style scoped lang="scss">
.status-bar {
  position: absolute;
  bottom: 0;
  width: calc(100% - 60px);
  height: 50px;
  background: linear-gradient(to right, #2414b5, #ececec,);
  display: flex;
  gap: 5px;
  padding-left: 30px;
  padding-top: 30px;
  justify-content: space-between;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  padding-bottom: 5px;
}

.left-side {
  display: flex;
}

.pipette-color {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 2px solid #1e90ff;
  margin-top: -15px;
}

.scale-control label {
  color: #1e90ff;
  font-weight: bold;
}



.scale {
  margin-right: 13px;
}
</style>
