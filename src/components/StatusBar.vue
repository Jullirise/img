<template>
  <div class="status-bar">
    <div class="scale-control">
      <label for="scale" style="color: white;" class="label-mas">Масштаб</label>

      <!-- Выпадающий список с предустановленными значениями масштаба -->
      <select v-model="selectedScale" @change="onScaleSelect" class="scale-dropdown">
        <option v-for="scale in predefinedScales" :key="scale" :value="scale">
          {{ scale }}%
        </option>
      </select>

      <!-- Ползунок для задания масштаба -->
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
      <!-- Отображение корректного масштаба в процентах -->
      
    </div>

    <!-- Информация о ширине и высоте изображения -->
    <div v-if="imgLoaded && imageWidth !== null && imageHeight !== null">
      Ширина: {{ imageWidth }} Высота: {{ imageHeight }}
    </div>

    <!-- Информация о выбранном цвете и координатах (для пипетки) -->
    <div v-if="state === 'pipette' && pickedColor">
      Цвет: {{ pickedColor }}
    </div>
    <div
      v-if="state === 'pipette' && pickedColor"
      class="pipette-color"
      :style="{ background: pickedColor }"
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
    scale: Number,        // Текущий масштаб
    imgLoaded: Boolean,   // Флаг для проверки загрузки изображения
    originalWidth: Number,  // Исходная ширина изображения
    originalHeight: Number, // Исходная высота изображения
  },
  data() {
    return {
      selectedScale: this.scale, // Для хранения выбранного масштаба
      predefinedScales: [12, 25, 50, 100, 150, 200, 300], // Предустановленные значения масштаба
    };
  },
  computed: {
    // Корректное вычисление масштаба в процентах относительно исходного размера изображения
    correctScalePercentage() {
      if (this.originalWidth && this.originalHeight && this.imageWidth && this.imageHeight) {
        const widthPercentage = (this.imageWidth / this.originalWidth) * 100;
        const heightPercentage = (this.imageHeight / this.originalHeight) * 100;
        return Math.round((widthPercentage + heightPercentage) / 2);
      }
      return 100;
    },
  },
  methods: {
    // Метод для обновления масштаба при изменении ползунка
    updateScale(event) {
      const newScale = +event.target.value;
      this.selectedScale = newScale; // Синхронизируем с выбранным масштабом
      this.$emit("updateScale", newScale);
    },
    // Метод для обновления масштаба при выборе в списке
    onScaleSelect() {
      this.$emit("updateScale", this.selectedScale);
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

.scale-dropdown {
  margin-right: 10px;
}

.label-mas {
  margin-right: 20px;
}

.scale {
  margin-right: 13px;
}
</style>
