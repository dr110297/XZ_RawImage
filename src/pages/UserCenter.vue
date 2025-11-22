<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import { User, Heart, Camera, Mail, Link as LinkIcon, Github, Twitter, Star, Plus } from 'lucide-vue-next'
import { cn } from '@/lib/utils'
import { store } from '@/store/favorites'
import GalleryCard from '@/components/ui/GalleryCard.vue'
import GalleryModal from '@/components/ui/GalleryModal.vue'
import { ElMessage } from 'element-plus'

const router = useRouter()
const activeTab = ref('profile')
const selectedFavorite = ref<any>(null)

// 直接使用所有收藏，不需要文件夹筛选
const favorites = computed(() => store.favorites)

const handleUseTemplate = (item: any) => {
  if (item) {
    router.push({ 
      path: '/generate', 
      state: { 
        prompt: item.prompt, 
        referenceImage: item.src,
        autoGenerate: true
      } 
    })
  }
}

const handleEditTemplate = (item: any) => {
  if (item) {
    router.push({ 
      path: '/generate', 
      state: { 
        prompt: item.prompt, 
        referenceImage: item.src,
        autoGenerate: false
      } 
    })
  }
}

const navigateToGallery = () => {
  router.push('/gallery')
}

// 处理取消收藏
const handleUnfavorite = (item: any) => {
  if (item && confirm('确定要取消收藏这张图片吗？')) {
    store.removeFavorite(item.id)
    ElMessage.info('已取消收藏')
    // 如果当前正在查看这个收藏项，关闭弹窗
    if (selectedFavorite.value?.id === item.id) {
      selectedFavorite.value = null
    }
  }
}
</script>

