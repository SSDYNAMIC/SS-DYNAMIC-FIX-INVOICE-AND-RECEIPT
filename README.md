# SS-DYNAMIC-FIX-INVOICE-AND-RECEIPT
SS DYNAMIC FIX INVOICE AND RECEIPT
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SS Dynamic Fix - Pro Invoice & Receipt Generator</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome 6 for Pro Icons -->
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
                        brandGray: '#18181B',
                        brandCard: '#27272A',
                    }
                }
            }
        }
    </script>

    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=JetBrains+Mono:wght@400;600;700&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0D0D0D;
            background-image: 
                radial-gradient(rgba(220, 38, 38, 0.12) 1px, transparent 0),
                linear-gradient(to bottom, rgba(13, 13, 13, 0.9), rgba(13, 13, 13, 0.98));
            background-size: 24px 24px, 100% 100%;
        }

        .font-mono {
            font-family: 'JetBrains Mono', monospace;
        }

        /* Printable Canvas Specific Constraints to Prevent PDF Cutoff */
        #invoiceCanvas {
            width: 800px !important;
            min-height: 1050px;
            margin: 0 auto;
            box-sizing: border-box;
            background-color: #ffffff !important;
            color: #000000 !important;
        }

        /* PDF capture override class */
        .pdf-export-mode {
            box-shadow: none !important;
            border: none !important;
            border-top: 8px solid #DC2626 !important;
            border-radius: 0 !important;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #18181B;
        }
        ::-webkit-scrollbar-thumb {
            background: #3F3F46;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #DC2626;
        }

        /* Direct Print Styles */
        @media print {
            body * {
                visibility: hidden;
            }
            #invoiceCanvas, #invoiceCanvas * {
                visibility: visible;
            }
            #invoiceCanvas {
                position: absolute;
                left: 0;
                top: 0;
                width: 100% !important;
                padding: 20px !important;
                box-shadow: none !important;
            }
            .no-print {
                display: none !important;
            }
        }
    </style>
