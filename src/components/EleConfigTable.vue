<script>
/* eslint-disable no-unused-vars */
// 扁平化
function flatArray(arr) {
  return arr.reduce((curr, next) => curr.concat(Array.isArray(next) ? flatArray(next) : next), [])
}

// 多级表头
function multipleTitle(data, h) {
  return [data.map(item => {

    const props = {
      ...item,
      ...formatProps({ ...item }),
    }

    return h('el-table-column', { props },
      (item.children && item.children.length) ? multipleTitle(item.children, h) : null)

  })]
}

// 格式化props
function formatProps({ formatter, prefix = '', suffix = '', prop }) {
  let fixFormatter = {}

  if (formatter) {
    // 有格式化数据情况
    fixFormatter.formatter = (...args) =>
      formatter(...args)
  } else if (prefix || suffix) {
    // 有前、后缀情况
    fixFormatter.formatter = row => `${prefix}${row[prop]}${suffix}`
  }
  return fixFormatter
}

// 样式对象转样式字符串
function styleO2S(styleO) {
  let styleS = ''
  for (let i in styleO) {
    styleS += `${i}:${styleO[i]};`
  }
  return styleS
}

let index = 0

export default {
  functional: true,
  name: 'EleConfigTable',
  render(h, context) {
    const { props: { data, columns, childType }, data: { staticStyle, scopedSlots } } = context

    // 存在子集情况
    // 获取合并行列的规则
    let _$start = 0
    let _$end = 0
    // 获取是否存在子集
    const isChildren = columns.some((column, index) => {
      if (Array.isArray(column)) {
        _$start = index
        _$end = index + column.length - 1
        return true
      }
    })

    // 格式化data
    const formatData = isChildren ? data.reduce((curr, next) => {
      curr.push(next)
      const childList = next[childType] || []
      if (Array.isArray(childList) && childList.length) {
        const last = curr[curr.length - 1]
        last._$len = childList.length
        curr[curr.length - 1] = { ...last, ...childList[0] }
        curr.push(...childList.slice(1))
      }
      return curr
    }, []) : data

    // 格式化columns
    const formatColumns = isChildren ? flatArray(columns) : columns

    const template = h('el-table', {
      attrs: {
        style: styleO2S(staticStyle)
      },
      props: {
        data: formatData,
        [isChildren ? 'spanMethod' : null]({ row, columnIndex }) {
          if (columnIndex < _$start || columnIndex > _$end) {
            if (row._$len >= 0) {
              return {
                rowspan: row._$len,
                colspan: 1
              }
            } else {
              return {
                rowspan: 0,
                colspan: 0
              }
            }
          }
        }
      }
    }, formatColumns.map((column) => {
      const { prefix = '', suffix = '', formatter, children, ...columnProps } = column

      const props = {
        ...columnProps,
        ...formatProps({ prefix, suffix, formatter, prop: columnProps.prop }),
      }

      let content = null
      if (children && children.length) {
        content = multipleTitle(children, h)
      }

      if (props.type === 'expand') {
        if (!props.expand || !scopedSlots[props.expand]) {
          throw new Error('Could not find the extension template slot or the slot name is inconsistent with the extension name')
        }
      }
      console.log(index);

      return h('el-table-column', {
        props,
      }, props.type === 'expand' ? scopedSlots[props.expand]({ index }) : content)
    }))


    index++
    return template
  }

}
</script>