<template>
  <div class="p-4 max-w-7xl mx-auto">
    <!-- Header -->
    <div class="bg-bg-base/50 backdrop-blur-md rounded-base border border-border-base p-8 shadow-lg mb-6 flex flex-col md:flex-row items-start md:items-center justify-between gap-6">
      <div class="flex items-center gap-6">
        <div class="relative group cursor-pointer">
          <div class="w-24 h-24 rounded-full bg-primary/10 border-2 border-primary flex items-center justify-center text-3xl font-bold text-primary overflow-hidden shadow-glow">
            JD
          </div>
          <div class="absolute inset-0 bg-black/40 rounded-full flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity">
            <Camera class="w-8 h-8 text-white" />
          </div>
        </div>
        <div>
          <h1 class="text-2xl font-bold text-text-primary mb-1 drop-shadow-md">John Doe</h1>
          <p class="text-text-secondary flex items-center gap-2">
            <Mail class="w-4 h-4" /> john@example.com
          </p>
        </div>
      </div>
      <div class="flex items-center gap-4 bg-bg-card p-4 rounded-base border border-border-base shadow-inner">
        <div class="text-right">
          <p class="text-xs text-text-secondary uppercase font-semibold">剩余积分</p>
          <p class="text-2xl font-bold text-primary drop-shadow-[0_0_5px_rgba(59,130,246,0.8)]">850</p>
        </div>
        <button class="el-button el-button-primary rounded-base text-sm shadow-glow px-4 py-2">
          充值
        </button>
      </div>
    </div>

    <!-- Tabs -->
    <div class="bg-bg-base/50 backdrop-blur-md rounded-base border border-border-base shadow-lg overflow-hidden min-h-[600px]">
      <div class="flex items-center border-b border-border-base px-6">
        <button
          v-for="tab in [
            { id: 'profile', label: '个人资料', icon: User },
            { id: 'favorites', label: '我的收藏', icon: Star },
          ]"
          :key="tab.id"
          @click="activeTab = tab.id"
          :class="cn(
            'flex items-center gap-2 px-6 py-4 text-sm font-medium transition-colors border-b-2 -mb-px',
            activeTab === tab.id
              ? 'border-primary text-primary bg-primary/10 shadow-[inset_0_-2px_4px_rgba(59,130,246,0.2)]'
              : 'border-transparent text-text-secondary hover:text-primary hover:bg-white/5'
          )"
        >
          <component :is="tab.icon" class="w-4 h-4" />
          {{ tab.label }}
        </button>
      </div>

      <div class="p-8">
        <!-- Profile Tab -->
        <div v-if="activeTab === 'profile'" class="max-w-3xl animate-in fade-in slide-in-from-bottom-4 duration-300">
          <h2 class="text-lg font-bold text-text-primary mb-6 pb-2 border-b border-border-base">基本信息</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-6">
            <div class="space-y-2">
              <label class="text-sm font-medium text-text-secondary">显示名称</label>
              <input type="text" value="John Doe" class="el-input" />
            </div>
            <div class="space-y-2">
              <label class="text-sm font-medium text-text-secondary">职业 / 头衔</label>
              <input type="text" placeholder="请输入" class="el-input" />
            </div>
            <div class="col-span-full space-y-2">
              <label class="text-sm font-medium text-text-secondary">个人简介</label>
              <textarea 
                class="w-full px-3 py-2 bg-bg-overlay border border-border-base rounded-base text-text-primary placeholder-text-placeholder transition-colors focus:outline-none focus:border-primary focus:ring-1 focus:ring-primary/50 hover:border-border-lighter h-32 resize-none"
                placeholder="介绍一下你自己..."
              />
            </div>
          </div>

          <h2 class="text-lg font-bold text-text-primary mb-6 pb-2 border-b border-border-base">社交链接</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-6">
            <div class="space-y-2">
              <label class="text-sm font-medium text-text-secondary flex items-center gap-2">
                <LinkIcon class="w-4 h-4" /> 个人网站
              </label>
              <input type="text" placeholder="https://" class="el-input" />
            </div>
            <div class="space-y-2">
              <label class="text-sm font-medium text-text-secondary flex items-center gap-2">
                <Github class="w-4 h-4" /> GitHub
              </label>
              <input type="text" placeholder="username" class="el-input" />
            </div>
          </div>

          <div class="flex justify-end pt-4">
            <button class="el-button el-button-primary shadow-glow px-6 py-2 rounded-base">
              保存更改
            </button>
          </div>
        </div>

        <!-- Favorites Tab -->
        <div v-if="activeTab === 'favorites'" class="animate-in fade-in slide-in-from-bottom-4 duration-300 h-full">
          <!-- 收藏统计头部 -->
          <div class="flex items-center justify-between mb-6">
            <div class="flex">
              <h2 class="text-xl font-bold text-text-primary">
                我的收藏
              </h2>
              <div style="color:#409EFF">({{ favorites.length }})</div>
            </div>
            <button 
              @click="navigateToGallery"
              class="el-button el-button-primary flex items-center gap-2 px-4 py-2 rounded-base"
            >
              <Plus class="w-4 h-4" />
              浏览更多
            </button>
          </div>

          <!-- 收藏图片区域 - 添加滚动容器 -->
          <div class="favorites-scroll-container">
            <!-- 收藏图片网格 -->
            <div v-if="favorites.length > 0" class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
              <GalleryCard
                v-for="item in favorites"
                :key="item.id"
                :item="item"
                aspect-ratio="aspect-square"
                @click="selectedFavorite = item"
                @unfavorite="handleUnfavorite"
              />
            </div>
            
            <!-- 空状态 -->
            <div v-else class="flex flex-col items-center justify-center py-20 text-text-secondary">
              <div class="w-16 h-16 rounded-full bg-bg-card border border-border-base flex items-center justify-center mb-4">
                <Heart class="w-8 h-8 text-text-secondary/50" />
              </div>
              <p class="mb-4 text-lg">暂无收藏内容</p>
              <p class="text-sm text-text-secondary/70 mb-6">去画廊发现并收藏你喜欢的作品吧</p>
              <button 
                @click="navigateToGallery"
                class="el-button el-button-primary px-6 py-3 rounded-base flex items-center gap-2"
              >
                <Plus class="w-4 h-4" />
                去画廊看看
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Detail Dialog -->
    <GalleryModal
      :isOpen="!!selectedFavorite"
      :item="selectedFavorite"
      @close="selectedFavorite = null"
      @use="handleUseTemplate"
      @edit="handleEditTemplate"
      @unfavorite="handleUnfavorite"
    />
  </div>
</template>

<style scoped>
/* 收藏区域滚动容器样式 */
.favorites-scroll-container {
  max-height: calc(100vh - 300px);
  overflow-y: auto;
  padding-right: 8px;
}

/* 自定义滚动条样式 */
.favorites-scroll-container::-webkit-scrollbar {
  width: 6px;
}

.favorites-scroll-container::-webkit-scrollbar-track {
  background: rgba(0, 0, 0, 0.05);
  border-radius: 3px;
}

.favorites-scroll-container::-webkit-scrollbar-thumb {
  background: rgba(59, 130, 246, 0.3);
  border-radius: 3px;
  transition: background 0.2s ease;
}

.favorites-scroll-container::-webkit-scrollbar-thumb:hover {
  background: rgba(59, 130, 246, 0.5);
}

/* Firefox 滚动条样式 */
.favorites-scroll-container {
  scrollbar-width: thin;
  scrollbar-color: rgba(59, 130, 246, 0.3) rgba(0, 0, 0, 0.05);
}
</style>