<template>
  <div @dragenter.prevent="toggleActive" @dragleave.prevent="toggleActive" @dragover.prevent @drop.prevent="handleDrop"
    :class="{ 'active-dropzone': active }"
    class="dropzone container-fluid d-flex flex-column justify-content-center align-items-center gap-2 dashed-border border border-primary rounded-5 border-opacity-50 p-4 my-4">
    <span>Arrastra/suelta un documento</span>
    <span>o</span>
    <div class="mb-3 text-center">
      <label for="formFile" class="form-label pb-2">Selecciona un documento</label>
      <input ref="fileInput" class="form-control" type="file" id="formFile" accept=".pdf, .doc, .docx, .ppt, .pptx"
        @change="handleFileSelect">
    </div>
  </div>
</template>

<script>
export default {
  name: "DropZone",
  data() {
    return {
      active: false
    };
  },
  methods: {
    toggleActive() {
      this.active = !this.active;
    },
    handleDrop(e) {
      this.active = false;
      const documento = e.dataTransfer.files[0];
      if (documento) this.$emit('file-selected', documento);
    },
    handleFileSelect(e) {
      const documento = e.target.files[0];
      if (documento) this.$emit('file-selected', documento);
    },
    resetInput() {
      this.$refs.fileInput.value = "";
    }
  }
}
</script>

<style>
.dashed-border {
  border-style: dashed !important;
}

.dropzone {
  transition: all 0.3s ease;
}

.active-dropzone {
  background-color: #2b82bc;
  color: white;
}
</style>
