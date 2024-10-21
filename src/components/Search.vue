<template>
    <section class="searchContainer">
      <section class="searchHeader">
        <section class="searchSection">
          <h3>The Ethereum Blockchain Explorer</h3>
          <section class="input_section">
            <input
              v-model="searchInput"
              class="inputField"
              type="text"
              id="inputField"
              maxLength="120"
              placeholder="Search by Address / Txn Hash / Block / Token / Domain Name"
              required
            />
            <button class="btn" @click="handleSearch">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                viewBox="0 0 24 24"
                fill="currentColor"
                class="magnifying"
              >
                <path
                  fill-rule="evenodd"
                  d="M10.5 3.75a6.75 6.75 0 100 13.5 6.75 6.75 0 000-13.5zM2.25 10.5a8.25 8.25 0 1114.59 5.28l4.69 4.69a.75.75 0 11-1.06 1.06l-4.69-4.69A8.25 8.25 0 012.25 10.5z"
                  clip-rule="evenodd"
                />
              </svg>
            </button>
          </section>
        </section>
        <section class="adSection">
          <Illustration logo="wizard" class="wizard" />
          <section>
            <Beans fontSize="50px" class="float" />
            <Illustration logo="wizard" class="wizard" />
          </section>
        </section>
      </section>
  
      <SearchResults v-if="showResult" :result="result" :searchInput="searchInput" />
    </section>
  </template>
  
  <script lang="ts" setup>
  import { ref } from 'vue';
  import axios from 'axios';
  import { Beans } from '@web3uikit/icons';
  import { Illustration } from '@web3uikit/core';
  import SearchResults from './SearchResults.vue'; // Adjust the path as necessary
  
  const searchInput = ref('');
  const showResult = ref(false);
  const result = ref([]);
  
  const handleSearch = async () => {
    const inputField = document.querySelector("#inputField");
    if (inputField) inputField.value = "";
  
    try {
      const response = await axios.get("http://localhost:5000/address", {
        params: { address: searchInput.value },
      });
  
      console.log("response", response);
      console.log("Result", response.data.result);
  
      result.value = response.data.result;
      showResult.value = true;
    } catch (error) {
      console.error('Error fetching data:', error);
    }
  };
  </script>
  
  <style scoped>

  .searchContainer {
    width: 100%;
  }

  .searchHeader {
    display: flex;
    background-color: rgba(0, 0, 0, 0.506);
    background-image: url("https://etherscan.io/images/svg/waves-light.svg");
    height: 17rem;
    padding: 0 11.5rem;
    padding-top: 3rem;
  }

  .searchSection > h3 {
    padding-bottom: 1rem;
    font-size: 1.3rem;
    letter-spacing: 0.02rem;
  }

  .input_section {
    display: flex;
    align-items: center;
    width: 50.5rem;
    height: 3rem;
    border: 1px solid #191919;
    border-radius: 0.3rem;
    background-color: #111111;
  }

  .inputField {
    width: 48rem;
    color: #ffffff;
    height: 2rem;
    padding-left: 2rem;
    border: none;
    background-color: #111111;
    font-size: 1rem;
  }

  .inputField::placeholder {
    color: #bbbbbb;
    font-size: 1rem;
  }

  .btn {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 2rem;
    height: 2rem;
    font-size: 1.2rem;
    background-color: #3a82be;
    border: none;
    outline: none;
    border-radius: 0.3rem;
    cursor: pointer;
  }

  .magnifying {
    width: 1.1rem;
  }

  .adSection {
    display: flex;
    justify-content: center;
    background-color: #3778adc7;
    /* color: #3778ad; */
    width: 30%;
    height: 60%;
    margin-left: 5rem;
    align-items: center;
    border-radius: 1rem;
  }

  .adtext {
    font-size: 1.2rem;
    font-weight: bold;
    text-align: center;
  }

  .adSection > section {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 13rem;
    margin-left: 1rem;
    padding-left: 0.5rem;
  }

  .float {
    animation-name: Floating;
    animation-duration: 3s;
    animation-iteration-count: infinite;
    animation-timing-function: ease-in-out;
  }

  @keyframes Floating {
    from {
      transform: translate(0, 0px);
    }
    50% {
      transform: translate(0, 10px);
    }
    to {
      transform: translate(0, -0px);
    }
  }

  .wizard {
    width: 8rem;
  }

  </style>
  