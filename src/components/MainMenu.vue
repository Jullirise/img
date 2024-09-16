<template>
  <div class="menu">
    <!-- <div class="upload-wrapper"> -->
      <button
        class="menu-button"
        @click="handleButtonClick"
        value="input"
        title="Выбрать файл"
      >
        <label for="fileInput" class="button-label">
          <img
            class="button-image"
            src="../assets/file.png"
            alt="Select Image"
          />
        </label>
        <input
          id="fileInput"
          class="button-input"
          type="file"
          @change="handleImageSelection"
        />
      </button>
      <div class="url">
        <input
          class="url-input"
          v-model="imageUrl"
          placeholder="Ссылка"
          @keydown.enter.prevent="loadImageFromUrl"
        />
      </div>
    <!-- </div> -->
    <!-- <div class="options"> -->
      <button
        class="menu-button"
        @click="handleButtonClick"
        :disabled="!selectedImage"
        value="hand"
        title="Переместить изображение"
      >
        <label
          class="button-label"
          :class="{
            'button-label-tapped': state === 'hand',
            disabled: !selectedImage,
          }"
        >
          <img class="button-image" src="../assets/hand.png" alt="Move Image" />
        </label>
      </button>
      <button
        class="menu-button"
        @click="handleButtonClick"
        :disabled="!selectedImage"
        value="pipette"
        title="Пипетка"
      >
        <label
          class="button-label"
          :class="{
            'button-label-tapped': state === 'pipette',
            disabled: !selectedImage,
          }"
        >
          <img class="button-image" src="../assets/pipette.png" alt="Pipette" />
        </label>
      </button>
      <button
        class="menu-button"
        @click="handleButtonClick"
        :disabled="!selectedImage"
        value="modal"
        title="Изменение размеров ищображения"
      >
        <label
          class="button-label"
          :class="{
            'button-label-tapped': state === 'modal',
            disabled: !selectedImage,
          }"
        >
          <img class="button-image" src="../assets/modal.png" alt="modal" />
        </label>
      </button>
      <button
        class="menu-button"
        @click="handleButtonClick"
        :disabled="!selectedImage"
        value="correct"
        title="Редактировать изображение"
      >
        <label class="button-label" :class="{ disabled: !selectedImage }">
          <img
            class="button-image"
            src="../assets/correction.png"
            alt="Correct Image"
          />
        </label>
      </button>
      <button
        class="menu-button"
        @click="handleButtonClick"
        :disabled="!selectedImage"
        value="filter"
        title="Использовать фильтры"
      >
        <label class="button-label" :class="{ disabled: !selectedImage }">
          <img
            class="button-image"
            src="../assets/filtration.png"
            alt="Filter Image"
          />
        </label>
      </button>
      <button
        class="menu-button"
        @click="handleButtonClick"
        :disabled="!selectedImage"
        value="save"
        title="Сохранить изображение"
      >
        <label class="button-label" :class="{ disabled: !selectedImage }">
          <img
            class="button-image"
            src="../assets/downloads.png"
            alt="Save Image"
          />
        </label>
      </button>
    <!-- </div> -->
  </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "MainMenu",
  props: {
    state: String,
  },
  data() {
    return {
      selectedImage: "",
      imageUrl: "",
    };
  },
  methods: {
    handleImageSelection(event) {
      const target = event.target;
      const selectedFile = target.files?.[0];

      if (selectedFile) {
        const imageUrl = URL.createObjectURL(selectedFile);
        this.selectedImage = imageUrl;
        this.$emit("onImageSelected", imageUrl);

        target.value = "";
      }
    },
    loadImageFromUrl() {
      if (this.imageUrl) {
        this.$emit("onImageSelected", this.imageUrl);
        this.selectedImage = this.imageUrl;
        this.imageUrl = "";
      }
    },
    handleButtonClick(event) {
      const target = event.target;
      const value = target.parentElement?.value
        ? target.parentElement.value
        : target.parentElement?.parentElement.value;
      this.$emit("changeState", value);
    },
  },
  beforeUnmount() {
    if (this.selectedImage) {
      URL.revokeObjectURL(this.selectedImage);
    }
  },
  watch: {
    state(newValue) {
      if (newValue === "pipette") {
        this.stateCursor = "pipette";
      } else this.stateCursor = null;
    },
  },
});
</script>

<style scoped lang="scss">
.menu {
  width: 70px;
  height: calc(100% - 10px);
  padding: 5px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  border-radius: 15px;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  background: linear-gradient(to bottom, #cfe9f1, #2d1ad7);
}

.url-input {
  width: 100%;
  padding: 10px;
  border: 2px solid #1e90ff;
  border-radius: 8px;
  background-color: #ffffff;
  font-size: 10px;
  color: #1e90ff;
}

.button {
  &-input {
    width: 100%;
    display: none;
  }

  &-label {
    cursor: pointer;
    padding: 10px;
    background: #dee4dd;
    border: 1.5px solid rgb(24, 3, 3);
    border-radius: 10px;
    display: block;
    transition: 0.3s;
    &:hover {
      background: #dba8bf;
      border-color: #120101;
    }
    &-tapped {
      background: #dba8bf;
      border-color: #dba8bf;
    }
  }

  &-image {
    height: 25px;
  }
}

.menu-button {
  background: transparent;
  border-radius: 10px; 
  transition: background-color 0.3s;
}

.button-image {
  width: 24px;
  height: 24px;
}

.url {
  &-input {
    width: 48px;
  }
}
.disabled {
  opacity: 0.3;
}
</style>
