<template>
  <div class="image-uploader">
    <label :class="['image-uploader__preview', {'image-uploader__preview-loading': isLoading}]" :style='`--bg-url: url(${value})`'>
      <span v-if="isLoading" class="image-uploader__text">'Загрузка...'</span>
      <span v-else-if="!isLoading && value" class="image-uploader__text">Удалить изображение</span>
      <span v-else class="image-uploader__text">Загрузить изображение</span>
      <input ref="inputRef" type="file" accept="image/*" class="image-uploader__input" v-bind="$attrs" @change="inputChange($event)" @cancel="console.log('Cancelled')" @click="onCLick" />
    </label>
  </div>
</template>

<script>
export default {
  name: 'UiImageUploader',

  inheritAttrs: false,

  props: {
    preview: {
      type: String,
      default: ''
    },
    uploader: {
      type: Function,
      default: () => {}
    }
  },

  emits: ['select', 'upload', 'error', 'remove'],

  data() {
    return {
      value: this.preview ?? null,
      isLoading: false,
    }
  },

  beforeUnmount() {
      // Чистим созданный идентификатор на изображение компонента
      if (this.value) {
        URL.revokeObjectURL(this.value);
      }
    },

  methods: {
    onCLick($event) {
      if(this.isLoading) {
        $event.preventDefault();
      } else if(!this.isLoading && this.value) {
        this.removeFile();
        this.$emit('remove');
      }
    },
    removeFile() {
      this.$refs.inputRef.value = '';
      this.value = null;
    },
    async inputChange($event) {
      const image = URL.createObjectURL($event.target.files[0]);
      this.value = image;
      this.$emit('select', $event.target.files[0])

      if(this.uploader) {
        this.isLoading = true;
        try {
          const result = await this.uploader($event.target.files[0]);
          this.$emit('upload', result)
        } catch(error) {
          this.$emit('error', error)
          this.removeFile();
        } finally {
          this.isLoading = false;
        }
      }
    },
  },
};
</script>

<style scoped>
.image-uploader {
}

.image-uploader__input {
  opacity: 0;
  height: 0;
}

.image-uploader__preview {
  --bg-url: var(--default-cover);
  background-size: cover;
  background-position: center;
  background-image: linear-gradient(0deg, rgba(0, 0, 0, 0.4), rgba(0, 0, 0, 0.4)), var(--bg-url);
  border: 2px solid var(--blue-light);
  border-radius: 8px;
  transition: 0.2s border-color;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  max-width: 512px;
  height: 228px;
}

.image-uploader__text {
  color: var(--white);
  font-family: 'Nunito', sans-serif;
  font-weight: 600;
  font-size: 20px;
  line-height: 28px;
}

.image-uploader__preview:hover {
  border-color: var(--blue);
}

.image-uploader__preview.image-uploader__preview-loading {
  cursor: no-drop;
}
</style>
