<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Panpraow Waste System - ระบบจัดการขยะโรงเรียนพานพร้าว</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        body {
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        .gradient-bg {
            background: linear-gradient(135deg, #059669 0%, #10b981 50%, #34d399 100%);
        }
        .gradient-bg-secondary {
            background: linear-gradient(135deg, #065f46 0%, #047857 50%, #059669 100%);
        }
        .card-shadow {
            box-shadow: 0 10px 25px rgba(16, 185, 129, 0.15);
        }
        .connection-status {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
        }
        .online { 
            background: linear-gradient(135deg, #10b981, #34d399); 
            color: white; 
            box-shadow: 0 4px 15px rgba(16, 185, 129, 0.3);
        }
        .offline { 
            background: linear-gradient(135deg, #ef4444, #f87171); 
            color: white; 
            box-shadow: 0 4px 15px rgba(239, 68, 68, 0.3);
        }
        .print-only { display: none; }
        @media print {
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            body { background: white !important; }
        }
        .waste-icon {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            color: white;
            margin-bottom: 10px;
        }
        .green-glow {
            box-shadow: 0 0 20px rgba(16, 185, 129, 0.3);
        }
        .animate-pulse-green {
            animation: pulse-green 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
        @keyframes pulse-green {
            0%, 100% {
                opacity: 1;
            }
            50% {
                opacity: .7;
            }
        }
        .eco-pattern {
            background-image: 
                radial-gradient(circle at 25% 25%, rgba(16, 185, 129, 0.1) 0%, transparent 50%),
                radial-gradient(circle at 75% 75%, rgba(52, 211, 153, 0.1) 0%, transparent 50%);
        }
    </style>
</head>
<body class="bg-gradient-to-br from-green-50 to-emerald-100 min-h-full eco-pattern">
    <!-- Connection Status -->
    <div id="connectionStatus" class="connection-status offline animate-pulse-green">
        <i class="fas fa-wifi"></i> ออฟไลน์
    </div>

    <!-- Header -->
    <header class="gradient-bg text-white shadow-xl no-print">
        <div class="container mx-auto px-4 py-8">
            <div class="flex items-center justify-between">
                <div class="flex items-center space-x-6">
                    <div class="bg-white bg-opacity-20 p-4 rounded-full green-glow">
                        <i class="fas fa-leaf text-5xl text-white"></i>
                    </div>
                    <div>
                        <h1 class="text-4xl font-bold mb-2">Panpraow Waste System</h1>
                        <p class="text-green-100 text-lg">ระบบจัดการขยะโรงเรียนพานพร้าว</p>
                        <p class="text-green-200 text-sm">🌱 เพื่อสิ่งแวดล้อมที่ยั่งยืน</p>
                    </div>
                </div>
            </div>
        </div>
    </header>

    <!-- Navigation -->
    <nav class="bg-white shadow-lg sticky top-0 z-50 no-print border-b-4 border-green-500">
        <div class="container mx-auto px-4">
            <div class="flex space-x-8">
                <button onclick="showTab('record')" class="nav-btn py-4 px-6 border-b-4 border-green-500 text-green-600 font-semibold">
                    <i class="fas fa-plus-circle mr-2"></i>บันทึกขยะ
                </button>
                <button onclick="showTab('dashboard')" class="nav-btn py-4 px-6 text-gray-600 hover:text-green-600 hover:border-green-300 border-b-4 border-transparent transition-all">
                    <i class="fas fa-chart-pie mr-2"></i>แดชบอร์ด
                </button>
                <button onclick="showTab('leaderboard')" class="nav-btn py-4 px-6 text-gray-600 hover:text-green-600 hover:border-green-300 border-b-4 border-transparent transition-all">
                    <i class="fas fa-trophy mr-2"></i>กระดานคะแนน
                </button>
                <button onclick="showTab('history')" class="nav-btn py-4 px-6 text-gray-600 hover:text-green-600 hover:border-green-300 border-b-4 border-transparent transition-all">
                    <i class="fas fa-history mr-2"></i>ประวัติ
                </button>
                <button onclick="showTab('config')" class="nav-btn py-4 px-6 text-gray-600 hover:text-green-600 hover:border-green-300 border-b-4 border-transparent transition-all">
                    <i class="fas fa-cog mr-2"></i>ตั้งค่า
                </button>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <main class="container mx-auto px-4 py-8">
        <!-- Record Tab -->
        <div id="recordTab" class="tab-content">
            <div class="bg-white rounded-2xl shadow-xl p-8 card-shadow border border-green-100">
                <div class="text-center mb-8">
                    <div class="inline-flex items-center justify-center w-20 h-20 bg-gradient-to-br from-green-500 to-emerald-600 rounded-full mb-4 green-glow">
                        <i class="fas fa-plus-circle text-3xl text-white"></i>
                    </div>
                    <h2 class="text-3xl font-bold text-gray-800 mb-2">บันทึกข้อมูลขยะ</h2>
                    <p class="text-green-600">ร่วมกันดูแลสิ่งแวดล้อม เพื่อโลกที่ยั่งยืน</p>
                </div>
                
                <form id="wasteForm" class="space-y-6">
                    <div class="grid md:grid-cols-2 gap-6">
                        <div>
                            <label for="recorderName" class="block text-sm font-semibold text-gray-700 mb-3">
                                <i class="fas fa-user text-green-500 mr-2"></i>ชื่อผู้บันทึก
                            </label>
                            <input type="text" id="recorderName" required 
                                   class="w-full px-4 py-3 border-2 border-green-200 rounded-xl focus:ring-4 focus:ring-green-200 focus:border-green-500 transition-all">
                        </div>
                        
                        <div>
                            <label for="wasteType" class="block text-sm font-semibold text-gray-700 mb-3">
                                <i class="fas fa-recycle text-green-500 mr-2"></i>ประเภทขยะ
                            </label>
                            <select id="wasteType" required 
                                    class="w-full px-4 py-3 border-2 border-green-200 rounded-xl focus:ring-4 focus:ring-green-200 focus:border-green-500 transition-all">
                                <option value="">เลือกประเภทขยะ</option>
                                <option value="wet">🟢 ขยะเปียก (ถังสีเขียว) - 20 แต้ม/กก.</option>
                                <option value="general">🔵 ขยะทั่วไป (ถังสีฟ้า) - 10 แต้ม/กก.</option>
                                <option value="recycle">🟡 ขยะรีไซเคิล (ถังสีเหลือง) - 30 แต้ม/กก.</option>
                                <option value="hazardous">🔴 ขยะอันตราย (ถังสีแดง) - 40 แต้ม/กก.</option>
                            </select>
                        </div>
                    </div>
                    
                    <div class="grid md:grid-cols-2 gap-6">
                        <div>
                            <label for="weight" class="block text-sm font-semibold text-gray-700 mb-3">
                                <i class="fas fa-weight text-green-500 mr-2"></i>น้ำหนัก (กิโลกรัม)
                            </label>
                            <input type="number" id="weight" step="0.1" min="0.1" required 
                                   class="w-full px-4 py-3 border-2 border-green-200 rounded-xl focus:ring-4 focus:ring-green-200 focus:border-green-500 transition-all">
                        </div>
                        
                        <div>
                            <label for="recordDate" class="block text-sm font-semibold text-gray-700 mb-3">
                                <i class="fas fa-calendar text-green-500 mr-2"></i>วันที่บันทึก
                            </label>
                            <input type="date" id="recordDate" required 
                                   class="w-full px-4 py-3 border-2 border-green-200 rounded-xl focus:ring-4 focus:ring-green-200 focus:border-green-500 transition-all">
                        </div>
                    </div>
                    
                    <div>
                        <label for="notes" class="block text-sm font-semibold text-gray-700 mb-3">
                            <i class="fas fa-sticky-note text-green-500 mr-2"></i>หมายเหตุ (ไม่บังคับ)
                        </label>
                        <textarea id="notes" rows="3" 
                                  class="w-full px-4 py-3 border-2 border-green-200 rounded-xl focus:ring-4 focus:ring-green-200 focus:border-green-500 transition-all"></textarea>
                    </div>
                    
                    <button type="submit" 
                            class="w-full bg-gradient-to-r from-green-500 via-emerald-500 to-green-600 text-white py-4 px-6 rounded-xl hover:from-green-600 hover:via-emerald-600 hover:to-green-700 transition-all duration-300 font-semibold text-lg shadow-lg hover:shadow-xl green-glow">
                        <i class="fas fa-save mr-3"></i>บันทึกข้อมูล
                    </button>
                </form>
            </div>
        </div>

        <!-- Dashboard Tab -->
        <div id="dashboardTab" class="tab-content hidden">
            <div class="grid lg:grid-cols-2 gap-8 mb-8">
                <!-- Chart Section -->
                <div class="bg-white rounded-2xl shadow-xl p-6 card-shadow border border-green-100">
                    <div class="flex justify-between items-center mb-6">
                        <h3 class="text-2xl font-bold text-gray-800">
                            <i class="fas fa-chart-pie text-green-500 mr-3"></i>สัดส่วนขยะตามประเภท
                        </h3>
                        <select id="chartPeriod" onchange="updateChart()" 
                                class="px-4 py-2 border-2 border-green-200 rounded-lg text-sm focus:ring-2 focus:ring-green-200">
                            <option value="week">สัปดาห์นี้</option>
                            <option value="month">เดือนนี้</option>
                            <option value="year">ปีนี้</option>
                        </select>
                    </div>
                    <div class="relative h-80">
                        <canvas id="wasteChart"></canvas>
                    </div>
                </div>

                <!-- Stats Cards -->
                <div class="space-y-4">
                    <div class="bg-gradient-to-br from-green-500 to-emerald-600 text-white rounded-2xl p-6 card-shadow green-glow">
                        <div class="flex items-center justify-between">
                            <div>
                                <p class="text-green-100 font-medium">ขยะเปียก</p>
                                <p class="text-3xl font-bold" id="wetWasteTotal">0 กก.</p>
                                <p class="text-green-200 text-sm">♻️ ย่อยสลายได้</p>
                            </div>
                            <div class="waste-icon bg-green-400">
                                <i class="fas fa-leaf"></i>
                            </div>
                        </div>
                    </div>
                    
                    <div class="bg-gradient-to-br from-blue-500 to-cyan-600 text-white rounded-2xl p-6 card-shadow">
                        <div class="flex items-center justify-between">
                            <div>
                                <p class="text-blue-100 font-medium">ขยะทั่วไป</p>
                                <p class="text-3xl font-bold" id="generalWasteTotal">0 กก.</p>
                                <p class="text-blue-200 text-sm">🗑️ ขยะทั่วไป</p>
                            </div>
                            <div class="waste-icon bg-blue-400">
                                <i class="fas fa-trash"></i>
                            </div>
                        </div>
                    </div>
                    
                    <div class="bg-gradient-to-br from-yellow-500 to-orange-500 text-white rounded-2xl p-6 card-shadow">
                        <div class="flex items-center justify-between">
                            <div>
                                <p class="text-yellow-100 font-medium">ขยะรีไซเคิล</p>
                                <p class="text-3xl font-bold" id="recycleWasteTotal">0 กก.</p>
                                <p class="text-yellow-200 text-sm">♻️ นำกลับมาใช้ได้</p>
                            </div>
                            <div class="waste-icon bg-yellow-400">
                                <i class="fas fa-recycle"></i>
                            </div>
                        </div>
                    </div>
                    
                    <div class="bg-gradient-to-br from-red-500 to-pink-600 text-white rounded-2xl p-6 card-shadow">
                        <div class="flex items-center justify-between">
                            <div>
                                <p class="text-red-100 font-medium">ขยะอันตราย</p>
                                <p class="text-3xl font-bold" id="hazardousWasteTotal">0 กก.</p>
                                <p class="text-red-200 text-sm">⚠️ ต้องระวัง</p>
                            </div>
                            <div class="waste-icon bg-red-400">
                                <i class="fas fa-exclamation-triangle"></i>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Leaderboard Tab -->
        <div id="leaderboardTab" class="tab-content hidden">
            <div class="bg-white rounded-2xl shadow-xl p-8 card-shadow border border-green-100">
                <div class="text-center mb-8">
                    <div class="inline-flex items-center justify-center w-20 h-20 bg-gradient-to-br from-yellow-500 to-orange-500 rounded-full mb-4">
                        <i class="fas fa-trophy text-3xl text-white"></i>
                    </div>
                    <h2 class="text-3xl font-bold text-gray-800 mb-2">กระดานคะแนนผู้ทิ้งขยะ</h2>
                    <p class="text-green-600">🏆 ผู้นำด้านการดูแลสิ่งแวดล้อม</p>
                </div>
                <div id="leaderboardList" class="space-y-4">
                    <!-- Leaderboard items will be populated here -->
                </div>
            </div>
        </div>

        <!-- History Tab -->
        <div id="historyTab" class="tab-content hidden">
            <div class="bg-white rounded-2xl shadow-xl p-8 card-shadow border border-green-100">
                <div class="flex justify-between items-center mb-8">
                    <div>
                        <h2 class="text-3xl font-bold text-gray-800 mb-2">
                            <i class="fas fa-history text-green-500 mr-3"></i>ประวัติการทิ้งขยะ
                        </h2>
                        <p class="text-green-600">📊 ติดตามข้อมูลย้อนหลัง</p>
                    </div>
                    <div class="flex space-x-4">
                        <select id="historyPeriod" onchange="loadHistory()" 
                                class="px-4 py-2 border-2 border-green-200 rounded-lg focus:ring-2 focus:ring-green-200">
                            <option value="week">สัปดาห์นี้</option>
                            <option value="month">เดือนนี้</option>
                            <option value="year">ปีนี้</option>
                        </select>
                        <input type="month" id="historyMonth" onchange="loadHistory()" 
                               class="px-4 py-2 border-2 border-green-200 rounded-lg focus:ring-2 focus:ring-green-200">
                    </div>
                </div>
                <div id="historyList" class="space-y-4">
                    <!-- History items will be populated here -->
                </div>
            </div>
        </div>

        <!-- Config Tab -->
        <div id="configTab" class="tab-content hidden">
            <div class="bg-white rounded-2xl shadow-xl p-8 card-shadow border border-green-100">
                <div class="text-center mb-8">
                    <div class="inline-flex items-center justify-center w-20 h-20 bg-gradient-to-br from-gray-500 to-gray-600 rounded-full mb-4">
                        <i class="fas fa-cog text-3xl text-white"></i>
                    </div>
                    <h2 class="text-3xl font-bold text-gray-800 mb-2">ตั้งค่าระบบ</h2>
                    <p class="text-green-600">⚙️ ปรับแต่งการให้คะแนน</p>
                </div>
                
                <div class="space-y-8">
                    <div>
                        <h3 class="text-xl font-semibold text-gray-700 mb-6 flex items-center">
                            <i class="fas fa-star text-yellow-500 mr-3"></i>ตั้งค่าคะแนนตามประเภทขยะ
                        </h3>
                        <div class="grid md:grid-cols-2 gap-6">
                            <div class="bg-blue-50 p-4 rounded-xl border-2 border-blue-200">
                                <label class="block text-sm font-semibold text-gray-700 mb-3">
                                    🔵 ขยะทั่วไป (แต้ม/กก.)
                                </label>
                                <input type="number" id="generalPoints" value="10" min="1" 
                                       class="w-full px-4 py-3 border-2 border-blue-200 rounded-lg focus:ring-2 focus:ring-blue-200">
                            </div>
                            <div class="bg-green-50 p-4 rounded-xl border-2 border-green-200">
                                <label class="block text-sm font-semibold text-gray-700 mb-3">
                                    🟢 ขยะเปียก (แต้ม/กก.)
                                </label>
                                <input type="number" id="wetPoints" value="20" min="1" 
                                       class="w-full px-4 py-3 border-2 border-green-200 rounded-lg focus:ring-2 focus:ring-green-200">
                            </div>
                            <div class="bg-yellow-50 p-4 rounded-xl border-2 border-yellow-200">
                                <label class="block text-sm font-semibold text-gray-700 mb-3">
                                    🟡 ขยะรีไซเคิล (แต้ม/กก.)
                                </label>
                                <input type="number" id="recyclePoints" value="30" min="1" 
                                       class="w-full px-4 py-3 border-2 border-yellow-200 rounded-lg focus:ring-2 focus:ring-yellow-200">
                            </div>
                            <div class="bg-red-50 p-4 rounded-xl border-2 border-red-200">
                                <label class="block text-sm font-semibold text-gray-700 mb-3">
                                    🔴 ขยะอันตราย (แต้ม/กก.)
                                </label>
                                <input type="number" id="hazardousPoints" value="40" min="1" 
                                       class="w-full px-4 py-3 border-2 border-red-200 rounded-lg focus:ring-2 focus:ring-red-200">
                            </div>
                        </div>
                    </div>
                    
                    <!-- Password Section for Config -->
                    <div class="bg-gradient-to-r from-orange-50 to-red-50 p-6 rounded-2xl border-2 border-orange-200">
                        <h3 class="text-lg font-semibold text-gray-700 mb-4 flex items-center">
                            <i class="fas fa-shield-alt text-orange-500 mr-3"></i>ยืนยันตัวตนเพื่อบันทึกการตั้งค่า
                        </h3>
                        <div class="mb-4">
                            <label for="configPassword" class="block text-sm font-semibold text-gray-700 mb-3">
                                <i class="fas fa-key text-orange-500 mr-2"></i>รหัสผ่านสำหรับแก้ไขการตั้งค่า
                            </label>
                            <input type="password" id="configPassword" placeholder="กรอกรหัสผ่าน" 
                                   class="w-full px-4 py-3 border-2 border-orange-200 rounded-xl focus:ring-4 focus:ring-orange-200 focus:border-orange-500 transition-all">
                        </div>
                        <p class="text-sm text-orange-600 bg-orange-100 p-3 rounded-lg">
                            <i class="fas fa-info-circle mr-2"></i>
                            จำเป็นต้องใส่รหัสผ่านที่ถูกต้องเพื่อบันทึกการเปลี่ยนแปลงการตั้งค่า
                        </p>
                    </div>
                    
                    <button onclick="saveConfig()" 
                            class="w-full bg-gradient-to-r from-green-500 to-emerald-600 text-white px-6 py-4 rounded-xl hover:from-green-600 hover:to-emerald-700 transition-all duration-300 font-semibold shadow-lg">
                        <i class="fas fa-save mr-3"></i>บันทึกการตั้งค่า
                    </button>
                </div>
            </div>
        </div>
    </main>

    <!-- Edit/Delete Modal -->
    <div id="editModal" class="fixed inset-0 bg-black bg-opacity-50 hidden items-center justify-center z-50 no-print">
        <div class="bg-white rounded-2xl p-8 max-w-md w-full mx-4 shadow-2xl border border-green-100">
            <div class="text-center mb-6">
                <div class="inline-flex items-center justify-center w-16 h-16 bg-gradient-to-br from-orange-500 to-red-500 rounded-full mb-4">
                    <i class="fas fa-lock text-2xl text-white"></i>
                </div>
                <h3 class="text-2xl font-bold text-gray-800">แก้ไข/ลบข้อมูล</h3>
                <p class="text-gray-600 mt-2">กรุณาใส่รหัสผ่านเพื่อดำเนินการ</p>
            </div>
            
            <div class="mb-6">
                <label class="block text-sm font-semibold text-gray-700 mb-3">
                    <i class="fas fa-key text-orange-500 mr-2"></i>รหัสผ่านสำหรับแก้ไข
                </label>
                <input type="password" id="adminPassword" placeholder="กรอกรหัสผ่าน" 
                       class="w-full px-4 py-3 border-2 border-gray-200 rounded-xl focus:ring-4 focus:ring-orange-200 focus:border-orange-500">
            </div>
            
            <div class="flex space-x-3">
                <button onclick="confirmEdit()" 
                        class="flex-1 bg-gradient-to-r from-blue-500 to-blue-600 text-white py-3 px-4 rounded-xl hover:from-blue-600 hover:to-blue-700 transition-all">
                    <i class="fas fa-edit mr-2"></i>แก้ไข
                </button>
                <button onclick="confirmDelete()" 
                        class="flex-1 bg-gradient-to-r from-red-500 to-red-600 text-white py-3 px-4 rounded-xl hover:from-red-600 hover:to-red-700 transition-all">
                    <i class="fas fa-trash mr-2"></i>ลบ
                </button>
                <button onclick="closeModal()" 
                        class="flex-1 bg-gradient-to-r from-gray-500 to-gray-600 text-white py-3 px-4 rounded-xl hover:from-gray-600 hover:to-gray-700 transition-all">
                    ยกเลิก
                </button>
            </div>
        </div>
    </div>

    <!-- Print Report Section -->
    <div class="print-only">
        <div class="text-center mb-8">
            <h1 class="text-4xl font-bold text-green-800 mb-2">รายงานระบบจัดการขยะ</h1>
            <h2 class="text-2xl text-green-600 mb-2">โรงเรียนพานพร้าว</h2>
            <p class="text-gray-600">🌱 เพื่อสิ่งแวดล้อมที่ยั่งยืน</p>
            <p class="text-gray-600 mt-2">วันที่พิมพ์: <span id="printDate"></span></p>
        </div>
        
        <div id="printContent">
            <!-- Print content will be populated here -->
        </div>
    </div>

    <!-- Firebase SDK -->
    <script type="module">
        import { initializeApp } from 'https://www.gstatic.com/firebasejs/9.23.0/firebase-app.js';
        import { getDatabase, ref, push, set, onValue, remove, update, onDisconnect, serverTimestamp } from 'https://www.gstatic.com/firebasejs/9.23.0/firebase-database.js';

        // Firebase configuration
        const firebaseConfig = {
            apiKey: "AIzaSyCsKL6P3bg2sRrkJsxDEvBXuH38_3G3-N0",
            authDomain: "waste-system-c7e54.firebaseapp.com",
            databaseURL: "https://waste-system-c7e54-default-rtdb.asia-southeast1.firebasedatabase.app",
            projectId: "waste-system-c7e54",
            storageBucket: "waste-system-c7e54.firebasestorage.app",
            messagingSenderId: "74843417033",
            appId: "1:74843417033:web:c9749cc41541f95dcb2018"
        };

        // Initialize Firebase
        let app, database;
        let isOnline = false;
        let wasteData = [];
        let currentEditId = null;
        let pointsConfig = {
            general: 10,
            wet: 20,
            recycle: 30,
            hazardous: 40
        };

        try {
            app = initializeApp(firebaseConfig);
            database = getDatabase(app);
            
            // Connection monitoring
            const connectedRef = ref(database, '.info/connected');
            onValue(connectedRef, (snapshot) => {
                isOnline = snapshot.val() === true;
                updateConnectionStatus();
                if (isOnline) {
                    syncLocalData();
                }
            });

            // Load data from Firebase
            const wasteRef = ref(database, 'wasteRecords');
            onValue(wasteRef, (snapshot) => {
                if (snapshot.exists()) {
                    wasteData = [];
                    snapshot.forEach((childSnapshot) => {
                        wasteData.push({
                            id: childSnapshot.key,
                            ...childSnapshot.val()
                        });
                    });
                    saveToLocalStorage();
                    updateAllDisplays();
                }
            });

            // Load config from Firebase
            const configRef = ref(database, 'config');
            onValue(configRef, (snapshot) => {
                if (snapshot.exists()) {
                    pointsConfig = { ...pointsConfig, ...snapshot.val() };
                    updateConfigDisplay();
                }
            });

        } catch (error) {
            console.error('Firebase initialization failed:', error);
            loadFromLocalStorage();
        }

        // Local Storage functions
        function saveToLocalStorage() {
            localStorage.setItem('panpraowWasteData', JSON.stringify(wasteData));
            localStorage.setItem('panpraowPointsConfig', JSON.stringify(pointsConfig));
        }

        function loadFromLocalStorage() {
            const savedData = localStorage.getItem('panpraowWasteData');
            const savedConfig = localStorage.getItem('panpraowPointsConfig');
            
            if (savedData) {
                wasteData = JSON.parse(savedData);
            }
            if (savedConfig) {
                pointsConfig = JSON.parse(savedConfig);
            }
            
            updateAllDisplays();
            updateConfigDisplay();
        }

        function syncLocalData() {
            if (!database) return;
            
            // Sync waste data
            const wasteRef = ref(database, 'wasteRecords');
            wasteData.forEach(record => {
                if (!record.synced) {
                    const newRecordRef = push(wasteRef);
                    set(newRecordRef, {
                        ...record,
                        synced: true,
                        timestamp: serverTimestamp()
                    });
                }
            });

            // Sync config
            const configRef = ref(database, 'config');
            set(configRef, pointsConfig);
        }

        function updateConnectionStatus() {
            const statusEl = document.getElementById('connectionStatus');
            if (isOnline) {
                statusEl.className = 'connection-status online';
                statusEl.innerHTML = '<i class="fas fa-wifi"></i> ออนไลน์';
            } else {
                statusEl.className = 'connection-status offline animate-pulse-green';
                statusEl.innerHTML = '<i class="fas fa-wifi"></i> ออฟไลน์';
            }
        }

        // Tab management
        window.showTab = function(tabName) {
            // Hide all tabs
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.add('hidden');
            });
            
            // Remove active class from all nav buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.className = 'nav-btn py-4 px-6 text-gray-600 hover:text-green-600 hover:border-green-300 border-b-4 border-transparent transition-all';
            });
            
            // Show selected tab
            document.getElementById(tabName + 'Tab').classList.remove('hidden');
            
            // Add active class to selected nav button
            event.target.className = 'nav-btn py-4 px-6 border-b-4 border-green-500 text-green-600 font-semibold';
            
            // Update displays based on tab
            if (tabName === 'dashboard') {
                updateChart();
                updateStats();
            } else if (tabName === 'leaderboard') {
                updateLeaderboard();
            } else if (tabName === 'history') {
                loadHistory();
            }
        };

        // Form submission
        document.getElementById('wasteForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const formData = {
                recorderName: document.getElementById('recorderName').value,
                wasteType: document.getElementById('wasteType').value,
                weight: parseFloat(document.getElementById('weight').value),
                date: document.getElementById('recordDate').value,
                notes: document.getElementById('notes').value,
                timestamp: new Date().toISOString(),
                synced: false
            };

            // Calculate points
            formData.points = formData.weight * pointsConfig[formData.wasteType];

            // Add to local data
            const id = 'local_' + Date.now();
            wasteData.push({ id, ...formData });
            
            // Save to Firebase if online
            if (isOnline && database) {
                const wasteRef = ref(database, 'wasteRecords');
                const newRecordRef = push(wasteRef);
                set(newRecordRef, {
                    ...formData,
                    synced: true,
                    timestamp: serverTimestamp()
                });
            }

            saveToLocalStorage();
            updateAllDisplays();
            
            // Reset form
            this.reset();
            document.getElementById('recordDate').value = new Date().toISOString().split('T')[0];
            
            // Show success message
            showNotification('🎉 บันทึกข้อมูลเรียบร้อยแล้ว!', 'success');
        });

        // Initialize date input
        document.getElementById('recordDate').value = new Date().toISOString().split('T')[0];
        document.getElementById('historyMonth').value = new Date().toISOString().slice(0, 7);

        // Chart management
        let wasteChart;
        
        window.updateChart = function() {
            const period = document.getElementById('chartPeriod').value;
            const filteredData = filterDataByPeriod(wasteData, period);
            
            const chartData = {
                wet: 0,
                general: 0,
                recycle: 0,
                hazardous: 0
            };

            filteredData.forEach(record => {
                chartData[record.wasteType] += record.weight;
            });

            const ctx = document.getElementById('wasteChart').getContext('2d');
            
            if (wasteChart) {
                wasteChart.destroy();
            }

            wasteChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['🟢 ขยะเปียก', '🔵 ขยะทั่วไป', '🟡 ขยะรีไซเคิล', '🔴 ขยะอันตราย'],
                    datasets: [{
                        data: [chartData.wet, chartData.general, chartData.recycle, chartData.hazardous],
                        backgroundColor: [
                            'linear-gradient(135deg, #10b981, #34d399)',
                            'linear-gradient(135deg, #3b82f6, #60a5fa)',
                            'linear-gradient(135deg, #f59e0b, #fbbf24)',
                            'linear-gradient(135deg, #ef4444, #f87171)'
                        ],
                        borderWidth: 3,
                        borderColor: '#ffffff',
                        hoverBorderWidth: 5
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                padding: 20,
                                font: {
                                    size: 14,
                                    weight: 'bold'
                                },
                                usePointStyle: true,
                                pointStyle: 'circle'
                            }
                        },
                        tooltip: {
                            backgroundColor: 'rgba(0, 0, 0, 0.8)',
                            titleColor: '#ffffff',
                            bodyColor: '#ffffff',
                            borderColor: '#10b981',
                            borderWidth: 2,
                            cornerRadius: 10,
                            callbacks: {
                                label: function(context) {
                                    return context.label + ': ' + context.parsed + ' กก.';
                                }
                            }
                        }
                    },
                    animation: {
                        animateRotate: true,
                        duration: 1000
                    }
                }
            });
        };

        function updateStats() {
            const period = document.getElementById('chartPeriod').value;
            const filteredData = filterDataByPeriod(wasteData, period);
            
            const stats = {
                wet: 0,
                general: 0,
                recycle: 0,
                hazardous: 0
            };

            filteredData.forEach(record => {
                stats[record.wasteType] += record.weight;
            });

            document.getElementById('wetWasteTotal').textContent = stats.wet.toFixed(1) + ' กก.';
            document.getElementById('generalWasteTotal').textContent = stats.general.toFixed(1) + ' กก.';
            document.getElementById('recycleWasteTotal').textContent = stats.recycle.toFixed(1) + ' กก.';
            document.getElementById('hazardousWasteTotal').textContent = stats.hazardous.toFixed(1) + ' กก.';
        }

        function updateLeaderboard() {
            const leaderboard = {};
            
            wasteData.forEach(record => {
                if (!leaderboard[record.recorderName]) {
                    leaderboard[record.recorderName] = {
                        name: record.recorderName,
                        totalPoints: 0,
                        totalWeight: 0,
                        records: 0
                    };
                }
                
                leaderboard[record.recorderName].totalPoints += record.points;
                leaderboard[record.recorderName].totalWeight += record.weight;
                leaderboard[record.recorderName].records += 1;
            });

            const sortedLeaderboard = Object.values(leaderboard)
                .sort((a, b) => b.totalPoints - a.totalPoints);

            const leaderboardHtml = sortedLeaderboard.map((user, index) => {
                const rankIcon = index === 0 ? '🥇' : index === 1 ? '🥈' : index === 2 ? '🥉' : `${index + 1}.`;
                const rankClass = index === 0 ? 'bg-gradient-to-r from-yellow-400 via-yellow-500 to-yellow-600 text-white shadow-xl' : 
                                 index === 1 ? 'bg-gradient-to-r from-gray-300 via-gray-400 to-gray-500 text-white shadow-lg' :
                                 index === 2 ? 'bg-gradient-to-r from-orange-400 via-orange-500 to-orange-600 text-white shadow-lg' :
                                 'bg-gradient-to-r from-green-50 to-emerald-50 border-2 border-green-200';
                
                const textClass = index < 3 ? 'text-white' : 'text-gray-800';
                
                return `
                    <div class="flex items-center justify-between p-6 rounded-2xl ${rankClass} transition-all hover:scale-105">
                        <div class="flex items-center space-x-6">
                            <div class="text-4xl font-bold">${rankIcon}</div>
                            <div>
                                <h4 class="font-bold text-xl ${textClass}">${user.name}</h4>
                                <p class="text-sm ${index < 3 ? 'text-white opacity-90' : 'text-green-600'}">
                                    🌱 ${user.records} ครั้ง • ${user.totalWeight.toFixed(1)} กก.
                                </p>
                            </div>
                        </div>
                        <div class="text-right">
                            <p class="text-3xl font-bold ${textClass}">${user.totalPoints.toFixed(0)}</p>
                            <p class="text-sm ${index < 3 ? 'text-white opacity-90' : 'text-green-600'}">แต้ม</p>
                        </div>
                    </div>
                `;
            }).join('');

            document.getElementById('leaderboardList').innerHTML = leaderboardHtml || 
                '<div class="text-center py-12"><div class="text-6xl mb-4">🌱</div><p class="text-gray-500 text-lg">ยังไม่มีข้อมูล</p><p class="text-green-600">เริ่มบันทึกขยะเพื่อสะสมคะแนนกันเถอะ!</p></div>';
        }

        window.loadHistory = function() {
            const period = document.getElementById('historyPeriod').value;
            const month = document.getElementById('historyMonth').value;
            
            let filteredData;
            if (period === 'month' && month) {
                filteredData = wasteData.filter(record => record.date.startsWith(month));
            } else {
                filteredData = filterDataByPeriod(wasteData, period);
            }

            const historyHtml = filteredData
                .sort((a, b) => new Date(b.date) - new Date(a.date))
                .map(record => {
                    const wasteTypeNames = {
                        wet: '🟢 ขยะเปียก',
                        general: '🔵 ขยะทั่วไป',
                        recycle: '🟡 ขยะรีไซเคิล',
                        hazardous: '🔴 ขยะอันตราย'
                    };
                    
                    const wasteTypeColors = {
                        wet: 'bg-gradient-to-r from-green-100 to-emerald-100 text-green-800 border border-green-200',
                        general: 'bg-gradient-to-r from-blue-100 to-cyan-100 text-blue-800 border border-blue-200',
                        recycle: 'bg-gradient-to-r from-yellow-100 to-orange-100 text-yellow-800 border border-yellow-200',
                        hazardous: 'bg-gradient-to-r from-red-100 to-pink-100 text-red-800 border border-red-200'
                    };

                    return `
                        <div class="flex items-center justify-between p-6 border-2 border-green-100 rounded-2xl hover:bg-green-50 transition-all hover:shadow-lg">
                            <div class="flex-1">
                                <div class="flex items-center space-x-6">
                                    <div class="text-center">
                                        <div class="w-12 h-12 bg-gradient-to-br from-green-500 to-emerald-600 rounded-full flex items-center justify-center text-white font-bold text-lg mb-2">
                                            ${record.recorderName.charAt(0).toUpperCase()}
                                        </div>
                                    </div>
                                    <div class="flex-1">
                                        <h4 class="font-bold text-lg text-gray-800">${record.recorderName}</h4>
                                        <p class="text-sm text-gray-600 mb-2">📅 ${new Date(record.date).toLocaleDateString('th-TH', {
                                            year: 'numeric',
                                            month: 'long',
                                            day: 'numeric'
                                        })}</p>
                                        <div class="flex items-center space-x-4">
                                            <span class="px-4 py-2 rounded-full text-sm font-bold ${wasteTypeColors[record.wasteType]}">
                                                ${wasteTypeNames[record.wasteType]}
                                            </span>
                                            <div class="bg-gray-100 px-4 py-2 rounded-full">
                                                <span class="font-bold text-gray-800">⚖️ ${record.weight} กก.</span>
                                            </div>
                                            <div class="bg-gradient-to-r from-green-100 to-emerald-100 px-4 py-2 rounded-full border border-green-200">
                                                <span class="font-bold text-green-800">⭐ ${record.points} แต้ม</span>
                                            </div>
                                        </div>
                                        ${record.notes ? `<p class="text-sm text-gray-500 mt-3 bg-gray-50 p-3 rounded-lg">💭 ${record.notes}</p>` : ''}
                                    </div>
                                </div>
                            </div>
                            <button onclick="openEditModal('${record.id}')" 
                                    class="ml-6 bg-gradient-to-r from-blue-500 to-blue-600 text-white p-3 rounded-xl hover:from-blue-600 hover:to-blue-700 transition-all shadow-lg hover:shadow-xl">
                                <i class="fas fa-edit text-lg"></i>
                            </button>
                        </div>
                    `;
                }).join('');

            document.getElementById('historyList').innerHTML = historyHtml || 
                '<div class="text-center py-12"><div class="text-6xl mb-4">📊</div><p class="text-gray-500 text-lg">ไม่พบข้อมูลในช่วงเวลานี้</p><p class="text-green-600">ลองเปลี่ยนช่วงเวลาที่ต้องการดูข้อมูล</p></div>';
        };

        function filterDataByPeriod(data, period) {
            const now = new Date();
            const startOfWeek = new Date(now.setDate(now.getDate() - now.getDay()));
            const startOfMonth = new Date(now.getFullYear(), now.getMonth(), 1);
            const startOfYear = new Date(now.getFullYear(), 0, 1);

            return data.filter(record => {
                const recordDate = new Date(record.date);
                switch (period) {
                    case 'week':
                        return recordDate >= startOfWeek;
                    case 'month':
                        return recordDate >= startOfMonth;
                    case 'year':
                        return recordDate >= startOfYear;
                    default:
                        return true;
                }
            });
        }

        // Modal functions
        window.openEditModal = function(id) {
            currentEditId = id;
            document.getElementById('editModal').classList.remove('hidden');
            document.getElementById('editModal').classList.add('flex');
        };

        window.closeModal = function() {
            document.getElementById('editModal').classList.add('hidden');
            document.getElementById('editModal').classList.remove('flex');
            document.getElementById('adminPassword').value = '';
            currentEditId = null;
        };

        window.confirmEdit = function() {
            const password = document.getElementById('adminPassword').value;
            if (password !== '$panpraow423') {
                showNotification('🔒 รหัสผ่านไม่ถูกต้อง!', 'error');
                return;
            }

            const record = wasteData.find(r => r.id === currentEditId);
            if (record) {
                // Populate form with existing data
                document.getElementById('recorderName').value = record.recorderName;
                document.getElementById('wasteType').value = record.wasteType;
                document.getElementById('weight').value = record.weight;
                document.getElementById('recordDate').value = record.date;
                document.getElementById('notes').value = record.notes || '';
                
                // Remove the record
                wasteData = wasteData.filter(r => r.id !== currentEditId);
                
                // Remove from Firebase if online
                if (isOnline && database && !currentEditId.startsWith('local_')) {
                    const recordRef = ref(database, `wasteRecords/${currentEditId}`);
                    remove(recordRef);
                }
                
                saveToLocalStorage();
                updateAllDisplays();
                showTab('record');
                closeModal();
                showNotification('✏️ ข้อมูลถูกโหลดเพื่อแก้ไขแล้ว', 'success');
            }
        };

        window.confirmDelete = function() {
            const password = document.getElementById('adminPassword').value;
            if (password !== '$panpraow423') {
                showNotification('🔒 รหัสผ่านไม่ถูกต้อง!', 'error');
                return;
            }

            // Remove from local data
            wasteData = wasteData.filter(r => r.id !== currentEditId);
            
            // Remove from Firebase if online
            if (isOnline && database && !currentEditId.startsWith('local_')) {
                const recordRef = ref(database, `wasteRecords/${currentEditId}`);
                remove(recordRef);
            }
            
            saveToLocalStorage();
            updateAllDisplays();
            closeModal();
            showNotification('🗑️ ลบข้อมูลเรียบร้อยแล้ว', 'success');
        };

        // Config functions
        function updateConfigDisplay() {
            document.getElementById('generalPoints').value = pointsConfig.general;
            document.getElementById('wetPoints').value = pointsConfig.wet;
            document.getElementById('recyclePoints').value = pointsConfig.recycle;
            document.getElementById('hazardousPoints').value = pointsConfig.hazardous;
        }

        window.saveConfig = function() {
            const password = document.getElementById('configPassword').value;
            
            // Check password
            if (password !== '$panpraow423') {
                showNotification('🔒 รหัสผ่านไม่ถูกต้อง! ไม่สามารถบันทึกการตั้งค่าได้', 'error');
                return;
            }

            pointsConfig = {
                general: parseInt(document.getElementById('generalPoints').value),
                wet: parseInt(document.getElementById('wetPoints').value),
                recycle: parseInt(document.getElementById('recyclePoints').value),
                hazardous: parseInt(document.getElementById('hazardousPoints').value)
            };

            // Save to Firebase if online
            if (isOnline && database) {
                const configRef = ref(database, 'config');
                set(configRef, pointsConfig);
            }

            saveToLocalStorage();
            
            // Clear password field
            document.getElementById('configPassword').value = '';
            
            showNotification('⚙️ บันทึกการตั้งค่าเรียบร้อยแล้ว!', 'success');
        };

        // Export function
        window.exportData = function() {
            const printDate = new Date().toLocaleDateString('th-TH', {
                year: 'numeric',
                month: 'long',
                day: 'numeric',
                weekday: 'long'
            });
            
            const printTime = new Date().toLocaleTimeString('th-TH', {
                hour: '2-digit',
                minute: '2-digit'
            });
            
            document.getElementById('printDate').textContent = `${printDate} เวลา ${printTime} น.`;
            
            // Generate summary statistics
            const totalStats = {
                wet: 0,
                general: 0,
                recycle: 0,
                hazardous: 0,
                totalWeight: 0,
                totalPoints: 0
            };

            wasteData.forEach(record => {
                totalStats[record.wasteType] += record.weight;
                totalStats.totalWeight += record.weight;
                totalStats.totalPoints += record.points;
            });

            // Generate detailed user statistics
            const userStats = {};
            wasteData.forEach(record => {
                if (!userStats[record.recorderName]) {
                    userStats[record.recorderName] = {
                        name: record.recorderName,
                        totalPoints: 0,
                        totalWeight: 0,
                        records: 0,
                        wasteTypes: {
                            wet: { weight: 0, count: 0, points: 0 },
                            general: { weight: 0, count: 0, points: 0 },
                            recycle: { weight: 0, count: 0, points: 0 },
                            hazardous: { weight: 0, count: 0, points: 0 }
                        },
                        lastRecord: record.date,
                        firstRecord: record.date
                    };
                }
                
                const user = userStats[record.recorderName];
                user.totalPoints += record.points;
                user.totalWeight += record.weight;
                user.records += 1;
                user.wasteTypes[record.wasteType].weight += record.weight;
                user.wasteTypes[record.wasteType].count += 1;
                user.wasteTypes[record.wasteType].points += record.points;
                
                // Update date range
                if (new Date(record.date) > new Date(user.lastRecord)) {
                    user.lastRecord = record.date;
                }
                if (new Date(record.date) < new Date(user.firstRecord)) {
                    user.firstRecord = record.date;
                }
            });

            const sortedUsers = Object.values(userStats)
                .sort((a, b) => b.totalPoints - a.totalPoints);

            // Generate recent records (last 20)
            const recentRecords = wasteData
                .sort((a, b) => new Date(b.date) - new Date(a.date))
                .slice(0, 20);

            const wasteTypeNames = {
                wet: '🟢 ขยะเปียก',
                general: '🔵 ขยะทั่วไป', 
                recycle: '🟡 ขยะรีไซเคิล',
                hazardous: '🔴 ขยะอันตราย'
            };

            const printContent = `
                <!-- Summary Statistics -->
                <div class="mb-8 p-6 border-2 border-green-200 rounded-lg bg-green-50">
                    <h3 class="text-2xl font-bold mb-6 text-green-800 flex items-center">
                        <span class="text-3xl mr-3">📊</span>สรุปสถิติขยะทั้งหมด
                    </h3>
                    <div class="grid grid-cols-2 gap-6 mb-6">
                        <div class="bg-white p-4 rounded-lg border border-green-200">
                            <div class="flex items-center">
                                <span class="text-2xl mr-3">🟢</span>
                                <div>
                                    <div class="font-bold">ขยะเปียก</div>
                                    <div class="text-xl text-green-600">${totalStats.wet.toFixed(1)} กก.</div>
                                </div>
                            </div>
                        </div>
                        <div class="bg-white p-4 rounded-lg border border-blue-200">
                            <div class="flex items-center">
                                <span class="text-2xl mr-3">🔵</span>
                                <div>
                                    <div class="font-bold">ขยะทั่วไป</div>
                                    <div class="text-xl text-blue-600">${totalStats.general.toFixed(1)} กก.</div>
                                </div>
                            </div>
                        </div>
                        <div class="bg-white p-4 rounded-lg border border-yellow-200">
                            <div class="flex items-center">
                                <span class="text-2xl mr-3">🟡</span>
                                <div>
                                    <div class="font-bold">ขยะรีไซเคิล</div>
                                    <div class="text-xl text-yellow-600">${totalStats.recycle.toFixed(1)} กก.</div>
                                </div>
                            </div>
                        </div>
                        <div class="bg-white p-4 rounded-lg border border-red-200">
                            <div class="flex items-center">
                                <span class="text-2xl mr-3">🔴</span>
                                <div>
                                    <div class="font-bold">ขยะอันตราย</div>
                                    <div class="text-xl text-red-600">${totalStats.hazardous.toFixed(1)} กก.</div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="bg-gradient-to-r from-green-600 to-emerald-600 text-white p-4 rounded-lg">
                        <div class="grid grid-cols-3 gap-4 text-center">
                            <div>
                                <div class="text-2xl font-bold">${totalStats.totalWeight.toFixed(1)} กก.</div>
                                <div>น้ำหนักรวม</div>
                            </div>
                            <div>
                                <div class="text-2xl font-bold">${totalStats.totalPoints.toFixed(0)} แต้ม</div>
                                <div>คะแนนรวม</div>
                            </div>
                            <div>
                                <div class="text-2xl font-bold">${wasteData.length}</div>
                                <div>รายการทั้งหมด</div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Detailed User Report -->
                <div class="mb-8">
                    <h3 class="text-2xl font-bold mb-6 text-green-800 flex items-center">
                        <span class="text-3xl mr-3">👥</span>รายชื่อผู้รายงานขยะพร้อมข้อมูลรายละเอียด
                    </h3>
                    <div class="text-sm text-gray-600 mb-4">
                        📋 จำนวนผู้รายงานทั้งหมด: <strong>${sortedUsers.length} คน</strong> | 
                        📅 ช่วงเวลา: ${wasteData.length > 0 ? new Date(Math.min(...wasteData.map(r => new Date(r.date)))).toLocaleDateString('th-TH') : '-'} 
                        ถึง ${wasteData.length > 0 ? new Date(Math.max(...wasteData.map(r => new Date(r.date)))).toLocaleDateString('th-TH') : '-'}
                    </div>
                    
                    ${sortedUsers.map((user, index) => {
                        const rankIcon = index === 0 ? '🥇' : index === 1 ? '🥈' : index === 2 ? '🥉' : `${index + 1}.`;
                        const daysDiff = Math.ceil((new Date(user.lastRecord) - new Date(user.firstRecord)) / (1000 * 60 * 60 * 24)) + 1;
                        const avgPerDay = (user.totalWeight / daysDiff).toFixed(2);
                        
                        return `
                        <div class="mb-6 p-6 border-2 border-gray-200 rounded-lg bg-white">
                            <div class="flex items-center justify-between mb-4">
                                <div class="flex items-center space-x-4">
                                    <div class="text-3xl font-bold text-green-600">${rankIcon}</div>
                                    <div>
                                        <h4 class="text-xl font-bold text-gray-800">${user.name}</h4>
                                        <div class="text-sm text-gray-600">
                                            📅 ช่วงเวลา: ${new Date(user.firstRecord).toLocaleDateString('th-TH')} - ${new Date(user.lastRecord).toLocaleDateString('th-TH')}
                                            ${daysDiff > 1 ? `(${daysDiff} วัน)` : ''}
                                        </div>
                                    </div>
                                </div>
                                <div class="text-right">
                                    <div class="text-2xl font-bold text-green-600">${user.totalPoints.toFixed(0)} แต้ม</div>
                                    <div class="text-sm text-gray-600">${user.totalWeight.toFixed(1)} กก. | ${user.records} ครั้ง</div>
                                    <div class="text-xs text-gray-500">เฉลี่ย ${avgPerDay} กก./วัน</div>
                                </div>
                            </div>
                            
                            <div class="grid grid-cols-4 gap-4">
                                <div class="bg-green-50 p-3 rounded-lg border border-green-200">
                                    <div class="text-center">
                                        <div class="text-lg font-bold text-green-600">${user.wasteTypes.wet.weight.toFixed(1)}</div>
                                        <div class="text-xs text-gray-600">🟢 เปียก (${user.wasteTypes.wet.count} ครั้ง)</div>
                                        <div class="text-xs text-green-600">${user.wasteTypes.wet.points.toFixed(0)} แต้ม</div>
                                    </div>
                                </div>
                                <div class="bg-blue-50 p-3 rounded-lg border border-blue-200">
                                    <div class="text-center">
                                        <div class="text-lg font-bold text-blue-600">${user.wasteTypes.general.weight.toFixed(1)}</div>
                                        <div class="text-xs text-gray-600">🔵 ทั่วไป (${user.wasteTypes.general.count} ครั้ง)</div>
                                        <div class="text-xs text-blue-600">${user.wasteTypes.general.points.toFixed(0)} แต้ม</div>
                                    </div>
                                </div>
                                <div class="bg-yellow-50 p-3 rounded-lg border border-yellow-200">
                                    <div class="text-center">
                                        <div class="text-lg font-bold text-yellow-600">${user.wasteTypes.recycle.weight.toFixed(1)}</div>
                                        <div class="text-xs text-gray-600">🟡 รีไซเคิล (${user.wasteTypes.recycle.count} ครั้ง)</div>
                                        <div class="text-xs text-yellow-600">${user.wasteTypes.recycle.points.toFixed(0)} แต้ม</div>
                                    </div>
                                </div>
                                <div class="bg-red-50 p-3 rounded-lg border border-red-200">
                                    <div class="text-center">
                                        <div class="text-lg font-bold text-red-600">${user.wasteTypes.hazardous.weight.toFixed(1)}</div>
                                        <div class="text-xs text-gray-600">🔴 อันตราย (${user.wasteTypes.hazardous.count} ครั้ง)</div>
                                        <div class="text-xs text-red-600">${user.wasteTypes.hazardous.points.toFixed(0)} แต้ม</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        `;
                    }).join('')}
                </div>

                <!-- Recent Records -->
                <div class="mb-8">
                    <h3 class="text-2xl font-bold mb-6 text-green-800 flex items-center">
                        <span class="text-3xl mr-3">📝</span>รายการล่าสุด (20 รายการ)
                    </h3>
                    <table class="w-full border-collapse border-2 border-green-300 rounded-lg overflow-hidden">
                        <thead>
                            <tr class="bg-gradient-to-r from-green-500 to-emerald-600 text-white">
                                <th class="border border-green-300 p-3 text-left">วันที่</th>
                                <th class="border border-green-300 p-3 text-left">ชื่อผู้รายงาน</th>
                                <th class="border border-green-300 p-3 text-center">ประเภทขยะ</th>
                                <th class="border border-green-300 p-3 text-center">น้ำหนัก (กก.)</th>
                                <th class="border border-green-300 p-3 text-center">คะแนน</th>
                                <th class="border border-green-300 p-3 text-left">หมายเหตุ</th>
                            </tr>
                        </thead>
                        <tbody>
                            ${recentRecords.map((record, index) => {
                                const bgClass = index % 2 === 0 ? 'bg-green-50' : 'bg-white';
                                return `
                                <tr class="${bgClass}">
                                    <td class="border border-green-200 p-3 text-sm">
                                        ${new Date(record.date).toLocaleDateString('th-TH', {
                                            day: '2-digit',
                                            month: '2-digit',
                                            year: 'numeric'
                                        })}
                                    </td>
                                    <td class="border border-green-200 p-3 font-semibold">${record.recorderName}</td>
                                    <td class="border border-green-200 p-3 text-center text-sm">
                                        ${wasteTypeNames[record.wasteType]}
                                    </td>
                                    <td class="border border-green-200 p-3 text-center font-bold">${record.weight}</td>
                                    <td class="border border-green-200 p-3 text-center font-bold text-green-600">${record.points.toFixed(0)}</td>
                                    <td class="border border-green-200 p-3 text-sm text-gray-600">
                                        ${record.notes || '-'}
                                    </td>
                                </tr>
                            `;
                            }).join('')}
                        </tbody>
                    </table>
                </div>

                <!-- Summary Table -->
                <div class="mb-8">
                    <h3 class="text-2xl font-bold mb-6 text-green-800 flex items-center">
                        <span class="text-3xl mr-3">🏆</span>สรุปอันดับผู้รายงานขยะ
                    </h3>
                    <table class="w-full border-collapse border-2 border-green-300 rounded-lg overflow-hidden">
                        <thead>
                            <tr class="bg-gradient-to-r from-green-500 to-emerald-600 text-white">
                                <th class="border border-green-300 p-4 text-left">อันดับ</th>
                                <th class="border border-green-300 p-4 text-left">ชื่อ</th>
                                <th class="border border-green-300 p-4 text-center">คะแนนรวม</th>
                                <th class="border border-green-300 p-4 text-center">น้ำหนักรวม (กก.)</th>
                                <th class="border border-green-300 p-4 text-center">จำนวนครั้ง</th>
                                <th class="border border-green-300 p-4 text-center">เฉลี่ย/ครั้ง (กก.)</th>
                            </tr>
                        </thead>
                        <tbody>
                            ${sortedUsers.map((user, index) => {
                                const rankIcon = index === 0 ? '🥇' : index === 1 ? '🥈' : index === 2 ? '🥉' : `${index + 1}.`;
                                const bgClass = index % 2 === 0 ? 'bg-green-50' : 'bg-white';
                                const avgPerRecord = (user.totalWeight / user.records).toFixed(2);
                                return `
                                <tr class="${bgClass}">
                                    <td class="border border-green-200 p-4 text-center font-bold text-lg">${rankIcon}</td>
                                    <td class="border border-green-200 p-4 font-semibold">${user.name}</td>
                                    <td class="border border-green-200 p-4 text-center font-bold text-green-600">${user.totalPoints.toFixed(0)}</td>
                                    <td class="border border-green-200 p-4 text-center font-bold">${user.totalWeight.toFixed(1)}</td>
                                    <td class="border border-green-200 p-4 text-center">${user.records}</td>
                                    <td class="border border-green-200 p-4 text-center text-sm">${avgPerRecord}</td>
                                </tr>
                            `;
                            }).join('')}
                        </tbody>
                    </table>
                </div>

                <!-- Footer -->
                <div class="mt-8 pt-6 border-t-2 border-green-200 text-center text-gray-600">
                    <p class="text-lg font-semibold text-green-800 mb-2">🌱 ขอบคุณทุกท่านที่ร่วมดูแลสิ่งแวดล้อม</p>
                    <p class="text-sm">รายงานนี้สร้างโดยระบบจัดการขยะโรงเรียนพานพร้าว</p>
                    <p class="text-xs text-gray-500 mt-2">
                        💡 เคล็ดลับ: การแยกขยะอย่างถูกต้องช่วยลดมลพิษและสร้างรายได้จากการรีไซเคิล
                    </p>
                </div>
            `;

            document.getElementById('printContent').innerHTML = printContent;
            window.print();
        };

        // Notification function
        function showNotification(message, type = 'info') {
            const notification = document.createElement('div');
            const bgColor = type === 'success' ? 'from-green-500 to-emerald-600' : 
                           type === 'error' ? 'from-red-500 to-pink-600' : 'from-blue-500 to-cyan-600';
            
            notification.className = `fixed top-24 right-4 z-50 px-6 py-4 rounded-2xl text-white font-semibold bg-gradient-to-r ${bgColor} shadow-xl transform transition-all duration-300 translate-x-full`;
            notification.innerHTML = `
                <div class="flex items-center space-x-3">
                    <div class="text-xl">${type === 'success' ? '✅' : type === 'error' ? '❌' : 'ℹ️'}</div>
                    <div>${message}</div>
                </div>
            `;
            
            document.body.appendChild(notification);
            
            // Animate in
            setTimeout(() => {
                notification.classList.remove('translate-x-full');
            }, 100);
            
            // Animate out and remove
            setTimeout(() => {
                notification.classList.add('translate-x-full');
                setTimeout(() => {
                    notification.remove();
                }, 300);
            }, 3000);
        }

        function updateAllDisplays() {
            updateChart();
            updateStats();
            updateLeaderboard();
            loadHistory();
        }

        // Initialize
        loadFromLocalStorage();
        updateConnectionStatus();

        // Make functions globally available
        window.database = database;
        window.wasteData = wasteData;
        window.pointsConfig = pointsConfig;
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9927066df6c08960',t:'MTc2MTExNTQ1Ni4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
