<template>
  <div class="col-md-4">
    <div class="form-group">
      <label for="isbn">Enter ISBN-12:</label>
      <div class="input-group">
        <input type="text" class="form-control input-box" id="isbn" v-model="isbn" />
        <div class="input-group-append">
          <button class="btn btn-primary button-calculate" @click="calculateCheckDigit">
            <i class="fas fa-calculator"></i>
            Calculate
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isbn: '',
    };
  },
  methods: {
    calculateCheckDigit() {
      this.$emit('calculate-check-digit', this.isbn);
    },
  },
};
</script>

<style>
.form-group {
  margin-bottom: 20px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
  font-size: 1.2rem;
}

.input-group {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
}

.input-box {
  flex-grow: 1;
  margin-right: 10px;
  padding: 10px;
  border-radius: 5px;
  border: none;
  background-color: #fff;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.1);
  font-size: 1.2rem;
}

.button-calculate {
  padding: 10px 20px;
  border-radius: 5px;
  background-color: #f44336;
  color: #fff;
  font-size: 1.2rem;
  border: none;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  transition: background-color 0.2s ease-in-out;
}

.button-calculate:hover {
  background-color: #d32f2f;
}
</style>
