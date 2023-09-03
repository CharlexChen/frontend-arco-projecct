<template>
  <a-space direction="vertical" fill>
    <a-input-search
      v-model="query.id"
      :style="{ width: '320px' }"
      placeholder="请输入ID"
      @search="onSearchClick"
    />
    <a-input-search
      v-model="query.username"
      :style="{ width: '320px' }"
      placeholder="请输入用户名"
      @search="onSearchClick"
    />
    <a-table :columns="columns" :data="data">
      <template #username="{ rowIndex }">
        <a-input
          v-model="data[rowIndex].username"
          @change="onAdminUpdate(rowIndex)"
        />
      </template>
    </a-table>
  </a-space>
</template>

<script lang="ts">
  import { reactive, onMounted, ref } from 'vue';
  import axios from 'axios';
  import { Message } from '@arco-design/web-vue';

  export default {
    setup() {
      const columns = [
        {
          title: 'ID',
          dataIndex: 'id',
        },
        {
          title: '用户名',
          dataIndex: 'username',
          slotName: 'username',
        },
        {
          title: '创建时间',
          dataIndex: 'createTime',
        },
        {
          title: '更新时间',
          dataIndex: 'updateTime',
        },
        {
          title: '状态',
          dataIndex: 'status',
        },
      ];
      const data = ref([
        // {
        //   id: '1',
        //   username: 'Jane Doe',
        //   create_time: '2023-09-08 00:00:00',
        //   update_time: '2023-09-08 00:00:00',
        //   status: 1,
        // },
        // {
        //   id: '2',
        //   username: 'Alisa Ross',
        //   create_time: '2023-09-08 00:00:00',
        //   update_time: '2023-09-08 00:00:00',
        //   status: 0,
        // },
        // {
        //   id: '3',
        //   username: 'Kevin Sandra',
        //   create_time: '2023-09-08 00:00:00',
        //   update_time: '2023-09-08 00:00:00',
        //   status: 1,
        // },
      ]);
      const query = reactive({
        id: undefined,
        username: undefined,
      });
      const onSearchClick = async (event) => {
        console.log('>>>onSearchClick', event, query);
        if (!query.id) {
          query.id = undefined;
        }
        if (!query.username) {
          query.username = undefined;
        }
        const res = await axios.get('/api/manager/getAdminList', {
          params: query,
        });
        if (res.error_code === 0) {
          data.value = res.data;
        }
      };
      const onAdminUpdate = async (rowIndex: number) => {
        console.log('>>>onAdminUpdate', data.value[rowIndex]);
        const { username } = data.value[rowIndex];
        const { id } = data.value[rowIndex];
        const res = await axios.post('/api/manager/updateAdminInfo', {
          id,
          username,
        });
        console.log('>>>res', res);
        if (res.error_code === 0) {
          Message.success('操作成功！');
        } else {
          Message.warning('操作成功！');
        }
      };
      onMounted(async () => {
        console.log('>>>start');
        const res = await axios.get('/api/manager/getAdminList');
        console.log('>>>res', res);
        if (res.error_code === 0) {
          data.value = res.data;
        }
      });
      return {
        columns,
        data,
        onSearchClick,
        onAdminUpdate,
        query,
      };
    },
  };
</script>

<style lang="less" scoped></style>
