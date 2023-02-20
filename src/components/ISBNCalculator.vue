<template>
  <div class="container mt-4">
    <div class="row">
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
      <div class="col-md-8">
        <div v-if="result" class="alert alert-success alert-result" role="alert">
          <div class="d-flex justify-content-between align-items-center">
            <span class="font-weight-bold">ISBN-13 with check digit:</span>
            <div class="result-box">{{ result }}</div>
            <div class="ml-2">
              <button class="btn btn-outline-secondary btn-sm button-copy" @click="copyToClipboard(result)">
                <i class="fas fa-copy"></i>
                Copy to clipboard
              </button>
            </div>
          </div>
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
      checkDigit: null,
      completeIsbn: null,
      result: null,
    };
  },
  methods: {
    calculateCheckDigit() {
      this.result = null;

      let isbn = this.isbn;

      // Ensure that the ISBN is a string
      if (typeof isbn !== 'string') {
        isbn = isbn.toString();
      }

      // Check if ISBN is valid
      if (!/^\d{12}$/.test(isbn)) {
        this.result = 'Invalid ISBN';
        return;
      }

      // Multiply digits alternatively by 1 and 3, from left to right
      const weights = [
        1, 3, 1, 3, 1, 3, 1, 3, 1, 3, 1, 3
      ];

      const digits = isbn.split('').map(Number);
      const products = digits.map((digit, i) => digit * weights[i]);

      // Sum the products
      const total = products.reduce((acc, product) => acc + product, 0);

      // Take mod 10 of the summed figure
      const remainder = total % 10;

      // Subtract from 10 and adjust for edge case
      const checkDigit = (10 - remainder) % 10;

      // Append the check digit to the ISBN-13 code
      // Show the result with an animation
      this.result = `${isbn}${checkDigit}`;

      // Wait for the animation to finish before scrolling to the result
      this.$nextTick(() => {
        const resultElement = document.querySelector('.alert-success');
       if (resultElement) {
          resultElement.scrollIntoView({ behavior: "smooth", block: "start" });
        } else {
          const errorElement = document.createElement("div");
          errorElement.textContent = "No results found.";
          errorElement.style.color = "red";
         let searchResultsContainer = document.querySelector(".search-results");
          searchResultsContainer.appendChild(errorElement);
        }
      });
    },
    copyToClipboard(text) {
      const input = document.createElement('input');
      input.setAttribute('value', text);
      document.body.appendChild(input);
      input.select();
      document.execCommand('copy');
      document.body.removeChild(input);
    },
  },
};

</script>

<style>
/* Set a custom font family for the entire page */
body {
  font-family: 'Montserrat', sans-serif;
}

/* Set background and text color for the page */
body {
  background-color: #f8f8f8;
  color: #333;
}

/* Center the container and set some padding */
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

/* Style the form */
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

/* Style the result box */
.alert-result {
  margin-top: 20px;
  padding: 20px;
  border-radius: 5px;
  background-color: #f44336;
  box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.3);
}

.alert-result span {
  display: block;
  margin-bottom: 10px;
  font-weight: bold;
  font-size: 1.2rem;
  color: #fff;
}

.result-box {
  margin-top: 10px;
  padding: 10px;
  border-radius: 5px;
  background-color: #fff;
  color: #333;
  font-size: 1.2rem;
  display: inline-block;
}

/* Style the copy button */
.button-copy {
  margin-top: 10px;
  padding: 10px 20px;
  border-radius: 5px;
  background-color: #fff;
  color: #333;
  font-size: 1.2rem;
  border: none;
  box-shadow: 0px 2px 10px rgba(0, 0, 0, 0.3);
  cursor: pointer;
  transition: background-color 0.2s ease-in-out;
}

.button-copy:hover {
  background-color: #f2f2f2;
}

/* Media queries for smaller devices */
@media only screen and (max-width: 600px) {
  .container {
    padding: 10px;
  }

  .input-group {
    flex-direction: column;
  }

  .input-box {
    margin-right: 0;
    margin-bottom: 10px;
  }

  .button-calculate {
    margin-top: 10px;
  }
}
</style>
