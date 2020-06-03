<template>
  <div>
    <!-- <EleConfigTable
      :data="tableData"
      :columns="columns"
      show-summary
    /> -->
    <EleConfigTable
      :data="tableData"
      :columns="columns2"
      show-summary
    >
      <template v-slot:user="slotProps">
        <div>
          <p>123</p>
          <p>{{JSON.stringify(slotProps)}}</p>
        </div>
      </template>
    </EleConfigTable>
  </div>
</template>

<script>
import EleConfigTable from './components/EleConfigTable'
export default {
  components: {
    EleConfigTable,
  },
  data() {
    return {
      columns2: [
        {
          type: 'expand',
          expand: 'user'
        },
        {
          label: '日期',
          formatter: (row) => this._c('el-tag', row.date)
        },
        {
          label: '姓名',
          prop: 'name'
        },
      ],
      columns: [
        {
          label: '日期',
          formatter: (row) => this._c('el-tag', row.date)
        },
        [
          {
            label: '早餐',
            formatter: (row) => this._c('el-tag', row.z)
          },
          { label: '晚餐', prop: 'w', suffix: '====' }
        ],
        {
          label: '配送信息',
          children: [
            { label: '姓名', formatter: (row) => row?.name },
            {
              label: '地址',
              children: [
                {
                  label: '省',
                  prop: 'sheng',
                  suffix: '===='
                },
                {
                  label: '市',
                  formatter: (row) => this._c('el-input', {
                    attrs: {
                      maxlength: "10",
                    },
                    props: {
                      showWordLimit: true
                    },
                    model: {
                      value: row.shi,
                      callback: (val) => {
                        row.shi = val
                      }
                    }
                  })
                },
                {
                  label: '区',
                  prop: 'qu'
                },
              ]
            }
          ]
        },
        { label: '地址', prop: 'address', suffix: '======' },
      ],
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄',
        sheng: '广东省',
        shi: '东莞市',
        qu: '莞城区',
        children: [
          { z: '苹果', w: '菠萝' },
          { z: '火龙果', w: '香蕉' },
        ]
      }, {
        date: '2016-05-04',
        name: '王22',
        address: '上海市普陀区金沙江路 1517 弄',
        sheng: '陕西省',
        shi: '西安市',
        qu: '雁塔区',
        children: [
          { z: '橘子', w: '柚子' },
          { z: '西瓜', w: '荔枝' },
        ]
      }]
    }
  }
}
</script>
