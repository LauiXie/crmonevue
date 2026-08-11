<template>
  <div
    :class="prefixCls"
    class="login-shell relative h-[100%] lt-md:px-10px lt-sm:px-10px lt-xl:px-10px lt-xl:px-10px"
  >
    <div class="relative mx-auto h-full flex">
      <div
        :class="`${prefixCls}__left flex-1 bg-gray-500 bg-opacity-20 relative p-30px lt-xl:hidden overflow-x-hidden overflow-y-auto`"
      >
        <!-- 左上角的 logo + 系统标题 -->
        <div class="relative flex items-center text-white">
          <img alt="CRM" class="mr-10px h-48px w-48px" src="@/assets/svgs/crm-logo.svg" />
          <span class="text-20px font-bold">{{ appStore.getTitle }}</span>
        </div>
        <!-- 左边的背景图 + 欢迎语 -->
        <div class="h-[calc(100%-60px)] flex items-center justify-center">
          <TransitionGroup
            appear
            enter-active-class="animate__animated animate__bounceInLeft"
            tag="div"
          >
            <img key="1" alt="" class="w-350px" src="@/assets/svgs/login-box-bg.svg" />
            <div key="2" class="text-3xl text-white">{{ t('login.welcome') }}</div>
            <div key="3" class="mt-5 text-14px font-normal text-white">
              {{ t('login.message') }}
            </div>
          </TransitionGroup>
        </div>
      </div>
      <div
        class="login-main relative flex-1 p-30px dark:bg-[var(--login-bg-color)] lt-sm:p-10px overflow-x-hidden overflow-y-auto"
      >
        <!-- 右上角的主题、语言选择 -->
        <div
          class="login-toolbar flex items-center justify-between at-2xl:justify-end at-xl:justify-end"
          style="color: var(--el-text-color-primary)"
        >
          <div class="login-brand flex items-center at-2xl:hidden at-xl:hidden">
            <img alt="CRM" class="mr-10px h-48px w-48px" src="@/assets/svgs/crm-logo.svg" />
            <span class="text-20px font-bold">{{ appStore.getTitle }}</span>
          </div>
          <div class="flex items-center justify-end space-x-10px h-48px">
            <ThemeSwitch />
            <LocaleDropdown />
          </div>
        </div>
        <!-- 右边的登录界面 -->
        <Transition appear enter-active-class="animate__animated animate__bounceInRight">
          <div
            class="login-panel m-auto h-[calc(100%-60px)] w-[100%] flex items-center at-2xl:max-w-500px at-lg:max-w-500px at-md:max-w-500px at-xl:max-w-500px"
          >
            <!-- 账号登录 -->
            <LoginForm
              class="login-form-card m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)"
            />
            <!-- 手机登录 -->
            <MobileForm class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" />
            <!-- 二维码登录 -->
            <QrCodeForm class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" />
            <!-- 注册 -->
            <RegisterForm class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" />
            <!-- 三方登录 -->
            <SSOLoginVue class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" />
            <!-- 忘记密码 -->
            <ForgetPasswordForm class="m-auto h-auto p-20px lt-xl:(rounded-3xl light:bg-white)" />
          </div>
        </Transition>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
import { useDesign } from '@/hooks/web/useDesign'
import { useAppStore } from '@/store/modules/app'
import { ThemeSwitch } from '@/layout/components/ThemeSwitch'
import { LocaleDropdown } from '@/layout/components/LocaleDropdown'

import {
  LoginForm,
  MobileForm,
  QrCodeForm,
  RegisterForm,
  SSOLoginVue,
  ForgetPasswordForm
} from './components'

defineOptions({ name: 'Login' })

const { t } = useI18n()
const appStore = useAppStore()
const { getPrefixCls } = useDesign()
const prefixCls = getPrefixCls('login')
</script>

<style lang="scss" scoped>
$prefix-cls: #{$namespace}-login;

.#{$prefix-cls} {
  overflow: auto;

  &__left {
    &::before {
      position: absolute;
      top: 0;
      left: 0;
      z-index: -1;
      width: 100%;
      height: 100%;
      background-image: url('@/assets/svgs/login-bg.svg');
      background-position: center;
      background-repeat: no-repeat;
      content: '';
    }
  }
}

@media (width <= 767px) {
  .login-shell {
    min-height: 100dvh;
    padding: env(safe-area-inset-top) 16px env(safe-area-inset-bottom) !important;
    background:
      radial-gradient(circle at 10% 0%, var(--el-color-primary-light-9), transparent 38%), #f7f9fc;
  }

  .login-main {
    padding: 12px 0 20px !important;
    background: transparent;
  }

  .login-toolbar {
    height: 48px;
  }

  .login-brand {
    min-width: 0;

    img {
      width: 40px;
      height: 40px;
    }

    span {
      overflow: hidden;
      font-size: 18px;
      text-overflow: ellipsis;
      white-space: nowrap;
    }
  }

  .login-panel {
    height: auto !important;
    min-height: calc(100dvh - 92px - env(safe-area-inset-top) - env(safe-area-inset-bottom));
    align-items: flex-start !important;
    padding-top: clamp(40px, 8vh, 68px);
  }

  :deep(.login-form-card) {
    width: 100%;
    max-width: 420px;
    padding: 22px 18px !important;
    margin: 0 auto !important;
    border: 1px solid rgb(37 99 235 / 8%);
    border-radius: 18px;
    box-shadow: 0 18px 55px rgb(36 64 114 / 10%);
  }

  :deep(.login-form-card .el-input__wrapper) {
    min-height: 48px;
  }

  :deep(.login-form-card .el-button) {
    min-height: 44px;
  }
}
</style>

<style lang="scss">
.dark .login-form {
  .el-divider__text {
    background-color: var(--login-bg-color);
  }

  .el-card {
    background-color: var(--login-bg-color);
  }
}
</style>
