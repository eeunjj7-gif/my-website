
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>iPhone 17 Simulator</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css');
        
        * {
            font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, Roboto, sans-serif;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }

        /* Custom Scrollbars */
        ::-webkit-scrollbar {
            width: 4px;
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.2);
            border-radius: 4px;
        }

        /* KakaoTalk Bubble Styles */
        .chat-bubble-yellow {
            background-color: #FEE500;
            color: #000000;
        }
        .chat-bubble-gray {
            background-color: #333333;
            color: #FFFFFF;
        }

        /* Keypad ripple animation */
        .key-active {
            background-color: rgba(255, 255, 255, 0.4) !important;
        }

        /* Screen transition */
        .screen-view {
            transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-10px); }
            40%, 80% { transform: translateX(10px); }
        }
        .animate-shake {
            animation: shake 0.4s ease-in-out;
        }
    </style>
</head>
<body class="bg-slate-950 min-h-screen flex items-center justify-center p-0 md:p-4 overflow-hidden select-none">

    <!-- Phone Bezel Box -->
    <div class="relative w-full max-w-[420px] h-[100vh] md:h-[860px] bg-black md:rounded-[55px] shadow-[0_0_60px_rgba(0,0,0,0.9)] border-[0px] md:border-[12px] border-[#2c2d30] flex flex-col overflow-hidden">
        
        <!-- Physical Buttons Visual Simulation (Desktop) -->
        <div class="hidden md:block absolute -left-[15px] top-[140px] w-[3px] h-[35px] bg-[#3a3b3e] rounded-l-md"></div>
        <div class="hidden md:block absolute -left-[15px] top-[190px] w-[3px] h-[60px] bg-[#3a3b3e] rounded-l-md"></div>
        <div class="hidden md:block absolute -left-[15px] top-[260px] w-[3px] h-[60px] bg-[#3a3b3e] rounded-l-md"></div>
        <div class="hidden md:block absolute -right-[15px] top-[200px] w-[3px] h-[85px] bg-[#3a3b3e] rounded-r-md"></div>

        <!-- Top Status Bar & Dynamic Island -->
        <div class="relative z-50 pt-3 px-7 pb-1 flex justify-between items-center text-white text-xs font-semibold select-none">
            <!-- Clock -->
            <span id="status-clock" class="w-12 text-center text-[13px] font-medium tracking-tight">07:20</span>

            <!-- Dynamic Island -->
            <div id="dynamic-island" class="bg-black border border-white/10 rounded-full h-[28px] w-[110px] flex items-center justify-between px-2 transition-all duration-300 shadow-lg cursor-pointer hover:scale-105" onclick="openContentEditor()">
                <div class="w-2.5 h-2.5 bg-neutral-800 rounded-full flex items-center justify-center">
                    <div class="w-1 h-1 bg-blue-900/60 rounded-full"></div>
                </div>
                <div id="dynamic-island-content" class="text-[10px] text-gray-400 font-normal truncate px-1 hidden">
                    통화 중...
                </div>
                <div class="flex items-center space-x-1">
                    <div id="island-wave" class="hidden flex items-end space-x-0.5 h-3">
                        <span class="w-0.5 bg-green-500 h-2 animate-bounce"></span>
                        <span class="w-0.5 bg-green-500 h-3 animate-bounce [animation-delay:0.2s]"></span>
                        <span class="w-0.5 bg-green-500 h-1 animate-bounce [animation-delay:0.4s]"></span>
                    </div>
                    <div class="w-2.5 h-2.5 bg-neutral-900 rounded-full border border-neutral-700/50"></div>
                </div>
            </div>

            <!-- Icons (Signal, Wifi, Battery) -->
            <div class="flex items-center space-x-1.5 text-[11px]">
                <i class="fa-solid fa-signal"></i>
                <i class="fa-solid fa-wifi"></i>
                <div class="flex items-center border border-white/80 rounded-[4px] px-0.5 py-[1px] w-[22px] h-[11px] relative">
                    <div class="bg-white h-full w-[85%] rounded-[1px]"></div>
                </div>
            </div>
        </div>

        <!-- Main Screen Container -->
        <div class="relative flex-1 w-full h-full bg-cover bg-center overflow-hidden flex flex-col justify-between" 
             style="background-image: url('https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?q=80&w=1000&auto=format&fit=crop');">
            
            <!-- Dynamic Screen Views -->
            
            <!-- 1. LOCK SCREEN (MAIN TIME & NOTIFICATIONS) -->
            <div id="view-lockscreen" class="screen-view absolute inset-0 bg-cover bg-center flex flex-col justify-between p-6 pt-4 text-white z-40"
                 style="background-image: url('https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?q=80&w=1000&auto=format&fit=crop');">
                <div class="flex flex-col items-center mt-6 text-center">
                    <i class="fa-solid fa-lock text-sm mb-1 opacity-80" id="lock-icon"></i>
                    <div id="lock-date" class="text-xs font-semibold tracking-wider uppercase opacity-90">10월 23일 금요일</div>
                    <div id="lock-time" class="text-7xl font-extralight tracking-tighter my-1">07:20</div>
                </div>

                <!-- Notifications Center Preview -->
                <div class="my-auto space-y-2">
                    <div onclick="showPasscodeScreen()" class="bg-black/40 backdrop-blur-xl border border-white/10 rounded-2xl p-3.5 shadow-lg cursor-pointer transform active:scale-95 transition">
                        <div class="flex items-center justify-between mb-1">
                            <div class="flex items-center space-x-2">
                                <div class="w-5 h-5 bg-[#FEE500] rounded-md flex items-center justify-center text-black text-[10px]">
                                    <i class="fa-solid fa-comment"></i>
                                </div>
                                <span class="text-xs font-bold text-gray-200">카카오톡</span>
                            </div>
                            <span class="text-[10px] text-gray-400">방금 전</span>
                        </div>
                        <div class="text-xs font-semibold text-white">최 요원</div>
                        <div class="text-xs text-gray-300 truncate">오늘 영화 볼까? 너 보고 싶다던 영화 있다고...</div>
                    </div>
                </div>

                <!-- Swipe Up Prompt & Shortcuts -->
                <div class="flex flex-col items-center pb-4 space-y-6">
                    <button onclick="showPasscodeScreen()" class="text-xs font-light tracking-wide text-white/80 animate-pulse flex items-center space-x-1">
                        <span>위로 쓸어올려서 잠금 해제</span>
                    </button>
                    <div class="w-full flex justify-between items-center px-4">
                        <div class="w-11 h-11 bg-black/40 backdrop-blur-xl rounded-full flex items-center justify-center text-lg text-white/90 border border-white/10">
                            <i class="fa-solid fa-flashlight text-sm"></i>
                        </div>
                        <button onclick="openContentEditor()" class="px-3 py-1 bg-white/20 backdrop-blur-md rounded-full text-[10px] text-white/80 border border-white/20">
                            <i class="fa-solid fa-gear mr-1"></i>데이터 수정
                        </button>
                        <div class="w-11 h-11 bg-black/40 backdrop-blur-xl rounded-full flex items-center justify-center text-lg text-white/90 border border-white/10">
                            <i class="fa-solid fa-camera text-sm"></i>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 2. PASSCODE ENTRY SCREEN (3020) -->
            <div id="view-passcode" class="screen-view hidden absolute inset-0 bg-black/80 backdrop-blur-2xl flex flex-col justify-between p-8 text-white z-45">
                <div class="flex flex-col items-center mt-12 space-y-4">
                    <i class="fa-solid fa-lock text-xl"></i>
                    <div class="text-sm font-normal tracking-tight">암호 입력</div>
                    <!-- 4 Passcode Dots -->
                    <div id="passcode-dots" class="flex space-x-4 my-2">
                        <div class="w-3.5 h-3.5 border-2 border-white rounded-full"></div>
                        <div class="w-3.5 h-3.5 border-2 border-white rounded-full"></div>
                        <div class="w-3.5 h-3.5 border-2 border-white rounded-full"></div>
                        <div class="w-3.5 h-3.5 border-2 border-white rounded-full"></div>
                    </div>
                </div>

                <!-- Keypad 0-9 -->
                <div class="mb-8">
                    <div class="grid grid-cols-3 gap-5 text-center max-w-[280px] mx-auto">
                        <button onclick="pressPasscode('1')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">1</button>
                        <button onclick="pressPasscode('2')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">2</button>
                        <button onclick="pressPasscode('3')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">3</button>
                        <button onclick="pressPasscode('4')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">4</button>
                        <button onclick="pressPasscode('5')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">5</button>
                        <button onclick="pressPasscode('6')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">6</button>
                        <button onclick="pressPasscode('7')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">7</button>
                        <button onclick="pressPasscode('8')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">8</button>
                        <button onclick="pressPasscode('9')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">9</button>
                        <div></div>
                        <button onclick="pressPasscode('0')" class="w-16 h-16 mx-auto bg-white/10 rounded-full flex items-center justify-center text-2xl font-light active:bg-white/30 transition">0</button>
                        <button onclick="deletePasscode()" class="text-xs font-medium flex items-center justify-center opacity-80 hover:opacity-100">취소</button>
                    </div>
                </div>
            </div>

            <!-- 3. HOME SCREEN -->
            <div id="view-home" class="screen-view hidden flex flex-col justify-between h-full p-6 pt-4">
                <!-- Clock Widget -->
                <div class="flex flex-col items-center mt-6 text-white text-center">
                    <div id="home-widget-date" class="text-xs font-semibold tracking-wider uppercase opacity-80">10월 23일 금요일</div>
                    <div id="home-widget-time" class="text-6xl font-light tracking-tighter my-1">07:20</div>
                </div>

                <!-- Apps Grid -->
                <div class="grid grid-cols-4 gap-y-6 gap-x-4 mb-12 px-1">
                    <!-- KakaoTalk -->
                    <button onclick="openApp('kakaotalk')" class="flex flex-col items-center group">
                        <div class="w-14 h-14 bg-[#FEE500] rounded-2xl flex items-center justify-center text-black shadow-lg group-active:scale-90 transition transform">
                            <i class="fa-solid fa-comment text-2xl"></i>
                        </div>
                        <span class="text-[11px] text-white mt-1.5 font-medium shadow-black drop-shadow">카카오톡</span>
                    </button>

                    <!-- Phone -->
                    <button onclick="openApp('phone')" class="flex flex-col items-center group">
                        <div class="w-14 h-14 bg-green-500 rounded-2xl flex items-center justify-center text-white shadow-lg group-active:scale-90 transition transform">
                            <i class="fa-solid fa-phone text-2xl"></i>
                        </div>
                        <span class="text-[11px] text-white mt-1.5 font-medium shadow-black drop-shadow">전화</span>
                    </button>

                    <!-- Messages -->
                    <button onclick="openApp('messages')" class="flex flex-col items-center group">
                        <div class="w-14 h-14 bg-emerald-500 rounded-2xl flex items-center justify-center text-white shadow-lg group-active:scale-90 transition transform">
                            <i class="fa-solid fa-message text-2xl"></i>
                        </div>
                        <span class="text-[11px] text-white mt-1.5 font-medium shadow-black drop-shadow">메시지</span>
                    </button>

                    <!-- GarageBand -->
                    <button onclick="openApp('garageband')" class="flex flex-col items-center group">
                        <div class="w-14 h-14 bg-gradient-to-tr from-red-600 via-orange-500 to-yellow-400 rounded-2xl flex items-center justify-center text-white shadow-lg group-active:scale-90 transition transform">
                            <i class="fa-solid fa-guitar text-2xl"></i>
                        </div>
                        <span class="text-[11px] text-white mt-1.5 font-medium shadow-black drop-shadow">GarageBand</span>
                    </button>

                    <!-- Weverse -->
                    <button onclick="openApp('weverse')" class="flex flex-col items-center group">
                        <div class="w-14 h-14 bg-teal-400 rounded-2xl flex items-center justify-center text-white shadow-lg group-active:scale-90 transition transform">
                            <i class="fa-solid fa-heart text-2xl"></i>
                        </div>
                        <span class="text-[11px] text-white mt-1.5 font-medium shadow-black drop-shadow">위버스</span>
                    </button>

                    <!-- Notes -->
                    <button onclick="openApp('notes')" class="flex flex-col items-center group">
                        <div class="w-14 h-14 bg-yellow-400 rounded-2xl flex items-center justify-center text-neutral-800 shadow-lg group-active:scale-90 transition transform">
                            <i class="fa-regular fa-note-sticky text-2xl"></i>
                        </div>
                        <span class="text-[11px] text-white mt-1.5 font-medium shadow-black drop-shadow">메모</span>
                    </button>

                    <!-- Calendar -->
                    <button onclick="openApp('calendar')" class="flex flex-col items-center group">
                        <div class="w-14 h-14 bg-white rounded-2xl flex flex-col items-center justify-center text-red-500 shadow-lg group-active:scale-90 transition transform overflow-hidden">
                            <span class="text-[9px] font-bold uppercase bg-red-500 text-white w-full text-center py-0.5">10월</span>
                            <span id="calendar-icon-date" class="text-xl font-extrabold text-black leading-tight">23</span>
                        </div>
                        <span class="text-[11px] text-white mt-1.5 font-medium shadow-black drop-shadow">캘린더</span>
                    </button>

                    <!-- Data Editor Quick Button -->
                    <button onclick="openContentEditor()" class="flex flex-col items-center group">
                        <div class="w-14 h-14 bg-indigo-600 rounded-2xl flex items-center justify-center text-white shadow-lg group-active:scale-90 transition transform">
                            <i class="fa-solid fa-pen-to-square text-2xl"></i>
                        </div>
                        <span class="text-[11px] text-white mt-1.5 font-medium shadow-black drop-shadow">데이터 설정</span>
                    </button>
                </div>

                <!-- Dock Section -->
                <div class="bg-white/20 backdrop-blur-2xl rounded-3xl p-3 flex justify-around items-center mb-1">
                    <button onclick="openApp('phone')" class="w-13 h-13 bg-green-500 rounded-2xl flex items-center justify-center text-white shadow-md active:scale-90 transition">
                        <i class="fa-solid fa-phone text-2xl"></i>
                    </button>
                    <button onclick="openApp('kakaotalk')" class="w-13 h-13 bg-[#FEE500] rounded-2xl flex items-center justify-center text-black shadow-md active:scale-90 transition">
                        <i class="fa-solid fa-comment text-2xl"></i>
                    </button>
                    <button onclick="openApp('messages')" class="w-13 h-13 bg-emerald-500 rounded-2xl flex items-center justify-center text-white shadow-md active:scale-90 transition">
                        <i class="fa-solid fa-message text-2xl"></i>
                    </button>
                    <button onclick="openApp('garageband')" class="w-13 h-13 bg-gradient-to-tr from-red-600 to-orange-400 rounded-2xl flex items-center justify-center text-white shadow-md active:scale-90 transition">
                        <i class="fa-solid fa-guitar text-2xl"></i>
                    </button>
                </div>
            </div>

            <!-- 4. KAKAOTALK APP -->
            <div id="view-kakaotalk" class="screen-view hidden absolute inset-0 bg-[#a0b1c2] flex flex-col z-20">
                <!-- Header -->
                <div class="bg-[#a0b1c2] pt-2 px-3 pb-2 flex items-center justify-between border-b border-black/5">
                    <button onclick="showHome()" class="text-black text-sm px-2 py-1 flex items-center space-x-1">
                        <i class="fa-solid fa-chevron-left"></i>
                        <span class="text-xs">목록</span>
                    </button>
                    <div class="flex flex-col items-center">
                        <span id="kt-header-name" class="font-bold text-black text-sm">최 요원</span>
                    </div>
                    <div class="flex items-center space-x-3 text-black text-sm pr-1">
                        <i class="fa-solid fa-magnifying-glass"></i>
                        <button onclick="triggerCallFromKakao()"><i class="fa-solid fa-phone"></i></button>
                        <i class="fa-solid fa-bars"></i>
                    </div>
                </div>

                <!-- Chat Scroll View -->
                <div id="kt-chat-box" class="flex-1 overflow-y-auto p-3 space-y-3 text-xs">
                    <!-- Dynamic rendering via JS -->
                </div>

                <!-- Input Footer Bar -->
                <div class="bg-white p-2 flex items-center space-x-2 border-t border-gray-200">
                    <i class="fa-solid fa-plus text-gray-500 text-base px-1"></i>
                    <input id="kt-input" type="text" placeholder="메시지 입력" 
                           class="flex-1 bg-gray-100 rounded-full px-4 py-2 text-xs focus:outline-none text-black"
                           onkeypress="if(event.key==='Enter') sendKakaoMessage()">
                    <button onclick="sendKakaoMessage()" class="w-7 h-7 bg-[#FEE500] text-black rounded-full flex items-center justify-center font-bold text-xs">
                        <i class="fa-solid fa-arrow-up"></i>
                    </button>
                </div>
            </div>

            <!-- 5. PHONE APP -->
            <div id="view-phone" class="screen-view hidden absolute inset-0 bg-black flex flex-col z-20 text-white">
                <!-- Tabs Header -->
                <div class="pt-4 px-4 flex justify-around text-xs border-b border-zinc-800 pb-2">
                    <button onclick="switchPhoneTab('keypad')" id="tab-btn-keypad" class="text-blue-500 font-bold">키패드</button>
                    <button onclick="switchPhoneTab('recents')" id="tab-btn-recents" class="text-gray-400">최근 통화</button>
                </div>

                <!-- Tab 1: Keypad -->
                <div id="phone-tab-keypad" class="flex-1 flex flex-col justify-between py-6">
                    <div class="text-center h-10">
                        <div id="phone-display-number" class="text-3xl font-light tracking-wider font-mono"></div>
                    </div>

                    <div class="grid grid-cols-3 gap-4 text-center px-8">
                        <button onclick="pressDial('1')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">1</span><span class="text-[8px] text-gray-400">&nbsp;</span></button>
                        <button onclick="pressDial('2')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">2</span><span class="text-[8px] text-gray-400">ABC</span></button>
                        <button onclick="pressDial('3')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">3</span><span class="text-[8px] text-gray-400">DEF</span></button>
                        <button onclick="pressDial('4')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">4</span><span class="text-[8px] text-gray-400">GHI</span></button>
                        <button onclick="pressDial('5')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">5</span><span class="text-[8px] text-gray-400">JKL</span></button>
                        <button onclick="pressDial('6')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">6</span><span class="text-[8px] text-gray-400">MNO</span></button>
                        <button onclick="pressDial('7')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">7</span><span class="text-[8px] text-gray-400">PQRS</span></button>
                        <button onclick="pressDial('8')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">8</span><span class="text-[8px] text-gray-400">TUV</span></button>
                        <button onclick="pressDial('9')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">9</span><span class="text-[8px] text-gray-400">WXYZ</span></button>
                        <button onclick="pressDial('*')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex items-center justify-center text-2xl active:bg-zinc-600">*</button>
                        <button onclick="pressDial('0')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex flex-col items-center justify-center active:bg-zinc-600"><span class="text-2xl font-normal">0</span><span class="text-[10px] text-gray-400">+</span></button>
                        <button onclick="pressDial('#')" class="w-16 h-16 mx-auto bg-zinc-800 rounded-full flex items-center justify-center text-2xl active:bg-zinc-600">#</button>
                    </div>

                    <div class="grid grid-cols-3 gap-4 items-center px-8">
                        <div></div>
                        <button onclick="startOutgoingCall()" class="w-16 h-16 mx-auto bg-green-500 rounded-full flex items-center justify-center text-2xl shadow-lg active:scale-95 transition">
                            <i class="fa-solid fa-phone"></i>
                        </button>
                        <button onclick="deleteDial()" class="text-zinc-500 text-xl hover:text-white flex items-center justify-center">
                            <i class="fa-solid fa-backspace"></i>
                        </button>
                    </div>
                </div>

                <!-- Tab 2: Recents Call Log -->
                <div id="phone-tab-recents" class="hidden flex-1 overflow-y-auto p-4 space-y-3">
                    <div class="text-xl font-bold mb-2">최근 통화 기록</div>
                    <div id="phone-call-log-list" class="space-y-2">
                        <!-- Dynamic rendering -->
                    </div>
                </div>
            </div>

            <!-- 6. MESSAGES APP -->
            <div id="view-messages" class="screen-view hidden absolute inset-0 bg-black flex flex-col z-20 text-white">
                <div class="p-4 border-b border-zinc-800 flex justify-between items-center">
                    <button onclick="showHome()" class="text-blue-500 text-sm flex items-center space-x-1">
                        <i class="fa-solid fa-chevron-left"></i><span>홈</span>
                    </button>
                    <span class="font-bold text-sm">최요원</span>
                    <i class="fa-solid fa-circle-info text-blue-500 text-lg"></i>
                </div>

                <div id="msg-chat-box" class="flex-1 overflow-y-auto p-4 space-y-3 text-xs">
                    <div class="text-center text-gray-500 text-[10px] my-2">iMessage • 오늘</div>
                    <div class="flex justify-start">
                        <div class="bg-zinc-800 text-white px-4 py-2.5 rounded-2xl max-w-[220px]">
                            메시지 기능입니다. 자유롭게 테스트해보세요!
                        </div>
                    </div>
                </div>

                <div class="p-3 border-t border-zinc-800 flex items-center space-x-2 bg-black">
                    <i class="fa-solid fa-camera text-gray-400 text-lg"></i>
                    <input id="msg-input" type="text" placeholder="iMessage" 
                           class="flex-1 bg-zinc-900 border border-zinc-700 rounded-full px-4 py-1.5 text-xs text-white focus:outline-none"
                           onkeypress="if(event.key==='Enter') sendiMessage()">
                    <button onclick="sendiMessage()" class="w-7 h-7 bg-blue-500 text-white rounded-full flex items-center justify-center font-bold text-xs">
                        <i class="fa-solid fa-arrow-up"></i>
                    </button>
                </div>
            </div>

            <!-- 7. GARAGEBAND (작곡 앱) -->
            <div id="view-garageband" class="screen-view hidden absolute inset-0 bg-zinc-900 flex flex-col z-20 text-white">
                <div class="p-3 bg-zinc-950 flex justify-between items-center border-b border-zinc-800">
                    <button onclick="showHome()" class="text-xs text-red-400 flex items-center space-x-1">
                        <i class="fa-solid fa-chevron-left"></i><span>홈</span>
                    </button>
                    <span class="font-bold text-xs tracking-wider uppercase text-amber-400"><i class="fa-solid fa-music mr-1"></i>GarageBand</span>
                    <span class="text-[10px] text-gray-400 bg-zinc-800 px-2 py-0.5 rounded">120 BPM</span>
                </div>

                <div class="flex-1 p-3 flex flex-col justify-between">
                    <div>
                        <div class="text-[11px] font-semibold text-gray-400 mb-2">DRUM BEAT PADS</div>
                        <div class="grid grid-cols-3 gap-2">
                            <button onclick="playDrum('kick')" class="h-16 bg-gradient-to-br from-red-600 to-red-800 rounded-xl font-bold text-xs flex flex-col items-center justify-center active:scale-95 transition border border-red-500/30">
                                <i class="fa-solid fa-drum text-lg mb-1"></i>KICK
                            </button>
                            <button onclick="playDrum('snare')" class="h-16 bg-gradient-to-br from-orange-600 to-amber-700 rounded-xl font-bold text-xs flex flex-col items-center justify-center active:scale-95 transition border border-orange-500/30">
                                <i class="fa-solid fa-drum-steelpan text-lg mb-1"></i>SNARE
                            </button>
                            <button onclick="playDrum('hihat')" class="h-16 bg-gradient-to-br from-yellow-600 to-amber-600 rounded-xl font-bold text-xs flex flex-col items-center justify-center active:scale-95 transition border border-yellow-500/30">
                                <i class="fa-solid fa-compact-disc text-lg mb-1"></i>HI-HAT
                            </button>
                        </div>
                    </div>

                    <div class="mb-2">
                        <div class="text-[11px] font-semibold text-gray-400 mb-2">PIANO KEYBOARD</div>
                        <div class="flex h-36 bg-zinc-950 p-2 rounded-2xl border border-zinc-800 space-x-1 justify-center">
                            <button onclick="playNote(261.63)" class="flex-1 bg-white rounded-b-lg text-black font-bold text-[10px] pb-2 flex items-end justify-center active:bg-blue-300 shadow">C</button>
                            <button onclick="playNote(293.66)" class="flex-1 bg-white rounded-b-lg text-black font-bold text-[10px] pb-2 flex items-end justify-center active:bg-blue-300 shadow">D</button>
                            <button onclick="playNote(329.63)" class="flex-1 bg-white rounded-b-lg text-black font-bold text-[10px] pb-2 flex items-end justify-center active:bg-blue-300 shadow">E</button>
                            <button onclick="playNote(349.23)" class="flex-1 bg-white rounded-b-lg text-black font-bold text-[10px] pb-2 flex items-end justify-center active:bg-blue-300 shadow">F</button>
                            <button onclick="playNote(392.00)" class="flex-1 bg-white rounded-b-lg text-black font-bold text-[10px] pb-2 flex items-end justify-center active:bg-blue-300 shadow">G</button>
                            <button onclick="playNote(440.00)" class="flex-1 bg-white rounded-b-lg text-black font-bold text-[10px] pb-2 flex items-end justify-center active:bg-blue-300 shadow">A</button>
                            <button onclick="playNote(493.88)" class="flex-1 bg-white rounded-b-lg text-black font-bold text-[10px] pb-2 flex items-end justify-center active:bg-blue-300 shadow">B</button>
                            <button onclick="playNote(523.25)" class="flex-1 bg-white rounded-b-lg text-black font-bold text-[10px] pb-2 flex items-end justify-center active:bg-blue-300 shadow">C5</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 8. WEVERSE APP -->
            <div id="view-weverse" class="screen-view hidden absolute inset-0 bg-neutral-900 flex flex-col z-20 text-white">
                <div class="p-3 bg-teal-500 text-black flex justify-between items-center font-bold">
                    <button onclick="showHome()" class="text-black text-xs flex items-center space-x-1">
                        <i class="fa-solid fa-chevron-left"></i><span>홈</span>
                    </button>
                    <span class="text-sm tracking-wide">weverse</span>
                    <i class="fa-solid fa-bell"></i>
                </div>

                <div class="flex-1 overflow-y-auto p-4 space-y-4">
                    <div class="bg-zinc-800 rounded-2xl p-4 space-y-3">
                        <div class="flex items-center space-x-3">
                            <div class="w-10 h-10 bg-teal-400 rounded-full flex items-center justify-center font-bold text-black">최</div>
                            <div>
                                <div class="font-bold text-xs flex items-center space-x-1">
                                    <span>최요원</span>
                                    <i class="fa-solid fa-circle-check text-teal-400 text-[10px]"></i>
                                </div>
                                <div class="text-[10px] text-gray-400">10분 전</div>
                            </div>
                        </div>
                        <p class="text-xs leading-relaxed text-gray-200">
                            오늘 촬영 잘 마무리했습니다! 항상 응원해주셔서 고마워요 ❤️
                        </p>
                        <div class="pt-2 border-t border-zinc-700 flex items-center justify-between text-xs text-gray-400">
                            <button onclick="toggleLike(this)" class="flex items-center space-x-1">
                                <i class="fa-regular fa-heart"></i>
                                <span class="like-count">1,284</span>
                            </button>
                            <span class="flex items-center space-x-1"><i class="fa-regular fa-comment"></i><span>342</span></span>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 9. NOTES APP -->
            <div id="view-notes" class="screen-view hidden absolute inset-0 bg-zinc-950 flex flex-col z-20 text-white">
                <div class="p-3 border-b border-zinc-800 flex justify-between items-center">
                    <button onclick="showHome()" class="text-yellow-400 text-xs flex items-center space-x-1">
                        <i class="fa-solid fa-chevron-left"></i><span>홈</span>
                    </button>
                    <span class="font-bold text-xs">메모</span>
                    <button onclick="saveNote()" class="text-yellow-400 font-semibold text-xs">저장</button>
                </div>

                <div class="p-4 flex-1 flex flex-col">
                    <input id="note-title" type="text" placeholder="제목" class="bg-transparent text-lg font-bold mb-2 focus:outline-none text-yellow-400">
                    <textarea id="note-body" placeholder="여기에 메모 내용을 직접 입력하세요..." class="w-full flex-1 bg-transparent text-xs leading-relaxed focus:outline-none resize-none text-gray-200"></textarea>
                </div>
            </div>

            <!-- 10. CALENDAR APP -->
            <div id="view-calendar" class="screen-view hidden absolute inset-0 bg-black flex flex-col z-20 text-white">
                <div class="p-3 border-b border-zinc-800 flex justify-between items-center">
                    <button onclick="showHome()" class="text-red-500 text-xs flex items-center space-x-1">
                        <i class="fa-solid fa-chevron-left"></i><span>홈</span>
                    </button>
                    <span class="font-bold text-xs">2026년 10월</span>
                    <i class="fa-solid fa-plus text-red-500 text-sm"></i>
                </div>

                <div class="p-4 flex-1">
                    <div class="grid grid-cols-7 text-center text-[11px] text-gray-500 mb-2 font-bold">
                        <span class="text-red-500">일</span><span>월</span><span>화</span><span>수</span><span>목</span><span>금</span><span>토</span>
                    </div>
                    <div class="grid grid-cols-7 text-center text-xs gap-y-3">
                        <span class="text-zinc-600">27</span><span class="text-zinc-600">28</span><span class="text-zinc-600">29</span><span class="text-zinc-600">30</span><span>1</span><span>2</span><span>3</span>
                        <span>4</span><span>5</span><span>6</span><span>7</span><span>8</span><span>9</span><span>10</span>
                        <span>11</span><span>12</span><span>13</span><span>14</span><span>15</span><span>16</span><span>17</span>
                        <span>18</span><span>19</span><span>20</span><span>21</span><span>22</span>
                        <span class="bg-red-500 text-white rounded-full font-bold w-6 h-6 flex items-center justify-center mx-auto shadow">23</span>
                        <span>24</span>
                        <span>25</span><span>26</span><span>27</span><span>28</span><span>29</span><span>30</span><span>31</span>
                    </div>

                    <div class="mt-8 bg-zinc-900 rounded-2xl p-4 border border-zinc-800">
                        <div class="text-xs font-bold text-red-400 mb-1">오늘의 일정</div>
                        <div id="calendar-event-display" class="text-xs text-gray-200 font-semibold">오후 1:00 - 최요원 선배 만남</div>
                    </div>
                </div>
            </div>

            <!-- 11. CUSTOM DATA EDITOR MODAL -->
            <div id="view-editor" class="screen-view hidden absolute inset-0 bg-zinc-950 flex flex-col z-50 text-white p-4 overflow-y-auto">
                <div class="flex justify-between items-center mb-4 pb-2 border-b border-zinc-800">
                    <span class="font-bold text-sm text-indigo-400"><i class="fa-solid fa-sliders mr-1"></i>커스텀 데이터 설정</span>
                    <button onclick="closeContentEditor()" class="bg-indigo-600 px-3 py-1 rounded-full text-xs font-bold">완료 및 적용</button>
                </div>

                <div class="space-y-4 text-xs">
                    <div>
                        <label class="block text-gray-400 mb-1 font-semibold">상대방 이름</label>
                        <input id="edit-partner-name" type="text" class="w-full bg-zinc-900 border border-zinc-700 rounded p-2 text-white">
                    </div>

                    <div>
                        <label class="block text-gray-400 mb-1 font-semibold">카카오톡 초깃 대화 (한 줄에 하나씩)</label>
                        <textarea id="edit-kakao-chats" rows="4" class="w-full bg-zinc-900 border border-zinc-700 rounded p-2 text-white text-[11px] font-mono"></textarea>
                    </div>

                    <div>
                        <label class="block text-gray-400 mb-1 font-semibold">전화 통화 실시간 대사 스크립트 (한 줄에 하나씩)</label>
                        <textarea id="edit-call-script" rows="4" class="w-full bg-zinc-900 border border-zinc-700 rounded p-2 text-white text-[11px] font-mono"></textarea>
                    </div>

                    <div>
                        <label class="block text-gray-400 mb-1 font-semibold">메모 내용</label>
                        <textarea id="edit-note-text" rows="3" class="w-full bg-zinc-900 border border-zinc-700 rounded p-2 text-white text-[11px]"></textarea>
                    </div>

                    <div>
                        <label class="block text-gray-400 mb-1 font-semibold">캘린더 일정 내용</label>
                        <input id="edit-calendar-text" type="text" class="w-full bg-zinc-900 border border-zinc-700 rounded p-2 text-white">
                    </div>
                </div>
            </div>

            <!-- INCOMING CALL OVERLAY -->
            <div id="overlay-incoming-call" class="hidden absolute inset-0 bg-zinc-900 flex flex-col justify-between p-8 z-50 text-white">
                <div class="text-center mt-12 space-y-2">
                    <div id="incoming-caller-name" class="text-2xl font-bold">최 요원</div>
                    <div class="text-xs text-gray-400">휴대전화...</div>
                </div>

                <div class="mb-12 space-y-12">
                    <div class="flex justify-around items-center">
                        <button onclick="declineCall()" class="w-16 h-16 bg-red-600 rounded-full flex items-center justify-center text-2xl shadow-lg active:scale-90 transition">
                            <i class="fa-solid fa-phone-slash"></i>
                        </button>
                        <button onclick="acceptCall()" class="w-16 h-16 bg-green-500 rounded-full flex items-center justify-center text-2xl shadow-lg active:scale-90 transition">
                            <i class="fa-solid fa-phone"></i>
                        </button>
                    </div>
                </div>
            </div>

            <!-- ACTIVE CALL OVERLAY -->
            <div id="overlay-active-call" class="hidden absolute inset-0 bg-zinc-950 flex flex-col justify-between p-6 z-50 text-white">
                <div class="text-center mt-6 space-y-1">
                    <div class="text-2xl font-bold" id="call-partner-name">최 요원</div>
                    <div id="call-timer" class="text-xs text-gray-400 font-mono">00:00</div>
                </div>

                <!-- SUBTITLE BOX -->
                <div class="my-auto bg-zinc-900/90 border border-zinc-700/60 rounded-2xl p-4 mx-2 text-center min-h-[110px] flex flex-col items-center justify-center shadow-xl">
                    <div class="text-[10px] text-amber-400 uppercase tracking-widest font-semibold mb-1">
                        통화 자막
                    </div>
                    <div id="call-subtitle" class="text-base font-semibold leading-snug text-white transition-all duration-300">
                        (통화가 연결되었습니다.)
                    </div>
                </div>

                <div class="mb-6 flex justify-center">
                    <button onclick="endCall()" class="w-16 h-16 bg-red-600 rounded-full flex items-center justify-center text-2xl shadow-lg active:scale-90 transition">
                        <i class="fa-solid fa-phone-slash"></i>
                    </button>
                </div>
            </div>

            <!-- iOS Bottom Bar / Home Indicator -->
            <div class="relative z-50 w-full pb-2 pt-1 flex justify-center cursor-pointer" onclick="handleHomeBarClick()">
                <div class="w-32 h-1 bg-white/60 rounded-full hover:bg-white transition"></div>
            </div>

        </div>
    </div>

    <!-- Logic JS -->
    <script>
        // App Custom Data State (Easily editable by user)
        let appData = {
            partnerName: "최 요원",
            kakaoInitialChats: [
                { sender: 'partner', text: '이상한데 가신거 아니죠?' },
                { sender: 'me', text: '어디세요?' },
                { sender: 'me', text: '야' },
                { sender: 'partner', text: '아 쓰리~\n알람 꺼놓고 늦잠 자부렀당 ㅋㅋ' },
                { sender: 'partner', text: '근데 선배한테 야?' },
                { sender: 'me', text: '오타입니다.' },
                { sender: 'me', text: '빨리 출근하세요.' },
                { sender: 'partner', text: '걱정했어? ㅋㅋ 웅~ 빨리 가께 ~ 막 이래 ㅋㅋ' },
                { sender: 'partner', text: '오늘 영화 볼까?' },
                { sender: 'partner', text: '너 보고 싶다던 영화 있다고 했잖아.' }
            ],
            callScript: [
                "여보세요?",
                "어, 어디야? 지금 거의 다 도착했는데!",
                "아 진짜? ㅋㅋ 천천히 나와, 내가 기다릴게.",
                "오늘 영화 재미있었으면 좋겠다!",
                "도착하면 꼭 전화해~ 끊을게!"
            ],
            callLogs: [
                { name: "최 요원", type: "부재중 전화", time: "오전 10:41", date: "오늘" },
                { name: "최 요원", type: "발신 전화", time: "오전 10:30", date: "오늘" }
            ],
            noteText: "최요원 선배 만날 때 챙길 것:\n1. 영화 티켓 예매 확인\n2. 우산 챙기기",
            calendarEvent: "오후 1:00 - 최요원 선배 만남 (영화 약속)"
        };

        // Passcode Lock System (Passcode: 3020)
        const CORRECT_PASSCODE = "3020";
        let enteredPasscode = "";
        let isUnlocked = false;

        function showPasscodeScreen() {
            document.getElementById('view-lockscreen').classList.add('hidden');
            document.getElementById('view-passcode').classList.remove('hidden');
            resetPasscodeDots();
        }

        function pressPasscode(digit) {
            if (enteredPasscode.length < 4) {
                enteredPasscode += digit;
                updatePasscodeDots();

                if (enteredPasscode.length === 4) {
                    setTimeout(() => {
                        if (enteredPasscode === CORRECT_PASSCODE) {
                            isUnlocked = true;
                            document.getElementById('view-passcode').classList.add('hidden');
                            showHome();
                        } else {
                            // Shake error
                            const dotsContainer = document.getElementById('passcode-dots');
                            dotsContainer.classList.add('animate-shake');
                            setTimeout(() => {
                                dotsContainer.classList.remove('animate-shake');
                                deletePasscode();
                            }, 500);
                        }
                    }, 100);
                }
            }
        }

        function deletePasscode() {
            enteredPasscode = "";
            resetPasscodeDots();
            if (!isUnlocked) {
                document.getElementById('view-passcode').classList.add('hidden');
                document.getElementById('view-lockscreen').classList.remove('hidden');
            }
        }

        function updatePasscodeDots() {
            const dots = document.getElementById('passcode-dots').children;
            for (let i = 0; i < 4; i++) {
                if (i < enteredPasscode.length) {
                    dots[i].classList.add('bg-white');
                } else {
                    dots[i].classList.remove('bg-white');
                }
            }
        }

        function resetPasscodeDots() {
            enteredPasscode = "";
            updatePasscodeDots();
        }

        function handleHomeBarClick() {
            if (!isUnlocked) {
                showPasscodeScreen();
            } else {
                showHome();
            }
        }

        // Web Audio Synthesizer
        let audioCtx = null;
        function getAudioContext() {
            if (!audioCtx) audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            if (audioCtx.state === 'suspended') audioCtx.resume();
            return audioCtx;
        }

        function playNote(freq) {
            try {
                const ctx = getAudioContext();
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(freq, ctx.currentTime);
                gain.gain.setValueAtTime(0.3, ctx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.0001, ctx.currentTime + 1.2);
                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start();
                osc.stop(ctx.currentTime + 1.2);
            } catch(e) {}
        }

        function playDrum(type) {
            try {
                const ctx = getAudioContext();
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                if (type === 'kick') {
                    osc.frequency.setValueAtTime(120, ctx.currentTime);
                    osc.frequency.exponentialRampToValueAtTime(0.01, ctx.currentTime + 0.3);
                    gain.gain.setValueAtTime(0.8, ctx.currentTime);
                } else if (type === 'snare') {
                    osc.type = 'square';
                    osc.frequency.setValueAtTime(250, ctx.currentTime);
                    gain.gain.setValueAtTime(0.4, ctx.currentTime);
                } else {
                    osc.frequency.setValueAtTime(800, ctx.currentTime);
                    gain.gain.setValueAtTime(0.2, ctx.currentTime);
                }
                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start();
                osc.stop(ctx.currentTime + 0.3);
            } catch(e) {}
        }

        // Clock logic
        function updateClock() {
            const now = new Date();
            const hours = String(now.getHours()).padStart(2, '0');
            const minutes = String(now.getMinutes()).padStart(2, '0');
            const timeString = `${hours}:${minutes}`;

            document.getElementById('status-clock').innerText = timeString;
            document.getElementById('lock-time').innerText = timeString;
            document.getElementById('home-widget-time').innerText = timeString;

            const days = ['일', '월', '화', '수', '목', '금', '토'];
            const dateStr = `${now.getMonth() + 1}월 ${now.getDate()}일 ${days[now.getDay()]}요일`;
            document.getElementById('lock-date').innerText = dateStr;
            document.getElementById('home-widget-date').innerText = dateStr;
            document.getElementById('calendar-icon-date').innerText = now.getDate();
        }
        setInterval(updateClock, 1000);
        updateClock();

        // Screen Navigation
        function openApp(appId) {
            document.querySelectorAll('.screen-view').forEach(view => view.classList.add('hidden'));
            const target = document.getElementById(`view-${appId}`);
            if (target) target.classList.remove('hidden');
        }

        function showHome() {
            document.querySelectorAll('.screen-view').forEach(view => view.classList.add('hidden'));
            document.getElementById('view-home').classList.remove('hidden');
        }

        // Render KakaoTalk Chat
        function renderKakaoChat() {
            document.getElementById('kt-header-name').innerText = appData.partnerName;
            const chatBox = document.getElementById('kt-chat-box');
            chatBox.innerHTML = '<div class="text-center my-2"><span class="bg-black/10 text-white text-[10px] px-2.5 py-1 rounded-full">2026년 10월 23일</span></div>';

            appData.kakaoInitialChats.forEach(chat => {
                if (chat.sender === 'me') {
                    chatBox.innerHTML += `
                        <div class="flex justify-end items-end space-x-1">
                            <span class="text-[9px] text-gray-600">오전 10:30</span>
                            <div class="chat-bubble-yellow px-3 py-2 rounded-2xl max-w-[200px] whitespace-pre-line">${chat.text}</div>
                        </div>
                    `;
                } else {
                    chatBox.innerHTML += `
                        <div class="flex items-start space-x-2">
                            <div class="w-8 h-8 rounded-xl bg-gray-400 flex items-center justify-center text-white font-bold text-xs flex-shrink-0">${appData.partnerName[0]}</div>
                            <div>
                                <div class="text-[11px] text-gray-700 mb-0.5">${appData.partnerName}</div>
                                <div class="flex items-end space-x-1">
                                    <div class="chat-bubble-gray px-3 py-2 rounded-2xl max-w-[200px] whitespace-pre-line">${chat.text}</div>
                                    <span class="text-[9px] text-gray-600">오전 10:41</span>
                                </div>
                            </div>
                        </div>
                    `;
                }
            });
            chatBox.scrollTop = chatBox.scrollHeight;
        }

        function sendKakaoMessage() {
            const input = document.getElementById('kt-input');
            const text = input.value.trim();
            if (!text) return;

            appData.kakaoInitialChats.push({ sender: 'me', text: text });
            renderKakaoChat();
            input.value = '';

            // Auto response after 1s
            setTimeout(() => {
                appData.kakaoInitialChats.push({ sender: 'partner', text: '답장입니다! (자동 응답)' });
                renderKakaoChat();
            }, 1000);
        }

        // Phone App Tabs
        function switchPhoneTab(tab) {
            document.getElementById('phone-tab-keypad').classList.add('hidden');
            document.getElementById('phone-tab-recents').classList.add('hidden');
            document.getElementById('tab-btn-keypad').className = "text-gray-400";
            document.getElementById('tab-btn-recents').className = "text-gray-400";

            if (tab === 'keypad') {
                document.getElementById('phone-tab-keypad').classList.remove('hidden');
                document.getElementById('tab-btn-keypad').className = "text-blue-500 font-bold";
            } else {
                document.getElementById('phone-tab-recents').classList.remove('hidden');
                document.getElementById('tab-btn-recents').className = "text-blue-500 font-bold";
                renderCallLogs();
            }
        }

        function renderCallLogs() {
            const list = document.getElementById('phone-call-log-list');
            list.innerHTML = '';
            appData.callLogs.forEach(log => {
                list.innerHTML += `
                    <div class="flex justify-between items-center bg-zinc-900 p-3 rounded-xl border border-zinc-800">
                        <div>
                            <div class="font-bold text-xs ${log.type.includes('부재중') ? 'text-red-400' : 'text-white'}">${log.name}</div>
                            <div class="text-[10px] text-gray-400">${log.type}</div>
                        </div>
                        <div class="text-[10px] text-gray-500">${log.time}</div>
                    </div>
                `;
            });
        }

        let dialNumber = "";
        function pressDial(num) {
            playNote(500);
            dialNumber += num;
            document.getElementById('phone-display-number').innerText = dialNumber;
        }
        function deleteDial() {
            dialNumber = dialNumber.slice(0, -1);
            document.getElementById('phone-display-number').innerText = dialNumber;
        }

        function triggerCallFromKakao() {
            triggerIncomingCall();
        }

        function triggerIncomingCall() {
            document.getElementById('incoming-caller-name').innerText = appData.partnerName;
            document.getElementById('overlay-incoming-call').classList.remove('hidden');
        }

        function declineCall() {
            document.getElementById('overlay-incoming-call').classList.add('hidden');
        }

        function acceptCall() {
            document.getElementById('overlay-incoming-call').classList.add('hidden');
            startCallSession();
        }

        function startOutgoingCall() {
            startCallSession();
        }

        let callInterval = null, subtitleInterval = null, callSecs = 0;
        function startCallSession() {
            document.getElementById('overlay-active-call').classList.remove('hidden');
            document.getElementById('call-partner-name').innerText = appData.partnerName;

            callSecs = 0;
            clearInterval(callInterval);
            callInterval = setInterval(() => {
                callSecs++;
                const m = String(Math.floor(callSecs / 60)).padStart(2, '0');
                const s = String(callSecs % 60).padStart(2, '0');
                document.getElementById('call-timer').innerText = `${m}:${s}`;
            }, 1000);

            let scriptIdx = 0;
            const subtitleEl = document.getElementById('call-subtitle');
            subtitleEl.innerText = appData.callScript[0] || "...";

            clearInterval(subtitleInterval);
            subtitleInterval = setInterval(() => {
                scriptIdx++;
                if (scriptIdx < appData.callScript.length) {
                    subtitleEl.innerText = appData.callScript[scriptIdx];
                } else {
                    clearInterval(subtitleInterval);
                }
            }, 3000);
        }

        function endCall() {
            document.getElementById('overlay-active-call').classList.add('hidden');
            clearInterval(callInterval);
            clearInterval(subtitleInterval);
        }

        // Messages App
        function sendiMessage() {
            const input = document.getElementById('msg-input');
            if (!input.value.trim()) return;
            const box = document.getElementById('msg-chat-box');
            box.innerHTML += `<div class="flex justify-end"><div class="bg-blue-500 text-white px-4 py-2 rounded-2xl max-w-[220px]">${input.value}</div></div>`;
            input.value = '';
            box.scrollTop = box.scrollHeight;
        }

        // Notes & Calendar App Render
        function renderNotesAndCalendar() {
            document.getElementById('note-body').value = appData.noteText;
            document.getElementById('calendar-event-display').innerText = appData.calendarEvent;
        }

        function saveNote() {
            appData.noteText = document.getElementById('note-body').value;
            alert("메모가 저장되었습니다.");
            showHome();
        }

        function toggleLike(btn) {
            const countEl = btn.querySelector('.like-count');
            countEl.innerText = (parseInt(countEl.innerText.replace(',', '')) + 1).toLocaleString();
        }

        // Custom Data Editor Logic
        function openContentEditor() {
            document.getElementById('edit-partner-name').value = appData.partnerName;
            document.getElementById('edit-kakao-chats').value = appData.kakaoInitialChats.map(c => `${c.sender === 'me' ? '[나]' : '[상대]'} ${c.text}`).join('\n');
            document.getElementById('edit-call-script').value = appData.callScript.join('\n');
            document.getElementById('edit-note-text').value = appData.noteText;
            document.getElementById('edit-calendar-text').value = appData.calendarEvent;

            openApp('editor');
        }

        function closeContentEditor() {
            appData.partnerName = document.getElementById('edit-partner-name').value || "최 요원";
            
            // Parse Kakao chats
            const rawKakao = document.getElementById('edit-kakao-chats').value.split('\n');
            appData.kakaoInitialChats = rawKakao.map(line => {
                if (line.startsWith('[나]')) return { sender: 'me', text: line.replace('[나]', '').trim() };
                return { sender: 'partner', text: line.replace('[상대]', '').trim() };
            }).filter(c => c.text);

            // Parse Call script
            appData.callScript = document.getElementById('edit-call-script').value.split('\n').filter(s => s.trim());
            
            appData.noteText = document.getElementById('edit-note-text').value;
            appData.calendarEvent = document.getElementById('edit-calendar-text').value;

            renderKakaoChat();
            renderNotesAndCalendar();
            showHome();
        }

        // Init
        renderKakaoChat();
        renderNotesAndCalendar();
    </script>
</body>
</html>