</head>
<body class="text-gray-200 min-h-screen pb-12 antialiased">

    <!-- Header / Top Navbar -->
    <header class="bg-brandGray/90 backdrop-blur-md border-b border-brandRed/30 sticky top-0 z-50 shadow-2xl">
        <div class="max-w-7xl mx-auto px-4 py-3 flex flex-col md:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3">
                <div class="w-11 h-11 rounded-xl bg-gradient-to-br from-red-500 to-brandRedDark flex items-center justify-center text-xl font-black text-white shadow-lg shadow-brandRed/40 border border-red-400/30">
                    SS
                </div>
                <div>
                    <h1 class="text-xl font-black tracking-wider text-white flex items-center gap-2">
                        SS DYNAMIC FIX <span class="text-[10px] bg-brandRed px-2 py-0.5 rounded text-white font-bold uppercase tracking-widest">Expert Repair Lab</span>
                    </h1>
                    <p class="text-xs text-gray-400">Phone & Laptop Technical Service Center • SSM: MA0200450</p>
                </div>
            </div>
            
            <div class="flex items-center gap-6 text-xs text-gray-300">
                <div class="hidden sm:block text-right">
                    <p><i class="fa-solid fa-location-dot text-brandRed mr-1.5"></i> Alor Gajah Sentral, Melaka</p>
                    <p><i class="fa-solid fa-phone text-brandRed mr-1.5"></i> +6011-5145 3147</p>
                </div>
                <div class="h-8 w-px bg-gray-800 hidden sm:block"></div>
                <button onclick="resetForm()" class="bg-zinc-800 hover:bg-zinc-700 text-gray-300 px-3 py-1.5 rounded-lg text-xs font-semibold transition border border-zinc-700 flex items-center gap-1.5">
                    <i class="fa-solid fa-rotate-left text-brandRed"></i> Reset
                </button>
            </div>
        </div>
    </header>

    <!-- Main Workspace Container -->
    <main class="max-w-7xl mx-auto px-4 mt-6 grid grid-cols-1 lg:grid-cols-12 gap-8">
        
        <!-- Left Column: Input Panel (5 Cols) -->
        <section class="lg:col-span-5 space-y-6">
            
            <div class="bg-brandGray/90 border border-zinc-800 p-5 rounded-2xl shadow-2xl backdrop-blur-sm">
                <div class="flex justify-between items-center mb-5 border-b border-zinc-800 pb-3">
                    <h2 class="text-base font-bold text-white flex items-center gap-2">
                        <i class="fa-solid fa-sliders text-brandRed"></i> Service Invoice Config
                    </h2>
                    <span class="text-[11px] text-gray-400 bg-black/40 px-2.5 py-1 rounded-full font-mono">Form Control</span>
                </div>

                <form id="invoiceForm" onsubmit="event.preventDefault();" class="space-y-4">
                    
                    <!-- Document Settings -->
                    <div class="grid grid-cols-3 gap-3">
                        <div>
                            <label class="block text-[11px] font-semibold text-gray-400 mb-1">Doc Type</label>
                            <select id="docType" onchange="updatePreview()" class="w-full bg-black/60 border border-zinc-700 rounded-lg px-2.5 py-2 text-xs text-white focus:border-brandRed focus:outline-none transition">
                                <option value="INVOICE">INVOICE</option>
                                <option value="OFFICIAL RECEIPT">RECEIPT</option>
                                <option value="QUOTATION">QUOTATION</option>
                            </select>
                        </div>
                        <div>
                            <label class="block text-[11px] font-semibold text-gray-400 mb-1">Invoice No.</label>
                            <input type="text" id="invNo" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-brandRed font-bold rounded-lg px-2.5 py-2 text-xs font-mono focus:border-brandRed focus:outline-none">
                        </div>
                        <div>
                            <label class="block text-[11px] font-semibold text-gray-400 mb-1">Date</label>
                            <input type="date" id="docDate" onchange="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-gray-300 rounded-lg px-2 py-2 text-xs focus:border-brandRed focus:outline-none">
                        </div>
                    </div>

                    <!-- Customer Information -->
                    <div class="space-y-2.5 pt-2 border-t border-zinc-800">
                        <h3 class="text-[11px] font-extrabold text-brandRed uppercase tracking-wider flex items-center gap-1.5">
                            <i class="fa-solid fa-user-gear"></i> Customer Profile
                        </h3>
                        <div>
                            <input type="text" id="clientName" placeholder="Customer Name *" oninput="updatePreview()" required class="w-full bg-black/60 border border-zinc-700 rounded-lg px-3 py-2 text-xs text-white focus:border-brandRed focus:outline-none placeholder-gray-500">
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <input type="tel" id="clientPhone" placeholder="Phone / WhatsApp *" oninput="updatePreview()" required class="w-full bg-black/60 border border-zinc-700 rounded-lg px-3 py-2 text-xs text-white focus:border-brandRed focus:outline-none placeholder-gray-500">
                            <input type="email" id="clientEmail" placeholder="Email Address (Optional)" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 rounded-lg px-3 py-2 text-xs text-white focus:border-brandRed focus:outline-none placeholder-gray-500">
                        </div>
                    </div>

                    <!-- Device Information -->
                    <div class="space-y-2.5 pt-2 border-t border-zinc-800">
                        <h3 class="text-[11px] font-extrabold text-brandRed uppercase tracking-wider flex items-center gap-1.5">
                            <i class="fa-solid fa-microchip"></i> Hardware Diagnostics
                        </h3>
                        <div class="grid grid-cols-3 gap-3">
                            <div>
                                <select id="deviceType" onchange="updatePreview()" class="w-full bg-black/60 border border-zinc-700 rounded-lg px-2 py-2 text-xs text-white focus:border-brandRed focus:outline-none">
                                    <option value="📱 Smartphone">📱 Smartphone</option>
                                    <option value="💻 Laptop">💻 Laptop</option>
                                    <option value="🖥️ Desktop PC">🖥️ Desktop PC</option>
                                    <option value="Tablet / iPad">Tablet / iPad</option>
                                </select>
                            </div>
                            <div class="col-span-2">
                                <input type="text" id="deviceBrand" placeholder="Brand & Series (e.g., Apple, Asus ROG)" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 rounded-lg px-3 py-2 text-xs text-white focus:border-brandRed focus:outline-none placeholder-gray-500">
                            </div>
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <input type="text" id="deviceModel" placeholder="Model Number / Spec" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 rounded-lg px-3 py-2 text-xs text-white focus:border-brandRed focus:outline-none placeholder-gray-500">
                            <input type="text" id="codeModel" placeholder="Serial No / IMEI / Passcode" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 rounded-lg px-3 py-2 text-xs text-white focus:border-brandRed focus:outline-none placeholder-gray-500">
                        </div>
                    </div>

                    <!-- Dynamic Line Items -->
                    <div class="space-y-2.5 pt-2 border-t border-zinc-800">
                        <div class="flex justify-between items-center">
                            <h3 class="text-[11px] font-extrabold text-brandRed uppercase tracking-wider flex items-center gap-1.5">
                                <i class="fa-solid fa-list-check"></i> Services & Spare Parts
                            </h3>
                            <button type="button" onclick="addServiceRow()" class="text-[11px] bg-brandRed/20 hover:bg-brandRed text-red-300 hover:text-white px-2.5 py-1 rounded transition font-semibold border border-brandRed/40">
                                + Add Row
                            </button>
                        </div>
                        
                        <div id="serviceRowsContainer" class="space-y-2 max-h-56 overflow-y-auto pr-1">
                            <!-- Dynamic Rows Injected Here -->
                        </div>
                    </div>

                    <!-- Service Terms & Payment Status -->
                    <div class="space-y-2.5 pt-2 border-t border-zinc-800">
                        <h3 class="text-[11px] font-extrabold text-brandRed uppercase tracking-wider flex items-center gap-1.5">
                            <i class="fa-solid fa-shield-halved"></i> Warranty & Settlement
                        </h3>
                        
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-[11px] font-semibold text-gray-400 mb-1">Warranty Period</label>
                                <select id="warranty" onchange="updatePreview()" class="w-full bg-black/60 border border-zinc-700 rounded-lg px-2.5 py-2 text-xs text-white focus:border-brandRed focus:outline-none">
                                    <option value="NO WARRANTY">NO WARRANTY</option>
                                    <option value="7 Days Warranty">7 Days Warranty</option>
                                    <option value="14 Days Warranty">14 Days Warranty</option>
                                    <option value="30 Days (1 Month)">30 Days (1 Month)</option>
                                    <option value="60 Days (2 Months)">60 Days (2 Months)</option>
                                    <option value="90 Days (3 Months)">90 Days (3 Months)</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-[11px] font-semibold text-gray-400 mb-1">Payment Status</label>
                                <select id="paymentStatus" onchange="updatePreview()" class="w-full bg-black/60 border border-zinc-700 rounded-lg px-2.5 py-2 text-xs font-bold text-white focus:border-brandRed focus:outline-none">
                                    <option value="UNPAID" class="text-red-400">UNPAID</option>
                                    <option value="PAID" class="text-green-400">PAID IN FULL</option>
                                    <option value="PARTIAL" class="text-yellow-400">DEPOSIT PAID</option>
                                </select>
                            </div>
                        </div>

                        <div class="grid grid-cols-2 gap-3 pt-1">
                            <div>
                                <label class="block text-[11px] text-gray-400 mb-1">Deposit Paid (MYR)</label>
                                <input type="number" id="depositAmount" value="0.00" step="0.01" min="0" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-green-400 font-mono font-bold rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none">
                            </div>
                            <div>
                                <label class="block text-[11px] text-gray-400 mb-1">Discount (MYR)</label>
                                <input type="number" id="discountAmount" value="0.00" step="0.01" min="0" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-yellow-400 font-mono font-bold rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none">
                            </div>
                        </div>
                    </div>

                </form>
            </div>
        </section>

        <!-- Right Column: Document Preview & Action Center (7 Cols) -->
        <section class="lg:col-span-7 flex flex-col items-center">
            
            <!-- Sticky Actions Header Bar -->
            <div class="w-full mb-4 bg-brandGray p-3.5 rounded-2xl border border-zinc-800 flex flex-wrap gap-3 items-center justify-between shadow-xl">
                <span class="text-xs font-semibold text-gray-300 flex items-center gap-2">
                    <i class="fa-solid fa-eye text-brandRed animate-pulse"></i> Document Live Canvas
                </span>
                <div class="flex flex-wrap gap-2">
                    <button onclick="window.print()" class="bg-zinc-800 hover:bg-zinc-700 text-gray-200 px-3.5 py-2 rounded-xl text-xs font-bold flex items-center gap-2 transition border border-zinc-700">
                        <i class="fa-solid fa-print"></i> Print
                    </button>
                    <button onclick="downloadPDF()" class="bg-brandRed hover:bg-brandRedDark text-white px-4 py-2 rounded-xl text-xs font-extrabold flex items-center gap-2 transition shadow-lg shadow-brandRed/30">
                        <i class="fa-solid fa-file-pdf"></i> Download PDF
                    </button>
                    <button onclick="sendWhatsApp()" class="bg-emerald-600 hover:bg-emerald-700 text-white px-4 py-2 rounded-xl text-xs font-extrabold flex items-center gap-2 transition shadow-lg shadow-emerald-600/30">
                        <i class="fa-brands fa-whatsapp text-sm"></i> WhatsApp
                    </button>
                </div>
            </div>

            <!-- Scrollable Viewport Wrapper for Visual Responsive Display -->
            <div class="w-full overflow-x-auto pb-6 flex justify-center">
                
                <!-- PRINTABLE A4 CANVAS TARGET -->
                <div id="invoiceCanvas" class="p-8 relative rounded-xl border-t-8 border-brandRed shadow-2xl transition-all">
                    
                    <!-- Top Branding Header -->
                    <div class="flex justify-between items-start border-b-2 border-gray-200 pb-5 mb-5">
                        <div class="space-y-1">
                            <div class="flex items-center gap-3">
                                <div class="w-12 h-12 bg-red-600 text-white font-black text-2xl flex items-center justify-center rounded-xl shadow-md">
                                    SS
                                </div>
                                <div>
                                    <h1 class="text-2xl font-black tracking-tight text-gray-900 leading-none">SS DYNAMIC FIX</h1>
                                    <p class="text-[10px] text-red-600 font-extrabold uppercase tracking-widest mt-1">Expert Phone & Laptop Repair Laboratory</p>
                                </div>
                            </div>
                            <p class="text-[11px] text-gray-600 leading-relaxed pt-2">
                                No 2, Bangunan Terminal Bas, Kompleks AG Sentral,<br>
                                78000 Alor Gajah, Melaka • <strong>SSM: MA0200450</strong><br>
                                <span class="text-gray-800">Phone/WhatsApp: +6011-5145 3147</span>
                            </p>
                        </div>

                        <div class="text-right">
                            <h2 id="prevDocType" class="text-3xl font-black text-gray-900 tracking-tight">INVOICE</h2>
                            <div class="mt-1 space-y-0.5">
                                <p class="text-xs font-mono font-bold text-red-600" id="prevInvNo">#INV-000000</p>
                                <p class="text-xs text-gray-500 font-medium" id="prevDate">Date: DD/MM/YYYY</p>
                            </div>
                        </div>
                    </div>

                    <!-- Customer & Payment Status Banner -->
                    <div class="grid grid-cols-2 gap-4 mb-5 bg-gray-50 p-4 rounded-xl border border-gray-200">
                        <div>
                            <span class="text-[9px] font-black uppercase text-gray-400 tracking-wider block mb-1">Customer Info</span>
                            <h3 id="prevClientName" class="font-bold text-gray-900 text-sm leading-tight">Customer Name</h3>
                            <p id="prevClientPhone" class="text-xs text-gray-600 mt-0.5 font-medium">+601XXXXXXXX</p>
                            <p id="prevClientEmail" class="text-xs text-gray-500">client@example.com</p>
                        </div>
                        <div class="text-right flex flex-col justify-between items-end">
                            <span class="text-[9px] font-black uppercase text-gray-400 tracking-wider block">Payment Status</span>
                            <div id="prevBadge" class="inline-block px-3 py-1 rounded-md text-xs font-black bg-red-100 text-red-700 border border-red-300">
                                UNPAID
                            </div>
                        </div>
                    </div>

                    <!-- Device Specifications Bar -->
                    <div class="mb-5 border border-gray-200 rounded-xl overflow-hidden shadow-sm">
                        <div class="bg-gray-900 text-white px-4 py-1.5 text-[11px] font-bold uppercase tracking-wider flex justify-between items-center">
                            <span>Hardware Diagnostic Profile</span>
                            <span id="prevDeviceType" class="text-red-400">📱 Smartphone</span>
                        </div>
                        <div class="grid grid-cols-3 gap-2 p-3 bg-white text-xs">
                            <div>
                                <span class="text-gray-400 block text-[9px] font-bold uppercase">Brand / Series</span>
                                <span id="prevBrand" class="font-semibold text-gray-800">-</span>
                            </div>
                            <div>
                                <span class="text-gray-400 block text-[9px] font-bold uppercase">Model Spec</span>
                                <span id="prevModel" class="font-semibold text-gray-800">-</span>
                            </div>
                            <div>
                                <span class="text-gray-400 block text-[9px] font-bold uppercase">Serial / IMEI</span>
                                <span id="prevCodeModel" class="font-mono text-gray-800">-</span>
                            </div>
                        </div>
                    </div>

                    <!-- Itemized Breakdown Table -->
                    <table class="w-full mb-5 border-collapse">
                        <thead>
                            <tr class="border-b-2 border-gray-900 text-left text-[10px] font-black text-gray-500 uppercase tracking-wider">
                                <th class="py-2 w-1/2">Repair Description & Services</th>
                                <th class="py-2 text-center w-1/6">Qty</th>
                                <th class="py-2 text-right w-1/6">Unit Price</th>
                                <th class="py-2 text-right w-1/6">Total</th>
                            </tr>
                        </thead>
                        <tbody id="prevServiceTableBody" class="text-xs divide-y divide-gray-200">
                            <!-- Dynamic Table Rows Rendered via JS -->
                        </tbody>
                    </table>

                    <!-- Financial Summary Breakdown -->
                    <div class="flex justify-between items-start mb-6 border-t border-gray-200 pt-4">
                        <div class="w-1/2 pr-4 space-y-1">
                            <div class="bg-red-50/60 p-2.5 rounded-lg border border-red-100">
                                <span class="text-[10px] font-extrabold uppercase text-red-800 block">Warranty Period:</span>
                                <p id="prevWarranty" class="text-xs font-bold text-red-700">NO WARRANTY</p>
                            </div>
                        </div>

                        <div class="w-1/2 space-y-1.5 text-xs">
                            <div class="flex justify-between text-gray-600">
                                <span>Subtotal:</span>
                                <span class="font-mono font-medium text-gray-800" id="summarySubtotal">RM 0.00</span>
                            </div>
                            <div class="flex justify-between text-gray-600" id="summaryDiscountRow">
                                <span>Discount:</span>
                                <span class="font-mono font-medium text-yellow-600" id="summaryDiscount">- RM 0.00</span>
                            </div>
                            <div class="flex justify-between text-gray-600">
                                <span>Deposit Paid:</span>
                                <span class="font-mono font-semibold text-emerald-600" id="summaryDeposit">- RM 0.00</span>
                            </div>
                            <div class="flex justify-between text-sm font-black border-t-2 border-gray-900 pt-2 text-gray-900">
                                <span>Balance Due:</span>
                                <span class="font-mono text-base text-red-600" id="summaryBalance">RM 0.00</span>
                            </div>
                        </div>
                    </div>

                    <!-- Terms & Official Sign-off -->
                    <div class="border-t border-gray-200 pt-4 text-[9.5px] text-gray-500 space-y-3">
                        <div>
                            <p class="font-extrabold text-gray-800 uppercase tracking-wider mb-1">Terms & Technical Policy:</p>
                            <ol class="list-decimal list-inside space-y-0.5 leading-relaxed">
                                <li>Warranty applies strictly to replaced parts under normal operational usage. Water contact, drop impact, or physical damage voids warranty immediately.</li>
                                <li>Devices left unclaimed over 30 days post-repair completion notice may incur storage fees or disposition without further notice.</li>
                                <li>Please present this digital invoice or physical receipt upon device pickup.</li>
                            </ol>
                        </div>

                        <!-- Signatures Box -->
                        <div class="pt-6 grid grid-cols-2 gap-8 text-center text-xs">
                            <div class="border-t border-dashed border-gray-300 pt-2">
                                <p class="font-bold text-gray-700">Customer Signature</p>
                                <p class="text-[9px] text-gray-400">Received in good condition</p>
                            </div>
                            <div class="border-t border-dashed border-gray-300 pt-2">
                                <p class="font-bold text-gray-900">SS DYNAMIC FIX</p>
                                <p class="text-[9px] text-gray-400">Authorized Technical Manager</p>
                            </div>
                        </div>

                        <div class="pt-2 text-center text-gray-400 text-[8.5px] uppercase tracking-widest font-extrabold border-t border-gray-100">
                            *** Thank You For Choosing SS Dynamic Fix - Quality Hardware Repairs ***
                        </div>
                    </div>

                </div>
            </div>
        </section>

    </main>

    <!-- Master Application Controller Script -->
    <script>
        // State Management for Dynamic Line Items
        let lineItems = [
            { description: 'General Diagnostic & Hardware Service', qty: 1, price: 0.00 }
        ];

        document.addEventListener('DOMContentLoaded', () => {
            // Set Default Invoice Number and Current Date
            const randomID = 'INV-' + new Date().getFullYear() + Math.floor(10000 + Math.random() * 90000);
            document.getElementById('invNo').value = randomID;
            
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('docDate').value = today;

            renderServiceRows();
            updatePreview();
        });

        // Add New Line Item Row
        function addServiceRow() {
            lineItems.push({ description: '', qty: 1, price: 0.00 });
            renderServiceRows();
            updatePreview();
        }

        // Remove Line Item Row
        function removeServiceRow(index) {
            if (lineItems.length === 1) {
                alert('Invoice must contain at least one service item.');
                return;
            }
            lineItems.splice(index, 1);
            renderServiceRows();
            updatePreview();
        }

        // Render Service Form Controls
        function renderServiceRows() {
            const container = document.getElementById('serviceRowsContainer');
            container.innerHTML = '';

            lineItems.forEach((item, index) => {
                const row = document.createElement('div');
                row.className = 'grid grid-cols-12 gap-1.5 items-center bg-black/40 p-2 rounded-lg border border-zinc-800';
                row.innerHTML = `
                    <div class="col-span-6">
                        <input type="text" placeholder="Item / Repair details" value="${item.description}" 
                            oninput="updateItem(${index}, 'description', this.value)" 
                            class="w-full bg-zinc-900 border border-zinc-700 rounded px-2 py-1 text-xs text-white focus:border-brandRed focus:outline-none">
                    </div>
                    <div class="col-span-2">
                        <input type="number" min="1" placeholder="Qty" value="${item.qty}" 
                            oninput="updateItem(${index}, 'qty', this.value)" 
                            class="w-full bg-zinc-900 border border-zinc-700 rounded px-1.5 py-1 text-xs text-center text-white focus:border-brandRed focus:outline-none font-mono">
                    </div>
                    <div class="col-span-3">
                        <input type="number" step="0.01" min="0" placeholder="RM" value="${item.price}" 
                            oninput="updateItem(${index}, 'price', this.value)" 
                            class="w-full bg-zinc-900 border border-zinc-700 rounded px-1.5 py-1 text-xs text-right text-white focus:border-brandRed focus:outline-none font-mono">
                    </div>
                    <div class="col-span-1 text-center">
                        <button type="button" onclick="removeServiceRow(${index})" class="text-red-400 hover:text-red-600 transition">
                            <i class="fa-solid fa-trash-can text-xs"></i>
                        </button>
                    </div>
                `;
                container.appendChild(row);
            });
        }

        // Update Item Array Values
        function updateItem(index, field, value) {
            if (field === 'qty') {
                lineItems[index].qty = parseInt(value) || 0;
            } else if (field === 'price') {
                lineItems[index].price = parseFloat(value) || 0;
            } else {
                lineItems[index].description = value;
            }
            updatePreview();
        }

        // Real-Time Canvas Update Handler
        function updatePreview() {
            // Dynamic Form Values
            const docType = document.getElementById('docType').value;
            const invNo = document.getElementById('invNo').value || 'INV-0000';
            const rawDate = document.getElementById('docDate').value;
            
            let formattedDate = 'DD/MM/YYYY';
            if (rawDate) {
                const [y, m, d] = rawDate.split('-');
                formattedDate = `${d}/${m}/${y}`;
            }

            const clientName = document.getElementById('clientName').value || 'Valued Customer';
            const clientPhone = document.getElementById('clientPhone').value || '+601XXXXXXXX';
            const clientEmail = document.getElementById('clientEmail').value || 'client@example.com';
            
            const deviceType = document.getElementById('deviceType').value;
            const brand = document.getElementById('deviceBrand').value || '-';
            const model = document.getElementById('deviceModel').value || '-';
            const codeModel = document.getElementById('codeModel').value || '-';
            
            const warranty = document.getElementById('warranty').value;
            const payStatus = document.getElementById('paymentStatus').value;
            
            const deposit = parseFloat(document.getElementById('depositAmount').value) || 0;
            const discount = parseFloat(document.getElementById('discountAmount').value) || 0;

            // Header & Customer Bindings
            document.getElementById('prevDocType').innerText = docType;
            document.getElementById('prevInvNo').innerText = `#${invNo}`;
            document.getElementById('prevDate').innerText = `Date: ${formattedDate}`;
            document.getElementById('prevClientName').innerText = clientName;
            document.getElementById('prevClientPhone').innerText = clientPhone;
            document.getElementById('prevClientEmail').innerText = clientEmail;

            // Device Specs
            document.getElementById('prevDeviceType').innerText = deviceType;
            document.getElementById('prevBrand').innerText = brand;
            document.getElementById('prevModel').innerText = model;
            document.getElementById('prevCodeModel').innerText = codeModel;
            document.getElementById('prevWarranty').innerText = warranty;

            // Table Render
            const tableBody = document.getElementById('prevServiceTableBody');
            tableBody.innerHTML = '';

            let subtotal = 0;

            lineItems.forEach(item => {
                const itemTotal = (item.qty || 0) * (item.price || 0);
                subtotal += itemTotal;

                const tr = document.createElement('tr');
                tr.className = 'border-b border-gray-100';
                tr.innerHTML = `
                    <td class="py-2.5 pr-2 font-medium text-gray-800">${item.description || 'General Service'}</td>
                    <td class="py-2.5 text-center font-mono text-gray-600">${item.qty}</td>
                    <td class="py-2.5 text-right font-mono text-gray-600">RM ${item.price.toFixed(2)}</td>
                    <td class="py-2.5 text-right font-mono font-bold text-gray-900">RM ${itemTotal.toFixed(2)}</td>
                `;
                tableBody.appendChild(tr);
            });

            // Financial Calculations
            const finalTotal = Math.max(0, subtotal - discount);
            const balanceDue = finalTotal - deposit;

            document.getElementById('summarySubtotal').innerText = `RM ${subtotal.toFixed(2)}`;
            document.getElementById('summaryDiscount').innerText = `- RM ${discount.toFixed(2)}`;
            document.getElementById('summaryDeposit').innerText = `- RM ${deposit.toFixed(2)}`;
            document.getElementById('summaryBalance').innerText = `RM ${balanceDue.toFixed(2)}`;

            // Badge Display Logic
            const badge = document.getElementById('prevBadge');
            if (payStatus === 'PAID') {
                badge.className = 'inline-block px-3 py-1 rounded-md text-xs font-black bg-emerald-100 text-emerald-800 border border-emerald-300';
                badge.innerText = 'PAID IN FULL';
            } else if (payStatus === 'PARTIAL') {
                badge.className = 'inline-block px-3 py-1 rounded-md text-xs font-black bg-yellow-100 text-yellow-800 border border-yellow-300';
                badge.innerText = `DEPOSIT (RM ${deposit.toFixed(2)})`;
            } else {
                badge.className = 'inline-block px-3 py-1 rounded-md text-xs font-black bg-red-100 text-red-700 border border-red-300';
                badge.innerText = 'UNPAID';
            }
        }

        // WhatsApp Direct Formatting
        function sendWhatsApp() {
            const rawPhone = document.getElementById('clientPhone').value;
            if (!rawPhone || rawPhone.trim() === '') {
                alert('Please provide a valid WhatsApp contact number.');
                return;
            }

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
            
            let subtotal = 0;
            lineItems.forEach(i => subtotal += (i.qty * i.price));
            const discount = parseFloat(document.getElementById('discountAmount').value) || 0;
            const deposit = parseFloat(document.getElementById('depositAmount').value) || 0;
            const balance = subtotal - discount - deposit;

            let msg = `*SS DYNAMIC FIX - ${docType}*\n`;
            msg += `----------------------------------------\n`;
            msg += `*Ref No:* #${invNo}\n`;
            msg += `*Customer:* ${name}\n`;
            msg += `*Device:* ${brand} ${model}\n`;
            msg += `*Warranty:* ${warranty}\n`;
            msg += `----------------------------------------\n`;
            msg += `*Services Rendered:*\n`;
            lineItems.forEach(item => {
                msg += `• ${item.description} (x${item.qty}) - RM ${(item.qty * item.price).toFixed(2)}\n`;
            });
            msg += `----------------------------------------\n`;
            if (discount > 0) msg += `*Discount:* RM ${discount.toFixed(2)}\n`;
            msg += `*Deposit Paid:* RM ${deposit.toFixed(2)}\n`;
            msg += `*Balance Due:* RM ${balance.toFixed(2)}\n`;
            msg += `*Status:* ${status}\n`;
            msg += `----------------------------------------\n`;
            msg += `Thank you for choosing SS Dynamic Fix!\n`;
            msg += `📍 Terminal Bas Alor Gajah, Melaka\n`;
            msg += `📞 +6011-5145 3147`;

            window.open(`https://wa.me/${phone}?text=${encodeURIComponent(msg)}`, '_blank');
        }

        // PERFECTED PDF GENERATION METHOD (No Cutoffs / Exact A4 Boundaries)
        async function downloadPDF() {
            const element = document.getElementById('invoiceCanvas');
            const invNo = document.getElementById('invNo').value || 'INVOICE';
            const clientName = document.getElementById('clientName').value || 'CUSTOMER';

            // Apply export mode class
            element.classList.add('pdf-export-mode');

            const opt = {
                margin:       [8, 8, 8, 8], // top, left, bottom, right in mm
                filename:     `${invNo}_${clientName.replace(/\s+/g, '_')}.pdf`,
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  {
                    scale: 2,
                    useCORS: true,
                    logging: false,
                    scrollX: 0,
                    scrollY: 0,
                    windowWidth: 800
                },
                jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' },
                pagebreak:    { mode: ['avoid-all', 'css', 'legacy'] }
            };

            try {
                await html2pdf().set(opt).from(element).save();
            } catch (error) {
                console.error('PDF Generation Error:', error);
                alert('An error occurred during PDF rendering. Please try again.');
            } finally {
                element.classList.remove('pdf-export-mode');
            }
        }

        // Reset Form
        function resetForm() {
            if (confirm('Are you sure you want to reset all form fields?')) {
                document.getElementById('invoiceForm').reset();
                lineItems = [{ description: 'General Diagnostic & Hardware Service', qty: 1, price: 0.00 }];
                
                const randomID = 'INV-' + new Date().getFullYear() + Math.floor(10000 + Math.random() * 90000);
                document.getElementById('invNo').value = randomID;
                document.getElementById('docDate').value = new Date().toISOString().split('T')[0];

                renderServiceRows();
                updatePreview();
            }
        }
    </script>
</body>
