<template>
  <div>
    <EleConfigTable
      :data="tableData"
      :columns="columns"
    />
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
      columns: [
        { label: '日期', prop: 'date' },
        [
          { label: '早餐', prop: 'z', prefix: '----' },
          { label: '晚餐', prop: 'w', suffix: '====' }
        ],
        {
          label: '配送信息',
          children: [
            { label: '姓名', prop: 'name', prefix: '-----', formatter: (row) => row.name },
            {
              label: '地址',
              children: [
                {
                  label: '省',
                  prop: 'sheng'
                },
                {
                  label: '市',
                  prop: 'shi'
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
        name: '王小虎',
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

<style lang="scss" scoped>
</style>
