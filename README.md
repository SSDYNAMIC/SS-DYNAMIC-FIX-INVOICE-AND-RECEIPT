# SS-DYNAMIC-FIX-INVOICE-AND-RECEIPT
SS DYNAMIC FIX INVOICE AND RECEIPT
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SS Dynamic Fix - Invoice & Receipt Generator</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- html2pdf.js CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brandRed: '#DC2626',
                        brandRedDark: '#991B1B',
                        brandBlack: '#0D0D0D',
                        brandGray: '#1F1F1F',
                    }
                }
            }
        }
    </script>

    <style>
        /* Custom Background Pattern with Tech Overlay */
        body {
            background-color: #0D0D0D;
            background-image: 
                radial-gradient(rgba(220, 38, 38, 0.15) 1px, transparent 0),
                linear-gradient(to bottom, rgba(13, 13, 13, 0.85), rgba(13, 13, 13, 0.98)),
                url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="80" height="80" viewBox="0 0 80 80"><text x="10" y="30" font-size="20" fill="rgba(255,255,255,0.03)">📱</text><text x="45" y="65" font-size="20" fill="rgba(255,255,255,0.03)">💻</text></svg>');
            background-size: 30px 30px, 100% 100%, 80px 80px;
        }

        /* Hide preview controls during PDF capture */
        .pdf-export .no-print {
            display: none !important;
        }

        .pdf-export {
            box-shadow: none !important;
            border: 1px solid #e5e7eb !important;
            background-color: #ffffff !important;
            color: #000000 !important;
        }
    </style>
