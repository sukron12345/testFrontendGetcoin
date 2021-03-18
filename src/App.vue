<template>
  <div id="app">
    <table class="table">
      <tr>
        <th>Id</th>
        <th>Coin</th>
        <th>Price</th>
        <th>24h Volume</th>
        <th>Market Cap</th>
      </tr>
      <tr
        v-for="(result, index) in results"
        :key="index"
        @click="tampilData(result)"
      >
        <td>{{ index + 1 }}</td>
        <td>
          <img :src="result.image" width="20px" height="20px" />{{
            " " + result.name
          }}
        </td>
        <td>{{ "IDR " + result.current_price }}</td>
        <td>{{ "IDR " + result.price_change_percentage_24h }}</td>
        <td>{{ "IDR " + result.market_cap }}</td>

        <!-- <td>{{ result.b }}</td> -->
        <!-- <td>{{ result.b }}</td> -->
      </tr>
    </table>

    <div id="myModal" class="modal" v-if="modalShow">
      <!-- Modal content -->
      <div class="modal-content">
        <span class="close" @click="modalShow = !modalShow">&times;</span>
        <div style="width:100%">
          <table style="width:100%">
            <tr>
              <td>Market Cap</td>
              <td>24 Hour Trading Vol</td>
            </tr>
            <tr>
              <td>{{ "IDR " + tampungData.market_cap }}</td>
              <td>{{ "IDR " + tampungData.market_cap_change_24h }}</td>
            </tr>
            <tr>
              <td>24 Low / 24h High</td>
              <td>Circulating Supply</td>
            </tr>
            <tr>
              <td>
                {{
                  "IDR " +
                    tampungData.low_24h +
                    " / IDR " +
                    tampungData.high_24h
                }}
              </td>
              <td>
                {{ tampungData.circulating_supply }}
              </td>
            </tr>

            <tr>
              <td>Fully diluted valuation</td>
              <td>Max Supply</td>
            </tr>
            <tr>
              <td>
                {{ "IDR " + tampungData.fully_diluted_valuation }}
              </td>
              <td>
                {{ tampungData.max_supply }}
              </td>
            </tr>
          </table>
        </div>
      </div>
    </div>

    <!-- <div>
      <input type="text" v-model="title" />
      <input type="text" v-model="body" />
      <button @click="postData">save</button>
    </div> -->
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      results: [],
      title: "",
      body: "",
      modalShow: false,
      tampungData: {},
    };
  },

  methods: {
    async getData() {
      const data = await fetch(
        "https://api.coingecko.com/api/v3/coins/markets?vs_currency=idr&order=market_cap_desc&per_page=100&page=5&sparkline=false"
      );
      const res = await data.json();

      this.results = res.map((el) => {
        const emc = el.market_cap;
        const emd = el.current_price;
        return {
          ...el,

          market_cap: emc.toLocaleString("id", "ID"),
          current_price: emd.toLocaleString("id", "ID"),
        };
      });
    },
    tampilData(res) {
      this.modalShow = true;
      this.tampungData = res;
      console.info(res);
    },
    // async postData() {
    //   const katalog = {
    //     title: this.title,
    //     body: this.body,
    //   };
    //   console.info(this.title, " ", this.body);
    //   let response = await fetch("https://jsonplaceholder.typicode.com/posts", {
    //     method: "POST",
    //     headers: {
    //       "Content-Type": "application/json;charset=utf-8",
    //     },
    //     body: JSON.stringify(katalog),
    //   });
    //   let result = await response.json();
    //   alert(result.message);
    // },
  },
  mounted() {
    this.getData();
  },
  created() {
    console.info("");
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  color: #2c3e50;
  margin-top: 60px;
}

.table > tr:hover {
  cursor: pointer;
  background-color: azure;
}
.table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

.table > tr > th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

.table > tr:nth-child(even) {
  background-color: #dddddd;
}

.modal {
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: auto; /* Enable scroll if needed */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
}

/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  margin: 15% auto; /* 15% from the top and centered */
  padding: 20px;
  border: 1px solid #888;
  width: 80%; /* Could be more or less, depending on screen size */
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}
</style>
