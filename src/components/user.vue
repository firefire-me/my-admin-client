<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";
import { message } from "ant-design-vue";

// ⚠️这里填你后端部署后的 Vercel 域名，本地测试填 http://localhost:3000
const API_URL = import.meta.env.VITE_API_URL || "http://localhost:3000";

const users = ref<any[]>([]);
const loading = ref(false);

// 获取用户列表
const fetchUsers = async () => {
  loading.value = true;
  try {
    const res = await axios.get(`${API_URL}/users`); // 假设你生成了 users 资源
    users.value = res.data;
  } catch (error) {
    message.error("获取数据失败");
  } finally {
    loading.value = false;
  }
};

// 添加测试用户
const addUser = async () => {
  try {
    await axios.post(`${API_URL}/users`, {
      email: `test${Date.now()}@example.com`,
      name: "Test User",
    });
    message.success("添加成功");
    fetchUsers();
  } catch (error) {
    message.error("添加失败");
  }
};

onMounted(() => {
  // 页面加载时获取数据
  // fetchUsers();
});
</script>

<template>
  <div style="padding: 50px; max-width: 800px; margin: 0 auto">
    <a-card title="个人后台管理系统 - 用户列表">
      <template #extra>
        <a-button type="primary" @click="addUser">模拟添加用户</a-button>
        <a-button style="margin-left: 10px" @click="fetchUsers"
          >刷新列表</a-button
        >
      </template>

      <a-table
        :dataSource="users"
        :loading="loading"
        rowKey="id"
        :columns="[
          { title: 'ID', dataIndex: 'id', key: 'id' },
          { title: '姓名', dataIndex: 'name', key: 'name' },
          { title: '邮箱', dataIndex: 'email', key: 'email' },
          { title: '创建时间', dataIndex: 'createdAt', key: 'createdAt' },
        ]"
      />
    </a-card>
  </div>
</template>
