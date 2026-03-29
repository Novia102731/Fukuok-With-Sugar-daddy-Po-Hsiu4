<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>姆逆爸爸 ＆ 妳 - 福岡惡搞旅</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --wakaba-green: #E9FCE5; /* 若葉綠 (背景) */
            --violet-hata: #B794F4; /* 哈達紫 (主色) */
            --violet-dark: #7B61FF;
        }
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: var(--wakaba-green); color: #333; overflow-x: hidden; }
        .glass-card { background: rgba(255, 255, 255, 0.9); backdrop-filter: blur(5px); border: 1px solid rgba(183, 148, 244, 0.1); }
        .tab-bar { box-shadow: 0 -5px 15px rgba(123, 97, 255, 0.1); border-top-left-radius: 30px; border-top-right-radius: 30px; }
        .date-scroll::-webkit-scrollbar { display: none; }
        /* 圖片銜接圓弧 */
        .header-curve { border-bottom-left-radius: 40px; border-bottom-right-radius: 40px; }
        .content-up { margin-top: -40px; }
    </style>
</head>
<body>
    <div id="app" class="max-w-md mx-auto min-h-screen pb-28 relative">
        
        <header class="relative h-[250px] w-full overflow-hidden header-curve shadow-lg z-10">
            <img src="0822" alt="Banner" class="w-full h-full object-cover">
            <div class="absolute inset-0 bg-gradient-to-b from-transparent to-purple-900/10"></div>
            <div class="absolute top-10 left-6 text-white drop-shadow-md">
                <span class="text-xs font-bold bg-[#B794F4] text-white px-2 py-0.5 rounded-full">Fukuoka</span>
                <h1 class="text-2xl font-black tracking-widest mt-1">哈達王子惡搞福岡之旅</h1>
                <p class="text-xs opacity-90">姆逆爸爸 & 妳 💖</p>
            </div>
        </header>

        <main class="content-up px-4 relative z-20">
            
            <div v-if="activeTab === 'itinerary'">
                <div class="glass-card rounded-2xl p-4 mb-4 flex items-center justify-between shadow-sm">
                    <div class="flex items-center">
                        <i class="fas fa-cloud-sun text-yellow-500 text-3xl mr-3"></i>
                        <div>
                            <p class="text-xs text-gray-400">福岡市 天氣 (預設)</p>
                            <p class="text-lg font-bold">18°C / 12°C <span class="text-sm font-normal text-gray-500">體感 17°C</span></p>
                        </div>
                    </div>
                </div>

                <div class="flex overflow-x-auto date-scroll space-x-3 mb-5 pb-1">
                    <button v-for="d in dates" :key="d.id" @click="selectedDate = d.id"
                            :class="['flex-shrink-0 flex flex-col items-center justify-center w-12 h-16 rounded-xl transition', 
                            selectedDate === d.id ? 'bg-[#7B61FF] text-white shadow-lg' : 'bg-white text-gray-400']">
                        <span class="text-[11px] uppercase">{{ d.day }}</span>
                        <span class="text-lg font-bold">{{ d.num }}</span>
                    </button>
                </div>

                <div class="space-y-3">
                    <div v-for="(item, index) in currentItinerary" :key="index" class="relative">
                        <div v-if="index > 0" class="flex items-center justify-center py-1">
                            <div class="bg-white/50 px-3 py-1 rounded-full text-[10px] text-gray-400 border border-gray-100 flex items-center">
                                <i :class="getTransportIcon(item.transport)"></i>
                                <span class="ml-1.5">{{ item.travelTime }} mins</span>
                            </div>
                        </div>

                        <div v-if="item.type === '航班'" class="bg-gray-800 rounded-2xl p-4 text-white shadow-md border-l-4 border-blue-400">
                             航班資訊在此展示
                        </div>

                        <div v-else class="glass-card rounded-2xl p-4 shadow-sm hover:shadow-md transition">
                            <div class="flex justify-between items-start mb-2">
                                <div class="flex items-center">
                                    <span class="p-2 bg-[#B794F4]/20 rounded-xl text-[#7B61FF] mr-3">
                                        <i :class="getIcon(item.type)"></i>
                                    </span>
                                    <div>
                                        <h3 class="font-bold text-gray-800">{{ item.name }}</h3>
                                        <p class="text-[10px] text-gray-400">{{ item.time }} | {{ item.type }}</p>
                                    </div>
                                </div>
                                <a :href="`googleusercontent.com/maps.google.com/4${encodeURIComponent(item.name)}`" target="_blank" class="p-2.5 bg-[#B794F4] text-white rounded-full shadow-lg">
                                    <i class="fas fa-location-arrow"></i>
                                </a>
                            </div>
                            <div class="text-[11px] text-gray-600 grid grid-cols-2 gap-x-2 gap-y-1 mb-2">
                                <p v-if="item.openTime">營業：{{ item.openTime }}</p>
                                <p v-if="item.price">門票：{{ item.price }}</p>
                            </div>
                            <div class="text-[11px] bg-gray-50 p-2 rounded-lg text-gray-500 border border-dashed border-gray-100">
                                ⭐ {{ item.note }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div v-if="activeTab === 'info'">
                <p>資訊頁面在此</p>
            </div>

            <div v-if="activeTab === 'shop'">
                <p>購物清單在此</p>
            </div>

            <div v-if="activeTab === 'wallet'">
                <p>花費記錄在此</p>
            </div>

        </main>

        <button v-if="activeTab !== 'info'" class="fixed bottom-28 right-6 w-14 h-14 bg-[#B794F4] text-white rounded-full shadow-2xl flex items-center justify-center active:scale-90 z-50">
            <i class="fas fa-plus text-2xl"></i>
        </button>
        
        <button v-if="activeTab === 'shop'" class="fixed bottom-48 right-6 w-12 h-12 bg-white text-[#B794F4] rounded-full shadow-lg flex items-center justify-center border-2 border-[#B794F4] z-50">
            <i class="fas fa-camera text-xl"></i>
        </button>

        <nav class="fixed bottom-0 left-0 right-0 max-w-md mx-auto h-20 bg-white glass-card tab-bar flex justify-around items-center px-4 z-50">
            <button @click="activeTab = 'itinerary'" :class="['flex flex-col items-center', activeTab === 'itinerary' ? 'text-[#7B61FF]' : 'text-gray-300']">
                <i class="fas fa-map-pin text-xl"></i><span class="text-[10px] font-bold mt-1">行程</span>
            </button>
            <button @click="activeTab = 'info'" :class="['flex flex-col items-center', activeTab === 'info' ? 'text-[#7B61FF]' : 'text-gray-300']">
                <i class="fas fa-info-circle text-xl"></i><span class="text-[10px] font-bold mt-1">資訊</span>
            </button>
            <button @click="activeTab = 'shop'" :class="['flex flex-col items-center', activeTab === 'shop' ? 'text-[#7B61FF]' : 'text-gray-300']">
                <i class="fas fa-shopping-basket text-xl"></i><span class="text-[10px] font-bold mt-1">清單</span>
            </button>
            <button @click="activeTab = 'wallet'" :class="['flex flex-col items-center', activeTab === 'wallet' ? 'text-[#7B61FF]' : 'text-gray-300']">
                <i class="fas fa-banknote text-xl"></i><span class="text-[10px] font-bold mt-1">花費</span>
            </button>
        </nav>
    </div>

    <script>
        const { createApp, ref, computed } = Vue;

        createApp({
            setup() {
                const activeTab = ref('itinerary');
                const selectedDate = ref('0426');

                const dates = [
                    { id: '0426', day: 'Sun', num: '26' },
                    { id: '0427', day: 'Mon', num: '27' },
                    { id: '0428', day: 'Tue', num: '28' },
                    { id: '0429', day: 'Wed', num: '29' },
                    { id: '0430', day: 'Thu', num: '30' },
                ];

                const itineraryData = {
                    '0426': [
                        { type: '航班', time: '11:30' }, // 特殊卡片
                        { type: '住宿', name: 'EN HOTEL Hakata', time: '16:00', note: '放行李先！', transport: 'walk', travelTime: 10 },
                        { type: '美食', name: '博多一蘭拉麵', time: '18:30', openTime: '24h', price: '¥1100', note: '福岡必吃，姆逆爸爸指定。', transport: 'walk', travelTime: 5 }
                    ],
                    // 其他日期的預設資料...
                };

                const currentItinerary = computed(() => itineraryData[selectedDate.ref] || []);

                const getIcon = (type) => {
                    const map = { '住宿': 'home', '美食': 'utensils', '景點': 'camera', '購物': 'shopping-bag', '其他': 'map-pin' };
                    return `fas fa-${map[type] || 'map-pin'}`;
                };

                const getTransportIcon = (type) => {
                    const map = { 'walk': 'walking', 'car': 'car', 'bus': 'bus' };
                    return `fas fa-${map[type] || 'walking'} text-[#B794F4]`;
                };

                return { activeTab, dates, selectedDate, currentItinerary, getIcon, getTransportIcon };
            }
        }).mount('#app');
    </script>
</body>
</html>