</head>
<text_content>
<body class="text-gray-200 font-sans min-h-screen pb-12">

    <!-- Header / Navbar -->
    <header class="bg-brandGray border-b border-brandRed/40 sticky top-0 z-50 shadow-lg shadow-black/50">
        <div class="max-w-7xl mx-auto px-4 py-4 flex flex-col md:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-4">
                <div class="w-12 h-12 rounded-xl bg-brandRed flex items-center justify-center text-2xl font-black text-white shadow-md shadow-brandRed/30">
                    SS
                </div>
                <div>
                    <h1 class="text-2xl font-black tracking-wider text-white flex items-center gap-2">
                        SS DYNAMIC FIX <span class="text-xs bg-brandRed px-2 py-0.5 rounded text-white font-semibold uppercase">Expert Repair Lab</span>
                    </h1>
                    <p class="text-xs text-gray-400">Phone & Laptop Service Repair Center</p>
                </div>
            </div>
            <div class="text-right text-xs text-gray-300">
                <p><i class="fa-solid fa-location-dot text-brandRed mr-1"></i> No 2, Bangunan Terminal Bas, Kompleks AG Sentral, 78000 Alor Gajah, Melaka</p>
                <p><i class="fa-solid fa-phone text-brandRed mr-1"></i> +601151453147</p>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="max-w-7xl mx-auto px-4 mt-8 grid grid-cols-1 lg:grid-cols-12 gap-8">
        
        <!-- Left Column: Input Form (5 Cols) -->
        <section class="lg:col-span-5 bg-brandGray/90 border border-red-900/30 p-6 rounded-2xl shadow-xl backdrop-blur-sm">
            <h2 class="text-xl font-bold text-white mb-6 flex items-center gap-2 border-b border-gray-800 pb-3">
                <i class="fa-solid fa-sliders text-brandRed"></i> Repair & Client Details
            </h2>

            <form id="invoiceForm" class="space-y-4">
                
                <!-- Invoice Type & Date -->
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 mb-1">Doc Type</label>
                        <select id="docType" onchange="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none">
                            <option value="INVOICE">INVOICE</option>
                            <option value="RECEIPT">RECEIPT</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 mb-1">Invoice No.</label>
                        <input type="text" id="invNo" readonly class="w-full bg-black/30 border border-gray-800 text-gray-400 rounded-lg px-3 py-2 text-sm font-mono cursor-not-allowed">
                    </div>
                </div>

                <!-- Customer Info -->
                <div class="space-y-3 pt-2">
                    <h3 class="text-xs font-bold text-brandRed uppercase tracking-wider">Customer Information</h3>
                    <div>
                        <input type="text" id="clientName" placeholder="Customer Name *" oninput="updatePreview()" required class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none">
                    </div>
                    <div class="grid grid-cols-2 gap-3">
                        <input type="tel" id="clientPhone" placeholder="Phone / Whatsapp *" oninput="updatePreview()" required class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none">
                        <input type="email" id="clientEmail" placeholder="Email Address" oninput="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none">
                    </div>
                </div>

                <!-- Device Info -->
                <div class="space-y-3 pt-2">
                    <h3 class="text-xs font-bold text-brandRed uppercase tracking-wider">Device Specs</h3>
                    <div class="grid grid-cols-3 gap-3">
                        <div>
                            <select id="deviceType" onchange="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none">
                                <option value="📱 Phone">📱 Phone</option>
                                <option value="💻 Laptop">💻 Laptop</option>
                            </select>
                        </div>
                        <div class="col-span-2">
                            <input type="text" id="deviceBrand" placeholder="Brand (e.g. Apple, Asus)" oninput="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none">
                        </div>
                    </div>
                    <div class="grid grid-cols-2 gap-3">
                        <input type="text" id="deviceModel" placeholder="Model (e.g. iPhone 13, ROG Strix)" oninput="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none">
                        <input type="text" id="codeModel" placeholder="Serial / Code Model" oninput="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none">
                    </div>
                </div>

                <!-- Service & Warranty -->
                <div class="space-y-3 pt-2">
                    <h3 class="text-xs font-bold text-brandRed uppercase tracking-wider">Service & Warranty</h3>
                    <textarea id="serviceDescription" rows="2" placeholder="Repair / Replacement Details..." oninput="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none"></textarea>
                    
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-semibold text-gray-400 mb-1">Warranty Period</label>
                            <select id="warranty" onchange="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none">
                                <option value="No Warranty">No Warranty</option>
                                <option value="7 Days">7 Days</option>
                                <option value="14 Days (2 Week)">2 Week</option>
                                <option value="30 Days (1 Months)">1 Months</option>
                                <option value="60 Days (2 Months)">3 Months</option>
                                <option value="90 Days (3 Months)">3 Months</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-xs font-semibold text-gray-400 mb-1">Payment Status</label>
                            <select id="paymentStatus" onchange="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm font-semibold focus:border-brandRed focus:outline-none">
                                <option value="UNPAID" class="text-red-500">UNPAID</option>
                                <option value="PAID" class="text-green-500">PAID IN FULL</option>
                                <option value="PARTIAL" class="text-yellow-500">DEPOSIT PAID</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Payment Figures -->
                <div class="space-y-3 pt-2">
                    <h3 class="text-xs font-bold text-brandRed uppercase tracking-wider">Cost Breakdown (MYR)</h3>
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">Total Amount (RM)</label>
                            <input type="number" id="totalAmount" value="0.00" step="0.01" oninput="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none font-mono">
                        </div>
                        <div>
                            <label class="block text-xs text-gray-400 mb-1">Deposit Paid (RM)</label>
                            <input type="number" id="depositAmount" value="0.00" step="0.01" oninput="updatePreview()" class="w-full bg-black/50 border border-gray-700 rounded-lg px-3 py-2 text-sm focus:border-brandRed focus:outline-none font-mono">
                        </div>
                    </div>
                </div>

            </form>
        </section>

        <!-- Right Column: Document Preview & Action Center (7 Cols) -->
        <section class="lg:col-span-7 flex flex-col">
            
            <!-- Actions Header -->
            <div class="mb-4 bg-brandGray p-4 rounded-xl border border-gray-800 flex flex-wrap gap-3 items-center justify-between">
                <span class="text-sm font-semibold text-gray-300 flex items-center gap-2">
                    <i class="fa-solid fa-eye text-brandRed"></i> Live Preview
                </span>
                <div class="flex flex-wrap gap-2">
                    <button onclick="downloadPDF()" class="bg-brandRed hover:bg-brandRedDark text-white px-4 py-2 rounded-lg text-sm font-bold flex items-center gap-2 transition shadow-md shadow-brandRed/20">
                        <i class="fa-solid fa-file-pdf"></i> Save PDF
                    </button>
                    <button onclick="sendWhatsApp()" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-lg text-sm font-bold flex items-center gap-2 transition shadow-md shadow-green-600/20">
                        <i class="fa-brands fa-whatsapp text-lg"></i> Send WhatsApp
                    </button>
                </div>
            </div>

            <!-- PDF Container Container -->
            <div class="overflow-x-auto">
                <!-- Printable Invoice Canvas -->
                <div id="invoiceCanvas" class="bg-white text-gray-900 p-8 rounded-xl shadow-2xl min-w-[650px] text-sm relative border-t-8 border-brandRed">
                    
                    <!-- Top Branding -->
                    <div class="flex justify-between items-start border-b border-gray-200 pb-6 mb-6">
                        <div>
                            <div class="flex items-center gap-3 mb-2">
                                <div class="w-10 h-10 bg-red-600 text-white font-black text-xl flex items-center justify-center rounded-lg">
                                    SS
                                </div>
                                <div>
                                    <h1 class="text-xl font-black tracking-tight text-gray-900 leading-none">SS DYNAMIC FIX</h1>
                                    <span class="text-[10px] text-red-600 font-bold uppercase tracking-widest">Phone & Laptop Service Repair Lab</span>
                                </div>
                            </div>
                            <p class="text-xs text-gray-500 max-w-xs mt-1">
                                No 2, Bangunan Terminal Bas, Kompleks AG Sentral, 78000 Alor Gajah, Melaka<br>
                                Phone/WA: +601151453147
                            </p>
                        </div>
                        <div class="text-right">
                            <h2 id="prevDocType" class="text-3xl font-black text-gray-800 tracking-wider">INVOICE</h2>
                            <p class="text-xs text-gray-500 font-mono mt-1" id="prevInvNo">#INV-000000</p>
                            <p class="text-xs text-gray-500 mt-0.5" id="prevDate">Date: DD/MM/YYYY</p>
                        </div>
                    </div>

                    <!-- Client & Status Banner -->
                    <div class="grid grid-cols-2 gap-6 mb-6 bg-gray-50 p-4 rounded-lg border border-gray-100">
                        <div>
                            <span class="text-[10px] uppercase font-bold text-gray-400 tracking-wider block mb-1">Customer Details</span>
                            <h3 id="prevClientName" class="font-bold text-gray-800 text-base">Client Name</h3>
                            <p id="prevClientPhone" class="text-xs text-gray-600">+601XXXXXXXX</p>
                            <p id="prevClientEmail" class="text-xs text-gray-600">client@example.com</p>
                        </div>
                        <div class="text-right flex flex-col justify-between items-end">
                            <span class="text-[10px] uppercase font-bold text-gray-400 tracking-wider block">Payment Status</span>
                            <div id="prevBadge" class="inline-block px-3 py-1 rounded-full text-xs font-black bg-red-100 text-red-700 border border-red-300">
                                UNPAID
                            </div>
                        </div>
                    </div>

                    <!-- Device Specifications Panel -->
                    <div class="mb-6 border border-gray-200 rounded-lg overflow-hidden">
                        <div class="bg-gray-900 text-white px-4 py-1.5 text-xs font-bold uppercase tracking-wider flex justify-between">
                            <span>Device Specifications</span>
                            <span id="prevDeviceType">📱 Phone</span>
                        </div>
                        <div class="grid grid-cols-3 gap-2 p-3 bg-gray-50 text-xs">
                            <div>
                                <span class="text-gray-400 block text-[10px] uppercase">Brand</span>
                                <span id="prevBrand" class="font-semibold text-gray-800">-</span>
                            </div>
                            <div>
                                <span class="text-gray-400 block text-[10px] uppercase">Model</span>
                                <span id="prevModel" class="font-semibold text-gray-800">-</span>
                            </div>
                            <div>
                                <span class="text-gray-400 block text-[10px] uppercase">Serial / Code Model</span>
                                <span id="prevCodeModel" class="font-mono text-gray-800">-</span>
                            </div>
                        </div>
                    </div>

                    <!-- Table of Services -->
                    <table class="w-full mb-6 border-collapse">
                        <thead>
                            <tr class="border-b-2 border-gray-200 text-left text-[11px] font-bold text-gray-400 uppercase tracking-wider">
                                <th class="py-2">Description / Repair Service</th>
                                <th class="py-2 text-center">Warranty</th>
                                <th class="py-2 text-right">Amount</th>
                            </tr>
                        </thead>
                        <tbody class="text-xs divide-y divide-gray-100">
                            <tr>
                                <td class="py-3 pr-2">
                                    <p id="prevService" class="font-medium text-gray-800 whitespace-pre-line">General Inspection & Repair</p>
                                </td>
                                <td class="py-3 text-center font-semibold text-gray-700" id="prevWarranty">
                                    No Warranty
                                </td>
                                <td class="py-3 text-right font-mono font-bold text-gray-800" id="tableTotal">
                                    RM 0.00
                                </td>
                            </tr>
                        </tbody>
                    </table>

                    <!-- Calculation Totals -->
                    <div class="flex justify-end mb-8 border-t border-gray-100 pt-4">
                        <div class="w-1/2 space-y-2 text-xs">
                            <div class="flex justify-between text-gray-600">
                                <span>Subtotal:</span>
                                <span font-mono id="summarySubtotal">RM 0.00</span>
                            </div>
                            <div class="flex justify-between text-gray-600">
                                <span>Deposit Paid:</span>
                                <span font-mono id="summaryDeposit" class="text-green-600 font-semibold">- RM 0.00</span>
                            </div>
                            <div class="flex justify-between text-sm font-black border-t-2 border-gray-900 pt-2 text-gray-900">
                                <span>Balance Due:</span>
                                <span font-mono id="summaryBalance" class="text-red-600">RM 0.00</span>
                            </div>
                        </div>
                    </div>

                    <!-- Terms & Footer -->
                    <div class="border-t border-gray-200 pt-4 text-[10px] text-gray-500 space-y-1">
                        <p class="font-bold text-gray-700">Terms & Conditions:</p>
                        <ol class="list-decimal list-inside space-y-0.5">
                            <li>Warranty covers hardware replacement under standard usage. Liquid & drop damages void warranty.</li>
                            <li>Devices left unclaimed over 30 days post-repair may incur storage fees or disposal.</li>
                            <li>Devices left unclaimed over 30 days post-repair may incur storage fees or disposal.</li>
                            <li>Please present this digital invoice/receipt upon device pickup.</li>
                        </ol>
                        <div class="pt-4 text-center text-gray-400 text-[9px] uppercase tracking-widest font-semibold">
                            *** Thank You For Your Business - Expert Hardware Repair Solutions ***
                        </div>
                    </div>

                </div>
            </div>
        </section>

    </main>

    <!-- Logic Script -->
    <script>
        // Auto Generate Invoice ID & Set Current Date
        document.addEventListener('DOMContentLoaded', () => {
            const randomID = 'INV-' + Math.floor(100000 + Math.random() * 900000);
            document.getElementById('invNo').value = randomID;
            
            const today = new Date();
            const formattedDate = today.toLocaleDateString('en-GB', {
                day: '2-digit', month: '2-digit', year: 'numeric'
            });
            document.getElementById('prevDate').innerText = `Date: ${formattedDate}`;

            updatePreview();
        });

        // Live Update Function
        function updatePreview() {
            // Inputs
            const docType = document.getElementById('docType').value;
            const invNo = document.getElementById('invNo').value;
            const name = document.getElementById('clientName').value || 'Customer Name';
            const phone = document.getElementById('clientPhone').value || '+601XXXXXXXX';
            const email = document.getElementById('clientEmail').value || 'client@example.com';
            
            const deviceType = document.getElementById('deviceType').value;
            const brand = document.getElementById('deviceBrand').value || '-';
            const model = document.getElementById('deviceModel').value || '-';
            const codeModel = document.getElementById('codeModel').value || '-';
            
            const service = document.getElementById('serviceDescription').value || 'General Technical Diagnostic & Repair';
            const warranty = document.getElementById('warranty').value;
            const payStatus = document.getElementById('paymentStatus').value;
            
            const total = parseFloat(document.getElementById('totalAmount').value) || 0;
            const deposit = parseFloat(document.getElementById('depositAmount').value) || 0;
            const balance = total - deposit;

            // DOM Binding
            document.getElementById('prevDocType').innerText = docType;
            document.getElementById('prevInvNo').innerText = `#${invNo}`;
            document.getElementById('prevClientName').innerText = name;
            document.getElementById('prevClientPhone').innerText = phone;
            document.getElementById('prevClientEmail').innerText = email;

            document.getElementById('prevDeviceType').innerText = deviceType;
            document.getElementById('prevBrand').innerText = brand;
            document.getElementById('prevModel').innerText = model;
            document.getElementById('prevCodeModel').innerText = codeModel;

            document.getElementById('prevService').innerText = service;
            document.getElementById('prevWarranty').innerText = warranty;

            // Financial Binding
            document.getElementById('tableTotal').innerText = `RM ${total.toFixed(2)}`;
            document.getElementById('summarySubtotal').innerText = `RM ${total.toFixed(2)}`;
            document.getElementById('summaryDeposit').innerText = `- RM ${deposit.toFixed(2)}`;
            document.getElementById('summaryBalance').innerText = `RM ${balance.toFixed(2)}`;

            // Badge Display Logic
            const badge = document.getElementById('prevBadge');
            if (payStatus === 'PAID') {
                badge.className = 'inline-block px-3 py-1 rounded-full text-xs font-black bg-green-100 text-green-700 border border-green-300';
                badge.innerText = 'PAID IN FULL';
            } else if (payStatus === 'PARTIAL') {
                badge.className = 'inline-block px-3 py-1 rounded-full text-xs font-black bg-yellow-100 text-yellow-800 border border-yellow-300';
                badge.innerText = `DEPOSIT (RM ${deposit.toFixed(2)})`;
            } else {
                badge.className = 'inline-block px-3 py-1 rounded-full text-xs font-black bg-red-100 text-red-700 border border-red-300';
                badge.innerText = 'UNPAID';
            }
        }

        // WhatsApp Export Logic
        function sendWhatsApp() {
            const rawPhone = document.getElementById('clientPhone').value;
            if (!rawPhone || rawPhone.trim() === '') {
                alert('Please enter a valid WhatsApp/Phone Number.');
                return;
            }

            // Sanitize phone number for Malaysia (+60)
            let phone = rawPhone.replace(/[^0-9]/g, '');
            if (phone.startsWith('0')) {
                phone = '6' + phone;
            }

            const docType = document.getElementById('docType').value;
            const invNo = document.getElementById('invNo').value;
            const name = document.getElementById('clientName').value;
            const brand = document.getElementById('deviceBrand').value;
            const model = document.getElementById('deviceModel').value;
            const warranty = document.getElementById('warranty').value;
            const status = document.getElementById('paymentStatus').value;
            const total = parseFloat(document.getElementById('totalAmount').value) || 0;
            const deposit = parseFloat(document.getElementById('depositAmount').value) || 0;
            const balance = total - deposit;

            // Formulate WhatsApp Message Template
            let message = `*SS DYNAMIC FIX - OFFICIAL ${docType}*\n`;
            message += `----------------------------------------\n`;
            message += `*Ref No:* #${invNo}\n`;
            message += `*Customer Name:* ${name}\n`;
            message += `*Device:* ${brand} ${model}\n`;
            message += `*Warranty:* ${warranty}\n`;
            message += `----------------------------------------\n`;
            message += `*Total Cost:* RM ${total.toFixed(2)}\n`;
            message += `*Deposit Paid:* RM ${deposit.toFixed(2)}\n`;
            message += `*Balance Due:* RM ${balance.toFixed(2)}\n`;
            message += `*Status:* ${status}\n`;
            message += `----------------------------------------\n`;
            message += `Thank you for choosing SS Dynamic Fix! For inquiries, reply directly to this message or call +601151453147.`;

            const url = `https://wa.me/${phone}?text=${encodeURIComponent(message)}`;
            window.open(url, '_blank');
        }

        // PDF Generation via html2pdf
        function downloadPDF() {
            const element = document.getElementById('invoiceCanvas');
            const invNo = document.getElementById('invNo').value;
            const clientName = document.getElementById('clientName').value || 'Customer';

            const opt = {
                margin:       0.3,
                filename:     `${invNo}_${clientName.replace(/\s+/g, '_')}.pdf`,
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 2, useCORS: true },
                jsPDF:        { unit: 'in', format: 'letter', orientation: 'portrait' }
            };

            html2pdf().set(opt).from(element).save();
        }
    </script>
</body>
