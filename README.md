# yangjyi417.github.io
theme: Nyota.top
[Nyota互动H5设计需求-996d4c62fe.html](https://github.com/user-attachments/files/24397043/Nyota.H5.-996d4c62fe.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nyota - 自在放空，'慢'游生活</title>
    <script src="https://cdn.tailwindcss.com/3.3.3"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css">
    <style>
        /* 全局样式 */
        body {
            font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif;
            background-color: #F9FAFB;
            color: #333;
            overflow-x: hidden;
            touch-action: manipulation;
        }
        
        /* 主色调定义 */
        :root {
            --primary-color: #E0C3FC;
            --secondary-color: #B5E4CA;
            --accent-color: #A5B4FC;
            --text-color: #4B5563;
            --light-bg: #F9FAFB;
        }
        
        /* 自定义字体 */
        .title-font {
            font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif;
            font-weight: 600;
            letter-spacing: -0.025em;
        }
        
        .body-font {
            font-family: 'PingFang SC', 'Microsoft YaHei', sans-serif;
            font-weight: 400;
            line-height: 1.6;
        }
        
        /* 云朵动画 */
        .cloud {
            position: absolute;
            background-color: white;
            border-radius: 50%;
            filter: blur(15px);
            opacity: 0.8;
            animation: float 15s infinite ease-in-out;
            z-index: -1;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0) translateX(0); }
            25% { transform: translateY(-20px) translateX(10px); }
            50% { transform: translateY(10px) translateX(-10px); }
            75% { transform: translateY(-10px) translateX(5px); }
        }
        
        /* 按钮样式 */
        .nyota-btn {
            background: linear-gradient(135deg, var(--primary-color), var(--accent-color));
            border: none;
            border-radius: 25px;
            padding: 12px 24px;
            color: white;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
            position: relative;
            overflow: hidden;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            min-width: 160px;
        }
        
        .nyota-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
        }
        
        .nyota-btn:active {
            transform: scale(0.95);
        }
        
        .nyota-btn:disabled {
            opacity: 0.7;
            cursor: not-allowed;
            transform: none;
        }
        
        /* 图片样式 */
        .nyota-image {
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            transition: transform 0.3s ease;
        }
        
        .nyota-image:hover {
            transform: translateY(-5px);
        }
        
        /* 页面过渡 */
        .page {
            opacity: 0;
            transform: translateY(20px);
            transition: opacity 0.6s ease, transform 0.6s ease;
        }
        
        .page.active {
            opacity: 1;
            transform: translateY(0);
        }
        
        /* Toast提示 */
        .toast {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 12px 24px;
            border-radius: 25px;
            z-index: 1000;
            opacity: 0;
            transition: opacity 0.3s ease;
            pointer-events: none;
        }
        
        .toast.show {
            opacity: 1;
        }
        
        /* 弹窗样式 */
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
        }
        
        .modal.active {
            opacity: 1;
            visibility: visible;
        }
        
        .modal-content {
            background-color: white;
            border-radius: 20px;
            padding: 24px;
            width: 80%;
            max-width: 400px;
            transform: scale(0.9);
            transition: transform 0.3s ease;
        }
        
        .modal.active .modal-content {
            transform: scale(1);
        }
        
        /* 云朵对话框 */
        .cloud-dialog {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0.9);
            width: 75%;
            background-color: white;
            border-radius: 20px;
            padding: 24px;
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.15);
            z-index: 1001;
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }
        
        .cloud-dialog.active {
            opacity: 1;
            visibility: visible;
            transform: translate(-50%, -50%) scale(1);
        }
        
        .cloud-dialog::before {
            content: '';
            position: absolute;
            bottom: -15px;
            left: 50%;
            transform: translateX(-50%);
            width: 30px;
            height: 15px;
            background-color: white;
            border-radius: 0 0 15px 15px;
        }
        
        /* 加载动画 */
        .loader {
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top: 3px solid white;
            width: 20px;
            height: 20px;
            animation: spin 1s linear infinite;
            margin-left: 8px;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        /* 文字逐字动画 */
        .typewriter-text {
            overflow: hidden;
            border-right: .15em solid var(--accent-color);
            white-space: nowrap;
            margin: 0 auto;
            animation: 
                typing 3.5s steps(40, end),
                blink-caret .75s step-end infinite;
        }
        
        @keyframes typing {
            from { width: 0 }
            to { width: 100% }
        }
        
        @keyframes blink-caret {
            from, to { border-color: transparent }
            50% { border-color: var(--accent-color); }
        }
        
        /* 星星扩散动画 */
        .star-burst {
            position: fixed;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            background: radial-gradient(circle, rgba(255,255,255,0.8) 0%, rgba(255,255,255,0) 70%);
            border-radius: 50%;
            transform: translate(-50%, -50%);
            z-index: 999;
            opacity: 0;
            transition: all 0.5s ease;
        }
        
        .star-burst.active {
            width: 300px;
            height: 300px;
            opacity: 1;
        }
        
        /* 背景模糊效果 */
        .blur-bg {
            filter: blur(8px);
            transition: filter 0.3s ease;
        }
        
        /* 悬浮元素 */
        .floating-element {
            animation: float 6s infinite ease-in-out;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-15px); }
        }
        
        /* 手办热区 */
        .figure-hotspot {
            position: absolute;
            border-radius: 50%;
            background-color: rgba(224, 195, 252, 0.3);
            transition: all 0.3s ease;
            cursor: pointer;
        }
        
        .figure-hotspot:hover {
            background-color: rgba(224, 195, 252, 0.6);
            transform: scale(1.1);
        }
    </style>
