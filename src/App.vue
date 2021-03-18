<template>
  <div id="app">
    <h2>List Jadwal Interview Frontend</h2>
    <table class="table">
      <tr>
        <th>Id</th>
        <th>Tanggal Pelaksanaan</th>
        <th>Waktu Pelaksanaan</th>
        <th>Media</th>
        <th>Tempat Pelaksanaan</th>
        <th>Aksi</th>
      </tr>
      <tr v-for="(result, index) in results" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ result.tgl_pelaksanaan }}</td>
        <td>{{ result.waktu_awal_pelaksanaan + " Sd " + result.waktu_akhir_pelaksanaan }}</td>
        <td>{{ result.media }}</td>
        <td>{{ result.tempat_pelaksanaan }}</td>
        <td><button @click="detailData(result)">Lihat</button></td>
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
              <td></td>
              <td></td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",

  data() {
    return {
      results: [],

      modalShow: false,
      tampungData: {},
    };
  },

  methods: {
    async getData() {
      const data = await fetch("http://localhost:3000/jadwalInterview");
      const res = await data.json();

      this.results = res;
      console.info(res);
    },
    detailData(res) {
      this.modalShow = true;
      this.tampungData = res;
      console.info(res);
    },
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

.table > tr {
  background-color: snow;
}
.table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

.table > tr > th {
  border: 2px solid #dddddd;
  text-align: left;
  padding: 18px;
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
