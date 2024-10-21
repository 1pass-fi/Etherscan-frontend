<template>
    <section class="header">
      <section class="topHeader">
        WBTC Price:
        <span class="blueText">{{ ethPrice.toFixed(2) }}</span>
      </section>
      <section class="navbar">
        <img :src="logo" alt="Etherscan Logo" class="logo" />
        <section class="menu">
          <p>Home</p>
          <p>
            Blockchain
            <span class="arrow">
              <!-- Your SVG here -->
            </span>
          </p>
          <!-- Other menu items -->
          <p class="signIn">
            <!-- Your sign-in icon here -->
            Sign In
          </p>
        </section>
      </section>
    </section>
</template>
  
  <script>
  import axios from 'axios';
  import logo from '../../public/assets/logo1.png';
  import styles from '../styles/Home.module.css'
  
  export default {
    data() {
      return {
        ethPrice: 0,
        logo,
      };
    },
    methods: {
      async getEthPrice() {
        try {
          const response = await axios.get('http://localhost:5000/getetherprice');
          this.ethPrice = response.data.usdPrice;
        } catch (error) {
          console.error('Error fetching ETH price:', error);
        }
      },
    },
    mounted() {
      this.getEthPrice();
    },
  };
  </script>
  
  <style scoped>
  .header {
    width: 100%;
    background-color: #111111;
  }
  
  .topHeader {
    display: flex;
    align-items: center;
    color: #bbbbbb;
    height: 3rem;
    font-size: 0.85rem;
    padding: 0 11.5rem;
    border-bottom: 1px solid #222222;
  }

  .navbar {
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 3.3rem;
    padding: 0 11.5rem;
    border-bottom: 1px solid #222222;
  }
  
  .logo {
    width: 9rem;
    height: auto;
  }
  .menu {
    display: flex;
    align-items: center;
    justify-content: space-between;
    color: #cccccc;
    width: 44rem;
    letter-spacing: 0.01rem;
  }
  
  .menu > p {
    display: flex;
    align-items: center;
  }
  
  .menu > p:hover {
    color: #3778ad;
    cursor: pointer;
  }

  .signIn {
    justify-content: space-between;
    width: 4.5rem;
  }
  
  .arrow {
    display: flex;
    width: 1rem;
  }

  .blueText {
    color: #7cb3d7;
  }
  </style>
  