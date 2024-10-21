<template>
    <section class="searchResults">
      <p class="amountOfTransactions">
        Latest 25 from a total of
        <span class="blueText">{{ result.length }}</span> transactions
      </p>
      <table class="txnSection">
        <thead>
          <tr class="txnTitle">
            <th>Transaction Hash</th>
            <th>Method</th>
            <th>Block</th>
            <th class="blueText">Age</th>
            <th>From</th>
            <th></th>
            <th>To</th>
            <th>Value</th>
            <th class="blueText">Txn Fee</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(txn, index) in result" :key="index" class="txn">
            <td class="blueText">{{ txn.hash.slice(0, 16) }}... </td>
            <td>
              <span class="transfer">
                {{ txn.decoded_call ? txn.decoded_call.label : 'Unknown' }}
              </span>
            </td>
            <td class="blueText">{{ txn.block_number }}</td>
            <td>{{ formatTimestamp(txn.block_timestamp) }}</td>
            <td>
              {{ txn.from_address.slice(0, 8) }}...{{ txn.from_address.slice(34) }}
            </td>
            <td>
              <span
                :class="txn.from_address.toLowerCase() !== props.searchInput.toLowerCase() ? 'inTxn' : 'outTxn'"
              >
                {{ txn.from_address.toLowerCase() !== props.searchInput.toLowerCase() ? 'IN' : 'OUT' }}
              </span>
            </td>
            <td class="blueText">
              {{ txn.to_address.slice(0, 8) }}...{{ txn.to_address.slice(34) }}
            </td>
            <td>{{ (txn.value / 10 ** 18).toFixed(5) }} ETH</td>
            <td>{{ (txn.gas_price / 10 ** 18).toFixed(12) }}</td>
          </tr>
        </tbody>
      </table>
    </section>
  </template>
  
  <script lang="ts" setup>
  import { defineProps } from 'vue';
  import moment from 'moment';
  
  const props = defineProps<{
    result: Array<any>; // Adjust type based on your expected result structure
    searchInput: string;
  }>();
  
  const formatTimestamp = (timestamp: string) => {
    return moment(timestamp).fromNow();
  };
  </script>
  
  <style scoped>
.searchResults {
  background-color: #111111;
  margin: 2rem 11.5rem;
  border: 1px solid #222222;
  border-radius: 0.5rem;
  box-shadow: 0rem 0 1.5rem #ffffff09;
  padding: 1rem;
}

.searchResults > p {
  letter-spacing: 0.02rem;
  padding-bottom: 1rem;
  border-bottom: 1px solid #191919;
  margin-bottom: 1rem;
}

.blueText {
  color: #7cb3d7;
}

.txnSection {
  display: flex;
  flex-direction: column;
  align-content: space-between;
}

.txnTitle {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #191919;
  margin-bottom: 1rem;
  padding-bottom: 1rem;
}

.txnTitle > th {
  width: 9rem;
  text-align: center;
}

.txn {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #191919;
}

.txn > td {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 9rem;
  height: 3rem;
  text-align: center;
}

.transfer {
  font-size: 0.8rem;
  width: 8rem;
  background-color: #151515;
  padding: 0.35rem 1.2rem;
  border: 1px solid #2c2c2c;
  border-radius: 0.3rem;
}

.inTxn {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 2.5rem;
  height: 1.5rem;
  font-size: 0.8rem;
  font-weight: bold;
  background-color: #161f1d;
  color: #479f87;
  border: 1px solid #223f37;
  border-radius: 0.3rem;
}

.outTxn {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 2.5rem;
  height: 1.5rem;
  font-size: 0.8rem;
  font-weight: bold;
  background-color: #2d2615;
  color: #96792a;
  border: 1px solid #4c3f1e;
  border-radius: 0.3rem;
}

  </style>
  