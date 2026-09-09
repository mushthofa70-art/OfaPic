<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>OfaPic - Generator Bukti Pengiriman Barang</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Inter & JetBrains Mono for barcode/resi -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@500;700&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        mono: ['JetBrains Mono', 'monospace'],
                    },
                    colors: {
                        brand: {
                            50: '#eff6ff',
                            100: '#dbeafe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                            900: '#1e3a8a',
                        }
                    }
                }
            }
        }
    </script>

    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f1f5f9;
        }

        .drop-zone {
            transition: all 0.25s ease-in-out;
            border: 2px dashed #cbd5e1;
        }

        .drop-zone:hover, .drop-zone.dragover {
            border-color: #2563eb;
            background-color: #eff6ff;
        }

        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(226, 232, 240, 0.8);
        }

        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }

        /* Pulse highlight */
        @keyframes subtle-pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.02); }
        }
        .animate-subtle {
            animation: subtle-pulse 3s infinite;
        }
    </style>
</head>
<body class="text-slate-800 min-h-screen flex flex-col justify-between">

    <!-- Header Navbar -->
    <header class="bg-slate-900 text-white shadow-lg sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
            <div class="flex items-center space-x-3">
                <div class="bg-blue-600 text-white p-2.5 rounded-xl shadow-md font-extrabold flex items-center justify-center">
                    <i class="fa-solid fa-boxes-packing text-xl"></i>
                </div>
                <div>
                    <h1 class="text-xl font-bold tracking-tight text-white flex items-center gap-2">
                        OfaPic <span class="bg-blue-500/20 text-blue-300 text-xs px-2 py-0.5 rounded-full font-medium border border-blue-400/30">Pro Documentation</span>
                    </h1>
                    <p class="text-xs text-slate-400 hidden sm:block">Penggabung Foto Dokumentasi Pengiriman Barang</p>
                </div>
            </div>

            <div class="flex items-center gap-2">
                <button onclick="loadSampleData()" class="px-3 py-1.5 text-xs font-semibold bg-slate-800 hover:bg-slate-700 text-blue-300 border border-slate-700 rounded-lg transition-all flex items-center gap-1.5 shadow-sm">
                    <i class="fa-solid fa-wand-magic-sparkles text-amber-400"></i>
                    <span>Isi Contoh Data</span>
                </button>
                <button onclick="resetAll()" class="px-3 py-1.5 text-xs font-semibold bg-rose-500/10 hover:bg-rose-500/20 text-rose-400 border border-rose-500/30 rounded-lg transition-all flex items-center gap-1.5">
                    <i class="fa-solid fa-rotate-left"></i>
                    <span class="hidden sm:inline">Reset All</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content Container -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 flex-grow w-full">
        
        <!-- Hero Title Banner -->
        <div class="mb-8 text-center sm:text-left bg-gradient-to-r from-blue-900 via-indigo-900 to-slate-900 text-white p-6 sm:p-8 rounded-2xl shadow-xl relative overflow-hidden">
            <div class="absolute -right-10 -bottom-10 opacity-10 text-white pointer-events-none">
                <i class="fa-solid fa-truck-fast text-9xl"></i>
            </div>
            <div class="relative z-10 max-w-3xl">
                <div class="inline-flex items-center gap-2 bg-blue-500/20 border border-blue-400/30 px-3 py-1 rounded-full text-xs font-medium text-blue-300 mb-3">
                    <i class="fa-solid fa-shield-halved"></i> Dokumentasi Pengiriman Resmi & Rapi
                </div>
                <h2 class="text-2xl sm:text-3xl font-extrabold tracking-tight text-white mb-2">
                    📦 BUKTI PENGIRIMAN BARANG
                </h2>
                <p class="text-slate-300 text-sm sm:text-base leading-relaxed">
                    Gabungkan foto pengirim, barang, dan penerima menjadi satu dokumentasi pengiriman yang bersih, profesional, dan siap diunduh dalam format high-resolution.
                </p>
            </div>
        </div>

        <!-- Notification Toast Box Container -->
        <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2"></div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
            
            <!-- LEFT COLUMN: Step 1 (Upload Photos & Merge Button) -->
            <div class="lg:col-span-12 xl:col-span-7 flex flex-col gap-6">
                
                <!-- SECTION 1: PHOTO UPLOADS -->
                <section class="glass-card rounded-2xl p-6 shadow-sm border border-slate-200">
                    <div class="flex items-center justify-between mb-4 pb-3 border-b border-slate-100">
                        <div class="flex items-center gap-2">
                            <span class="w-7 h-7 rounded-lg bg-blue-600 text-white font-bold text-xs flex items-center justify-center shadow">1</span>
                            <h3 class="font-bold text-slate-800 text-lg">Upload Foto Dokumentasi (3 Foto)</h3>
                        </div>
                        <span class="text-xs text-slate-500 font-medium"><i class="fa-solid fa-circle-info text-blue-500"></i> JPG / PNG max 10MB</span>
                    </div>

                    <!-- 3 Upload Cards Grid -->
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                        
                        <!-- Upload Slot 1: Pengirim -->
                        <div class="flex flex-col">
                            <div class="flex items-center justify-between mb-1.5">
                                <label class="text-xs font-bold uppercase tracking-wider text-slate-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-user-tag text-blue-600"></i> 1. Pengirim
                                </label>
                                <span id="status-badge-sender" class="text-[10px] bg-slate-100 text-slate-500 px-2 py-0.5 rounded font-semibold">Kosong</span>
                            </div>

                            <div id="drop-zone-sender" class="drop-zone relative rounded-xl p-3 flex flex-col items-center justify-center bg-slate-50 min-h-[220px] text-center cursor-pointer overflow-hidden group">
                                <input type="file" id="file-sender" accept="image/*" class="absolute inset-0 opacity-0 cursor-pointer z-20" onchange="handleFileSelect(event, 'sender')">
                                
                                <div id="preview-container-sender" class="hidden absolute inset-0 w-full h-full bg-slate-900 z-10 flex items-center justify-center">
                                    <img id="img-preview-sender" class="w-full h-full object-contain" alt="Foto Pengirim">
                                    <div class="absolute top-2 right-2 flex gap-1 z-30">
                                        <button type="button" onclick="triggerFileInput('sender')" class="p-1.5 bg-slate-900/80 hover:bg-slate-900 text-white rounded-lg shadow text-xs backdrop-blur-sm" title="Ganti Foto">
                                            <i class="fa-solid fa-arrows-rotate"></i>
                                        </button>
                                        <button type="button" onclick="removeImage('sender')" class="p-1.5 bg-rose-600/90 hover:bg-rose-700 text-white rounded-lg shadow text-xs backdrop-blur-sm" title="Hapus Foto">
                                            <i class="fa-solid fa-trash"></i>
                                        </button>
                                    </div>
                                    <div class="absolute bottom-2 left-2 bg-slate-900/80 backdrop-blur-sm px-2 py-0.5 rounded text-[11px] text-white font-medium">
                                        Pengirim
                                    </div>
                                </div>

                                <div id="placeholder-sender" class="flex flex-col items-center gap-2 p-2 pointer-events-none">
                                    <div class="w-12 h-12 rounded-full bg-blue-50 text-blue-600 flex items-center justify-center text-xl group-hover:scale-110 transition-transform">
                                        <i class="fa-solid fa-user-check"></i>
                                    </div>
                                    <p class="text-xs font-semibold text-slate-700">Upload Foto Pengirim</p>
                                    <p class="text-[11px] text-slate-400">Drag & drop atau klik</p>
                                </div>
                            </div>
                        </div>

                        <!-- Upload Slot 2: Barang -->
                        <div class="flex flex-col">
                            <div class="flex items-center justify-between mb-1.5">
                                <label class="text-xs font-bold uppercase tracking-wider text-slate-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-box text-amber-600"></i> 2. Barang
                                </label>
                                <span id="status-badge-goods" class="text-[10px] bg-slate-100 text-slate-500 px-2 py-0.5 rounded font-semibold">Kosong</span>
                            </div>

                            <div id="drop-zone-goods" class="drop-zone relative rounded-xl p-3 flex flex-col items-center justify-center bg-slate-50 min-h-[220px] text-center cursor-pointer overflow-hidden group">
                                <input type="file" id="file-goods" accept="image/*" class="absolute inset-0 opacity-0 cursor-pointer z-20" onchange="handleFileSelect(event, 'goods')">
                                
                                <div id="preview-container-goods" class="hidden absolute inset-0 w-full h-full bg-slate-900 z-10 flex items-center justify-center">
                                    <img id="img-preview-goods" class="w-full h-full object-contain" alt="Foto Barang">
                                    <div class="absolute top-2 right-2 flex gap-1 z-30">
                                        <button type="button" onclick="triggerFileInput('goods')" class="p-1.5 bg-slate-900/80 hover:bg-slate-900 text-white rounded-lg shadow text-xs backdrop-blur-sm" title="Ganti Foto">
                                            <i class="fa-solid fa-arrows-rotate"></i>
                                        </button>
                                        <button type="button" onclick="removeImage('goods')" class="p-1.5 bg-rose-600/90 hover:bg-rose-700 text-white rounded-lg shadow text-xs backdrop-blur-sm" title="Hapus Foto">
                                            <i class="fa-solid fa-trash"></i>
                                        </button>
                                    </div>
                                    <div class="absolute bottom-2 left-2 bg-slate-900/80 backdrop-blur-sm px-2 py-0.5 rounded text-[11px] text-white font-medium">
                                        Barang
                                    </div>
                                </div>

                                <div id="placeholder-goods" class="flex flex-col items-center gap-2 p-2 pointer-events-none">
                                    <div class="w-12 h-12 rounded-full bg-amber-50 text-amber-600 flex items-center justify-center text-xl group-hover:scale-110 transition-transform">
                                        <i class="fa-solid fa-boxes-stacked"></i>
                                    </div>
                                    <p class="text-xs font-semibold text-slate-700">Upload Foto Barang</p>
                                    <p class="text-[11px] text-slate-400">Drag & drop atau klik</p>
                                </div>
                            </div>
                        </div>

                        <!-- Upload Slot 3: Penerima -->
                        <div class="flex flex-col">
                            <div class="flex items-center justify-between mb-1.5">
                                <label class="text-xs font-bold uppercase tracking-wider text-slate-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-hand-holding-hand text-emerald-600"></i> 3. Penerima
                                </label>
                                <span id="status-badge-receiver" class="text-[10px] bg-slate-100 text-slate-500 px-2 py-0.5 rounded font-semibold">Kosong</span>
                            </div>

                            <div id="drop-zone-receiver" class="drop-zone relative rounded-xl p-3 flex flex-col items-center justify-center bg-slate-50 min-h-[220px] text-center cursor-pointer overflow-hidden group">
                                <input type="file" id="file-receiver" accept="image/*" class="absolute inset-0 opacity-0 cursor-pointer z-20" onchange="handleFileSelect(event, 'receiver')">
                                
                                <div id="preview-container-receiver" class="hidden absolute inset-0 w-full h-full bg-slate-900 z-10 flex items-center justify-center">
                                    <img id="img-preview-receiver" class="w-full h-full object-contain" alt="Foto Penerima">
                                    <div class="absolute top-2 right-2 flex gap-1 z-30">
                                        <button type="button" onclick="triggerFileInput('receiver')" class="p-1.5 bg-slate-900/80 hover:bg-slate-900 text-white rounded-lg shadow text-xs backdrop-blur-sm" title="Ganti Foto">
                                            <i class="fa-solid fa-arrows-rotate"></i>
                                        </button>
                                        <button type="button" onclick="removeImage('receiver')" class="p-1.5 bg-rose-600/90 hover:bg-rose-700 text-white rounded-lg shadow text-xs backdrop-blur-sm" title="Hapus Foto">
                                            <i class="fa-solid fa-trash"></i>
                                        </button>
                                    </div>
                                    <div class="absolute bottom-2 left-2 bg-slate-900/80 backdrop-blur-sm px-2 py-0.5 rounded text-[11px] text-white font-medium">
                                        Penerima
                                    </div>
                                </div>

                                <div id="placeholder-receiver" class="flex flex-col items-center gap-2 p-2 pointer-events-none">
                                    <div class="w-12 h-12 rounded-full bg-emerald-50 text-emerald-600 flex items-center justify-center text-xl group-hover:scale-110 transition-transform">
                                        <i class="fa-solid fa-circle-user"></i>
                                    </div>
                                    <p class="text-xs font-semibold text-slate-700">Upload Foto Penerima</p>
                                    <p class="text-[11px] text-slate-400">Drag & drop atau klik</p>
                                </div>
                            </div>
                        </div>

                    </div>
                </section>

                <!-- ACTION BUTTON: GABUNGKAN FOTO -->
                <div class="flex flex-col sm:flex-row gap-3">
                    <button type="button" onclick="generateCombinedImage()" class="flex-1 py-4 px-6 bg-gradient-to-r from-blue-600 via-indigo-600 to-blue-700 hover:from-blue-700 hover:to-indigo-800 text-white font-bold text-base rounded-2xl shadow-lg hover:shadow-xl transition-all duration-200 transform hover:-translate-y-0.5 flex items-center justify-center gap-3">
                        <i class="fa-solid fa-layer-group text-xl"></i>
                        <span>GABUNGKAN FOTO SEKARANG</span>
                    </button>
                </div>

            </div>

            <!-- RIGHT COLUMN: Output Preview & Controls -->
            <div class="lg:col-span-12 xl:col-span-5 flex flex-col gap-6">
                
                <section class="glass-card rounded-2xl p-6 shadow-sm border border-slate-200 sticky top-24">
                    <div class="flex items-center justify-between mb-4 pb-3 border-b border-slate-100">
                        <div class="flex items-center gap-2">
                            <span class="w-7 h-7 rounded-lg bg-emerald-600 text-white font-bold text-xs flex items-center justify-center shadow">2</span>
                            <h3 class="font-bold text-slate-800 text-lg">Preview Hasil</h3>
                        </div>
                        <span class="text-xs text-slate-500 bg-slate-100 px-2 py-1 rounded font-mono">1920 × 1080 (16:9)</span>
                    </div>

                    <!-- Layout Theme Selector -->
                    <div class="mb-4 bg-slate-50 p-2.5 rounded-xl border border-slate-200 flex items-center justify-between">
                        <span class="text-xs font-semibold text-slate-600">Tema Layout:</span>
                        <select id="select-theme" onchange="generateCombinedImage(false)" class="text-xs font-medium bg-white border border-slate-300 rounded-lg px-2.5 py-1.5 focus:ring-2 focus:ring-blue-500 outline-none">
                            <option value="modern-slate" selected>Modern Navy / Slate Blue</option>
                            <option value="clean-light">Clean Professional White</option>
                            <option value="emerald-trust">Emerald Enterprise</option>
                        </select>
                    </div>

                    <!-- Image Preview Container -->
                    <div class="relative bg-slate-900 rounded-xl overflow-hidden shadow-inner border border-slate-800 aspect-video flex items-center justify-center group">
                        
                        <!-- Empty Placeholder View -->
                        <div id="preview-empty-state" class="p-6 text-center text-slate-400 flex flex-col items-center gap-3">
                            <div class="w-16 h-16 rounded-full bg-slate-800 text-slate-500 flex items-center justify-center text-2xl border border-slate-700">
                                <i class="fa-regular fa-image"></i>
                            </div>
                            <div>
                                <p class="text-sm font-semibold text-slate-300">Belum Ada Hasil</p>
                                <p class="text-xs text-slate-500 mt-1 max-w-xs">Upload 3 foto dan klik tombol <span class="text-blue-400 font-medium">"Gabungkan Foto"</span>.</p>
                            </div>
                        </div>

                        <!-- Rendered Image Output -->
                        <img id="final-result-image" class="hidden w-full h-full object-contain cursor-pointer" onclick="openFullscreenModal()" title="Klik untuk memperbesar">

                        <!-- Overlay Floating Preview Zoom Button -->
                        <button id="btn-zoom-overlay" onclick="openFullscreenModal()" class="hidden absolute top-3 right-3 bg-slate-900/80 hover:bg-slate-900 text-white p-2 rounded-lg text-xs backdrop-blur border border-white/20 transition opacity-0 group-hover:opacity-100 flex items-center gap-1.5">
                            <i class="fa-solid fa-expand"></i> Perbesar
                        </button>
                    </div>

                    <!-- Result Controls & Download Buttons -->
                    <div class="mt-5 flex flex-col gap-3">
                        <div class="grid grid-cols-2 gap-2">
                            <button id="btn-download-jpg" onclick="downloadImage('jpg')" disabled class="py-3 px-4 bg-emerald-600 hover:bg-emerald-700 disabled:bg-slate-200 disabled:text-slate-400 text-white font-bold text-xs rounded-xl transition shadow flex items-center justify-center gap-2">
                                <i class="fa-solid fa-download"></i> Download JPG
                            </button>
                            <button id="btn-download-png" onclick="downloadImage('png')" disabled class="py-3 px-4 bg-slate-800 hover:bg-slate-900 disabled:bg-slate-200 disabled:text-slate-400 text-white font-bold text-xs rounded-xl transition shadow flex items-center justify-center gap-2">
                                <i class="fa-solid fa-file-image"></i> Download PNG
                            </button>
                        </div>
                        
                        <p class="text-[11px] text-center text-slate-500 flex items-center justify-center gap-1">
                            <i class="fa-solid fa-lock text-slate-400"></i> Mempertahankan proporsi & kualitas asli foto tanpa pemotongan wajah.
                        </p>
                    </div>

                </section>

            </div>

        </div>

    </main>

    <!-- Hidden HTML5 Canvas element used for processing and merging -->
    <canvas id="export-canvas" width="1920" height="1080" class="hidden"></canvas>

    <!-- Fullscreen Preview Modal -->
    <div id="fullscreen-modal" class="fixed inset-0 bg-slate-950/90 z-50 hidden flex-col justify-between p-4 sm:p-6 backdrop-blur-md">
        <div class="flex items-center justify-between text-white max-w-7xl mx-auto w-full">
            <div class="flex items-center gap-2">
                <i class="fa-solid fa-circle-check text-emerald-400"></i>
                <span class="font-bold text-sm sm:text-base">Pratinjau Hasil Dokumentasi OfaPic</span>
            </div>
            <button onclick="closeFullscreenModal()" class="p-2 bg-slate-800 hover:bg-slate-700 text-slate-300 hover:text-white rounded-lg transition">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>
        </div>

        <div class="flex-grow flex items-center justify-center my-4 overflow-hidden">
            <img id="modal-image" class="max-w-full max-h-full object-contain rounded-xl shadow-2xl border border-slate-800" alt="Full Result Preview">
        </div>

        <div class="flex items-center justify-center gap-3 max-w-7xl mx-auto w-full">
            <button onclick="downloadImage('jpg'); closeFullscreenModal();" class="py-2.5 px-6 bg-emerald-600 hover:bg-emerald-700 text-white font-bold text-sm rounded-xl transition flex items-center gap-2 shadow-lg">
                <i class="fa-solid fa-download"></i> Unduh Sekarang (JPG)
            </button>
            <button onclick="closeFullscreenModal()" class="py-2.5 px-5 bg-slate-800 hover:bg-slate-700 text-slate-300 font-semibold text-sm rounded-xl transition">
                Tutup Pratinjau
            </button>
        </div>
    </div>

    <footer class="bg-slate-900 text-slate-400 text-xs py-6 border-t border-slate-800 mt-12">
        <div class="max-w-7xl mx-auto px-4 text-center flex flex-col sm:flex-row items-center justify-between gap-3">
            <p>© 2026 <strong>OfaPic</strong>. Hak Cipta Dilindungi. Aplikasi Penggabung Dokumen Pengiriman.</p>
            <div class="flex items-center gap-4 text-slate-500">
                <span class="flex items-center gap-1"><i class="fa-solid fa-bolt text-amber-400"></i> Render 1920x1080 Full HD</span>
                <span>•</span>
                <span class="flex items-center gap-1"><i class="fa-solid fa-shield-check text-blue-400"></i> Privacy Safe (Browser Local Processing)</span>
            </div>
        </div>
    </footer>

    <script>
        // State storage for uploaded image base64 or Image objects
        const imagesState = {
            sender: null,   // { file, url, imgObject }
            goods: null,
            receiver: null
        };

        // Initialize defaults on window load
        window.onload = function() {
            // Setup drag and drop listeners
            setupDragAndDrop('sender');
            setupDragAndDrop('goods');
            setupDragAndDrop('receiver');
        };

        // Drag and Drop Helper setup
        function setupDragAndDrop(type) {
            const dropZone = document.getElementById(`drop-zone-${type}`);

            ['dragenter', 'dragover'].forEach(eventName => {
                dropZone.addEventListener(eventName, (e) => {
                    e.preventDefault();
                    e.stopPropagation();
                    dropZone.classList.add('dragover');
                }, false);
            });

            ['dragleave', 'drop'].forEach(eventName => {
                dropZone.addEventListener(eventName, (e) => {
                    e.preventDefault();
                    e.stopPropagation();
                    dropZone.classList.remove('dragover');
                }, false);
            });

            dropZone.addEventListener('drop', (e) => {
                const dt = e.dataTransfer;
                const files = dt.files;
                if (files && files.length > 0) {
                    processFile(files[0], type);
                }
            });
        }

        // Trigger hidden file input click
        function triggerFileInput(type) {
            document.getElementById(`file-${type}`).click();
        }

        // Handle file change event
        function handleFileSelect(event, type) {
            const files = event.target.files;
            if (files && files.length > 0) {
                processFile(files[0], type);
            }
        }

        // Read and process image file
        function processFile(file, type) {
            if (!file.type.startsWith('image/')) {
                showToast('Format file harus berupa gambar (JPG, PNG, WebP)', 'error');
                return;
            }

            const reader = new FileReader();
            reader.onload = function(e) {
                const imgUrl = e.target.result;
                const img = new Image();
                img.onload = function() {
                    imagesState[type] = {
                        file: file,
                        url: imgUrl,
                        imgObject: img
                    };

                    // Update UI Preview
                    document.getElementById(`img-preview-${type}`).src = imgUrl;
                    document.getElementById(`preview-container-${type}`).classList.remove('hidden');
                    document.getElementById(`placeholder-${type}`).classList.add('hidden');
                    
                    // Update Status Badge
                    const badge = document.getElementById(`status-badge-${type}`);
                    badge.innerText = 'Tersedia';
                    badge.className = 'text-[10px] bg-emerald-100 text-emerald-700 px-2 py-0.5 rounded font-semibold';

                    showToast(`Foto ${getLabelTitle(type)} berhasil diunggah`, 'success');
                };
                img.src = imgUrl;
            };
            reader.readAsDataURL(file);
        }

        // Remove photo from slot
        function removeImage(type) {
            imagesState[type] = null;
            document.getElementById(`file-${type}`).value = '';
            document.getElementById(`preview-container-${type}`).classList.add('hidden');
            document.getElementById(`placeholder-${type}`).classList.remove('hidden');
            
            const badge = document.getElementById(`status-badge-${type}`);
            badge.innerText = 'Kosong';
            badge.className = 'text-[10px] bg-slate-100 text-slate-500 px-2 py-0.5 rounded font-semibold';

            showToast(`Foto ${getLabelTitle(type)} telah dihapus`, 'info');
        }

        function getLabelTitle(type) {
            if (type === 'sender') return 'Pengirim';
            if (type === 'goods') return 'Barang';
            if (type === 'receiver') return 'Penerima';
            return '';
        }

        // Toast notification helper
        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            
            let bgClass = 'bg-slate-900 text-white';
            let iconClass = 'fa-circle-info text-blue-400';
            
            if (type === 'success') {
                bgClass = 'bg-slate-900 text-white border-l-4 border-emerald-500';
                iconClass = 'fa-circle-check text-emerald-400';
            } else if (type === 'error') {
                bgClass = 'bg-slate-900 text-white border-l-4 border-rose-500';
                iconClass = 'fa-circle-xmark text-rose-400';
            }

            toast.className = `${bgClass} shadow-xl rounded-lg p-3 text-xs font-medium flex items-center gap-2 backdrop-blur transform transition-all duration-300 translate-y-2 opacity-0`;
            toast.innerHTML = `<i class="fa-solid ${iconClass} text-sm"></i> <span>${message}</span>`;
            
            container.appendChild(toast);

            setTimeout(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            }, 50);

            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        // Main function to render composite canvas
        function generateCombinedImage(showNotification = true) {
            // Check if all 3 photos are uploaded
            if (!imagesState.sender || !imagesState.goods || !imagesState.receiver) {
                showToast('Mohon lengkapi ketiga foto (Pengirim, Barang, Penerima) terlebih dahulu!', 'error');
                return;
            }

            const canvas = document.getElementById('export-canvas');
            const ctx = canvas.getContext('2d');
            
            // Fixed 16:9 canvas HD dimensions
            const W = 1920;
            const H = 1080;
            canvas.width = W;
            canvas.height = H;

            // Formatted Date & Time (Automatic)
            const now = new Date();
            const dateStr = now.toLocaleDateString('id-ID', { day: '2-digit', month: '2-digit', year: 'numeric' });
            const timeStr = now.toLocaleTimeString('id-ID', { hour: '2-digit', minute: '2-digit' });
            const timestamp = `${dateStr} • ${timeStr} WIB`;

            // Selected Theme Colors
            const theme = document.getElementById('select-theme').value;
            let bgColor = '#f8fafc';
            let headerBg = '#0f172a';
            let headerText = '#ffffff';
            let accentColor = '#2563eb';
            let cardBg = '#ffffff';
            let cardBorder = '#cbd5e1';

            if (theme === 'clean-light') {
                bgColor = '#ffffff';
                headerBg = '#1e293b';
                accentColor = '#0284c7';
            } else if (theme === 'emerald-trust') {
                bgColor = '#f0fdf4';
                headerBg = '#064e3b';
                accentColor = '#059669';
            }

            // 1. Draw Overall Canvas Background
            ctx.fillStyle = bgColor;
            ctx.fillRect(0, 0, W, H);

            // Subtle background grid lines
            ctx.strokeStyle = '#e2e8f0';
            ctx.lineWidth = 1;
            for (let x = 0; x < W; x += 60) {
                ctx.beginPath();
                ctx.moveTo(x, 0);
                ctx.lineTo(x, H);
                ctx.stroke();
            }

            // 2. HEADER BANNER
            const headerHeight = 140;
            ctx.fillStyle = headerBg;
            ctx.fillRect(0, 0, W, headerHeight);

            // Header Accent Line
            ctx.fillStyle = accentColor;
            ctx.fillRect(0, headerHeight - 6, W, 6);

            // Header Title Text
            ctx.fillStyle = headerText;
            ctx.font = 'bold 42px Inter, sans-serif';
            ctx.fillText('📦 BUKTI PENGIRIMAN BARANG', 60, 65);

            ctx.fillStyle = '#94a3b8';
            ctx.font = '500 22px Inter, sans-serif';
            ctx.fillText('DOKUMENTASI RESMI DOKUMEN PENGIRIMAN', 60, 105);

            // Header Timestamp Badge Box (Right side)
            ctx.fillStyle = '#1e293b';
            roundRect(ctx, W - 520, 30, 460, 80, 14, true, false);
            
            ctx.fillStyle = '#94a3b8';
            ctx.font = 'bold 14px Inter, sans-serif';
            ctx.fillText('WAKTU DOKUMENTASI', W - 490, 56);

            ctx.fillStyle = '#38bdf8';
            ctx.font = 'bold 22px "JetBrains Mono", monospace';
            ctx.fillText(timestamp, W - 490, 88);

            // 3. MIDDLE AREA: 3 LARGE PHOTO CARDS
            const startY = 175;
            const cardWidth = 570;
            const cardHeight = 820;
            const gap = 35;
            const startX = 60;

            const photoSlots = [
                { type: 'sender', label: '1. PENGIRIM', data: imagesState.sender.imgObject },
                { type: 'goods', label: '2. BARANG', data: imagesState.goods.imgObject },
                { type: 'receiver', label: '3. PENERIMA', data: imagesState.receiver.imgObject }
            ];

            photoSlots.forEach((slot, index) => {
                const x = startX + index * (cardWidth + gap);
                const y = startY;

                // Card Shadow & Container
                ctx.save();
                ctx.shadowColor = 'rgba(15, 23, 42, 0.08)';
                ctx.shadowBlur = 18;
                ctx.shadowOffsetY = 8;
                ctx.fillStyle = cardBg;
                roundRect(ctx, x, y, cardWidth, cardHeight, 18, true, false);
                ctx.restore();

                // Card Border
                ctx.strokeStyle = cardBorder;
                ctx.lineWidth = 2;
                roundRect(ctx, x, y, cardWidth, cardHeight, 18, false, true);

                // Card Label Header Ribbon
                ctx.fillStyle = accentColor;
                roundRectCustom(ctx, x, y, cardWidth, 54, { tl: 18, tr: 18, br: 0, bl: 0 }, true, false);

                ctx.fillStyle = '#ffffff';
                ctx.font = 'bold 22px Inter, sans-serif';
                ctx.fillText(slot.label, x + 24, y + 35);

                // Draw Image Frame inside card
                const imgFrameX = x + 16;
                const imgFrameY = y + 70;
                const imgFrameW = cardWidth - 32;
                const imgFrameH = cardHeight - 86;

                // Inner Dark Frame Background for photo containment
                ctx.fillStyle = '#0f172a';
                roundRect(ctx, imgFrameX, imgFrameY, imgFrameW, imgFrameH, 12, true, false);

                // Draw Photo with Object-Fit Contain (no distortion, no cropping)
                drawImageContain(ctx, slot.data, imgFrameX, imgFrameY, imgFrameW, imgFrameH);
            });

            // 4. FOOTER BAR
            ctx.fillStyle = '#0f172a';
            ctx.fillRect(0, H - 45, W, 45);

            ctx.fillStyle = '#94a3b8';
            ctx.font = '500 15px Inter, sans-serif';
            ctx.fillText('OfaPic Digital Documentation System • Generated automatically without photo editing', 60, H - 18);

            ctx.fillStyle = '#38bdf8';
            ctx.font = 'bold 15px Inter, sans-serif';
            ctx.fillText('STATUS: BARANG TELAH DITERIMA', W - 360, H - 18);

            // 5. Update Output Image Preview in DOM
            const dataUrl = canvas.toDataURL('image/jpeg', 0.95);
            const previewImg = document.getElementById('final-result-image');
            previewImg.src = dataUrl;
            previewImg.classList.remove('hidden');

            document.getElementById('preview-empty-state').classList.add('hidden');
            document.getElementById('btn-zoom-overlay').classList.remove('hidden');
            
            // Enable Download Buttons
            document.getElementById('btn-download-jpg').disabled = false;
            document.getElementById('btn-download-png').disabled = false;

            if (showNotification) {
                showToast('Gambar dokumentasi berhasil digabungkan!', 'success');
            }
        }

        // Draw image containing aspect ratio inside designated box
        function drawImageContain(ctx, img, x, y, w, h) {
            const imgRatio = img.width / img.height;
            const containerRatio = w / h;

            let renderW, renderH, renderX, renderY;

            if (imgRatio > containerRatio) {
                renderW = w;
                renderH = w / imgRatio;
                renderX = x;
                renderY = y + (h - renderH) / 2;
            } else {
                renderH = h;
                renderW = h * imgRatio;
                renderY = y;
                renderX = x + (w - renderW) / 2;
            }

            ctx.drawImage(img, renderX, renderY, renderW, renderH);
        }

        // Rounded Rectangle Helper with all corners
        function roundRect(ctx, x, y, width, height, radius, fill, stroke) {
            ctx.beginPath();
            ctx.moveTo(x + radius, y);
            ctx.lineTo(x + width - radius, y);
            ctx.quadraticCurveTo(x + width, y, x + width, y + radius);
            ctx.lineTo(x + width, y + height - radius);
            ctx.quadraticCurveTo(x + width, y + height, x + width - radius, y + height);
            ctx.lineTo(x + radius, y + height);
            ctx.quadraticCurveTo(x, y + height, x, y + height - radius);
            ctx.lineTo(x, y + radius);
            ctx.quadraticCurveTo(x, y, x + radius, y);
            ctx.closePath();
            if (fill) ctx.fill();
            if (stroke) ctx.stroke();
        }

        // Custom Rounded Rectangle Helper with individual corners
        function roundRectCustom(ctx, x, y, width, height, radii, fill, stroke) {
            const tl = radii.tl || 0;
            const tr = radii.tr || 0;
            const br = radii.br || 0;
            const bl = radii.bl || 0;

            ctx.beginPath();
            ctx.moveTo(x + tl, y);
            ctx.lineTo(x + width - tr, y);
            ctx.quadraticCurveTo(x + width, y, x + width, y + tr);
            ctx.lineTo(x + width, y + height - br);
            ctx.quadraticCurveTo(x + width, y + height, x + width - br, y + height);
            ctx.lineTo(x + bl, y + height);
            ctx.quadraticCurveTo(x, y + height, x, y + height - bl);
            ctx.lineTo(x, y + tl);
            ctx.quadraticCurveTo(x, y, x + tl, y);
            ctx.closePath();
            if (fill) ctx.fill();
            if (stroke) ctx.stroke();
        }

        // Download Image File
        function downloadImage(format = 'jpg') {
            const canvas = document.getElementById('export-canvas');
            
            let mimeType = 'image/jpeg';
            let filename = `Bukti_Pengiriman_OfaPic.jpg`;

            if (format === 'png') {
                mimeType = 'image/png';
                filename = `Bukti_Pengiriman_OfaPic.png`;
            }

            const link = document.createElement('a');
            link.download = filename;
            link.href = canvas.toDataURL(mimeType, 0.95);
            link.click();

            showToast(`Gambar (${format.toUpperCase()}) berhasil diunduh!`, 'success');
        }

        // Fullscreen Modal handlers
        function openFullscreenModal() {
            const canvas = document.getElementById('export-canvas');
            const modal = document.getElementById('fullscreen-modal');
            const modalImg = document.getElementById('modal-image');

            if (!imagesState.sender || !imagesState.goods || !imagesState.receiver) {
                return;
            }

            modalImg.src = canvas.toDataURL('image/jpeg', 0.95);
            modal.classList.remove('hidden');
            modal.classList.add('flex');
        }

        function closeFullscreenModal() {
            const modal = document.getElementById('fullscreen-modal');
            modal.classList.add('hidden');
            modal.classList.remove('flex');
        }

        // Reset All state and input fields
        function resetAll() {
            ['sender', 'goods', 'receiver'].forEach(type => {
                removeImage(type);
            });

            // Clear Preview output
            document.getElementById('final-result-image').classList.add('hidden');
            document.getElementById('final-result-image').src = '';
            document.getElementById('preview-empty-state').classList.remove('hidden');
            document.getElementById('btn-zoom-overlay').classList.add('hidden');

            document.getElementById('btn-download-jpg').disabled = true;
            document.getElementById('btn-download-png').disabled = true;

            showToast('Semua foto telah di-reset', 'info');
        }

        // Load Sample Data (For Instant Demo & Testing)
        function loadSampleData() {
            // Generate Canvas Placeholders for 3 photos
            createSampleImage('sender', 'PENGIRIM', '#2563eb', '👤 Kurir Pengirim');
            createSampleImage('goods', 'BARANG', '#d97706', '📦 Paket Barang');
            createSampleImage('receiver', 'PENERIMA', '#059669', '🤝 Penerima Paket');

            setTimeout(() => {
                generateCombinedImage(true);
            }, 300);
        }

        // Generate synthetic canvas image for sample demonstration
        function createSampleImage(type, title, color, text) {
            const canvas = document.createElement('canvas');
            canvas.width = 600;
            canvas.height = 800;
            const ctx = canvas.getContext('2d');

            // Soft Gradient
            const grad = ctx.createLinearGradient(0, 0, 600, 800);
            grad.addColorStop(0, '#1e293b');
            grad.addColorStop(1, '#0f172a');
            ctx.fillStyle = grad;
            ctx.fillRect(0, 0, 600, 800);

            // Inner circle badge
            ctx.fillStyle = color;
            ctx.beginPath();
            ctx.arc(300, 360, 120, 0, Math.PI * 2);
            ctx.fill();

            // Text
            ctx.fillStyle = '#ffffff';
            ctx.font = 'bold 38px Inter, sans-serif';
            ctx.textAlign = 'center';
            ctx.fillText(text, 300, 375);

            ctx.fillStyle = '#94a3b8';
            ctx.font = 'bold 24px Inter, sans-serif';
            ctx.fillText(`DOKUMEN SAMPLE (${title})`, 300, 560);

            ctx.fillStyle = '#64748b';
            ctx.font = '18px Inter, sans-serif';
            ctx.fillText('OfaPic Auto Generator Demo', 300, 600);

            const imgUrl = canvas.toDataURL('image/png');
            const img = new Image();
            img.onload = function() {
                imagesState[type] = {
                    file: null,
                    url: imgUrl,
                    imgObject: img
                };

                document.getElementById(`img-preview-${type}`).src = imgUrl;
                document.getElementById(`preview-container-${type}`).classList.remove('hidden');
                document.getElementById(`placeholder-${type}`).classList.add('hidden');

                const badge = document.getElementById(`status-badge-${type}`);
                badge.innerText = 'Tersedia';
                badge.className = 'text-[10px] bg-emerald-100 text-emerald-700 px-2 py-0.5 rounded font-semibold';
            };
            img.src = imgUrl;
        }
    </script>
</body>
</html>
