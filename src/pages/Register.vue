<script setup lang="ts">
import { RouterLink } from 'vue-router'
import { Phone, ShieldCheck } from 'lucide-vue-next'
</script>

<template>
  <div class="w-full">
    <div class="text-center mb-8">
      <h1 class="text-2xl font-bold text-text-primary mb-2">创建帐户</h1>
      <p class="text-text-secondary">立即开始生成令人惊叹的AI艺术</p>
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
        <label class="block text-sm font-medium text-text-secondary mb-1">密码</label>
        <input 
          type="password" 
          class="w-full bg-white border border-border-base rounded-lg py-3 px-4 text-text-primary placeholder-text-placeholder focus:outline-none focus:border-primary focus:ring-1 focus:ring-primary transition-colors shadow-sm"
          placeholder="创建密码"
        />
      </div>

      <div>
        <label class="block text-sm font-medium text-text-secondary mb-1">确认密码</label>
        <input 
          type="password" 
          class="w-full bg-white border border-border-base rounded-lg py-3 px-4 text-text-primary placeholder-text-placeholder focus:outline-none focus:border-primary focus:ring-1 focus:ring-primary transition-colors shadow-sm"
          placeholder="请再次输入密码"
        />
      </div>

      <div>
        <label class="block text-sm font-medium text-text-secondary mb-1">验证码</label>
        <div class="flex gap-3">
          <div class="relative flex-1">
            <ShieldCheck class="absolute left-3 top-1/2 -translate-y-1/2 w-5 h-5 text-text-placeholder" />
            <input 
              type="text" 
              class="w-full bg-white border border-border-base rounded-lg py-3 pl-10 pr-4 text-text-primary placeholder-text-placeholder focus:outline-none focus:border-primary focus:ring-1 focus:ring-primary transition-colors shadow-sm"
              placeholder="请输入验证码"
            />
          </div>
          <div class="w-32 h-[46px] bg-gray-200 rounded-lg border border-border-base flex items-center justify-center cursor-pointer hover:bg-gray-300 transition-colors overflow-hidden">
            <!-- Placeholder for CAPTCHA image -->
            <div class="text-lg font-bold tracking-widest text-gray-500 italic select-none bg-white w-full h-full flex items-center justify-center">
              A 7 X 9
            </div>
          </div>
        </div>
      </div>

      <button class="w-full bg-primary text-white font-bold py-3 rounded-lg hover:bg-primary-hover transition-colors shadow-md">
        创建帐户
      </button>
    </form>

    <p class="mt-8 text-center text-sm text-text-secondary">
      已有帐户？{' '}
      <RouterLink to="/login" class="text-primary hover:text-primary-hover font-medium">
        登录
      </RouterLink>
    </p>
  </div>
</template>
