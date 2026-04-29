<script setup>
import { Link, usePage } from '@inertiajs/vue3';
import { ref, computed } from 'vue';

const props = defineProps({
    show: Boolean
});

const emit = defineEmits(['close']);

const page = usePage();

const isUrl = (...urls) => {
    let currentUrl = page.url.split('?')[0];
    if (urls[0] === '/') {
        return currentUrl === '/';
    }
    return urls.filter(url => currentUrl.startsWith(url)).length > 0;
};

const dropdowns = ref({
    emodul: isUrl('/emodul'),
});

const toggleDropdown = (key) => {
    dropdowns.value[key] = !dropdowns.value[key];
};
</script>

<template>
    <!-- Mobile Overlay -->
    <div
        v-if="show"
        class="fixed inset-0 bg-black/60 z-20 lg:hidden"
        @click="emit('close')"
    />

    <!-- Sidebar -->
    <aside
        :class="[
            'fixed inset-y-0 left-0 z-30 w-64 bg-[#171717] flex flex-col shrink-0 border-r border-white/5 transition-transform duration-300 ease-in-out',
            'lg:relative lg:translate-x-0 lg:w-56 lg:z-auto',
            show ? 'translate-x-0' : '-translate-x-full'
        ]"
    >
        <!-- Brand -->
        <div class="flex items-center justify-between px-5 py-4 border-b border-white/5">
            <span class="font-semibold text-sm tracking-wide text-white/90">Admin Dashboard</span>
            <!-- Close button (mobile only) -->
            <button
                class="lg:hidden p-1 rounded text-white/40 hover:text-white hover:bg-white/10 transition-colors"
                @click="emit('close')"
            >
                <svg class="w-4 h-4" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                    <path d="M18 6L6 18M6 6l12 12"/>
                </svg>
            </button>
        </div>

        <!-- Navigation -->
        <nav class="flex-1 px-3 py-4 space-y-0.5 overflow-y-auto custom-scrollbar">
            <Link href="/" :class="[isUrl('/') ? 'bg-white/10 text-white' : 'text-white/50 hover:text-white hover:bg-white/5', 'flex items-center gap-3 px-3 py-2.5 rounded-md text-sm font-medium transition-colors']">
                <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                    <rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/>
                    <rect x="3" y="14" width="7" height="7" rx="1"/><rect x="14" y="14" width="7" height="7" rx="1"/>
                </svg>
                Dashboard
            </Link>

            <!-- Dropdown Group: E-Modul -->
            <div>
                <button
                    @click="toggleDropdown('emodul')"
                    :class="[
                        isUrl('/emodul') ? 'text-white' : 'text-white/50 hover:text-white hover:bg-white/5',
                        'w-full flex items-center justify-between px-3 py-2.5 rounded-md text-sm font-medium transition-colors group'
                    ]"
                >
                    <div class="flex items-center gap-3">
                        <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                            <path d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253"/>
                        </svg>
                        E-Modul
                    </div>
                    <svg
                        :class="['w-3.5 h-3.5 text-white/20 group-hover:text-white/40 transition-transform duration-200', dropdowns.emodul ? 'rotate-180' : '']"
                        fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24"
                    >
                        <path d="M19 9l-7 7-7-7"/>
                    </svg>
                </button>
                <div v-show="dropdowns.emodul" class="mt-1 ml-4 pl-3 border-l border-white/5 space-y-0.5">
                    <Link href="#" :class="[isUrl('/emodul/materi') ? 'text-white bg-white/5' : 'text-white/40 hover:text-white hover:bg-white/5', 'block px-3 py-2 rounded-md text-sm transition-colors']">
                        Materi Kuliah
                    </Link>
                    <Link href="#" :class="[isUrl('/emodul/tugas') ? 'text-white bg-white/5' : 'text-white/40 hover:text-white hover:bg-white/5', 'block px-3 py-2 rounded-md text-sm transition-colors']">
                        Tugas & Quiz
                    </Link>
                    <Link href="#" :class="[isUrl('/emodul/referensi') ? 'text-white bg-white/5' : 'text-white/40 hover:text-white hover:bg-white/5', 'block px-3 py-2 rounded-md text-sm transition-colors']">
                        Daftar Pustaka
                    </Link>
                </div>
            </div>

            <Link href="/order" :class="[isUrl('/order') ? 'bg-white/10 text-white' : 'text-white/50 hover:text-white hover:bg-white/5', 'flex items-center gap-3 px-3 py-2.5 rounded-md text-sm font-medium transition-colors']">
                <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                    <path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/>
                </svg>
                Orders
            </Link>
            <Link href="#" :class="[isUrl('/products') ? 'bg-white/10 text-white' : 'text-white/50 hover:text-white hover:bg-white/5', 'flex items-center gap-3 px-3 py-2.5 rounded-md text-sm font-medium transition-colors']">
                <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                    <path d="M20 7H4a2 2 0 00-2 2v6a2 2 0 002 2h16a2 2 0 002-2V9a2 2 0 00-2-2z"/><path d="M16 21V5a2 2 0 00-2-2h-4a2 2 0 00-2 2v16"/>
                </svg>
                Products
            </Link>
            <Link href="#" :class="[isUrl('/customers') ? 'bg-white/10 text-white' : 'text-white/50 hover:text-white hover:bg-white/5', 'flex items-center gap-3 px-3 py-2.5 rounded-md text-sm font-medium transition-colors']">
                <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                    <path d="M17 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2"/><circle cx="9" cy="7" r="4"/>
                    <path d="M23 21v-2a4 4 0 00-3-3.87M16 3.13a4 4 0 010 7.75"/>
                </svg>
                Customers
            </Link>
            <Link href="#" :class="[isUrl('/analytics') ? 'bg-white/10 text-white' : 'text-white/50 hover:text-white hover:bg-white/5', 'flex items-center gap-3 px-3 py-2.5 rounded-md text-sm font-medium transition-colors']">
                <svg class="w-4 h-4 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                    <polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/>
                </svg>
                Analytics
            </Link>
        </nav>

        <!-- User Profile -->
        <div class="px-3 py-3 border-t border-white/5">
            <div class="flex items-center gap-3 px-2 py-2 rounded-md hover:bg-white/5 cursor-pointer transition-colors">
                <div class="w-7 h-7 rounded-full bg-gradient-to-br from-indigo-500 to-purple-600 flex items-center justify-center text-xs font-semibold shrink-0">S</div>
                <div class="flex-1 min-w-0">
                    <p class="text-xs font-medium text-white/90 truncate">shadcn</p>
                    <p class="text-xs text-white/40 truncate">m@example.com</p>
                </div>
                <svg class="w-3.5 h-3.5 text-white/30 shrink-0" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
                    <path d="M8 9l4-4 4 4M8 15l4 4 4-4"/>
                </svg>
            </div>
        </div>
    </aside>
</template>

<style scoped>
.custom-scrollbar::-webkit-scrollbar {
    width: 4px;
}
.custom-scrollbar::-webkit-scrollbar-track {
    background: transparent;
}
.custom-scrollbar::-webkit-scrollbar-thumb {
    background: rgba(255, 255, 255, 0.05);
    border-radius: 10px;
}
.custom-scrollbar::-webkit-scrollbar-thumb:hover {
    background: rgba(255, 255, 255, 0.1);
}
</style>
