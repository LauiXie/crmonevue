<template>
  <div class="home-page">
    <el-card class="welcome-card" shadow="never">
      <div class="welcome-content flex items-center gap-16px">
        <el-avatar :src="avatar" :size="64" class="welcome-avatar">
          <img src="@/assets/svgs/default-avatar.svg" alt="" />
        </el-avatar>
        <div class="min-w-0">
          <div class="welcome-label">CRM 工作台</div>
          <div class="welcome-title">你好，{{ username }}</div>
          <div class="welcome-subtitle">{{ todayText }} · 欢迎使用 CRM 演示系统</div>
        </div>
      </div>
    </el-card>

    <el-card shadow="never" class="shortcut-section mt-12px">
      <template #header>
        <div>
          <div class="section-title">常用功能</div>
          <div class="section-subtitle">快速进入核心业务</div>
        </div>
      </template>
      <el-row :gutter="12" class="gap-y-12px">
        <el-col v-for="item in shortcuts" :key="item.url" :xl="4" :lg="6" :md="8" :sm="12" :xs="12">
          <button
            :aria-label="`打开${item.name}`"
            class="shortcut-card"
            type="button"
            @click="router.push(item.url)"
          >
            <span class="shortcut-icon" :style="{ color: item.color }">
              <Icon :icon="item.icon" :size="24" />
            </span>
            <span class="shortcut-copy">
              <span class="shortcut-name">{{ item.name }}</span>
              <span class="shortcut-description">{{ item.description }}</span>
            </span>
          </button>
        </el-col>
      </el-row>
    </el-card>
  </div>
</template>

<script lang="ts" setup>
import { useRouter } from 'vue-router'
import { useUserStore } from '@/store/modules/user'

defineOptions({ name: 'Index' })

const router = useRouter()
const userStore = useUserStore()
const avatar = computed(() => userStore.getUser.avatar)
const username = computed(() => userStore.getUser.nickname || '管理员')
const todayText = new Intl.DateTimeFormat('zh-CN', {
  month: 'long',
  day: 'numeric',
  weekday: 'short'
}).format(new Date())

const shortcuts = [
  {
    name: 'CRM 待办',
    description: '集中处理提醒',
    icon: 'fa-solid:tasks',
    url: '/crm/backlog',
    color: '#2563eb'
  },
  {
    name: '客户管理',
    description: '查看客户资料',
    icon: 'fa:address-book-o',
    url: '/crm/customer',
    color: '#0891b2'
  },
  {
    name: '商机管理',
    description: '跟进销售机会',
    icon: 'fa:bus',
    url: '/crm/business',
    color: '#7c3aed'
  },
  {
    name: '合同管理',
    description: '查询合同进度',
    icon: 'ep:notebook',
    url: '/crm/contract',
    color: '#c45d16'
  },
  {
    name: '回款管理',
    description: '核对回款记录',
    icon: 'ep:money',
    url: '/crm/receivable',
    color: '#16835b'
  }
]
</script>

<style lang="scss" scoped>
.welcome-card {
  overflow: hidden;
  background:
    radial-gradient(circle at 92% 18%, var(--el-color-primary-light-8), transparent 34%),
    var(--el-bg-color);
}

.welcome-label {
  margin-bottom: 4px;
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 0.08em;
  color: var(--el-color-primary);
}

.welcome-title {
  overflow: hidden;
  font-size: 20px;
  font-weight: 600;
  line-height: 1.35;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.welcome-subtitle,
.section-subtitle,
.shortcut-description {
  color: var(--el-text-color-secondary);
}

.welcome-subtitle {
  margin-top: 6px;
  font-size: 13px;
}

.section-title {
  font-size: 16px;
  font-weight: 600;
}

.section-subtitle {
  margin-top: 3px;
  font-size: 12px;
}

.shortcut-card {
  display: flex;
  width: 100%;
  min-height: 84px;
  padding: 14px;
  color: var(--el-text-color-primary);
  cursor: pointer;
  background: var(--el-bg-color);
  border: 1px solid var(--el-border-color-light);
  border-radius: 10px;
  transition:
    border-color 0.2s,
    box-shadow 0.2s,
    transform 0.2s;
  align-items: center;
  gap: 12px;
}

.shortcut-card:hover {
  border-color: var(--el-color-primary-light-5);
  transform: translateY(-1px);
  box-shadow: 0 8px 24px rgb(30 64 120 / 10%);
}

.shortcut-card:active {
  transform: scale(0.98);
}

.shortcut-card:focus-visible {
  outline: 2px solid var(--el-color-primary);
  outline-offset: 2px;
}

.shortcut-icon {
  display: inline-flex;
  width: 42px;
  height: 42px;
  background: var(--el-fill-color-light);
  border-radius: 9px;
  flex: none;
  align-items: center;
  justify-content: center;
}

.shortcut-copy {
  display: flex;
  min-width: 0;
  text-align: left;
  flex-direction: column;
  align-items: flex-start;
}

.shortcut-name {
  font-size: 14px;
  font-weight: 600;
}

.shortcut-description {
  margin-top: 4px;
  overflow: hidden;
  font-size: 12px;
  text-overflow: ellipsis;
  white-space: nowrap;
}

@media (width <= 767px) {
  .welcome-card :deep(.el-card__body) {
    padding: 18px;
  }

  .welcome-content {
    gap: 12px;
  }

  .welcome-avatar {
    width: 52px !important;
    height: 52px !important;
  }

  .welcome-title {
    font-size: 18px;
  }

  .welcome-subtitle {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .shortcut-section :deep(.el-card__header) {
    padding: 14px 16px;
  }

  .shortcut-section :deep(.el-card__body) {
    padding: 12px;
  }

  .shortcut-card {
    min-height: 104px;
    flex-direction: column;
    align-items: flex-start;
    justify-content: space-between;
    gap: 8px;
    padding: 12px;
  }

  .shortcut-description {
    max-width: 100%;
  }
}
</style>
