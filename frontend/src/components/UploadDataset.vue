<template>
  <div class="m-3 p-3">
    <div class="my-2">
      <h3>
        Primero suba un dataset que ya se encuentre codificado en one-hot
      </h3>
    </div>
    <div class="my-4">
      <input type="file" name="dataset" id="dataset" accept=".xls, .xlsx" @change="uploadDataset">
    </div>
    <div>
      <button :disabled="disable" class="btn btn-success" @click="changeComponent">Continuar</button>
    </div>
  </div>
</template>

<script>
import * as XLSX from 'xlsx';

export default {
  data() {
    return {
      dataset: undefined,
      disable: true
    }
  },
  methods: {
    uploadDataset(event) {
      const datasetFile = event.target.files ? event.target.files[0] : null;
      if (datasetFile) {
        const reader = new FileReader();
        reader.readAsBinaryString(datasetFile);
        reader.onload = () => {
          const fileData = reader.result;
          const workbook = XLSX.read(fileData, {type: 'binary'});
          const firstSheetName = workbook.SheetNames[0];
          const sheet1 = workbook.Sheets[firstSheetName];
          this.dataset = XLSX.utils.sheet_to_json(sheet1);
          this.disable = false;
        };
      }
    },
    changeComponent() {
      if (this.dataset) {
        this.$emit('nextStep', {componentName: 'DataFrameTable', dataset: this.dataset});
      } else {
        alert("Debe seleccionar un dataset antes de continuar");
      }
    }
  }
}
</script>


