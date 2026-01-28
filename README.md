[index.html](https://github.com/user-attachments/files/24922407/index.html)
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مبادرة وسم التراث الأردني | Wasm Initiative</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Amiri:ital,wght@0,400;0,700;1,400&family=Cairo:wght@300;400;600;800&display=swap" rel="stylesheet">

    <style>
        /* Custom Fonts */
        .font-amiri { font-family: 'Amiri', serif; }
        .font-cairo { font-family: 'Cairo', sans-serif; }
        
        /* Custom Animations & Patterns */
        .pattern-bg {
            background-image: radial-gradient(#C5A065 1px, transparent 1px);
            background-size: 20px 20px;
            opacity: 0.1;
        }
        
        @keyframes pulse-gold {
            0%, 100% { box-shadow: 0 0 0 0px rgba(197, 160, 101, 0.7); }
            50% { box-shadow: 0 0 0 10px rgba(197, 160, 101, 0); }
        }
        .animate-pulse-gold { animation: pulse-gold 2s infinite; }
        
        .fade-out {
            animation: fadeOut 1s forwards;
            pointer-events: none;
        }
        
        @keyframes fadeOut {
            from { opacity: 1; }
            to { opacity: 0; visibility: hidden; }
        }

        /* 3D Flip Card Styles */
        .perspective { perspective: 1000px; }
        .transform-style-3d { transform-style: preserve-3d; }
        .backface-hidden { backface-visibility: hidden; }
        .rotate-y-180 { transform: rotateY(180deg); }
        .group:hover .group-hover\:rotate-y-180 { transform: rotateY(180deg); }

        /* Custom Scrollbar */
        .custom-scrollbar::-webkit-scrollbar { width: 6px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: #f1f1f1; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #C5A065; border-radius: 10px; }
        .custom-scrollbar::-webkit-scrollbar-thumb:hover { background: #4A3728; }
    </style>
</head>
<body class="bg-[#FDFBF7] text-[#4A3728] font-sans m-0 p-0 overflow-x-hidden">

    <!-- Intro Screen -->
    <div id="intro-screen" class="fixed inset-0 z-50 bg-[#4A3728] flex flex-col items-center justify-center text-center transition-opacity duration-1000">
        <div class="relative">
            <div class="absolute inset-0 bg-[#C5A065] blur-3xl opacity-20 rounded-full animate-pulse"></div>
            <div class="relative z-10 p-8 border-4 border-[#C5A065] rounded-full mb-6">
                <i data-lucide="fingerprint" class="w-20 h-20 text-[#C5A065]"></i>
            </div>
        </div>
        <h1 class="text-6xl font-amiri font-bold text-[#C5A065] mb-2 animate-bounce">وســم</h1>
        <p class="text-[#eaddcf] text-xl font-cairo tracking-widest uppercase">التراث الأردني.. هوية ومستقبل</p>
    </div>

    <!-- Admin Login Modal -->
    <div id="login-modal" class="fixed inset-0 z-50 bg-black/50 hidden items-center justify-center backdrop-blur-sm p-4">
        <div class="bg-white p-8 rounded-2xl w-full max-w-sm shadow-2xl">
            <h3 class="text-xl font-bold text-center mb-4 text-[#4A3728]">دخول الإدارة</h3>
            <input type="password" id="admin-pass" placeholder="كلمة المرور (admin)" class="w-full p-3 border rounded-xl mb-4 text-center">
            <button onclick="loginAdmin()" class="w-full bg-[#4A3728] text-white py-3 rounded-xl font-bold mb-2">دخول</button>
            <button onclick="toggleModal('login-modal')" class="w-full text-gray-500 text-sm">إلغاء</button>
        </div>
    </div>

    <!-- Admin Dashboard (Overlay) -->
    <div id="admin-dashboard" class="fixed inset-0 z-50 bg-[#FDFBF7] hidden flex-col overflow-y-auto">
        <div class="bg-[#4A3728] p-4 flex justify-between items-center text-white shadow-md sticky top-0 z-10">
            <h2 class="text-xl font-bold font-cairo flex items-center gap-2"><i data-lucide="lock" class="w-5 h-5"></i> وحدة التحكم - مبادرة وسم</h2>
            <button onclick="logoutAdmin()" class="bg-red-500/20 hover:bg-red-600/40 text-red-100 px-4 py-2 rounded-lg text-sm transition-colors">تسجيل خروج</button>
        </div>
        
        <div class="flex flex-col md:flex-row flex-1">
            <!-- Sidebar -->
            <div class="w-full md:w-64 bg-white border-l border-[#eaddcf] p-4 gap-2 flex flex-row md:flex-col overflow-x-auto">
                <button onclick="switchAdminTab('dashboard')" class="p-3 rounded-xl text-right font-bold text-sm flex items-center gap-3 hover:bg-gray-100 whitespace-nowrap"><i data-lucide="layout-dashboard" class="w-4 h-4"></i> لوحة المعلومات</button>
                <button onclick="switchAdminTab('messages')" class="p-3 rounded-xl text-right font-bold text-sm flex items-center gap-3 hover:bg-gray-100 whitespace-nowrap"><i data-lucide="inbox" class="w-4 h-4"></i> الرسائل</button>
                <button onclick="switchAdminTab('opportunities')" class="p-3 rounded-xl text-right font-bold text-sm flex items-center gap-3 hover:bg-gray-100 whitespace-nowrap"><i data-lucide="calendar-days" class="w-4 h-4"></i> إدارة الفرص</button>
                <button onclick="switchAdminTab('broadcast')" class="p-3 rounded-xl text-right font-bold text-sm flex items-center gap-3 hover:bg-gray-100 whitespace-nowrap"><i data-lucide="megaphone" class="w-4 h-4"></i> إرسال رسائل</button>
            </div>

            <!-- Content -->
            <div class="flex-1 p-8 bg-gray-50 min-h-screen">
                <div id="admin-tab-dashboard">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
                            <h3 class="text-gray-500 text-sm font-bold mb-2">إجمالي المتطوعين الجدد</h3>
                            <p class="text-4xl font-bold text-[#4A3728]" id="admin-msg-count">0</p>
                        </div>
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
                            <h3 class="text-gray-500 text-sm font-bold mb-2">عدد الفعاليات الحالية</h3>
                            <p class="text-4xl font-bold text-[#C5A065]" id="admin-event-count">3</p>
                        </div>
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100">
                            <h3 class="text-gray-500 text-sm font-bold mb-2">المشتركين بالنشرة</h3>
                            <p class="text-4xl font-bold text-green-600" id="admin-sub-count">0</p>
                        </div>
                    </div>
                </div>

                <div id="admin-tab-messages" class="hidden">
                    <h3 class="text-2xl font-bold text-[#4A3728] mb-4">الرسائل الواردة</h3>
                    <div id="admin-messages-list" class="space-y-4">
                        <p class="text-gray-400 text-center py-10">لا توجد رسائل جديدة.</p>
                    </div>
                </div>

                <div id="admin-tab-opportunities" class="hidden">
                    <h3 class="text-2xl font-bold text-[#4A3728] mb-6">إدارة الفرص</h3>
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-[#eaddcf] mb-8">
                        <h4 class="font-bold text-[#C5A065] mb-4 flex items-center gap-2"><i data-lucide="plus" class="w-4 h-4"></i> إضافة فرصة جديدة</h4>
                        <form onsubmit="addEvent(event)" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                            <input type="text" id="new-event-title" placeholder="عنوان الفعالية" required class="p-3 border rounded-xl text-sm">
                            <input type="text" id="new-event-date" placeholder="التاريخ" required class="p-3 border rounded-xl text-sm">
                            <input type="text" id="new-event-loc" placeholder="الموقع" required class="p-3 border rounded-xl text-sm">
                            <select id="new-event-type" class="p-3 border rounded-xl text-sm">
                                <option value="ميداني">ميداني</option>
                                <option value="أونلاين">أونلاين</option>
                                <option value="ثقافي">ثقافي</option>
                            </select>
                            <button type="submit" class="md:col-span-2 bg-[#C5A065] text-[#4A3728] font-bold py-3 rounded-xl hover:bg-[#d4b075]">نشر</button>
                        </form>
                    </div>
                    <div id="admin-events-list" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <!-- Events injected via JS -->
                    </div>
                </div>

                <div id="admin-tab-broadcast" class="hidden">
                    <div class="bg-white p-6 rounded-2xl shadow-sm border border-gray-100 h-full flex flex-col">
                        <h3 class="text-2xl font-bold text-[#4A3728] mb-2">إرسال رسائل جماعية</h3>
                        <p class="text-sm text-gray-500 mb-6">إرسال تنبيه للمشتركين.</p>
                        <textarea class="flex-1 p-4 border border-[#eaddcf] rounded-xl mb-4 focus:outline-none focus:border-[#C5A065] resize-none h-40" placeholder="نص الرسالة..."></textarea>
                        <button onclick="alert('تم الإرسال!')" class="bg-[#4A3728] text-white py-4 rounded-xl font-bold hover:bg-[#5d4a3a] flex items-center justify-center gap-2">
                            <i data-lucide="send" class="w-4 h-4"></i> إرسال للجميع
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Main Container -->
    <div id="main-content" class="max-w-4xl mx-auto bg-white shadow-2xl rounded-3xl overflow-hidden border border-[#eaddcf] my-4 md:my-8 opacity-0 transition-opacity duration-1000">
        
        <!-- Header -->
        <div class="bg-gradient-to-b from-[#4A3728] to-[#2e2218] text-[#C5A065] pt-20 pb-16 px-12 text-center relative overflow-hidden">
            <div class="absolute top-0 right-0 w-64 h-64 bg-[#C5A065] opacity-5 rounded-full -translate-y-1/2 translate-x-1/2 blur-3xl"></div>
            <div class="absolute bottom-0 left-0 w-48 h-48 bg-[#C5A065] opacity-5 rounded-full translate-y-1/2 -translate-x-1/2 blur-2xl"></div>
            <div class="absolute inset-0 pattern-bg opacity-20"></div>
            
            <div class="relative z-10 flex flex-col items-center justify-center">
                <h1 class="text-6xl md:text-8xl font-amiri font-bold text-[#C5A065] mb-2 tracking-wide drop-shadow-md">وســم</h1>
                <p class="text-lg md:text-xl font-cairo font-light tracking-widest text-[#eaddcf] opacity-80 mb-6 uppercase">Wasm Initiative</p>
                <div class="flex items-center gap-4 justify-center text-[#eaddcf] opacity-90">
                    <span class="h-[2px] w-12 bg-[#C5A065]"></span>
                    <span class="w-2 h-2 rotate-45 bg-[#C5A065]"></span>
                    <h2 class="text-2xl md:text-3xl font-cairo font-light tracking-widest uppercase">التراث الأردني</h2>
                    <span class="w-2 h-2 rotate-45 bg-[#C5A065]"></span>
                    <span class="h-[2px] w-12 bg-[#C5A065]"></span>
                </div>
                <p class="mt-4 text-[#eaddcf]/50 text-xs md:text-sm tracking-wider font-light uppercase">Jordanian Heritage</p>
            </div>
        </div>

        <div class="p-6 md:p-12 space-y-16 bg-white relative">
            
            <!-- About Section -->
            <section class="bg-[#FAF8F5] rounded-3xl p-8 border border-[#eaddcf] text-center">
                <h3 class="text-3xl font-bold text-[#4A3728] font-cairo mb-6">عن المبادرة</h3>
                <p class="text-lg leading-loose text-gray-700 max-w-2xl mx-auto italic font-amiri">"وسم هو البصمة التي لا تُمحى، هويةٌ نغرسها في قلب التكنولوجيا لنحفظ تاريخ الأجداد."</p>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mt-10">
                    <div class="bg-white p-4 rounded-xl shadow-sm border-t-4 border-[#C5A065]">
                        <i data-lucide="eye" class="w-6 h-6 mx-auto mb-2 text-[#C5A065]"></i>
                        <h4 class="font-bold text-[#4A3728]">الرؤية</h4>
                        <p class="text-xs text-gray-600">المرجع الأول لتوثيق الهوية الأردنية بصرياً.</p>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border-t-4 border-[#4A3728]">
                        <i data-lucide="book-open" class="w-6 h-6 mx-auto mb-2 text-[#4A3728]"></i>
                        <h4 class="font-bold text-[#4A3728]">الرسالة</h4>
                        <p class="text-xs text-gray-600">تمكين الشباب وحماية التراث بأدوات عصرية.</p>
                    </div>
                    <div class="bg-white p-4 rounded-xl shadow-sm border-t-4 border-[#C5A065]">
                        <i data-lucide="target" class="w-6 h-6 mx-auto mb-2 text-[#C5A065]"></i>
                        <h4 class="font-bold text-[#4A3728]">الهدف</h4>
                        <p class="text-xs text-gray-600">ترسيخ الهوية الوطنية وتعزيز السياحة.</p>
                    </div>
                </div>
            </section>

            <!-- Linguistic Analysis -->
            <section>
                <div class="flex items-center gap-3 mb-10 text-center justify-center md:justify-start border-b border-[#C5A065]/20 pb-4">
                    <div class="p-2 bg-[#FAF8F5] rounded-lg border border-[#C5A065]/30">
                        <i data-lucide="pen-tool" class="w-6 h-6 text-[#C5A065]"></i>
                    </div>
                    <h3 class="text-3xl font-bold text-[#4A3728] font-cairo">تحليل الاسم (وسم)</h3>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                    <div class="flex flex-col items-center p-6 bg-[#4A3728] text-white rounded-2xl relative overflow-hidden group hover:-translate-y-1 transition-transform">
                        <span class="text-6xl font-amiri font-bold mb-2 text-[#C5A065]">و</span>
                        <h4 class="text-xl font-bold mb-2">وَصل</h4>
                        <p class="text-center text-sm font-light opacity-90">حلقة وصل بين عراقة الماضي وحداثة المستقبل.</p>
                    </div>
                    <div class="flex flex-col items-center p-6 bg-gradient-to-br from-[#C5A065] to-[#a8824a] text-white rounded-2xl relative overflow-hidden group hover:-translate-y-1 transition-transform">
                        <span class="text-6xl font-amiri font-bold mb-2 text-[#4A3728]">سـ</span>
                        <h4 class="text-xl font-bold mb-2">سِمة</h4>
                        <p class="text-center text-sm font-light opacity-90">السمة المميزة والبصمة الخاصة للهوية الأردنية.</p>
                    </div>
                    <div class="flex flex-col items-center p-6 bg-[#4A3728] text-white rounded-2xl relative overflow-hidden group hover:-translate-y-1 transition-transform">
                        <span class="text-6xl font-amiri font-bold mb-2 text-[#C5A065]">ـم</span>
                        <h4 class="text-xl font-bold mb-2">مَوروث</h4>
                        <p class="text-center text-sm font-light opacity-90">الموروث الحضاري والثقافي الذي نسعى لحفظه.</p>
                    </div>
                </div>
            </section>

            <!-- Color Palette & Typography -->
            <section class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <!-- Colors -->
                <div class="bg-white p-6 rounded-2xl border border-[#eaddcf]">
                    <div class="flex items-center gap-2 mb-4">
                        <i data-lucide="palette" class="w-6 h-6 text-[#C5A065]"></i>
                        <h3 class="text-xl font-bold text-[#4A3728]">ألوان الهوية</h3>
                    </div>
                    <div class="flex gap-4 justify-center">
                        <div class="text-center">
                            <div class="w-16 h-16 bg-[#4A3728] rounded-full shadow-lg mx-auto mb-2 border-2 border-white"></div>
                            <span class="text-xs font-bold text-[#4A3728]">بني داكن</span>
                        </div>
                        <div class="text-center">
                            <div class="w-16 h-16 bg-[#C5A065] rounded-full shadow-lg mx-auto mb-2 border-2 border-white"></div>
                            <span class="text-xs font-bold text-[#b8860b]">ذهبي</span>
                        </div>
                        <div class="text-center">
                            <div class="w-16 h-16 bg-[#FDFBF7] rounded-full shadow-md mx-auto mb-2 border-2 border-gray-200"></div>
                            <span class="text-xs font-bold text-gray-500">أوف وايت</span>
                        </div>
                    </div>
                </div>
                <!-- Fonts -->
                <div class="bg-white p-6 rounded-2xl border border-[#eaddcf]">
                    <div class="flex items-center gap-2 mb-4">
                        <i data-lucide="type" class="w-6 h-6 text-[#C5A065]"></i>
                        <h3 class="text-xl font-bold text-[#4A3728]">الخطوط</h3>
                    </div>
                    <div class="space-y-4">
                        <div class="bg-[#FAF8F5] p-3 rounded-lg border-r-4 border-[#4A3728]">
                            <p class="font-amiri text-2xl text-[#4A3728]">خط أميري</p>
                            <p class="text-xs text-gray-500">للعناوين والعبارات التراثية</p>
                        </div>
                        <div class="bg-[#FAF8F5] p-3 rounded-lg border-r-4 border-[#C5A065]">
                            <p class="font-cairo text-2xl text-[#4A3728] font-bold">خط كايرو</p>
                            <p class="text-xs text-gray-500">للنصوص الحديثة والرقمية</p>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Stats -->
            <section class="bg-[#4A3728] text-white rounded-2xl p-8 flex flex-wrap justify-around shadow-xl gap-4">
                <div class="text-center p-2">
                    <i data-lucide="users" class="w-7 h-7 mx-auto text-[#C5A065] mb-2"></i>
                    <h4 class="text-3xl font-bold font-cairo text-[#C5A065]">+500</h4>
                    <p class="text-[10px] opacity-70 uppercase tracking-widest">عضو متطوع</p>
                </div>
                <div class="text-center p-2">
                    <!-- Flag Icon SVG -->
                    <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mx-auto text-[#C5A065] mb-2"><path d="M4 15s1-1 4-1 5 2 8 2 4-1 4-1V3s-1 1-4 1-5-2-8-2-4 1-4 1z"></path><line x1="4" y1="22" x2="4" y2="15"></line></svg>
                    <h4 class="text-3xl font-bold font-cairo text-[#C5A065]">+50</h4>
                    <p class="text-[10px] opacity-70 uppercase tracking-widest">فعالية ميدانية</p>
                </div>
                <div class="text-center p-2">
                    <i data-lucide="map" class="w-7 h-7 mx-auto text-[#C5A065] mb-2"></i>
                    <h4 class="text-3xl font-bold font-cairo text-[#C5A065]">+12</h4>
                    <p class="text-[10px] opacity-70 uppercase tracking-widest">محافظة مغطاة</p>
                </div>
                <div class="text-center p-2">
                    <i data-lucide="clock" class="w-7 h-7 mx-auto text-[#C5A065] mb-2"></i>
                    <h4 class="text-3xl font-bold font-cairo text-[#C5A065]">+1000</h4>
                    <p class="text-[10px] opacity-70 uppercase tracking-widest">ساعة تطوعية</p>
                </div>
            </section>

            <!-- Partners -->
            <section>
                <div class="flex items-center gap-3 mb-10 text-center justify-center md:justify-start border-b border-[#C5A065]/20 pb-4">
                    <i data-lucide="handshake" class="w-6 h-6 text-[#C5A065]"></i>
                    <h3 class="text-3xl font-bold text-[#4A3728] font-cairo">شركاء النجاح</h3>
                </div>
                <div class="flex flex-wrap justify-center gap-8 opacity-90" id="partners-list">
                    <!-- Injected by JS -->
                </div>
            </section>

            <!-- Team -->
            <section>
                <div class="flex items-center gap-3 mb-10 text-center justify-center md:justify-start border-b border-[#C5A065]/20 pb-4">
                    <i data-lucide="users" class="w-6 h-6 text-[#C5A065]"></i>
                    <h3 class="text-3xl font-bold text-[#4A3728] font-cairo">فريق العمل</h3>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="team-list">
                    <!-- Injected by JS -->
                </div>
            </section>

            <!-- Roadmap -->
            <section>
                <div class="flex items-center gap-3 mb-10 border-b border-[#C5A065]/20 pb-4">
                    <i data-lucide="calendar" class="w-6 h-6 text-[#C5A065]"></i>
                    <h3 class="text-3xl font-bold text-[#4A3728] font-cairo">خارطة الطريق</h3>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-4 gap-4" id="roadmap-list">
                    <!-- Injected by JS -->
                </div>
            </section>

            <!-- Governorates -->
            <section>
                <div class="flex items-center gap-3 mb-10 border-b border-[#C5A065]/20 pb-4">
                    <i data-lucide="map" class="w-6 h-6 text-[#C5A065]"></i>
                    <h3 class="text-3xl font-bold text-[#4A3728] font-cairo">كنوز المحافظات الأردنية</h3>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="governorates-list">
                    <!-- Injected by JS -->
                </div>
            </section>

            <!-- Events -->
            <section>
                <div class="flex items-center gap-3 mb-10 text-center justify-center md:justify-start border-b border-[#C5A065]/20 pb-4">
                    <i data-lucide="calendar-days" class="w-6 h-6 text-[#C5A065]"></i>
                    <h3 class="text-3xl font-bold text-[#4A3728] font-cairo">أجندة الفعاليات</h3>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6" id="events-list-display">
                    <!-- Injected by JS -->
                </div>
            </section>

            <!-- Volunteer Journey -->
            <section>
                <div class="flex items-center gap-3 mb-10 text-center justify-center md:justify-start border-b border-[#C5A065]/20 pb-4">
                    <i data-lucide="history" class="w-6 h-6 text-[#C5A065]"></i>
                    <h3 class="text-3xl font-bold text-[#4A3728] font-cairo">رحلة المتطوع</h3>
                </div>
                <div class="flex flex-col md:flex-row justify-between items-center gap-4 relative" id="journey-list">
                    <div class="hidden md:block absolute top-1/2 left-0 w-full h-1 bg-[#eaddcf] -z-10 -translate-y-1/2"></div>
                    <!-- Injected by JS -->
                </div>
            </section>

            <!-- Membership Types -->
            <section>
                <div class="flex items-center gap-3 mb-10 text-center justify-center md:justify-start border-b border-[#C5A065]/20 pb-4">
                    <i data-lucide="badge-check" class="w-6 h-6 text-[#C5A065]"></i>
                    <h3 class="text-3xl font-bold text-[#4A3728] font-cairo">أنواع العضوية</h3>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6" id="membership-list">
                    <!-- Injected by JS -->
                </div>
            </section>

            <!-- ================= INTERACTIVE ZONE ================= -->
            <div class="bg-[#FAF8F5] rounded-3xl p-8 border-4 border-dashed border-[#eaddcf] space-y-16">
                <div class="text-center mb-12">
                    <h2 class="text-4xl font-amiri font-bold text-[#4A3728] mb-2">واحة وسم التفاعلية</h2>
                    <p class="text-gray-500">اكتشف، العب، وشارك في التراث</p>
                </div>

                <!-- Digital Museum -->
                <section>
                    <div class="flex items-center gap-3 mb-8 border-b border-[#C5A065]/20 pb-4">
                        <i data-lucide="award" class="w-6 h-6 text-[#C5A065]"></i>
                        <h3 class="text-2xl font-bold font-cairo">متحف وسم الرقمي</h3>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-3 lg:grid-cols-5 gap-4" id="museum-list">
                        <!-- Injected by JS -->
                    </div>
                </section>

                <!-- Artisan Souq -->
                <section>
                    <div class="flex items-center gap-3 mb-8 border-b border-[#C5A065]/20 pb-4">
                        <i data-lucide="shopping-bag" class="w-6 h-6 text-[#C5A065]"></i>
                        <h3 class="text-2xl font-bold font-cairo">سوق الحرفيين (دعم المجتمع)</h3>
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6" id="crafts-list">
                        <!-- Injected by JS -->
                    </div>
                </section>

                <!-- Dialect Dictionary -->
                <section>
                    <div class="flex items-center gap-3 mb-8 border-b border-[#C5A065]/20 pb-4">
                        <i data-lucide="book" class="w-6 h-6 text-[#C5A065]"></i>
                        <h3 class="text-2xl font-bold font-cairo">قاموس اللهجة الأردنية</h3>
                    </div>
                    <div class="grid grid-cols-2 md:grid-cols-4 gap-4" id="dialect-list">
                        <!-- Injected by JS -->
                    </div>
                </section>

                <!-- Proverbs -->
                <section class="bg-[#4A3728] text-white rounded-3xl p-10 relative overflow-hidden text-center shadow-xl">
                    <div class="absolute top-0 right-0 p-4 opacity-10"><i data-lucide="quote" class="w-24 h-24"></i></div>
                    <h3 class="text-2xl font-bold font-cairo text-[#C5A065] mb-6">كنز الأمثال الشعبية</h3>
                    <div class="min-h-[140px] flex flex-col justify-center items-center relative z-10">
                        <p id="proverb-text" class="text-2xl md:text-3xl font-amiri mb-4 leading-relaxed"></p>
                        <div class="h-px w-20 bg-[#C5A065]/50 mb-4"></div>
                        <p id="proverb-meaning" class="text-base text-[#eaddcf] font-light"></p>
                    </div>
                    <div class="flex justify-center gap-4 mt-8 relative z-10">
                        <button onclick="prevProverb()" class="p-3 rounded-full bg-white/10 hover:bg-[#C5A065] hover:text-[#4A3728] transition-all"><i data-lucide="chevron-right" class="w-5 h-5"></i></button>
                        <button onclick="nextProverb()" class="px-6 py-2 rounded-full bg-[#C5A065] text-[#4A3728] font-bold hover:bg-white transition-all flex items-center gap-2"><i data-lucide="shuffle" class="w-4 h-4"></i> مثل جديد</button>
                        <button onclick="nextProverb()" class="p-3 rounded-full bg-white/10 hover:bg-[#C5A065] hover:text-[#4A3728] transition-all"><i data-lucide="chevron-left" class="w-5 h-5"></i></button>
                    </div>
                </section>

                <!-- AI Tools -->
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <!-- Name Story -->
                    <section class="bg-white border border-[#eaddcf] rounded-3xl p-8 shadow-md hover:border-[#C5A065] transition-colors">
                        <div class="flex items-center gap-3 mb-6">
                            <div class="p-3 bg-[#FAF8F5] rounded-xl text-[#C5A065]"><i data-lucide="fingerprint" class="w-6 h-6"></i></div>
                            <h3 class="text-xl font-bold text-[#4A3728]">وسم الأسماء</h3>
                        </div>
                        <p class="text-sm text-gray-500 mb-4">اكتشف المعنى التراثي لاسمك.</p>
                        <div class="flex gap-2 mb-4">
                            <input type="text" id="visitor-name" placeholder="اكتب اسمك..." class="flex-1 p-3 border rounded-xl text-sm focus:outline-none focus:border-[#C5A065]">
                            <button onclick="generateNameStory()" class="bg-[#4A3728] text-white px-6 rounded-xl text-sm font-bold hover:bg-[#5d4a3a]">انسج ✨</button>
                        </div>
                        <div id="name-story-result" class="hidden bg-[#FAF8F5] p-4 rounded-xl border border-[#eaddcf] text-sm leading-loose font-amiri text-[#4A3728]"></div>
                    </section>

                    <!-- Trip Planner -->
                    <section class="bg-white border border-[#eaddcf] rounded-3xl p-8 shadow-md hover:border-[#C5A065] transition-colors">
                        <div class="flex items-center gap-3 mb-6">
                            <div class="p-3 bg-[#FAF8F5] rounded-xl text-[#C5A065]"><i data-lucide="compass" class="w-6 h-6"></i></div>
                            <h3 class="text-xl font-bold text-[#4A3728]">مخطط الرحلات</h3>
                        </div>
                        <p class="text-sm text-gray-500 mb-4">خطط رحلة تراثية في ربوع الأردن.</p>
                        <div class="flex gap-2 mb-4">
                            <select id="trip-city" class="flex-1 p-3 border rounded-xl text-sm bg-white focus:outline-none focus:border-[#C5A065]"></select>
                            <button onclick="generateTrip()" class="bg-[#4A3728] text-white px-6 rounded-xl text-sm font-bold hover:bg-[#5d4a3a] whitespace-nowrap">خطط ✨</button>
                        </div>
                        <div id="trip-result" class="hidden bg-[#FAF8F5] p-4 rounded-xl border border-[#eaddcf] max-h-40 overflow-y-auto custom-scrollbar text-xs leading-relaxed text-gray-700 whitespace-pre-line"></div>
                    </section>

                    <!-- Rababa Poet -->
                    <section class="bg-white border border-[#eaddcf] rounded-3xl p-8 shadow-md hover:border-[#C5A065] transition-colors">
                        <div class="flex items-center gap-3 mb-6">
                            <div class="p-3 bg-[#FAF8F5] rounded-xl text-[#C5A065]"><i data-lucide="music" class="w-6 h-6"></i></div>
                            <h3 class="text-xl font-bold text-[#4A3728]">شاعر الربابة</h3>
                        </div>
                        <div class="flex gap-2 mb-4">
                            <input type="text" id="poet-topic" placeholder="الموضوع (القهوة، الكرم...)" class="flex-1 p-3 border rounded-xl text-sm focus:outline-none focus:border-[#C5A065]">
                            <button onclick="generatePoem()" class="bg-[#4A3728] text-white px-6 rounded-xl text-sm font-bold hover:bg-[#5d4a3a]">انظم ✨</button>
                        </div>
                        <div id="poet-result" class="hidden bg-[#FAF8F5] p-4 rounded-xl border border-[#eaddcf] text-sm leading-loose font-amiri text-[#4A3728] text-center whitespace-pre-line"></div>
                    </section>

                    <!-- Kitchen -->
                    <section class="bg-white border border-[#eaddcf] rounded-3xl p-8 shadow-md hover:border-[#C5A065] transition-colors">
                        <div class="flex items-center gap-3 mb-6">
                            <div class="p-3 bg-[#FAF8F5] rounded-xl text-[#C5A065]"><i data-lucide="utensils" class="w-6 h-6"></i></div>
                            <h3 class="text-xl font-bold text-[#4A3728]">مطبخ وسم</h3>
                        </div>
                        <div class="flex gap-2 mb-4">
                            <input type="text" id="kitchen-ing" placeholder="المكونات (جميد، لحم...)" class="flex-1 p-3 border rounded-xl text-sm focus:outline-none focus:border-[#C5A065]">
                            <button onclick="generateRecipe()" class="bg-[#4A3728] text-white px-6 rounded-xl text-sm font-bold hover:bg-[#5d4a3a]">اطبخ ✨</button>
                        </div>
                        <div id="kitchen-result" class="hidden bg-[#FAF8F5] p-4 rounded-xl border border-[#eaddcf] text-xs leading-relaxed text-gray-700 whitespace-pre-line"></div>
                    </section>
                </div>

                <!-- Hakawati -->
                <section class="bg-gradient-to-br from-[#2e2218] to-[#4A3728] rounded-3xl p-8 text-white border border-[#C5A065]/30 shadow-2xl relative">
                    <div class="flex items-center gap-4 mb-6 relative z-10">
                        <div class="p-4 bg-[#C5A065] rounded-full animate-pulse-gold text-[#4A3728] shadow-lg"><i data-lucide="message-square" class="w-8 h-8"></i></div>
                        <div>
                            <h3 class="text-3xl font-bold font-cairo text-[#C5A065]">الحكواتي الرقمي</h3>
                            <p class="text-sm text-[#eaddcf] opacity-80">اسألني عن تاريخ، أكلة، أو قصة أردنية قديمة..</p>
                        </div>
                    </div>
                    <div class="flex gap-3 mb-6 relative z-10">
                        <input type="text" id="hakawati-q" placeholder="مثال: ليش سموا البحر الميت بهذا الاسم؟" class="flex-1 bg-white/10 border border-[#C5A065]/30 rounded-2xl px-5 py-4 text-white focus:outline-none focus:bg-white/20 transition-all placeholder-white/30">
                        <button onclick="askHakawati()" class="bg-[#C5A065] text-[#4A3728] px-8 rounded-2xl font-bold hover:bg-white transition-colors shadow-lg">اسأل ✨</button>
                    </div>
                    <div class="bg-black/20 backdrop-blur-md rounded-2xl p-6 border border-[#C5A065]/10 min-h-[120px] relative z-10">
                        <p id="hakawati-res" class="text-[#eaddcf] leading-loose font-amiri text-lg">أهلاً بك يا نشمي.. أنا جاهز لأي سؤال يدور ببالك عن تراثنا العريق.</p>
                    </div>
                </section>

                <!-- Quiz -->
                <section class="bg-white border-2 border-[#eaddcf] rounded-3xl p-8 text-center shadow-lg relative overflow-hidden">
                    <div class="mb-8">
                        <div class="inline-block p-4 bg-[#FAF8F5] rounded-full mb-4 border border-[#C5A065]/30"><i data-lucide="award" class="w-12 h-12 text-[#C5A065]"></i></div>
                        <h3 class="text-3xl font-bold text-[#4A3728] font-cairo">تحدي وسم التراثي</h3>
                    </div>
                    <div id="quiz-start">
                        <button onclick="startQuiz()" class="bg-[#4A3728] text-white px-12 py-4 rounded-full font-bold shadow-xl hover:bg-[#5d4a3a] transition-all flex items-center gap-2 mx-auto"><i data-lucide="gamepad-2" class="w-5 h-5"></i> ابدأ التحدي الآن</button>
                    </div>
                    <div id="quiz-question" class="hidden max-w-2xl mx-auto">
                        <div class="w-full bg-gray-200 rounded-full h-2 mb-6"><div id="quiz-progress" class="bg-[#C5A065] h-2 rounded-full w-0 transition-all duration-500"></div></div>
                        <h4 id="q-text" class="text-2xl font-bold mb-8 text-[#4A3728]"></h4>
                        <div id="q-options" class="grid grid-cols-1 gap-4"></div>
                    </div>
                    <div id="quiz-result" class="hidden py-8">
                        <div class="w-32 h-32 bg-[#C5A065] text-white rounded-full flex flex-col items-center justify-center mx-auto mb-6 shadow-2xl border-4 border-white">
                            <span id="final-score" class="text-4xl font-bold">0</span>
                        </div>
                        <h4 class="text-3xl font-bold mb-2 text-[#4A3728]">النتيجة النهائية</h4>
                        <p id="final-title" class="text-xl text-[#C5A065] font-bold mb-8 bg-[#FAF8F5] inline-block px-6 py-2 rounded-lg border border-[#eaddcf]"></p>
                        <button onclick="resetQuiz()" class="text-[#4A3728] border-b-2 border-[#4A3728] font-bold">إعادة المحاولة</button>
                    </div>
                </section>

                <!-- Pledge -->
                <section class="bg-gradient-to-r from-[#4A3728] to-[#2e2218] rounded-3xl p-10 text-white text-center relative overflow-hidden shadow-2xl">
                    <div class="relative z-10">
                        <h3 class="text-4xl font-bold font-cairo text-[#C5A065] mb-6 flex justify-center items-center gap-2"><i data-lucide="handshake" class="w-8 h-8"></i> ميثاق وسم</h3>
                        <div class="bg-white/5 p-6 rounded-2xl border border-white/10 max-w-2xl mx-auto mb-8 backdrop-blur-sm">
                            <p class="text-lg leading-loose font-amiri">"أتعهد أنا الموقع أدناه، بأن أكون حارساً أميناً لتراث الأردن، مدافعاً عن هويته، وناقلاً لرسالته بكل فخر واعتزاز."</p>
                        </div>
                        <button id="pledge-btn" onclick="handlePledge()" class="group relative px-10 py-4 rounded-full font-bold text-xl transition-all shadow-lg bg-transparent border-2 border-[#C5A065] text-[#C5A065] hover:bg-[#C5A065] hover:text-[#4A3728]">
                            <span class="flex items-center gap-2"><i data-lucide="feather" class="w-5 h-5"></i> وقع الميثاق الآن</span>
                        </button>
                        <div class="mt-6 bg-black/30 px-8 py-3 rounded-full backdrop-blur-md border border-white/5 inline-flex items-center gap-3">
                            <i data-lucide="users" class="w-4 h-4 text-[#C5A065]"></i>
                            <span class="text-[#eaddcf] text-sm">عدد الموقعين: <span id="pledge-count" class="text-xl font-bold text-[#C5A065] font-mono">1,250</span></span>
                        </div>
                    </div>
                </section>
            </div>

            <!-- Newsletter -->
            <section class="bg-white border border-[#eaddcf] rounded-2xl p-8 text-center shadow-sm">
                <div class="max-w-xl mx-auto">
                    <div class="flex justify-center mb-4"><i data-lucide="bell" class="w-6 h-6 text-[#C5A065]"></i></div>
                    <h3 class="text-xl font-bold text-[#4A3728] mb-2">ابق على اتصال</h3>
                    <form onsubmit="handleNewsletter(event)" class="flex gap-2">
                        <input type="email" id="news-email" placeholder="بريدك الإلكتروني" required class="flex-1 p-3 border border-gray-200 rounded-xl text-sm focus:outline-none focus:border-[#C5A065]">
                        <button type="submit" class="bg-[#4A3728] text-white px-6 rounded-xl text-sm font-bold hover:bg-[#5d4a3a]">اشترك</button>
                    </form>
                </div>
            </section>

            <!-- Registration Form -->
            <section id="register" class="bg-[#4A3728] rounded-3xl p-8 md:p-12 shadow-2xl relative text-white">
                <div class="text-center mb-10">
                    <h3 class="text-3xl font-bold font-cairo mb-3 text-[#C5A065]">انضم لعائلة وسم</h3>
                    <p class="text-[#eaddcf] max-w-lg mx-auto text-sm leading-relaxed">كن جزءاً من فريقنا وساهم في حفظ الهوية الأردنية للأجيال القادمة.</p>
                </div>
                <form onsubmit="handleRegister(event)" class="max-w-2xl mx-auto space-y-4 text-[#4A3728]">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <input type="text" id="reg-name" placeholder="الاسم الكامل" required class="w-full bg-white rounded-xl px-4 py-3 outline-none focus:ring-2 focus:ring-[#C5A065] text-sm">
                        <input type="number" id="reg-age" placeholder="العمر" required class="w-full bg-white rounded-xl px-4 py-3 outline-none focus:ring-2 focus:ring-[#C5A065] text-sm">
                    </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <input type="tel" id="reg-phone" placeholder="رقم الهاتف" required class="w-full bg-white rounded-xl px-4 py-3 outline-none focus:ring-2 focus:ring-[#C5A065] text-sm">
                        <select id="reg-city" class="w-full bg-white rounded-xl px-4 py-3 outline-none focus:ring-2 focus:ring-[#C5A065] text-sm"></select>
                    </div>
                    <button type="submit" class="w-full bg-[#C5A065] text-[#4A3728] font-bold py-4 rounded-xl mt-6 hover:bg-white transition-all shadow-lg flex items-center justify-center gap-2 group">
                        <span>إرسال الطلب عبر البريد</span>
                        <i data-lucide="send" class="w-4 h-4 group-hover:-translate-x-1 transition-transform"></i>
                    </button>
                </form>
            </section>

        </div>

        <!-- Footer -->
        <div class="bg-[#2e2218] text-[#eaddcf] px-8 py-12">
            <div class="flex flex-col md:flex-row justify-between items-center gap-8 text-center md:text-right">
                <div>
                    <h2 class="text-3xl font-amiri font-bold text-[#C5A065] mb-2">وســم</h2>
                    <p class="text-sm opacity-60 font-cairo">حيث يلتقي التراث بالمستقبل.</p>
                </div>
                <div class="flex flex-col items-center md:items-end gap-4">
                    <div class="flex gap-6">
                        <i data-lucide="facebook" class="w-5 h-5 hover:text-[#C5A065] cursor-pointer transition-colors"></i>
                        <i data-lucide="instagram" class="w-5 h-5 hover:text-[#C5A065] cursor-pointer transition-colors"></i>
                        <a href="mailto:altrathalardnynady@gmail.com"><i data-lucide="mail" class="w-5 h-5 hover:text-[#C5A065] cursor-pointer transition-colors"></i></a>
                    </div>
                    <a href="mailto:altrathalardnynady@gmail.com" class="text-[#C5A065] text-sm font-sans hover:text-white transition-colors">altrathalardnynady@gmail.com</a>
                    <button onclick="toggleModal('login-modal')" class="text-xs opacity-30 hover:opacity-100 transition-opacity mt-4 flex items-center gap-1"><i data-lucide="lock" class="w-3 h-3"></i> دخول الإدارة</button>
                </div>
            </div>
            <div class="border-t border-white/10 mt-8 pt-8 text-center text-[10px] opacity-40 uppercase tracking-widest font-cairo">
                © <span id="year"></span> مبادرة وسم التراث الأردني | جميع الحقوق محفوظة
            </div>
        </div>
    </div>

    <!-- JavaScript Logic -->
    <script>
        // Data
        const governorates = [
            { name: 'عمّان', desc: 'قلب الأردن النابض، تجمع بين عراقة المدرج الروماني وجبل القلعة وحداثة المدينة العصرية.', icon: 'landmark' },
            { name: 'إربد', desc: 'عروس الشمال، تشتهر بآثار أم قيس، وسهول حوران الخصبة الملقبة بسلة غذاء روما.', icon: 'wheat' },
            { name: 'البلقاء', desc: 'حاضنة السلط العريقة، تتميز ببيوتها التراثية الصفراء وجبالها المطلة على الأغوار.', icon: 'history' },
            { name: 'جرش', desc: 'مدينة الألف عمود، واحدة من أكبر المدن الرومانية وأكثرها حفاظاً على معالمها في العالم.', icon: 'users' },
            { name: 'عجلون', desc: 'رئة الأردن الخضراء، تحتضن قلعة عجلون التاريخية وغاباتها الكثيفة ومحمياتها.', icon: 'trees' },
            { name: 'المفرق', desc: 'بوابة الصحراء، موطن الكنائس القديمة، وتجمع بين البادية والآثار الأموية العريقة.', icon: 'tent' },
            { name: 'الزرقاء', desc: 'تتميز بتنوعها السكاني وقصر شبيب التاريخي ومحمية الأزرق المائية الفريدة.', icon: 'cpu' },
            { name: 'مأدبا', desc: 'مدينة الفسيفساء، تضم خارطة الأرض المقدسة وتعتبر نموذجاً عالمياً للعيش المشترك.', icon: 'palette' },
            { name: 'الكرك', desc: 'أرض الهيبة والتاريخ، تشمخ فيها قلعة الكرك الحصينة التي روت قصص البطولات.', icon: 'shield' },
            { name: 'الطفيلة', desc: 'تحتضن محمية ضانا الطبيعية الساحرة وحمامات عفرا المعدنية العلاجية.', icon: 'mountain' },
            { name: 'معان', desc: 'خزنة التاريخ، تضم البترا الوردية وصحراء وادي رم وقلعة الشوبك المهيبة.', icon: 'sparkles' },
            { name: 'العقبة', desc: 'ثغر الأردن الباسم، النافذة البحرية الوحيدة وتتميز بالشعب المرجانية والتاريخ الإسلامي.', icon: 'waves' },
        ];

        const partners = ['مؤسسة ولي العهد', 'جامعة آل البيت', 'وزارة الشباب', 'التراث الملكي الأردني', 'وزارة الثقافة', 'وزارة السياحة'];
        
        const teamMembers = [
            { name: "معاذ الخوالدة", role: "رئيس المبادرة", desc: "المشرف العام على الرؤية والتطوير الاستراتيجي." },
            { name: "أيهم الخوالدة", role: "نائب الرئيس", desc: "مدير العمليات والتنسيق التنفيذي للمبادرة." },
            { name: "سجود شديفات", role: "عضو فريق العمل", desc: "المساهمة في التنسيق والمتابعة الإدارية." },
            { name: "فرح الزيود", role: "عضو فريق العمل", desc: "المساهمة في تنظيم الأنشطة الميدانية." },
            { name: "الاء عليمات", role: "عضو فريق العمل", desc: "المساهمة في التوثيق والمحتوى الإبداعي." }
        ];

        const roadmap = [
            { title: "التأسيس والهوية", desc: "إطلاق الهوية البصرية وتشكيل الفريق الأساسي للمبادرة." },
            { title: "الانطلاق الميداني", desc: "تنفيذ أولى الجولات التعريفية في المحافظات الأردنية." },
            { title: "المنصة الرقمية", desc: "إطلاق مكتبة صور وتصاميم تراثية مفتوحة المصدر للجميع." },
            { title: "التوسع والشراكات", desc: "عقد شراكات مع الجامعات والمؤسسات الرسمية والخاصة." }
        ];

        const journeySteps = [
            { title: "التسجيل", icon: 'file-text' },
            { title: "المقابلة", icon: 'user-plus' },
            { title: "التدريب", icon: 'clipboard-check' },
            { title: "الميدان", icon: 'flag' },
            { title: "التكريم", icon: 'award' },
        ];

        const memberships = [
            { title: "عضو عام", icon: 'user', desc: "المشاركة في الفعاليات العامة.", color: "bg-[#FAF8F5]", txtColor: "text-[#4A3728]" },
            { title: "عضو متخصص", icon: 'zap', desc: "مهارات (تصميم، برمجة، بحث).", color: "bg-[#eaddcf]", txtColor: "text-[#4A3728]" },
            { title: "سفير وسم", icon: 'badge-check', desc: "قيادة الفرق في الجامعات.", color: "bg-[#C5A065]", txtColor: "text-white" },
        ];

        const museumItems = [
            { title: "الدلة والقهوة", desc: "رمز الكرم والضيافة.", icon: 'coffee' },
            { title: "المهباش", desc: "آلة طحن القهوة.", icon: 'hammer' },
            { title: "الشماغ المهدب", desc: "تاج الرأس ورمز الهوية.", icon: 'smile' },
            { title: "الثوب الأردني", desc: "تنوع في التطريز والألوان.", icon: 'shirt' },
            { title: "الربابة", desc: "صوت البادية الشجي.", icon: 'music' },
        ];

        const crafts = [
            { title: "الفسيفساء", place: "مأدبا", desc: "فن الرسم بالحجارة الملونة." },
            { title: "صناعة الصابون", place: "عجلون", desc: "منتجات طبيعية من زيت الزيتون." },
            { title: "البسط والنسيج", place: "البلقاء", desc: "حياكة يدوية بخيوط الصوف." },
            { title: "الرمل الملون", place: "العقبة/البترا", desc: "تعبئة الرمل في زجاجات فنية." },
        ];

        const dialectTerms = [
            { word: "هسّا", meaning: "الآن / في هذه اللحظة" },
            { word: "قوّطر", meaning: "ذهب أو غادر المكان" },
            { word: "دشرني", meaning: "اتركني وشأني" },
            { word: "عجاجة", meaning: "غبار كثيف أو فوضى" },
            { word: "كشرة", meaning: "ملامح الوجه الجادة" },
            { word: "التعليلة", meaning: "سهرة المساء والسمر" },
            { word: "لويش", meaning: "لماذا / لأي سبب؟" },
            { word: "يا باي", meaning: "تعبير عن التعجب والدهشة" },
        ];

        const proverbs = [
            { text: "يا ماخذ القرد على ماله، بروح المال وبضل القرد على حاله.", meaning: "لا تطمع في المال على حساب الجوهر." },
            { text: "الجار قبل الدار.", meaning: "اختر جيرانك بعناية قبل أن تختار منزلك." },
            { text: "كل شي بوقته حلو.", meaning: "الصبر جميل، والأشياء تأتي في موعدها." },
            { text: "ما بحرث الأرض غير عجولها.", meaning: "أهل البلد هم الأقدر على بنائها." },
            { text: "الجود من الموجود.", meaning: "الكرم الحقيقي هو أن تقدم ما تملك." },
        ];

        const quizData = [
            { q: "ما هي المدينة الأردنية الملقبة بمدينة الألف عمود؟", options: ["البترا", "جرش", "أم قيس"], correct: 1 },
            { q: "ما هو العنصر الذي تم إدراجه مؤخراً في اليونسكو؟", options: ["المنسف", "الدبكة", "السامر"], correct: 0 },
            { q: "في أي محافظة تقع قلعة الربض؟", options: ["الكرك", "عجلون", "معان"], correct: 1 },
            { q: "ما هي الزهرة الوطنية للأردن؟", options: ["السوسنة السوداء", "الياسمين", "الدحنون"], correct: 0 },
        ];

        let eventsList = [
            { id: 1, date: "15 آذار", title: "جولة تصوير في جبل القلعة", type: "ميداني", location: "عمّان" },
            { id: 2, date: "22 آذار", title: "ورشة التوثيق الرقمي", type: "أونلاين", location: "Zoom" },
            { id: 3, date: "5 نيسان", title: "أمسية حكايات إربد", type: "ثقافي", location: "بيت عرار" },
        ];

        // Init
        document.addEventListener('DOMContentLoaded', () => {
            // Intro Timeout
            setTimeout(() => {
                document.getElementById('intro-screen').classList.add('fade-out');
                document.getElementById('main-content').classList.remove('opacity-0');
            }, 4000);

            // Set Year
            document.getElementById('year').textContent = new Date().getFullYear();

            // Populate Dynamic Content
            renderPartners();
            renderTeam();
            renderRoadmap();
            renderGovernorates();
            renderEvents();
            renderJourney();
            renderMemberships();
            renderMuseum();
            renderCrafts();
            renderDialect();
            renderProverb();
            populateSelects();
            
            // Lucide Init
            lucide.createIcons();
        });

        // Render Functions
        function renderPartners() {
            const container = document.getElementById('partners-list');
            partners.forEach(p => {
                container.innerHTML += `
                    <div class="flex flex-col items-center gap-2 group hover:-translate-y-1 transition-transform cursor-default">
                        <div class="w-24 h-24 bg-gray-50 rounded-full flex items-center justify-center border-2 border-dashed border-[#C5A065]/30 group-hover:border-[#C5A065] transition-colors shadow-sm">
                            <span class="text-[10px] text-gray-400 text-center px-1">شعار ${p}</span>
                        </div>
                        <span class="text-sm font-bold text-[#4A3728] text-center max-w-[120px]">${p}</span>
                    </div>`;
            });
        }

        function renderTeam() {
            const container = document.getElementById('team-list');
            teamMembers.forEach(m => {
                container.innerHTML += `
                    <div class="flex-1 bg-gradient-to-l from-[#FAF8F5] to-white p-6 rounded-xl border-r-4 border-[#C5A065] shadow-sm flex items-center gap-4 hover:shadow-md transition-shadow">
                        <div class="bg-[#4A3728] text-[#C5A065] p-4 rounded-full shadow-lg"><i data-lucide="user" class="w-6 h-6"></i></div>
                        <div>
                            <h4 class="text-lg font-bold text-[#4A3728]">${m.name}</h4>
                            <div class="text-sm text-[#C5A065] font-semibold mb-1">${m.role}</div>
                            <p class="text-xs text-gray-500">${m.desc}</p>
                        </div>
                    </div>`;
            });
        }

        function renderRoadmap() {
            const container = document.getElementById('roadmap-list');
            roadmap.forEach((item, i) => {
                container.innerHTML += `
                    <div class="bg-[#FAF8F5] p-5 rounded-2xl border border-[#eaddcf] relative group">
                        <div class="absolute -top-3 right-4 w-8 h-8 rounded-full bg-[#C5A065] text-[#4A3728] flex items-center justify-center font-bold shadow-md">${i+1}</div>
                        <h4 class="font-bold text-[#4A3728] mb-2 mt-2">${item.title}</h4>
                        <p class="text-xs text-gray-500 leading-relaxed">${item.desc}</p>
                    </div>`;
            });
        }

        function renderGovernorates() {
            const container = document.getElementById('governorates-list');
            governorates.forEach(g => {
                container.innerHTML += `
                    <div class="bg-white border border-[#eaddcf] rounded-2xl p-6 hover:shadow-xl transition-all group hover:border-[#C5A065]">
                        <div class="flex items-start gap-4">
                            <div class="p-3 bg-[#4A3728]/5 text-[#4A3728] rounded-xl group-hover:bg-[#C5A065] group-hover:text-white transition-colors"><i data-lucide="${g.icon}" class="w-6 h-6"></i></div>
                            <div>
                                <h4 class="font-bold text-lg text-[#4A3728] font-cairo mb-2">${g.name}</h4>
                                <p class="text-xs text-gray-500 leading-relaxed text-justify">${g.desc}</p>
                            </div>
                        </div>
                    </div>`;
            });
        }

        function renderEvents() {
            const container = document.getElementById('events-list-display');
            const adminContainer = document.getElementById('admin-events-list');
            container.innerHTML = '';
            adminContainer.innerHTML = ''; // For Admin
            
            eventsList.forEach(e => {
                // Public View
                container.innerHTML += `
                    <div class="bg-white p-6 rounded-2xl border-r-4 border-[#C5A065] shadow-sm hover:shadow-md transition-shadow">
                        <div class="flex justify-between items-start mb-4">
                            <span class="bg-[#FAF8F5] text-[#4A3728] px-3 py-1 rounded-lg text-xs font-bold border border-[#eaddcf]">${e.type}</span>
                            <span class="text-[#C5A065] font-bold text-sm flex items-center gap-1"><i data-lucide="calendar" class="w-3 h-3"></i> ${e.date}</span>
                        </div>
                        <h4 class="font-bold text-lg text-[#4A3728] mb-2">${e.title}</h4>
                        <div class="flex items-center gap-2 text-gray-500 text-xs">
                            <i data-lucide="map-pin" class="w-3 h-3"></i><span>${e.location}</span>
                        </div>
                    </div>`;
                
                // Admin View
                adminContainer.innerHTML += `
                    <div class="bg-white p-4 rounded-xl border border-gray-200 flex justify-between items-center group">
                        <div>
                            <h4 class="font-bold text-[#4A3728]">${e.title}</h4>
                            <p class="text-xs text-gray-500">${e.date} | ${e.location}</p>
                        </div>
                        <button onclick="deleteEvent(${e.id})" class="text-red-400 p-2 hover:bg-red-50 rounded-lg transition-colors"><i data-lucide="trash-2" class="w-4 h-4"></i></button>
                    </div>`;
            });
            document.getElementById('admin-event-count').innerText = eventsList.length;
            lucide.createIcons();
        }

        function renderJourney() {
            const container = document.getElementById('journey-list');
            journeySteps.forEach(s => {
                container.innerHTML += `
                    <div class="flex flex-col items-center text-center bg-white p-4 rounded-xl border border-[#eaddcf] shadow-sm w-full md:w-32 z-10 hover:scale-105 transition-transform">
                        <div class="w-12 h-12 rounded-full bg-[#4A3728] text-[#C5A065] flex items-center justify-center mb-3 shadow-md"><i data-lucide="${s.icon}" class="w-5 h-5"></i></div>
                        <h4 class="font-bold text-[#4A3728] text-sm mb-1">${s.title}</h4>
                    </div>`;
            });
        }

        function renderMemberships() {
            const container = document.getElementById('membership-list');
            memberships.forEach(m => {
                container.innerHTML += `
                    <div class="${m.color} p-6 rounded-2xl text-center border border-[#eaddcf] transition-transform hover:-translate-y-2">
                        <div class="mx-auto w-fit p-3 rounded-full mb-4 ${m.title === "سفير وسم" ? "bg-white text-[#C5A065]" : "bg-[#4A3728]/10 text-[#4A3728]"}"><i data-lucide="${m.icon}" class="w-8 h-8"></i></div>
                        <h4 class="text-xl font-bold mb-2 font-cairo ${m.txtColor}">${m.title}</h4>
                        <p class="text-sm opacity-80 ${m.txtColor}">${m.desc}</p>
                    </div>`;
            });
        }

        function renderMuseum() {
            const container = document.getElementById('museum-list');
            museumItems.forEach(i => {
                container.innerHTML += `
                    <div class="bg-white p-4 rounded-xl border border-[#eaddcf] hover:shadow-lg transition-shadow text-center group">
                        <div class="w-16 h-16 mx-auto bg-[#4A3728]/5 rounded-full flex items-center justify-center text-[#4A3728] mb-3 group-hover:bg-[#C5A065] group-hover:text-white transition-colors"><i data-lucide="${i.icon}" class="w-8 h-8"></i></div>
                        <h4 class="font-bold text-[#4A3728] mb-1">${i.title}</h4>
                        <p class="text-xs text-gray-500 leading-tight">${i.desc}</p>
                    </div>`;
            });
        }

        function renderCrafts() {
            const container = document.getElementById('crafts-list');
            crafts.forEach(c => {
                container.innerHTML += `
                    <div class="relative bg-white rounded-2xl overflow-hidden shadow-sm border border-[#eaddcf] group">
                        <div class="h-24 bg-[#4A3728]/10 flex items-center justify-center"><i data-lucide="palette" class="w-8 h-8 text-[#4A3728] opacity-50"></i></div>
                        <div class="p-4">
                            <h4 class="font-bold text-lg text-[#4A3728]">${c.title}</h4>
                            <div class="flex items-center gap-1 text-xs text-[#C5A065] mb-2"><i data-lucide="map-pin" class="w-3 h-3"></i> ${c.place}</div>
                            <p class="text-xs text-gray-500 mb-4">${c.desc}</p>
                            <button class="w-full py-2 rounded-lg border border-[#4A3728] text-[#4A3728] text-xs font-bold hover:bg-[#4A3728] hover:text-white transition-colors">دعم الحرفي</button>
                        </div>
                    </div>`;
            });
        }

        function renderDialect() {
            const container = document.getElementById('dialect-list');
            dialectTerms.forEach(t => {
                container.innerHTML += `
                    <div class="group perspective cursor-pointer h-32">
                        <div class="relative w-full h-full duration-500 transform-style-3d group-hover:rotate-y-180 transition-transform">
                            <div class="absolute w-full h-full bg-white border border-[#eaddcf] rounded-xl flex items-center justify-center backface-hidden shadow-sm hover:border-[#C5A065] transition-colors">
                                <h4 class="text-xl font-bold text-[#4A3728] font-amiri">${t.word}</h4>
                            </div>
                            <div class="absolute w-full h-full bg-[#4A3728] text-white rounded-xl flex items-center justify-center backface-hidden rotate-y-180 px-2 text-center shadow-lg">
                                <p class="text-sm font-cairo">${t.meaning}</p>
                            </div>
                        </div>
                    </div>`;
            });
        }

        let currentProverbIdx = 0;
        function renderProverb() {
            document.getElementById('proverb-text').innerText = `"${proverbs[currentProverbIdx].text}"`;
            document.getElementById('proverb-meaning').innerText = `💡 ${proverbs[currentProverbIdx].meaning}`;
        }
        function nextProverb() {
            currentProverbIdx = (currentProverbIdx + 1) % proverbs.length;
            renderProverb();
        }
        function prevProverb() {
            currentProverbIdx = (currentProverbIdx === 0) ? proverbs.length - 1 : currentProverbIdx - 1;
            renderProverb();
        }

        function populateSelects() {
            const selects = ['reg-city', 'trip-city', 'new-event-loc']; // Added new-event-loc for generic location if needed, but it's text input in form
            // Populate Trip City select specifically
            const tripSel = document.getElementById('trip-city');
            const regSel = document.getElementById('reg-city');
            
            governorates.forEach(g => {
                const opt = `<option value="${g.name}">${g.name}</option>`;
                if(tripSel) tripSel.innerHTML += opt;
                if(regSel) regSel.innerHTML += opt;
            });
        }

        // Logic - AI Simulation
        const apiKey = ""; // Keep empty
        async function fetchGemini(prompt) {
            try {
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`, {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify({ contents: [{ parts: [{ text: prompt }] }] }),
                });
                if (!response.ok) throw new Error("API Error");
                const data = await response.json();
                return data.candidates?.[0]?.content?.parts?.[0]?.text;
            } catch (e) {
                return null;
            }
        }

        async function askHakawati(e) {
            if(e) e.preventDefault();
            const q = document.getElementById('hakawati-q').value;
            if(!q) return;
            const resDiv = document.getElementById('hakawati-res');
            resDiv.innerText = "جاري التفكير...";
            const prompt = `أنت "حكواتي وسم"، خبير في التراث الأردني. أجب بأسلوب شيق وباللهجة الأردنية البيضاء بحدود 4 أسطر على: ${q}`;
            const ans = await fetchGemini(prompt);
            resDiv.innerText = ans || "الحكواتي بياخذ استراحة، جرب كمان شوي!";
        }

        async function generateNameStory() {
            const name = document.getElementById('visitor-name').value;
            if(!name) return;
            const btn = event.target;
            btn.innerText = '...';
            const prompt = `اكتب "وسماً" تراثياً أردنياً لاسم "${name}" يربط حروف الاسم بصفات الكرم والنخوة بأسلوب أدبي. (3 أسطر)`;
            const ans = await fetchGemini(prompt);
            const resDiv = document.getElementById('name-story-result');
            resDiv.innerText = ans || "حدث خطأ بسيط..";
            resDiv.classList.remove('hidden');
            btn.innerText = 'انسج ✨';
        }

        async function generateTrip() {
            const city = document.getElementById('trip-city').value;
            if(!city) return;
            const btn = event.target;
            btn.innerText = '...';
            const prompt = `صمم برنامج رحلة يوم واحد في محافظة ${city} الأردنية يركز على التراث. نقاط مختصرة.`;
            const ans = await fetchGemini(prompt);
            const resDiv = document.getElementById('trip-result');
            resDiv.innerText = ans || "فشل التخطيط..";
            resDiv.classList.remove('hidden');
            btn.innerText = 'خطط ✨';
        }

        async function generatePoem() {
            const topic = document.getElementById('poet-topic').value;
            if(!topic) return;
            const btn = event.target;
            btn.innerText = '...';
            const prompt = `اكتب أبيات شعرية قصيرة باللهجة النبطية الأردنية عن "${topic}".`;
            const ans = await fetchGemini(prompt);
            const resDiv = document.getElementById('poet-result');
            resDiv.innerText = ans || "الشاعر يعتذر..";
            resDiv.classList.remove('hidden');
            btn.innerText = 'انظم ✨';
        }

        async function generateRecipe() {
            const ing = document.getElementById('kitchen-ing').value;
            if(!ing) return;
            const btn = event.target;
            btn.innerText = '...';
            const prompt = `اقترح طبقاً تراثياً أردنياً باستخدام: "${ing}". وصف مختصر للتحضير.`;
            const ans = await fetchGemini(prompt);
            const resDiv = document.getElementById('kitchen-result');
            resDiv.innerText = ans || "المطبخ مغلق..";
            resDiv.classList.remove('hidden');
            btn.innerText = 'اطبخ ✨';
        }

        // Logic - Quiz
        let currentQIdx = 0;
        let score = 0;
        
        function startQuiz() {
            document.getElementById('quiz-start').classList.add('hidden');
            document.getElementById('quiz-question').classList.remove('hidden');
            renderQuestion();
        }

        function renderQuestion() {
            const q = quizData[currentQIdx];
            document.getElementById('q-text').innerText = q.q;
            document.getElementById('quiz-progress').style.width = `${((currentQIdx + 1) / quizData.length) * 100}%`;
            const optsDiv = document.getElementById('q-options');
            optsDiv.innerHTML = '';
            q.options.forEach((opt, idx) => {
                optsDiv.innerHTML += `<button onclick="handleAnswer(${idx})" class="p-4 rounded-xl border-2 border-[#eaddcf] hover:bg-[#FAF8F5] hover:border-[#C5A065] text-[#4A3728] font-bold text-lg transition-all text-right px-6 flex justify-between group"><span>${opt}</span><span class="opacity-0 group-hover:opacity-100 text-[#C5A065]">👈</span></button>`;
            });
        }

        function handleAnswer(idx) {
            if (idx === quizData[currentQIdx].correct) score++;
            if (currentQIdx + 1 < quizData.length) {
                currentQIdx++;
                renderQuestion();
            } else {
                showResult();
            }
        }

        function showResult() {
            document.getElementById('quiz-question').classList.add('hidden');
            document.getElementById('quiz-result').classList.remove('hidden');
            document.getElementById('final-score').innerText = score;
            let title = "سائح جديد 📷";
            const percentage = (score / quizData.length) * 100;
            if (percentage === 100) title = "شيخ التراث 👑";
            else if (percentage >= 80) title = "مؤرخ وسم 📚";
            else if (percentage >= 50) title = "عاشق للأرض 🌱";
            document.getElementById('final-title').innerText = title;
        }

        function resetQuiz() {
            currentQIdx = 0;
            score = 0;
            document.getElementById('quiz-result').classList.add('hidden');
            document.getElementById('quiz-start').classList.remove('hidden');
        }

        // Logic - Pledge
        let pledgeCount = 1250;
        let hasPledged = false;
        function handlePledge() {
            if(!hasPledged) {
                pledgeCount++;
                hasPledged = true;
                document.getElementById('pledge-count').innerText = pledgeCount.toLocaleString();
                const btn = document.getElementById('pledge-btn');
                btn.innerHTML = `<span class="flex items-center gap-2"><i data-lucide="check" class="w-5 h-5"></i> تم التوقيع بنجاح</span>`;
                btn.classList.add('bg-[#C5A065]', 'text-[#4A3728]', 'cursor-default');
                btn.classList.remove('bg-transparent', 'border-2');
                lucide.createIcons();
            }
        }

        // Logic - Forms
        function handleRegister(e) {
            e.preventDefault();
            const name = document.getElementById('reg-name').value;
            const phone = document.getElementById('reg-phone').value;
            const msg = { id: Date.now(), name, phone, date: new Date().toLocaleDateString(), email: "example@mail.com", city: "غير محدد", track: "عام", skills: "-" };
            
            // Add to admin messages (In-memory)
            const list = document.getElementById('admin-messages-list');
            // Remove empty state if exists
            if(list.innerText.includes('لا توجد')) list.innerHTML = '';
            
            list.innerHTML = `
                <div class="bg-white p-4 rounded-xl shadow-sm border border-gray-100 flex flex-col md:flex-row justify-between gap-4">
                    <div>
                        <div class="flex items-center gap-2 mb-1"><h4 class="font-bold text-[#4A3728]">${name}</h4><span class="text-xs bg-gray-100 px-2 py-1 rounded-full text-gray-500">جديد</span></div>
                        <p class="text-sm text-gray-600 mb-1">📞 ${phone}</p>
                    </div>
                </div>` + list.innerHTML;
            
            // Update Stats
            document.getElementById('admin-msg-count').innerText = parseInt(document.getElementById('admin-msg-count').innerText) + 1;

            // Mailto
            window.location.href = `mailto:altrathalardnynady@gmail.com?subject=تسجيل جديد&body=الاسم: ${name}%0D%0Aالهاتف: ${phone}`;
            alert('تم تجهيز البريد الإلكتروني!');
        }

        function handleNewsletter(e) {
            e.preventDefault();
            const email = document.getElementById('news-email').value;
            if(email) {
                document.getElementById('admin-sub-count').innerText = parseInt(document.getElementById('admin-sub-count').innerText) + 1;
                alert('تم الاشتراك بنجاح!');
                document.getElementById('news-email').value = '';
            }
        }

        // Logic - Admin
        function toggleModal(id) {
            const el = document.getElementById(id);
            el.classList.toggle('hidden');
            el.classList.toggle('flex');
        }

        function loginAdmin() {
            const pass = document.getElementById('admin-pass').value;
            if (pass === 'admin') {
                toggleModal('login-modal');
                document.getElementById('admin-dashboard').classList.remove('hidden');
                document.getElementById('admin-dashboard').classList.add('flex');
            } else {
                alert('كلمة المرور غير صحيحة');
            }
        }

        function logoutAdmin() {
            document.getElementById('admin-dashboard').classList.add('hidden');
            document.getElementById('admin-dashboard').classList.remove('flex');
        }

        function switchAdminTab(tab) {
            ['dashboard', 'messages', 'opportunities', 'broadcast'].forEach(t => {
                document.getElementById(`admin-tab-${t}`).classList.add('hidden');
            });
            document.getElementById(`admin-tab-${tab}`).classList.remove('hidden');
        }

        function addEvent(e) {
            e.preventDefault();
            const title = document.getElementById('new-event-title').value;
            const date = document.getElementById('new-event-date').value;
            const loc = document.getElementById('new-event-loc').value;
            const type = document.getElementById('new-event-type').value;
            
            const newEv = { id: Date.now(), title, date, location: loc, type };
            eventsList.push(newEv);
            renderEvents();
            alert('تمت إضافة الفرصة!');
        }

        function deleteEvent(id) {
            eventsList = eventsList.filter(e => e.id !== id);
            renderEvents();
        }

    </script>
</body>
</html>
