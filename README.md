```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>官梯2.0 - 履职与仕途</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.8.49/Tone.js"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        parchment: '#F7F5F0',
                        oxblood: {
                            DEFAULT: '#6E1E24',
                            hover: '#5A181D',
                            active: '#451014',
                            light: '#FAF1F2'
                        },
                        bronze: '#8D683A'
                    },
                    fontFamily: {
                        serif: ['"Noto Serif SC"', '"Songti SC"', 'STSong', 'serif'],
                        sans: ['"PingFang SC"', '"Hiragino Sans GB"', '"Microsoft YaHei"', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #F7F5F0;
            color: #2D251E;
            -webkit-tap-highlight-color: transparent;
            font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", sans-serif;
            user-select: none;
        }
        .main-card {
            background-color: #FFFFFF;
            border: 1px solid #ECE7DE;
            box-shadow: 0 1px 3px rgba(60, 48, 36, 0.04);
        }
        .stat-tile {
            background-color: #FAF8F4;
            border: 1px solid #EFEAE1;
        }
        .btn-pill {
            background-color: #FAF8F4;
            border: 1px solid #E5DFD3;
            color: #4A4036;
            transition: all 0.15s ease;
        }
        .btn-pill:active {
            background-color: #EFE7DA;
            transform: scale(0.98);
        }
        .btn-oxblood {
            background-color: #6E1E24;
            color: #FFFFFF;
            transition: all 0.15s ease;
        }
        .btn-oxblood:hover {
            background-color: #5A181D;
        }
        .btn-oxblood:active {
            background-color: #451014;
            transform: scale(0.99);
        }
        .modal-parchment {
            background-color: #FAF8F4;
            border: 1px solid #DDD5C7;
            box-shadow: 0 20px 40px -8px rgba(45, 35, 25, 0.25);
        }
        ::-webkit-scrollbar {
            width: 4px;
            height: 4px;
        }
        ::-webkit-scrollbar-thumb {
            background: #D8D1C2;
            border-radius: 4px;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between selection:bg-red-100">

    <header class="sticky top-0 z-30 bg-[#F7F5F0]/90 backdrop-blur-md border-b border-[#EBE5DA] px-4 py-2.5">
        <div class="max-w-md mx-auto flex items-center justify-between">
            <div class="flex items-center space-x-1.5">
                <button id="btn-sound" onclick="toggleSound()" class="btn-pill px-2.5 py-1 text-xs rounded-full flex items-center space-x-1 shadow-sm">
                    <span id="sound-icon">🔈</span>
                    <span>音效</span>
                </button>
                <button id="btn-music" onclick="toggleMusic()" class="btn-pill px-2.5 py-1 text-xs rounded-full flex items-center space-x-1 shadow-sm">
                    <span id="music-icon">🎵</span>
                    <span>音乐</span>
                </button>
            </div>
            
            <div class="text-center">
                <h1 class="font-serif text-lg font-bold tracking-wider text-stone-900 leading-none">官梯2.0</h1>
                <span class="text-[9px] text-stone-500 tracking-widest block mt-0.5">履职与仕途</span>
            </div>

            <div class="flex items-center space-x-1.5">
                <button onclick="openPromotionLadderModal()" title="查看升迁天梯" class="btn-pill px-2.5 py-1 text-xs rounded-full shadow-sm text-oxblood font-medium">
                    🪜 官阶
                </button>
                <button onclick="openArchiveModal()" class="btn-pill px-2.5 py-1 text-xs rounded-full flex items-center space-x-1 shadow-sm">
                    <span>💾</span>
                    <span>存档</span>
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-md mx-auto w-full px-4 pt-3.5 pb-36 flex-1">
        
        <div class="main-card rounded-xl p-4 mb-3 relative overflow-hidden">
            <div class="flex space-x-3.5 items-stretch">
                <!-- 左侧全身立姿免冠正装照 -->
                <div class="flex flex-col items-center flex-shrink-0 w-28">
                    <div onclick="openAvatarModal()" class="w-full h-40 bg-[#EDE8E0] rounded-sm overflow-hidden relative shadow-inner flex items-center justify-center cursor-pointer group" title="点击更换干部全身免冠公职正装照">
                        <img id="char-avatar-img" src="" alt="干部履职全身立姿免冠照" class="w-full h-full object-contain">
                        <div class="absolute inset-0 bg-stone-900/30 opacity-0 group-hover:opacity-100 flex items-center justify-center text-white text-[11px] transition">
                            <span>📷 更换照</span>
                        </div>
                    </div>
                    <div id="char-path-pill" class="w-full mt-1.5 bg-[#EDE8DF] text-[10px] text-stone-700 py-0.5 text-center rounded-sm font-semibold tracking-wide truncate px-1">
                        中组部选调 · 实职
                    </div>
                </div>

                <!-- 右侧任职关键档案 -->
                <div class="flex-1 flex flex-col justify-between py-0.5">
                    <div>
                        <div class="flex items-center justify-between text-[11px] text-stone-400 tracking-wider">
                            <span>个人履职档案</span>
                            <span id="char-badge-tag" class="text-[10px] text-oxblood font-serif font-bold bg-oxblood-light px-1.5 py-0.2 rounded">中选 · 北大硕研</span>
                        </div>
                        <div class="flex items-baseline space-x-2 mt-1">
                            <h2 id="char-name" class="font-serif text-2xl font-bold text-stone-900 tracking-tight leading-tight">高育良</h2>
                            <span id="char-age" class="text-xs text-stone-500 font-medium">24岁</span>
                        </div>
                        
                        <div id="char-post" class="mt-1 font-serif text-stone-900 font-bold text-[14px] leading-snug">
                            清河镇党委委员、副镇长
                        </div>
                        
                        <div class="mt-1 text-xs text-stone-500">
                            <span id="char-dept-label">乡镇领导班子</span>
                            <span> · </span>
                            <span id="char-difficulty-label">分管实务</span>
                        </div>

                        <div class="mt-1 text-xs text-stone-500 space-y-0.5">
                            <div>现层次 <span id="stat-level-year">0</span> 年</div>
                            <div>现岗位 <span id="stat-post-year">0</span> 年</div>
                        </div>
                    </div>

                    <div onclick="openFinanceModal()" class="mt-2 flex items-baseline space-x-1.5 cursor-pointer hover:opacity-80 transition" title="点击查阅财产申报与收入细目">
                        <span class="text-xs text-stone-500">可用存款</span>
                        <span id="char-savings" class="font-serif text-xl font-bold text-bronze">12.50</span>
                        <span class="text-xs text-bronze font-medium">万</span>
                    </div>
                </div>
            </div>
        </div>

        <div class="flex items-center justify-between px-1 py-1 mb-1.5 text-xs text-stone-600">
            <div id="current-timeline-str" class="tracking-wide">
                第1年 · 春 · 第1季度
            </div>
            <div onclick="toggleCoreMetricsCollapse()" class="cursor-pointer hover:text-stone-900 flex items-center space-x-1 select-none">
                <span>核心指标</span>
                <span id="core-metrics-arrow" class="text-[10px]">⌃</span>
            </div>
        </div>

        <!-- 核心三大支柱：健康、压力、廉洁 -->
        <div id="core-metrics-panel" class="main-card rounded-xl p-3.5 mb-3 grid grid-cols-3 divide-x divide-[#ECE7DE] text-left transition-all duration-200">
            <div class="px-2">
                <div class="text-xs text-stone-500 mb-0.5">健康</div>
                <div class="flex items-baseline space-x-0.5">
                    <span id="stat-health" class="font-serif text-2xl font-bold text-stone-900">92</span>
                    <span class="text-[10px] text-stone-400">/100</span>
                </div>
                <div class="w-full bg-[#E8E2D6] h-1 rounded-full mt-2 overflow-hidden">
                    <div id="bar-health" class="bg-stone-700 h-full transition-all duration-300" style="width: 92%"></div>
                </div>
            </div>

            <div class="px-2">
                <div class="text-xs text-stone-500 mb-0.5">压力</div>
                <div class="flex items-baseline space-x-0.5">
                    <span id="stat-stress" class="font-serif text-2xl font-bold text-stone-900">18</span>
                </div>
                <div class="text-[10px] text-stone-500 mt-2 truncate" id="stress-desc">目前可承受</div>
            </div>

            <div onclick="openIntegrityModal()" class="px-2 cursor-pointer hover:bg-stone-50 transition" title="点击查看廉洁自律与巡视风险">
                <div class="text-xs text-stone-500 mb-0.5">廉洁</div>
                <div class="flex items-baseline space-x-0.5">
                    <span id="stat-integrity" class="font-serif text-2xl font-bold text-stone-900">100</span>
                    <span class="text-[10px] text-stone-500">%</span>
                </div>
                <div class="w-full bg-[#E8E2D6] h-1 rounded-full mt-2 overflow-hidden">
                    <div id="bar-integrity" class="bg-stone-700 h-full transition-all duration-300" style="width: 100%"></div>
                </div>
            </div>
        </div>

        <div class="grid grid-cols-3 gap-2.5 mb-3">
            <div onclick="openMetricDetailModal('merit')" class="stat-tile rounded-xl p-3 cursor-pointer hover:border-stone-400 transition">
                <div class="text-xs text-stone-500">实干政绩</div>
                <span id="stat-merit" class="font-serif text-2xl font-bold text-stone-900 mt-1 block">28</span>
            </div>

            <div onclick="openMetricDetailModal('reputation')" class="stat-tile rounded-xl p-3 cursor-pointer hover:border-stone-400 transition">
                <div class="text-xs text-stone-500">民主口碑</div>
                <span id="stat-reputation" class="font-serif text-2xl font-bold text-stone-900 mt-1 block">32</span>
            </div>

            <div onclick="openConnectionsModal()" class="stat-tile rounded-xl p-3 cursor-pointer hover:border-stone-400 transition">
                <div class="text-xs text-stone-500">组织背书</div>
                <span id="stat-connections" class="font-serif text-2xl font-bold text-stone-900 mt-1 block">15</span>
            </div>

            <div onclick="openMetricDetailModal('eval')" class="stat-tile rounded-xl p-3 cursor-pointer hover:border-stone-400 transition">
                <div class="text-xs text-stone-500">本年考核</div>
                <span id="stat-eval" class="font-serif text-2xl font-bold text-stone-900 mt-1 block">22</span>
            </div>

            <div onclick="openMetricDetailModal('demerits')" class="stat-tile rounded-xl p-3 cursor-pointer hover:border-stone-400 transition">
                <div class="text-xs text-stone-500">处分过失</div>
                <span id="stat-demerits" class="font-serif text-2xl font-bold text-stone-900 mt-1 block">0</span>
            </div>

            <div onclick="openActionsModal()" class="stat-tile rounded-xl p-3 cursor-pointer hover:border-oxblood transition border-oxblood/20">
                <div class="text-xs text-oxblood font-medium">季度精力</div>
                <div class="flex items-baseline mt-1">
                    <span id="stat-actions" class="font-serif text-2xl font-bold text-oxblood">4</span>
                    <span class="text-xs text-stone-400">/4</span>
                </div>
            </div>
        </div>

        <div onclick="openDossierModal()" class="py-2.5 px-3 mb-3 bg-white border border-[#ECE7DE] rounded-xl flex items-center justify-between text-xs text-stone-700 cursor-pointer hover:border-stone-400 transition shadow-sm">
            <span class="flex items-center space-x-1.5">
                <span class="text-oxblood font-serif">📋</span>
                <span class="font-medium">查阅《干部任免审批表》与立功卷宗</span>
                <span id="dossier-pill-indicator" class="hidden text-[10px] bg-amber-100 text-amber-900 border border-amber-300 px-1.5 py-0.2 rounded font-medium">
                    有重大功勋
                </span>
            </span>
            <span class="text-stone-400 font-mono">&gt;</span>
        </div>

        <div class="main-card rounded-xl p-3.5 text-xs text-stone-600 space-y-1.5">
            <div class="text-[11px] text-stone-400 tracking-wider flex justify-between items-center border-b border-stone-100 pb-1.5">
                <span>近期督办批示与履职纪要</span>
                <button onclick="openHistoryTimelineModal()" class="text-stone-500 hover:text-oxblood font-medium">完整卷宗 &gt;</button>
            </div>
            <div id="recent-logs-container" class="space-y-1.5 pt-0.5">
                <p class="text-stone-400 text-[11px]">按部就班履行领导岗位职责，抓牢主责主业。</p>
            </div>
        </div>

    </main>

    <div class="fixed bottom-0 left-0 right-0 z-20 bg-[#F7F5F0]/90 backdrop-blur-md border-t border-[#E5DEC9] pt-2 pb-4 px-4 shadow-[0_-4px_16px_rgba(45,35,25,0.05)]">
        <div class="max-w-md mx-auto">
            <div class="text-center text-[10px] text-stone-400 mb-1.5 tracking-wider select-none flex items-center justify-center space-x-1">
                <span class="inline-block w-1.5 h-1.5 rounded-full bg-emerald-600/70"></span>
                <span>已完成的操作即刻自动归档</span>
            </div>
            <button id="btn-next-quarter" onclick="nextQuarter()" class="w-full py-3 rounded-lg btn-oxblood font-serif tracking-wider text-base font-semibold shadow-md flex items-center justify-center space-x-1.5 transition">
                <span id="btn-next-quarter-text">推进至下一季度</span>
                <span class="text-sm font-sans">&gt;</span>
            </button>
        </div>
    </div>

    <div id="modal-onboarding" class="fixed inset-0 z-50 bg-stone-900/60 backdrop-blur-md hidden flex items-center justify-center p-3">
        <div class="modal-parchment max-w-md w-full max-h-[92vh] rounded-xl flex flex-col shadow-2xl overflow-hidden border border-[#D5CCBA]">
            <div class="bg-oxblood text-white px-4 py-3 text-center">
                <div class="text-[10px] tracking-widest text-red-200">中共中央组织部直管年轻干部选拔任用调配系统</div>
                <h3 class="font-serif font-bold text-base tracking-wider mt-0.5">新录用战略后备干部自主建档与履新核准</h3>
            </div>

            <div class="p-4 overflow-y-auto space-y-3.5 text-xs text-stone-700">
                <div class="bg-white p-3 rounded-lg border border-[#DDD5C7] space-y-2.5">
                    <div class="font-bold text-stone-900 border-b border-stone-100 pb-1 flex justify-between">
                        <span>一、个人基础身份信息</span>
                        <span class="text-[10px] text-stone-400 font-normal">支持完全自定义</span>
                    </div>
                    <div class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="block text-[11px] text-stone-500 mb-0.5">干部姓名</label>
                            <input id="setup-name" type="text" value="高育良" class="w-full p-2 bg-[#FAF8F4] border border-[#DDD5C7] rounded text-stone-900 text-xs focus:outline-none focus:border-oxblood">
                        </div>
                        <div>
                            <label class="block text-[11px] text-stone-500 mb-0.5">到任年龄</label>
                            <input id="setup-age" type="number" value="24" class="w-full p-2 bg-[#FAF8F4] border border-[#DDD5C7] rounded text-stone-900 text-xs focus:outline-none focus:border-oxblood">
                        </div>
                    </div>
                    <div>
                        <label class="block text-[11px] text-stone-500 mb-0.5">招录学历背景与培养渠道</label>
                        <select id="setup-edu" class="w-full p-2 bg-[#FAF8F4] border border-[#DDD5C7] rounded text-stone-900 text-xs focus:outline-none focus:border-oxblood">
                            <option value="北大硕研">北京大学 硕士研究生（中组部专项中央选调 · 实职直定）</option>
                            <option value="清华博研">清华大学 博士研究生（战略储备年轻干部选任 · 重点跟踪）</option>
                            <option value="政法硕研">中国政法大学 硕士研究生（全日制公法专业 · 专项定向选调）</option>
                        </select>
                    </div>
                </div>

                <div class="bg-white p-3 rounded-lg border border-[#DDD5C7] space-y-2.5">
                    <div class="font-bold text-stone-900 border-b border-stone-100 pb-1 flex justify-between">
                        <span>二、任职行政区划自定义</span>
                        <span class="text-[10px] text-stone-400 font-normal">全系统公文实时联动</span>
                    </div>
                    <div class="grid grid-cols-2 gap-2">
                        <div>
                            <label class="block text-[11px] text-stone-500 mb-0.5">所属省份名称</label>
                            <input id="setup-province" type="text" value="江东省" class="w-full p-2 bg-[#FAF8F4] border border-[#DDD5C7] rounded text-stone-900 text-xs focus:outline-none focus:border-oxblood">
                        </div>
                        <div>
                            <label class="block text-[11px] text-stone-500 mb-0.5">所属地级市名称</label>
                            <input id="setup-city" type="text" value="海州市" class="w-full p-2 bg-[#FAF8F4] border border-[#DDD5C7] rounded text-stone-900 text-xs focus:outline-none focus:border-oxblood">
                        </div>
                        <div>
                            <label class="block text-[11px] text-stone-500 mb-0.5">所属县/区名称</label>
                            <input id="setup-county" type="text" value="清河县" class="w-full p-2 bg-[#FAF8F4] border border-[#DDD5C7] rounded text-stone-900 text-xs focus:outline-none focus:border-oxblood">
                        </div>
                        <div>
                            <label class="block text-[11px] text-stone-500 mb-0.5">首发任职乡镇名称</label>
                            <input id="setup-town" type="text" value="清河镇" class="w-full p-2 bg-[#FAF8F4] border border-[#DDD5C7] rounded text-stone-900 text-xs focus:outline-none focus:border-oxblood">
                        </div>
                    </div>
                </div>

                <div class="bg-white p-3 rounded-lg border border-[#DDD5C7] space-y-2">
                    <div class="font-bold text-stone-900 border-b border-stone-100 pb-1">
                        三、公职免冠正装立姿肖像
                    </div>
                    <div id="setup-avatar-preview-box" class="grid grid-cols-4 gap-2">
                    </div>
                </div>
            </div>

            <div class="bg-[#FAF8F4] border-t border-[#E5DEC9] px-4 py-3 flex justify-end">
                <button onclick="confirmOnboarding()" class="btn-oxblood px-6 py-2 rounded text-xs font-serif font-bold shadow-md">
                    核准档案并正式就任
                </button>
            </div>
        </div>
    </div>

    <div id="modal-event" class="fixed inset-0 z-50 bg-stone-900/60 backdrop-blur-md hidden f
