<template>
  <div class="el-data-table">
      <el-data-table v-bind="tableConfig"
      ></el-data-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableConfig: {
        url: '/api/example/on-get',
        id: 'id',
        columns: [
          {
            type: 'selection'
          },
          { prop: 'name',
            label: '用户名'
          },
          {
            prop: 'createdBy',
            label: '创建人'
          },
          {
            prop: 'createTime',
            label: '创建时间',
            formatter: row => this.$formatDate(new Date(row.createTime-0).getTime(), 'YYYY/MM/DD')
          },
          {
            prop: 'status',
            label: '状态',
            formatter: row => (row.status === 'normal' ? '启用' : '禁用')
          }
        ],
        form: [
          {
            type: 'input',
            id: 'name',
            label: '用户名',
            rules: [
              {
                required: true,
                message: '请输入用户名',
                trigger: 'blur',
                transform: v => v && v.trim()
              }
            ]
          }
        ],
        searchForm: [
          {
            type: 'input',
            id: 'name',
            label: '用户名',
            el: {placeholder: '请输入用户名'}
          }
        ],
        extraButtons: [
          {
            type: 'success',
            text: row => row.status === 'normal' ? '禁用' : '启用',
            atClick(row) {
              alert(row.name);
              row.status = row.status === 'normal'? 'abnormal': 'normal';
              this.$axios.put(
                '/api/example/on-edit/',
                row
              )
            }
          },
        ],
        paginationSizes: [2, 3, 5, 10, 20],
        onNew(data, row) {
          return this.$axios.post(
            '/api/example/on-new',
            data
          )
        },
        onEdit(data, row) {
          return this.$axios.put(
            '/api/example/on-edit/',
            data
          )
        },
        onDelete(selected) {
          return this.$axios.delete(
            '/api/example/on-delete',
            {
              data: {
                rows: selected.id || selected.map(v => v.id)
              }
            }
          )
        }
      }
    }
  },
  methods: {

  }
}
</script>
<style lang="less">
.index {
  .home-img {
    width: 100%;
  }
}
</style>
