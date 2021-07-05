<template>
  <q-pag padding>
    <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
          <div class="col-auto">
            <div class="left blue"></div>
          </div>
          <div class="col">
            <q-banner inline-actions class="text-blue-grey-14">
              <div class="text-h6">My Transaksi</div>
              <div>Data Transaksi Pembelian dan Pemesanan</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>
    <q-card>
      <q-table
        :rows="data"
        :columns="columns"
        row-key="name"
      >
        <template v-slot:body="props">
          <q-tr :props="props">
            <q-td key="judulFilm" :props="props">
              {{ props.row.dataMovie[0].judulFilm }}
            </q-td>
            <q-td key="harga" :props="props">
              Rp {{ props.row.harga }},-
            </q-td>
            <q-td key="jumlah" :props="props">
              {{ props.row.jumlah }}
            </q-td>
            <q-td key="total" :props="props">
              Rp {{ props.row.total }},-
            </q-td>
            <q-td key="status" :props="props">
              <q-badge v-if="props.row.status === 1" color="red" class="q-pa-sm">
                Belum Dikonfirmasi
              </q-badge>
              <q-badge v-if="props.row.status === 2" color="orange" class="q-pa-sm">
                Sedang Dalam Pengiriman
              </q-badge>
              <q-badge v-else color="green" class="q-pa-sm">
                Sudah Diterima
              </q-badge>
            </q-td>
            <q-td key="aksi" :props="props">
              <q-btn :disable="props.row.status !== 2" @click="confirm(props.row._id)" label="Terima Barang" class="q-ml-lg" color="green" unelevated/>
            </q-td>
          </q-tr>
        </template>
      </q-table>
    </q-card>
  </q-pag>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      columns: [
        { name: 'judulFilm', align: 'center', label: 'Judul Film', field: 'judulFilm', sortable: true },
        { name: 'harga', align: 'center', label: 'Harga', field: 'harga', sortable: true },
        { name: 'jumlah', align: 'center', label: 'Jumlah Beli', field: 'jumlah', sortable: true },
        { name: 'total', align: 'center', label: 'Total', field: 'total', sortable: true },
        { name: 'status', align: 'center', label: 'Status', field: 'status', sortable: true },
        { name: 'aksi', align: 'center', label: 'Aksi', field: 'aksi' }
      ],
      data: [],
      detail: false,
      activeData: null
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get(`order/getorderbyuser/${this.$q.localStorage.getItem('dataUser')._id}`)
        .then((res) => {
          if (res.data.sukses) {
            this.data = res.data.data
          }
        })
    },
    confirm (id) {
      this.$q.dialog({
        title: 'Konfirmasi',
        message: 'Apakah Anda Yakin Sudah Menerima Barang?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios.put(`order/terimabarang/${id}`)
          .then((res) => {
            if (res.data.sukses) {
              this.$showNotif(res.data.pesan, 'positive')
              this.getData()
            } else {
              this.$showNotif(res.data.pesan, 'negative')
            }
          })
      })
    }
  }
}
</script>
<style scoped>
.left {
  width: 5px;
  height: 100%;
  background: rgb(3, 196, 180);
}
</style>
