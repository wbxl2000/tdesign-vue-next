<template>
  <t-table
    :data="data"
    :columns="columns"
    :row-key="rowKey"
    :loading="isLoading"
    :pagination="pagination"
    :selected-row-keys="selectedRowKeys"
    bordered
    stripe
    @change="rehandleChange"
    @page-change="onPageChange"
    @select-change="onSelectChange"
  />
</template>
<script setup>
import { ref, reactive, onMounted } from 'vue';

const columns = [
  {
    colKey: 'row-select',
    type: 'multiple',
    width: 46,
  },
  {
    colKey: 'serial-number',
    title: '序号',
    width: 60,
  },
  {
    width: 200,
    colKey: 'name',
    title: '姓名',
    render(h, { row: { name } }) {
      return name ? `${name.first} ${name.last}` : 'UNKNOWN_USER';
    },
  },
  // {
  //   width: 200,
  //   colKey: 'gender',
  //   title: '性别',
  // },
  {
    width: 200,
    colKey: 'phone',
    title: '联系方式',
    render(h, { row: { phone } }) {
      return phone;
    },
  },
  {
    colKey: 'email',
    title: '邮箱',
    width: 180,
    ellipsis: true,
  },
];

const data = ref([]);
const isLoading = ref(false);
const selectedRowKeys = ref([]);

const pagination = reactive({
  current: 1,
  pageSize: 10,
  // defaultCurrent: 1,
  // defaultPageSize: 10,
  total: 0,
  showJumper: true,
  onChange: (pageInfo) => {
    console.log('pagination.onChange', pageInfo);
  },
});

const fetchData = async (paginationInfo) => {
  try {
    isLoading.value = true;
    const { current, pageSize } = paginationInfo;
    // 请求可能存在跨域问题
    const response = await fetch(`https://randomuser.me/api?page=${current}&results=${pageSize}`);
    const { results } = await response.json();
    data.value = results;
    // 数据加载完成，设置数据总条数
    pagination.total = 120;
  } catch (err) {
    console.log(err);
    data.value = [];
  }
  isLoading.value = false;
};

// BaseTable 中只有 page-change 事件，没有 change 事件
const rehandleChange = (changeParams, triggerAndData) => {
  console.log('分页、排序、过滤等发生变化时会触发 change 事件：', changeParams, triggerAndData);
};

// BaseTable 中只有 page-change 事件，没有 change 事件
const onPageChange = async (pageInfo) => {
  console.log('page-change', pageInfo);
  pagination.current = pageInfo.current;
  pagination.pageSize = pageInfo.pageSize;
  await fetchData(pageInfo);
};

onMounted(async () => {
  await fetchData({
    current: pagination.current || pagination.defaultCurrent,
    pageSize: pagination.pageSize || pagination.defaultPageSize,
  });
});

const onSelectChange = (value, params) => {
  selectedRowKeys.value = value;
  console.log(value, params);
};

const rowKey = 'phone';
</script>
