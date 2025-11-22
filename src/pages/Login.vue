<script setup lang="ts">
import { RouterLink } from 'vue-router'
import { Phone } from 'lucide-vue-next'
</script>

<template>
  <div class="w-full">
    <div class="text-center mb-8">
      <h1 class="text-2xl font-bold text-text-primary mb-2">欢迎回来</h1>
      <p class="text-text-secondary">输入您的详细信息以访问您的帐户</p>
    </div>

    <form class="space-y-4">
      <div>
        <label class="block text-sm font-medium text-text-secondary mb-1">手机号 / 用户名</label>
        <div class="relative">
          <Phone class="absolute left-3 top-1/2 -translate-y-1/2 w-5 h-5 text-text-placeholder" />
          <input 
            type="text" 
            class="w-full bg-white border border-border-base rounded-lg py-3 pl-10 pr-4 text-text-primary placeholder-text-placeholder focus:outline-none focus:border-primary focus:ring-1 focus:ring-primary transition-colors shadow-sm"
            placeholder="请输入手机号或用户名"
          />
        </div>
      </div>
      <div>
        <div class="flex items-center justify-between mb-1">
          <label class="block text-sm font-medium text-text-secondary">密码</label>
          <RouterLink to="#" class="text-sm text-primary hover:text-primary-hover">忘记密码？</RouterLink>
        </div>
        <input 
          type="password" 
          class="w-full bg-white border border-border-base rounded-lg py-3 px-4 text-text-primary placeholder-text-placeholder focus:outline-none focus:border-primary focus:ring-1 focus:ring-primary transition-colors shadow-sm"
          placeholder="••••••••"
        />
      </div>

      <button class="w-full bg-primary text-white font-bold py-3 rounded-lg hover:bg-primary-hover transition-colors shadow-md">
        登录
      </button>
    </form>

    <p class="mt-8 text-center text-sm text-text-secondary">
      还没有帐户？{' '}
      <RouterLink to="/register" class="text-primary hover:text-primary-hover font-medium">
        注册
      </RouterLink>
    </p>
  </div>
</template>
