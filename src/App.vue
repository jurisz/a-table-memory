<template>
  <div id="app">

<!--             :row-selection="{selectedRowKeys: selectedDevices, onChange: onDeviceSelectChange}"-->
    <a-table :columns="columnDefinitions"
             :data-source="items"
             :pagination="false"
             @change="tableChange"
             rowKey="id">
    </a-table>

    <a-pagination
        background
        layout="prev, pager, next"
        :current="page"
        :pageSize="50"
        :total="250"
        size="default"
        @change="pageChange">
    </a-pagination>

  </div>
</template>

<script>

const columnDefinitions = [
  {
    title: 'Id',
    dataIndex: 'id',
    sorter: true,
  },
  {
    title: 'Building',
    dataIndex: 'buildingAddress',
    sorter: true,
  },
  {
    title: 'RoomGroup',
    dataIndex: 'roomGroupName',
    sorter: true
  },
  {
    title: 'Info',
    dataIndex: 'info',
    sorter: true,
  },
  {
    title: 'Device',
    dataIndex: 'deviceName',
    sorter: true,
  },
  {
    title: 'Gateway',
    dataIndex: 'gwName',
    sorter: true,
  },
  {
    title: 'Address',
    dataIndex: 'address',
    sorter: true,
  },
  {
    title: 'Status',
    dataIndex: 'online',
    sorter: true,
  }
]

export default {
  name: 'App',
  components: {},
  data() {
    return {
      columnDefinitions,
      items: [],
      page: 1,
      selectedDevices: [],
    }
  },
  async created() {
    await this.loadPage(1)
  },
  methods: {
    async loadPage(nr) {
      const resp = await this.$http.get(`/assets/page${nr}.json`)
      this.items = resp.data.results
    },
    pageChange(page) {
      this.page = page;
      this.loadPage(page);
    },
    tableChange(pagination, filter, sorter) {
      this.loadPage(this.page);
    },
    onDeviceSelectChange(selectedRowKeys) {
      this.selectedDevices = selectedRowKeys;
    },
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
