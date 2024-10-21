<template>
    <section class="heroSectionContainer">
      <section v-if="showResult">
        <section class="latestResults_header">
          <section>
            <section class="latestResults_box">
              <section class="svgSection">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  viewBox="0 0 256 417"
                  preserveAspectRatio="xMidYMid"
                  class="svgEth"
                >
                  <path
                    fill="#fff"
                    d="M127.961 0l-2.795 9.5v275.668l2.795 2.79 127.962-75.638z"
                  />
                  <path
                    fill="#fff"
                    d="M127.962 0L0 212.32l127.962 75.639V154.158z"
                  />
                  <path
                    fill="#fff"
                    d="M127.961 312.187l-1.575 1.92v98.199l1.575 4.6L256 236.587z"
                  />
                  <path fill="#fff" d="M127.962 416.905v-104.72L0 236.585z" />
                  <path
                    fill="#eee"
                    d="M127.961 287.958l127.96-75.637-127.96-58.162z"
                  />
                  <path fill="#bbb" d="M0 212.32l127.96 75.638v-133.8z" />
                </svg>
              </section>
              <section class="hero_box">
                <p>WBTC PRICE</p>
                <p class="heroValues">
                  ${{ Number(ethPrice).toFixed(2) }}
                </p>
              </section>
            </section>
            <span class="divider"></span>
            <section class="latestResults_box">
              <section class="svgSection">
                <FontAwesomeIcon :icon="['fas', 'globe']" class="svgIcons" />
              </section>
              <section class="hero_box">
                <p>MARKET CAP</p>
                <p class="heroValues">$230,888,751,724.00</p>
              </section>
            </section>
          </section>
          <section>
            <section class="latestResults_box">
              <section class="svgSection">
                <FontAwesomeIcon :icon="['fas', 'server']" class="svgIcons" />
              </section>
              <section class="hero_box">
                <p>TRANSACTIONS</p>
                <p class="heroValues">{{ totalTransactions }}</p>
              </section>
            </section>
            <span class="divider"></span>
            <section class="latestResults_box">
              <section class="svgSection">
                <FontAwesomeIcon :icon="['fas', 'gauge']" class="svgIcons" />
              </section>
              <section class="hero_box">
                <p>LAST FINALIZED BLOCK</p>
                <p class="heroValues">{{ latestBlock }}</p>
              </section>
            </section>
          </section>
          <section>
            <section class="hero_averageValue">
              <p>Average Transaction Value</p>
              <img :src="Chart" alt="Chart" class="chart" />
            </section>
          </section>
        </section>
        <section class="latestResults_body">
          <section>
            <section class="latestResults_body_title">Latest Blocks</section>
            <table class="latestResults_body_table">
              <tbody>
                <tr v-for="(block, index) in blockResult" :key="index" class="latestResults_body_tr">
                  <td class="tdIcon">
                    <FontAwesomeIcon icon="cube" />
                  </td>
                  <td class="tdBlock">
                    <section class="blueText">{{ block.blockNumber }}</section>
                    <section>{{ moment(block.time).fromNow() }}</section>
                  </td>
                  <td class="tdTxns">
                    <section>
                      Fee Recipient
                      <span class="blueText">
                        {{ block.miner.slice(0, 6) }}...{{ block.miner.slice(36) }}
                      </span>
                    </section>
                    <section>
                      <span class="blueText">{{ block.totalTransactions }} txns</span>
                    </section>
                  </td>
                  <td class="tdValue">{{ block.gasUsed }} Eth</td>
                </tr>
              </tbody>
            </table>
          </section>
          <section>
            <section class="latestResults_body_title">Latest Transactions</section>
            <table class="latestResults_body_table">
              <tbody>
                <tr v-for="(txn, index) in transactionsResult" :key="index" class="latestResults_body_tr">
                  <td class="tdContract">
                    <FontAwesomeIcon icon="file-contract" />
                  </td>
                  <td class="tdBlock">
                    <section class="blueText">{{ txn.transactionHash?.slice(0, 14) }}...</section>
                    <section>{{ moment(txn.time).fromNow() }}</section>
                  </td>
                  <td class="tdFromTo">
                    <section>
                      From
                      <span class="blueText">{{ txn.fromAddress?.slice(0, 6) }}...{{ txn.fromAddress?.slice(36) }}</span>
                    </section>
                    <section>
                      To
                      <span class="blueText">{{ txn.toAddress?.slice(0, 6) }}...{{ txn.toAddress?.slice(36) }}</span>
                    </section>
                  </td>
                  <td class="tdValue">{{ (Number(txn.value) / 10 ** 18).toFixed(4) }} Eth</td>
                </tr>
              </tbody>
            </table>
          </section>
        </section>
      </section>
    </section>
  </template>
  
  <script lang="ts" setup>
  import { ref, onMounted } from 'vue';
  import axios from 'axios';
  import {FontAwesomeIcon} from '@fortawesome/vue-fontawesome';
  import moment from 'moment';
  import Chart from '../../public/assets/chart.png';
  const showResult = ref(true);
  const blockResult = ref([]);
  const transactionsResult = ref([]);
  const ethPrice = ref("");
  const totalTransactions = ref("");
  const latestBlock = ref("");
  
  // Function to fetch ETH price
  const getEthPrice = async () => {
    const response = await axios.get("http://localhost:5000/getetherprice");
    ethPrice.value = response.data.usdPrice;
  };
  
  // Function to fetch block info
  const getBlockInfo = async () => {
    const response = await axios.get("http://localhost:5000/blockinfo");
    const blockArray = response.data.previousBlockInfo.slice(1, 6); // Get blocks
    totalTransactions.value = response.data.previousBlockInfo[1].totalTransactions;
    latestBlock.value = response.data.latestBlock;
    blockResult.value = blockArray;
    console.log("blockResult=", blockResult);
    transactionsResult.value = response.data.previousBlockInfo[0].transactions;
    console.log("transactionResult=", transactionsResult);
  };
  
  // Fetch data on component mount
  onMounted(() => {
    getEthPrice();
    getBlockInfo();
  });
  </script>
  
  <style scoped>
  /* Add your styles here */
  </style>
  