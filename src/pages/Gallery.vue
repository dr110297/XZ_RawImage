<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRouter } from 'vue-router'
import { Search, SlidersHorizontal } from 'lucide-vue-next'
import { cn } from '@/lib/utils'
import GalleryCard from '@/components/ui/GalleryCard.vue'
import GalleryModal from '@/components/ui/GalleryModal.vue'

const router = useRouter()
const activeFilter = ref('all')
const selectedItem = ref<any>(null)
const gallerySearch = ref('')

// Mock Data for Gallery
const GALLERY_ITEMS = Array.from({ length: 24 }).map((_, i) => ({
  id: `gallery_${i}`,
  src: [
    "https://public.youware.com/users-website-assets/prod/faf7c4ec-0acf-42c0-b174-678e35ae8c70/93629169d5504c54b49b89961ff3a71d",
    "https://public.youware.com/users-website-assets/prod/faf7c4ec-0acf-42c0-b174-678e35ae8c70/2dd2239d444142d3afc0fb2db153df7b",
    "https://public.youware.com/users-website-assets/prod/faf7c4ec-0acf-42c0-b174-678e35ae8c70/00bdd1645172419eb35df7fa3dd99321",
    "https://public.youware.com/users-website-assets/prod/faf7c4ec-0acf-42c0-b174-678e35ae8c70/c19331b743f442838b6dd7026c17ae0f"
  ][i % 4],
  title: i % 2 === 0 ? "赛博朋克 2077 风格人物" : "梦幻森林场景",
  prompt: i % 2 === 0 ? "Cyberpunk character, neon lights, high detail, 8k" : "Dreamy forest, magical atmosphere, soft lighting, 8k",
  user: {
    name: `User_${i + 100}`,
    avatar: "https://public.youware.com/users-website-assets/prod/faf7c4ec-0acf-42c0-b174-678e35ae8c70/93629169d5504c54b49b89961ff3a71d" // Mock avatar
  },
  likes: Math.floor(Math.random() * 1000),
  runs: Math.floor(Math.random() * 5000),
  height: i % 3 === 0 ? 'aspect-[2/3]' : i % 3 === 1 ? 'aspect-square' : 'aspect-[3/4]',
  type: (i % 4 === 2 ? 'video' : 'image') as 'image' | 'video',
  style: ['Cyberpunk', 'Fantasy', 'Abstract', 'Portrait'][i % 4]
}))

const filteredItems = computed(() => {
  return GALLERY_ITEMS.filter(item => {
    if (gallerySearch.value && !item.title.toLowerCase().includes(gallerySearch.value.toLowerCase())) return false
    return true
  })
})

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
</script>

<template>
  <div class="min-h-screen bg-bg-page pt-20 pb-12">
    <div class="max-w-[1600px] mx-auto px-4 sm:px-6 lg:px-8 flex gap-8">
      
      <!-- Sidebar Filters (Desktop) -->
      <aside class="w-64 hidden lg:block flex-shrink-0 space-y-8 sticky top-24 h-[calc(100vh-6rem)] overflow-y-auto pr-2 scrollbar-thin scrollbar-thumb-border-base">
        <div>
          <h3 class="font-bold text-text-primary mb-4 flex items-center gap-2">
            <SlidersHorizontal class="w-4 h-4" /> 筛选
          </h3>
          <div class="space-y-2">
            <button
              v-for="item in ['全部模型', 'Checkpoint', 'LoRA', 'Embedding', 'VAE']"
              :key="item"
              :class="cn(
                'w-full text-left px-3 py-2 rounded-base text-sm transition-colors',
                activeFilter === item 
                  ? 'bg-primary/10 text-primary font-medium' 
                  : 'text-text-secondary hover:bg-bg-card hover:text-text-primary'
              )"
              @click="activeFilter = item"
            >
              {{ item }}
            </button>
          </div>
        </div>

        <div>
          <h3 class="font-bold text-text-primary mb-4">风格</h3>
          <div class="flex flex-wrap gap-2">
            <button v-for="tag in ['二次元', '写实', '3D', '插画', '风景', '人物', '科幻']" :key="tag" class="px-3 py-1 rounded-full bg-bg-card border border-border-base text-xs text-text-secondary hover:border-primary hover:text-primary transition-colors">
              {{ tag }}
            </button>
          </div>
        </div>

        <div>
          <h3 class="font-bold text-text-primary mb-4">排序</h3>
          <select class="w-full bg-bg-card border border-border-base rounded-base px-3 py-2 text-sm text-text-primary focus:outline-none focus:border-primary">
            <option>热门推荐</option>
            <option>最新发布</option>
            <option>最多喜欢</option>
            <option>最多使用</option>
          </select>
        </div>
      </aside>

      <!-- Main Content -->
      <div class="flex-1">
        <!-- Search & Tags -->
        <div class="mb-8 space-y-4">
          <div class="relative max-w-2xl">
            <Search class="absolute left-4 top-1/2 -translate-y-1/2 w-5 h-5 text-text-secondary" />
            <input 
              type="text" 
              v-model="gallerySearch"
              placeholder="搜索模型、图片、灵感..." 
              class="w-full pl-12 pr-4 py-3 bg-bg-card border border-border-base rounded-full text-text-primary focus:outline-none focus:border-primary focus:ring-1 focus:ring-primary/50 shadow-sm transition-all"
            />
          </div>
          <div class="flex items-center gap-2 overflow-x-auto pb-2 scrollbar-hide">
            <span class="text-sm text-text-secondary whitespace-nowrap">热门搜索:</span>
            <button v-for="tag in ['Cyberpunk', 'Ghibli', 'Portrait', 'Landscape', 'Mecha', 'Watercolor']" :key="tag" class="px-4 py-1.5 rounded-full bg-bg-card/50 border border-border-base text-sm text-text-secondary hover:bg-primary/10 hover:text-primary hover:border-primary/30 transition-all whitespace-nowrap">
              {{ tag }}
            </button>
          </div>
        </div>

        <!-- Masonry Grid -->
        <div class="columns-2 md:columns-3 xl:columns-4 gap-6 space-y-6">
          <GalleryCard
            v-for="item in filteredItems"
            :key="item.id"
            :item="item"
            :aspect-ratio="item.height"
            class="break-inside-avoid"
            @click="selectedItem = item"
          />
        </div>
        
        <!-- Load More -->
        <div class="mt-12 text-center">
          <button class="el-button el-button-default px-8 py-3 rounded-full bg-bg-card border-border-base hover:border-primary hover:text-primary">
            加载更多
          </button>
        </div>
      </div>
    </div>

    <!-- Detail Dialog -->
    <GalleryModal
      :isOpen="!!selectedItem"
      :item="selectedItem"
      @close="selectedItem = null"
      @use="handleUseTemplate"
      @edit="handleEditTemplate"
    />
  </div>
</template>