</head>
<body class="min-h-screen">
    <!-- 云朵背景 -->
    <div class="cloud w-64 h-32 top-20 left-10" style="animation-delay: 0s;"></div>
    <div class="cloud w-80 h-40 top-40 right-10" style="animation-delay: 2s;"></div>
    <div class="cloud w-72 h-36 bottom-20 left-20" style="animation-delay: 4s;"></div>
    <div class="cloud w-60 h-30 bottom-40 right-20" style="animation-delay: 6s;"></div>
    
    <!-- 星星扩散动画元素 -->
    <div id="starBurst" class="star-burst"></div>
    
    <!-- Toast提示 -->
    <div id="toast" class="toast">正在前往抽盒机</div>
    
    <!-- 失败弹窗 -->
    <div id="errorModal" class="modal">
        <div class="modal-content">
            <h3 class="text-xl font-bold mb-4 text-center">打开失败</h3>
            <p class="text-gray-600 mb-6 text-center">请复制链接到微信打开</p>
            <div class="flex justify-center">
                <button id="copyLinkBtn" class="nyota-btn">
                    复制链接
                </button>
            </div>
        </div>
    </div>
    
    <!-- 小剧场对话框 -->
    <div id="dialog" class="cloud-dialog">
        <p id="dialogText" class="text-center text-gray-700 mb-4"></p>
        <div class="flex justify-center">
            <button id="closeDialogBtn" class="nyota-btn">
                关闭
            </button>
        </div>
    </div>
    
    <!-- 页面容器 -->
    <div class="container mx-auto px-4 py-8">
        <!-- 首页/慢时光捕捉器 -->
        <section id="home" class="page active min-h-screen flex flex-col items-center justify-center">
            <!-- 封面图 -->
            <div class="relative w-full max-w-md mb-8">
                <img id="coverImage" src="photo-1766997809757.jpg" alt="Nyota封面图" class="nyota-image w-full">
                <!-- 封面图热区 -->
                <div id="swingNyotaHotspot" class="figure-hotspot w-32 h-32 top-1/4 left-1/2 transform -translate-x-1/2"></div>
            </div>
            
            <h1 class="title-font text-3xl md:text-4xl text-center mb-4">自在放空，'慢'游生活</h1>
            <p class="body-font text-gray-600 text-center mb-8 max-w-md">在快节奏中寻找小确幸，与Nyota一起捕捉生活中的美好瞬间</p>
            
            <!-- 功能区 -->
            <div class="w-full max-w-md bg-white rounded-20 p-6 shadow-lg mb-8">
                <h2 class="title-font text-xl mb-4 text-center">慢时光捕捉器</h2>
                
                <!-- 图片上传区 -->
                <div class="border-2 border-dashed border-gray-300 rounded-xl p-6 text-center mb-6">
                    <div class="flex flex-col items-center">
                        <img id="penNyotaIcon" src="photo-72.jpg" alt="执笔Nyota" class="nyota-image w-20 h-20 mb-4 cursor-pointer">
                        <p class="body-font text-gray-500 mb-2">上传一张日常照片</p>
                        <p class="body-font text-sm text-gray-400">咖啡杯、窗台绿植、天空...</p>
                        <input type="file" id="photoUpload" class="hidden" accept="image/*">
                    </div>
                </div>
                
                <!-- 预览区 -->
                <div id="previewContainer" class="hidden mb-6">
                    <h3 class="title-font text-lg mb-2 text-center">Nyota风格插画</h3>
                    <div class="relative">
                        <img id="previewImage" src="" alt="Nyota风格插画" class="nyota-image w-full">
                        <!-- 水印 -->
                        <div class="absolute bottom-2 right-2 text-xs text-gray-400">Nyota IP</div>
                    </div>
                </div>
                
                <!-- 按钮区 -->
                <div class="flex flex-col items-center space-y-4">
                    <button id="generateBtn" class="nyota-btn hidden">
                        生成Nyota风格插画
                    </button>
                    <button id="shareBtn" class="nyota-btn hidden">
                        <i class="fas fa-share-alt mr-2"></i> 分享到社交平台
                    </button>
                    <button id="toStarWishBtn" class="nyota-btn">
                        探索星愿盲盒
                    </button>
                </div>
            </div>
            
            <!-- 带走Nyota盲盒按钮 -->
            <button id="takeNyotaBtn1" class="nyota-btn">
                <span id="btnText1">带走Nyota盲盒</span>
                <span id="btnLoader1" class="loader hidden"></span>
            </button>
        </section>
        
        <!-- 星愿盲盒互动页 -->
        <section id="starWish" class="page min-h-screen hidden flex flex-col items-center justify-center">
            <h1 class="title-font text-3xl md:text-4xl text-center mb-4">星愿盲盒互动</h1>
            <p class="body-font text-gray-600 text-center mb-8 max-w-md">点击云朵，解锁属于你的治愈故事</p>
            
            <!-- 盲盒互动区 -->
            <div class="relative w-full max-w-md h-80 bg-gradient-to-b from-blue-50 to-purple-50 rounded-20 p-6 shadow-lg mb-8 flex items-center justify-center">
                <!-- 悬浮云朵 -->
                <div id="floatingCloud" class="floating-element cursor-pointer">
                    <div class="w-32 h-20 bg-white rounded-full flex items-center justify-center shadow-lg">
                        <i class="fas fa-gift text-3xl text-purple-400"></i>
                    </div>
                </div>
                
                <!-- 故事内容区 -->
                <div id="storyContainer" class="hidden absolute inset-0 flex flex-col items-center justify-center p-6">
                    <h3 id="storyTitle" class="title-font text-xl mb-4 text-center"></h3>
                    <p id="storyContent" class="body-font text-gray-600 text-center mb-6"></p>
                    <button id="nextStoryBtn" class="nyota-btn">
                        继续
                    </button>
                </div>
                
                <!-- 星愿签区 -->
                <div id="starWishContainer" class="hidden absolute inset-0 flex flex-col items-center justify-center p-6">
                    <div class="bg-white rounded-20 p-6 shadow-lg w-full max-w-xs text-center">
                        <h3 class="title-font text-xl mb-4 text-purple-600">专属星愿签</h3>
                        <p id="starWishText" class="body-font text-gray-700 mb-6 text-lg"></p>
                        <button id="takeNyotaBtn2" class="nyota-btn w-full">
                            <span id="btnText2">带走Nyota盲盒</span>
                            <span id="btnLoader2" class="loader hidden"></span>
                        </button>
                    </div>
                </div>
            </div>
            
            <!-- 导航按钮 -->
            <div class="flex space-x-4">
                <button id="backToHomeBtn" class="nyota-btn">
                    <i class="fas fa-arrow-left mr-2"></i> 返回首页
                </button>
                <button id="toGuideBtn" class="nyota-btn">
                    探索放空指南 <i class="fas fa-arrow-right ml-2"></i>
                </button>
            </div>
        </section>
        
        <!-- 放空指南页 -->
        <section id="guide" class="page min-h-screen hidden">
            <h1 class="title-font text-3xl md:text-4xl text-center mb-8">放空指南</h1>
            
            <!-- 全家福手办图 -->
            <div class="relative w-full max-w-md mx-auto mb-12">
                <img id="familyImage" src="photo-1766997836005.jpg" alt="Nyota全家福" class="nyota-image w-full">
                <!-- 角色热区 -->
                <div id="starNyotaHotspot" class="figure-hotspot w-24 h-24 top-1/4 left-1/4"></div>
                <div id="umbrellaNyotaHotspot" class="figure-hotspot w-24 h-24 top-1/3 right-1/4"></div>
                <div id="lampNyotaHotspot" class="figure-hotspot w-24 h-24 bottom-1/4 left-1/3"></div>
            </div>
            
            <!-- 场景化内容 -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-12">
                <!-- 晨间放空 -->
                <div class="bg-white rounded-20 p-6 shadow-lg">
                    <h2 class="title-font text-xl mb-4 text-center">晨间放空</h2>
                    <div class="w-full h-40 bg-gradient-to-r from-pink-50 to-purple-50 rounded-xl mb-4 flex items-center justify-center">
                        <i class="fas fa-sun text-4xl text-yellow-400"></i>
                    </div>
                    <p class="body-font text-gray-600 text-center">跟随Nyota一起做窗台瑜伽，唤醒活力满满的一天</p>
                </div>
                
                <!-- 通勤放空 -->
                <div class="bg-white rounded-20 p-6 shadow-lg">
                    <h2 class="title-font text-xl mb-4 text-center">通勤放空</h2>
                    <div class="w-full h-40 bg-gradient-to-r from-blue-50 to-cyan-50 rounded-xl mb-4 flex items-center justify-center">
                        <i class="fas fa-train text-4xl text-blue-400"></i>
                    </div>
                    <p class="body-font text-gray-600 text-center">地铁场景下的"云朵形状观察指南"，让通勤不再枯燥</p>
                </div>
                
                <!-- 睡前放空 -->
                <div class="bg-white rounded-20 p-6 shadow-lg">
                    <h2 class="title-font text-xl mb-4 text-center">睡前放空</h2>
                    <div class="w-full h-40 bg-gradient-to-r from-indigo-50 to-purple-50 rounded-xl mb-4 flex items-center justify-center">
                        <i class="fas fa-moon text-4xl text-indigo-400"></i>
                    </div>
                    <p class="body-font text-gray-600 text-center">记录3件小美好，让每一天都有值得回味的瞬间</p>
                </div>
            </div>
            
            <!-- 带走Nyota盲盒按钮 -->
            <div class="flex justify-center">
                <button id="takeNyotaBtn3" class="nyota-btn">
                    <span id="btnText3">带走Nyota盲盒</span>
                    <span id="btnLoader3" class="loader hidden"></span>
                </button>
            </div>
            
            <!-- 返回按钮 -->
            <div class="flex justify-center mt-8">
                <button id="backToStarWishBtn" class="nyota-btn">
                    <i class="fas fa-arrow-left mr-2"></i> 返回星愿盲盒
                </button>
            </div>
        </section>
    </div>
    
    <script>
        // 页面元素
        const pages = {
            home: document.getElementById('home'),
            starWish: document.getElementById('starWish'),
            guide: document.getElementById('guide')
        };
        
        // 导航按钮
        const toStarWishBtn = document.getElementById('toStarWishBtn');
        const backToHomeBtn = document.getElementById('backToHomeBtn');
        const toGuideBtn = document.getElementById('toGuideBtn');
        const backToStarWishBtn = document.getElementById('backToStarWishBtn');
        
        // 小程序跳转按钮
        const takeNyotaBtns = [
            { btn: document.getElementById('takeNyotaBtn1'), text: document.getElementById('btnText1'), loader: document.getElementById('btnLoader1') },
            { btn: document.getElementById('takeNyotaBtn2'), text: document.getElementById('btnText2'), loader: document.getElementById('btnLoader2') },
            { btn: document.getElementById('takeNyotaBtn3'), text: document.getElementById('btnText3'), loader: document.getElementById('btnLoader3') }
        ];
        
        // Toast和弹窗
        const toast = document.getElementById('toast');
        const errorModal = document.getElementById('errorModal');
        const copyLinkBtn = document.getElementById('copyLinkBtn');
        
        // 图片上传相关
        const photoUpload = document.getElementById('photoUpload');
        const penNyotaIcon = document.getElementById('penNyotaIcon');
        const generateBtn = document.getElementById('generateBtn');
        const previewContainer = document.getElementById('previewContainer');
        const previewImage = document.getElementById('previewImage');
        const shareBtn = document.getElementById('shareBtn');
        
        // 星愿盲盒相关
        const floatingCloud = document.getElementById('floatingCloud');
        const storyContainer = document.getElementById('storyContainer');
        const storyTitle = document.getElementById('storyTitle');
        const storyContent = document.getElementById('storyContent');
        const nextStoryBtn = document.getElementById('nextStoryBtn');
        const starWishContainer = document.getElementById('starWishContainer');
        const starWishText = document.getElementById('starWishText');
        
        // 小剧场相关
        const starBurst = document.getElementById('starBurst');
        const dialog = document.getElementById('dialog');
        const dialogText = document.getElementById('dialogText');
        const closeDialogBtn = document.getElementById('closeDialogBtn');
        
        // 角色热区
        const swingNyotaHotspot = document.getElementById('swingNyotaHotspot');
        const starNyotaHotspot = document.getElementById('starNyotaHotspot');
        const umbrellaNyotaHotspot = document.getElementById('umbrellaNyotaHotspot');
        const lampNyotaHotspot = document.getElementById('lampNyotaHotspot');
        
        // 小程序链接
        const miniProgramLink = '#小程序://泡泡抽盒机/pIYOBMbHpHSHIAJ';
        
        // 故事内容
        const stories = [
            {
                title: '《窗台的约定》',
                content: 'Nyota在窗台遇见了小猫Lumi，它们成为了最好的朋友，一起度过了无数个温暖的午后。'
            },
            {
                title: '《受伤的小鸟》',
                content: '一只受伤的小鸟落在了Nyota的窗台，她温柔地照料它，直到它恢复健康重新飞向天空。'
            },
            {
                title: '《刘海修剪日记》',
                content: 'Nyota决定自己修剪刘海，虽然过程有点笨拙，但最终的成果让她感到满意，这就是生活中的小确幸。'
            },
            {
                title: '《蒲公英的约定》',
                content: 'Nyota种下了一颗蒲公英种子，每天耐心地浇水等待，终于看到它绽放，然后带着她的愿望飞向远方。'
            }
        ];
        
        // 星愿签内容
        const starWishes = [
            '今天允许自己发10分钟呆',
            '给窗台的植物取个名字吧',
            '抬头看看天空，发现不一样的美好',
            '给自己一个温暖的微笑',
            '记录下今天的小确幸'
        ];
        
        // 小剧场内容
        const dialogs = {
            swingNyota: '今天的星星好像格外明亮呢...要不要一起数云朵？',
            penNyota: '把今天的小确幸写下来吧，说不定会变成星星哦~',
            starNyota: '每颗星星都在守护一个梦想',
            umbrellaNyota: '雨天也没关系，听听雨滴的旋律吧',
            lampNyota: '深夜的灵感，比咖啡更提神呢'
        };
        
        // 页面切换函数
        function switchPage(pageName) {
            // 隐藏所有页面
            Object.values(pages).forEach(page => {
                page.classList.remove('active');
                page.classList.add('hidden');
            });
            
            // 显示目标页面
            pages[pageName].classList.remove('hidden');
            setTimeout(() => {
                pages[pageName].classList.add('active');
            }, 50);
        }
        
        // 显示Toast函数
        function showToast(message, duration = 1500) {
            toast.textContent = message;
            toast.classList.add('show');
            
            setTimeout(() => {
                toast.classList.remove('show');
            }, duration);
        }
        
        // 显示弹窗函数
        function showModal(modal) {
            modal.classList.add('active');
            document.body.classList.add('blur-bg');
        }
        
        // 隐藏弹窗函数
        function hideModal(modal) {
            modal.classList.remove('active');
            document.body.classList.remove('blur-bg');
        }
        
        // 显示小剧场函数
        function showDialog(message) {
            // 触发星星扩散动画
            starBurst.classList.add('active');
            
            setTimeout(() => {
                // 显示对话框
                dialogText.textContent = message;
                dialog.classList.add('active');
                document.body.classList.add('blur-bg');
                
                // 移除星星扩散动画
                setTimeout(() => {
                    starBurst.classList.remove('active');
                }, 500);
            }, 300);
        }
        
        // 隐藏小剧场函数
        function hideDialog() {
            dialog.classList.remove('active');
            document.body.classList.remove('blur-bg');
        }
        
        // 小程序跳转函数
        function navigateToMiniProgram(btn, text, loader) {
            // 显示加载状态
            btn.disabled = true;
            text.textContent = '正在前往';
            loader.classList.remove('hidden');
            
            // 显示Toast
            showToast('正在前往抽盒机', 1500);
            
            // 尝试跳转
            setTimeout(() => {
                try {
                    // 模拟跳转行为
                    window.location.href = miniProgramLink;
                    
                    // 如果跳转失败（通常是在非微信环境），显示错误弹窗
                    setTimeout(() => {
                        // 恢复按钮状态
                        btn.disabled = false;
                        text.textContent = '带走Nyota盲盒';
                        loader.classList.add('hidden');
                        
                        // 显示错误弹窗
                        showModal(errorModal);
                    }, 1000);
                } catch (error) {
                    // 恢复按钮状态
                    btn.disabled = false;
                    text.textContent = '带走Nyota盲盒';
                    loader.classList.add('hidden');
                    
                    // 显示错误弹窗
                    showModal(errorModal);
                }
            }, 1500);
        }
        
        // 复制链接函数
        function copyToClipboard(text) {
            navigator.clipboard.writeText(text)
                .then(() => {
                    showToast('链接已复制');
                    hideModal(errorModal);
                })
                .catch(err => {
                    console.error('无法复制链接: ', err);
                    showToast('复制失败，请手动复制');
                });
        }
        
        // 生成Nyota风格插画函数
        function generateNyotaStyle() {
            // 模拟生成过程
            generateBtn.disabled = true;
            generateBtn.innerHTML = '<i class="fas fa-spinner fa-spin mr-2"></i> 生成中...';
            
            setTimeout(() => {
                // 显示预览图（使用原图作为示例）
                previewImage.src = URL.createObjectURL(photoUpload.files[0]);
                previewContainer.classList.remove('hidden');
                shareBtn.classList.remove('hidden');
                
                // 恢复按钮状态
                generateBtn.disabled = false;
                generateBtn.innerHTML = '重新生成';
            }, 2000);
        }
        
        // 随机选择故事
        function selectRandomStory() {
            const randomIndex = Math.floor(Math.random() * stories.length);
            return stories[randomIndex];
        }
        
        // 随机选择星愿签
        function selectRandomStarWish() {
            const randomIndex = Math.floor(Math.random() * starWishes.length);
            return starWishes[randomIndex];
        }
        
        // 事件监听器
        document.addEventListener('DOMContentLoaded', function() {
            // 页面导航
            toStarWishBtn.addEventListener('click', () => switchPage('starWish'));
            backToHomeBtn.addEventListener('click', () => switchPage('home'));
            toGuideBtn.addEventListener('click', () => switchPage('guide'));
            backToStarWishBtn.addEventListener('click', () => switchPage('starWish'));
            
            // 小程序跳转按钮
            takeNyotaBtns.forEach(({ btn, text, loader }) => {
                btn.addEventListener('click', () => navigateToMiniProgram(btn, text, loader));
            });
            
            // 错误弹窗
            copyLinkBtn.addEventListener('click', () => copyToClipboard(miniProgramLink));
            
            // 点击弹窗外部关闭弹窗
            errorModal.addEventListener('click', (e) => {
                if (e.target === errorModal) {
                    hideModal(errorModal);
                }
            });
            
            // 图片上传
            penNyotaIcon.addEventListener('click', () => photoUpload.click());
            
            photoUpload.addEventListener('change', function() {
                if (this.files && this.files[0]) {
                    generateBtn.classList.remove('hidden');
                }
            });
            
            generateBtn.addEventListener('click', generateNyotaStyle);
            
            // 星愿盲盒互动
            floatingCloud.addEventListener('click', function() {
                // 隐藏云朵，显示故事
                this.classList.add('hidden');
                const story = selectRandomStory();
                storyTitle.textContent = story.title;
                storyContent.textContent = story.content;
                storyContainer.classList.remove('hidden');
            });
            
            nextStoryBtn.addEventListener('click', function() {
                // 隐藏故事，显示星愿签
                storyContainer.classList.add('hidden');
                starWishText.textContent = selectRandomStarWish();
                starWishContainer.classList.remove('hidden');
            });
            
            // 小剧场交互
            closeDialogBtn.addEventListener('click', hideDialog);
            
            // 点击对话框外部关闭对话框
            dialog.addEventListener('click', (e) => {
                if (e.target === dialog) {
                    hideDialog();
                }
            });
            
            // 角色热区交互
            swingNyotaHotspot.addEventListener('click', () => showDialog(dialogs.swingNyota));
            starNyotaHotspot.addEventListener('click', () => showDialog(dialogs.starNyota));
            umbrellaNyotaHotspot.addEventListener('click', () => showDialog(dialogs.umbrellaNyota));
            lampNyotaHotspot.addEventListener('click', () => showDialog(dialogs.lampNyota));
            penNyotaIcon.addEventListener('click', function(e) {
                // 如果不是因为上传图片触发的点击，则显示小剧场
                if (!e.target.closest('input')) {
                    showDialog(dialogs.penNyota);
                }
            });
        });
    </script>
</body>
</html>
