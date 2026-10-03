# SS-DYNAMIC-FIX-INVOICE-AND-RECEIPT
SS DYNAMIC FIX INVOICE AND RECEIPT
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SS Dynamic Fix - Pro Invoice & Receipt Suite</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome 6 Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- html2pdf.js Bundle -->
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
                        brandCard: '#27272A'
                    }
                }
            }
        }
    </script>

    <style>
        /* Modern Tech Grid Pattern Background */
        body {
            background-color: #0D0D0D;
            background-image: 
                radial-gradient(rgba(220, 38, 38, 0.12) 1px, transparent 0),
                linear-gradient(to bottom, rgba(13, 13, 13, 0.88), rgba(13, 13, 13, 0.98));
            background-size: 28px 28px, 100% 100%;
            background-attachment: fixed;
        }

        /* Precise A4 Paper Canvas Dimensions for PDF Generation */
        .a4-canvas {
            width: 210mm;
            min-height: 297mm;
            margin: auto;
            background: #ffffff !important;
            color: #111827 !important;
            box-sizing: border-box;
        }

        /* Screen Preview Responsive Scaler */
        @media screen and (max-width: 1024px) {
            .preview-scroll-wrapper {
                overflow-x: auto;
            }
        }

        /* Print Media Styles */
        @media print {
            body {
                background: none !important;
                padding: 0 !important;
            }
            .no-print {
                display: none !important;
            }
            .a4-canvas {
                box-shadow: none !important;
                border: none !important;
                width: 100% !important;
                height: auto !important;
            }
        }
    </style>
</head>
<body class="text-gray-200 font-sans min-h-screen pb-16 antialiased selection:bg-brandRed selection:text-white">

    <!-- Navbar Header -->
    <header class="bg-brandGray/90 border-b border-brandRed/30 sticky top-0 z-50 backdrop-blur-md shadow-2xl">
        <div class="max-w-7xl mx-auto px-4 py-3.5 flex flex-col md:flex-row justify-between items-center gap-4">
            <div class="flex items-center gap-3.5">
                <div class="w-11 h-11 rounded-xl bg-gradient-to-tr from-brandRedDark to-brandRed flex items-center justify-center text-xl font-black text-white shadow-lg shadow-brandRed/30 border border-red-500/30">
                    SS
                </div>
                <div>
                    <h1 class="text-xl font-black tracking-wider text-white flex items-center gap-2">
                        SS DYNAMIC FIX 
                        <span class="text-[10px] bg-brandRed/20 text-red-400 border border-brandRed/40 px-2 py-0.5 rounded font-bold uppercase tracking-wider">Pro Tech Suite</span>
                    </h1>
                    <p class="text-xs text-gray-400">Smartphones & Laptops Hardware Repair Specialist</p>
                </div>
            </div>
            <div class="text-right text-xs text-gray-300 flex flex-wrap gap-4 items-center">
                <span class="bg-black/40 px-3 py-1.5 rounded-lg border border-gray-800">
                    <i class="fa-solid fa-location-dot text-brandRed mr-1.5"></i> Kompleks AG Sentral, Alor Gajah, Melaka
                </span>
                <span class="bg-black/40 px-3 py-1.5 rounded-lg border border-gray-800 font-mono">
                    <i class="fa-solid fa-phone text-brandRed mr-1.5"></i> +601151453147
                </span>
            </div>
        </div>
    </header>

    <!-- Main Content Container -->
    <main class="max-w-7xl mx-auto px-4 mt-6 grid grid-cols-1 lg:grid-cols-12 gap-8">
        
        <!-- Input Form Section (5 Columns) -->
        <section class="lg:col-span-5 bg-brandGray border border-zinc-800 p-5 rounded-2xl shadow-2xl space-y-5">
            
            <div class="flex items-center justify-between border-b border-zinc-800 pb-3">
                <h2 class="text-lg font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-sliders text-brandRed"></i> Billing & Repair Control
                </h2>
                <button type="button" onclick="resetForm()" class="text-xs text-gray-400 hover:text-red-400 transition flex items-center gap-1">
                    <i class="fa-solid fa-rotate-right"></i> Reset
                </button>
            </div>

            <form id="invoiceForm" onsubmit="event.preventDefault();" class="space-y-4">
                
                <!-- Document Classification -->
                <div class="grid grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 mb-1">Document Type</label>
                        <select id="docType" onchange="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-white rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none transition">
                            <option value="INVOICE">INVOICE</option>
                            <option value="OFFICIAL RECEIPT">OFFICIAL RECEIPT</option>
                            <option value="QUOTATION">QUOTATION</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 mb-1">Reference No.</label>
                        <div class="relative">
                            <input type="text" id="invNo" readonly class="w-full bg-black/40 border border-zinc-800 text-red-400 rounded-lg px-3 py-2 text-xs font-mono font-bold cursor-not-allowed">
                            <button type="button" onclick="regenerateID()" class="absolute right-2 top-2 text-gray-500 hover:text-white text-xs" title="Generate New ID">
                                <i class="fa-solid fa-arrows-rotate"></i>
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Customer Details -->
                <div class="space-y-2.5 pt-1">
                    <h3 class="text-[11px] font-bold text-brandRed uppercase tracking-wider flex items-center gap-1.5">
                        <i class="fa-solid fa-user text-[10px]"></i> Customer Details
                    </h3>
                    <input type="text" id="clientName" placeholder="Customer Full Name *" oninput="updatePreview()" required class="w-full bg-black/60 border border-zinc-700 text-white rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none transition">
                    <div class="grid grid-cols-2 gap-3">
                        <input type="tel" id="clientPhone" placeholder="Phone / WhatsApp *" oninput="updatePreview()" required class="w-full bg-black/60 border border-zinc-700 text-white rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none transition">
                        <input type="email" id="clientEmail" placeholder="Email (Optional)" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-white rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none transition">
                    </div>
                </div>

                <!-- Device Specifications -->
                <div class="space-y-2.5 pt-1">
                    <h3 class="text-[11px] font-bold text-brandRed uppercase tracking-wider flex items-center gap-1.5">
                        <i class="fa-solid fa-laptop-medical text-[10px]"></i> Device Information
                    </h3>
                    <div class="grid grid-cols-3 gap-2.5">
                        <select id="deviceType" onchange="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-white rounded-lg px-2.5 py-2 text-xs focus:border-brandRed focus:outline-none">
                            <option value="📱 Phone">📱 Phone</option>
                            <option value="💻 Laptop">💻 Laptop</option>
                            <option value="🖥️ Desktop">🖥️ Desktop</option>
                            <option value="🎮 Console">🎮 Console</option>
                        </select>
                        <input type="text" id="deviceBrand" placeholder="Brand (Apple, Asus)" oninput="updatePreview()" class="col-span-2 bg-black/60 border border-zinc-700 text-white rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none">
                    </div>
                    <div class="grid grid-cols-2 gap-2.5">
                        <input type="text" id="deviceModel" placeholder="Model (e.g. iPhone 13 Pro)" oninput="updatePreview()" class="bg-black/60 border border-zinc-700 text-white rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none">
                        <input type="text" id="codeModel" placeholder="Serial / Passcode Code" oninput="updatePreview()" class="bg-black/60 border border-zinc-700 text-white rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none font-mono">
                    </div>
                </div>

                <!-- Dynamic Itemized Repair Services -->
                <div class="space-y-2.5 pt-1">
                    <div class="flex justify-between items-center">
                        <h3 class="text-[11px] font-bold text-brandRed uppercase tracking-wider flex items-center gap-1.5">
                            <i class="fa-solid fa-wrench text-[10px]"></i> Services & Hardware Parts
                        </h3>
                        <button type="button" onclick="addItemRow()" class="text-[11px] bg-red-950/60 hover:bg-brandRed text-red-300 hover:text-white border border-red-800/50 px-2.5 py-1 rounded-md transition flex items-center gap-1">
                            <i class="fa-solid fa-plus text-[9px]"></i> Add Line
                        </button>
                    </div>

                    <div id="itemsContainer" class="space-y-2 max-h-48 overflow-y-auto pr-1">
                        <!-- Item Row Template generated via JS -->
                    </div>
                </div>

                <!-- Warranty & Status -->
                <div class="grid grid-cols-2 gap-3 pt-1">
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 mb-1">Warranty Period</label>
                        <select id="warranty" onchange="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-white rounded-lg px-3 py-2 text-xs focus:border-brandRed focus:outline-none">
                            <option value="NO WARRANTY">NO WARRANTY</option>
                            <option value="7 Days Warranty">7 Days Warranty</option>
                            <option value="14 Days Warranty">14 Days Warranty</option>
                            <option value="30 Days (1 Month)">30 Days (1 Month)</option>
                            <option value="60 Days (2 Months)">60 Days (2 Months)</option>
                            <option value="90 Days (3 Months)">90 Days (3 Months)</option>
                            <option value="180 Days (6 Months)">180 Days (6 Months)</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-xs font-semibold text-gray-400 mb-1">Payment Status</label>
                        <select id="paymentStatus" onchange="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-white rounded-lg px-3 py-2 text-xs font-bold focus:border-brandRed focus:outline-none">
                            <option value="UNPAID" class="text-red-500">UNPAID</option>
                            <option value="PAID" class="text-green-500">PAID IN FULL</option>
                            <option value="PARTIAL" class="text-yellow-500">DEPOSIT PAID</option>
                        </select>
                    </div>
                </div>

                <!-- Financial Totals -->
                <div class="bg-black/40 border border-zinc-800 p-3 rounded-xl space-y-2">
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-[11px] text-gray-400 mb-1">Deposit / Advance (RM)</label>
                            <input type="number" id="depositAmount" value="0.00" min="0" step="0.01" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-green-400 font-mono font-bold rounded-lg px-3 py-1.5 text-xs focus:border-brandRed focus:outline-none">
                        </div>
                        <div>
                            <label class="block text-[11px] text-gray-400 mb-1">Discount (RM)</label>
                            <input type="number" id="discountAmount" value="0.00" min="0" step="0.01" oninput="updatePreview()" class="w-full bg-black/60 border border-zinc-700 text-yellow-400 font-mono font-bold rounded-lg px-3 py-1.5 text-xs focus:border-brandRed focus:outline-none">
                        </div>
                    </div>
                </div>

            </form>
        </section>

        <!-- Preview & Output Section (7 Columns) -->
        <section class="lg:col-span-7 flex flex-col space-y-4">
            
            <!-- Actions Toolbar -->
            <div class="bg-brandGray p-3.5 rounded-2xl border border-zinc-800 flex flex-wrap gap-2.5 items-center justify-between shadow-xl">
                <span class="text-xs font-bold text-gray-300 flex items-center gap-2">
                    <span class="w-2.5 h-2.5 rounded-full bg-green-500 animate-pulse"></span>
                    Interactive Print Canvas
                </span>
                
                <div class="flex flex-wrap gap-2">
                    <!-- WhatsApp Direct Share Button -->
                    <button type="button" onclick="handleWhatsAppDispatch()" class="bg-emerald-600 hover:bg-emerald-500 text-white px-3.5 py-2 rounded-xl text-xs font-bold flex items-center gap-2 transition shadow-lg shadow-emerald-900/30">
                        <i class="fa-brands fa-whatsapp text-sm"></i> WhatsApp PDF
                    </button>
                    <!-- PDF Save Button -->
                    <button type="button" onclick="downloadPDF()" class="bg-brandRed hover:bg-brandRedDark text-white px-3.5 py-2 rounded-xl text-xs font-bold flex items-center gap-2 transition shadow-lg shadow-brandRed/30">
                        <i class="fa-solid fa-file-pdf text-sm"></i> Download PDF
                    </button>
                    <!-- System Print -->
                    <button type="button" onclick="window.print()" class="bg-zinc-800 hover:bg-zinc-700 text-gray-200 px-3 py-2 rounded-xl text-xs font-bold flex items-center gap-1.5 transition">
                        <i class="fa-solid fa-print"></i> Print
                    </button>
                </div>
            </div>

            <!-- PDF Scroll Canvas Wrapper -->
            <div class="preview-scroll-wrapper bg-zinc-950 p-2 sm:p-4 rounded-2xl border border-zinc-800 shadow-2xl flex justify-center">
                
                <!-- Printable A4 Standard Document Canvas -->
                <div id="invoiceCanvas" class="a4-canvas p-8 relative flex flex-col justify-between text-xs font-sans">
                    
                    <!-- Top Branding & Document Details -->
                    <div>
                        <div class="flex justify-between items-start border-b-2 border-zinc-900 pb-5 mb-5">
                            <div>
                                <div class="flex items-center gap-3 mb-2">
                                    <div class="w-10 h-10 bg-red-600 text-white font-black text-xl flex items-center justify-center rounded-lg shadow-md">
                                        SS
                                    </div>
                                    <div>
                                        <h1 class="text-xl font-black tracking-tight text-gray-900 leading-none">SS DYNAMIC FIX</h1>
                                        <span class="text-[9px] text-red-600 font-bold uppercase tracking-widest">Expert Smartphone & Laptop Service Center</span>
                                    </div>
                                </div>
                                <p class="text-[10px] text-gray-600 leading-relaxed max-w-xs">
                                    SS DYNAMIC TELECOMMUNICATION (MA0200450)<br>
                                    No 2, Bangunan Terminal Bas, Kompleks AG Sentral,<br>
                                    78000 Alor Gajah, Melaka.<br>
                                    <span class="font-bold text-gray-800">Phone / WhatsApp:</span> +601151453147
                                </p>
                            </div>
                            <div class="text-right">
                                <h2 id="prevDocType" class="text-2xl font-black text-gray-900 tracking-wider">INVOICE</h2>
                                <p class="text-xs text-red-600 font-mono font-bold mt-1" id="prevInvNo">#INV-000000</p>
                                <p class="text-[11px] text-gray-500 mt-0.5" id="prevDate">Date: --/--/----</p>
                            </div>
                        </div>

                        <!-- Customer & Status Info Box -->
                        <div class="grid grid-cols-2 gap-4 mb-5 bg-gray-50 p-3.5 rounded-lg border border-gray-200">
                            <div>
                                <span class="text-[9px] uppercase font-bold text-gray-400 tracking-wider block mb-1">Customer Info</span>
                                <h3 id="prevClientName" class="font-bold text-gray-900 text-sm">Customer Name</h3>
                                <p id="prevClientPhone" class="text-xs text-gray-600 font-mono">+601XXXXXXXX</p>
                                <p id="prevClientEmail" class="text-[11px] text-gray-500">client@example.com</p>
                            </div>
                            <div class="text-right flex flex-col justify-between items-end">
                                <span class="text-[9px] uppercase font-bold text-gray-400 tracking-wider block">Payment Status</span>
                                <div id="prevBadge" class="inline-block px-3 py-1 rounded-full text-[10px] font-black bg-red-100 text-red-700 border border-red-300">
                                    UNPAID
                                </div>
                            </div>
                        </div>

                        <!-- Device Specs Box -->
                        <div class="mb-5 border border-gray-300 rounded-lg overflow-hidden">
                            <div class="bg-gray-900 text-white px-3 py-1.5 text-[10px] font-bold uppercase tracking-wider flex justify-between">
                                <span>Device Diagnostic Overview</span>
                                <span id="prevDeviceType">📱 Phone</span>
                            </div>
                            <div class="grid grid-cols-3 gap-2 p-2.5 bg-gray-50 text-xs">
                                <div>
                                    <span class="text-gray-400 block text-[9px] uppercase font-bold">Brand</span>
                                    <span id="prevBrand" class="font-semibold text-gray-800">-</span>
                                </div>
                                <div>
                                    <span class="text-gray-400 block text-[9px] uppercase font-bold">Model</span>
                                    <span id="prevModel" class="font-semibold text-gray-800">-</span>
                                </div>
                                <div>
                                    <span class="text-gray-400 block text-[9px] uppercase font-bold">Serial / Passcode</span>
                                    <span id="prevCodeModel" class="font-mono text-gray-800">-</span>
                                </div>
                            </div>
                        </div>

                        <!-- Itemized Table -->
                        <table class="w-full mb-5 border-collapse">
                            <thead>
                                <tr class="border-b-2 border-gray-300 text-left text-[10px] font-bold text-gray-500 uppercase tracking-wider">
                                    <th class="py-2 pr-2">#</th>
                                    <th class="py-2 pr-2">Service / Replacement Item</th>
                                    <th class="py-2 text-center px-2">Qty</th>
                                    <th class="py-2 text-right px-2">Unit Price</th>
                                    <th class="py-2 text-right">Total (RM)</th>
                                </tr>
                            </thead>
                            <tbody id="prevItemsBody" class="divide-y divide-gray-200 text-xs">
                                <!-- Dynamic Rendered Rows -->
                            </tbody>
                        </table>
                    </div>

                    <!-- Bottom Calculations & Legal Info -->
                    <div>
                        <!-- Cost Totals -->
                        <div class="flex justify-between items-start border-t-2 border-gray-200 pt-3 mb-6">
                            <!-- Left: Banking Info for Payment -->
                            <div class="w-1/2 pr-4 text-[10px] text-gray-600 space-y-1">
                                <p class="font-bold text-gray-800 uppercase tracking-wider text-[9px]">Payment Options & Bank Details:</p>
                                <p><span class="font-semibold text-gray-700">Bank:</span> Bank Islam / Maybank</p>
                                <p><span class="font-semibold text-gray-700">Account Name:</span> SS DYNAMIC TELECOMMUNICATION</p>
                                <p><span class="font-semibold text-gray-700">Warranty Term:</span> <span id="prevWarranty" class="font-bold text-red-600">NO WARRANTY</span></p>
                            </div>

                            <!-- Right: Calculations Breakdown -->
                            <div class="w-1/2 space-y-1.5 text-xs text-right">
                                <div class="flex justify-between text-gray-600">
                                    <span>Subtotal:</span>
                                    <span id="summarySubtotal" class="font-mono">RM 0.00</span>
                                </div>
                                <div class="flex justify-between text-gray-600" id="discountRow">
                                    <span>Discount:</span>
                                    <span id="summaryDiscount" class="font-mono text-yellow-600">- RM 0.00</span>
                                </div>
                                <div class="flex justify-between text-gray-600">
                                    <span>Deposit / Advance:</span>
                                    <span id="summaryDeposit" class="font-mono text-green-600">- RM 0.00</span>
                                </div>
                                <div class="flex justify-between text-sm font-black border-t-2 border-gray-900 pt-1.5 text-gray-900">
                                    <span>Balance Due:</span>
                                    <span id="summaryBalance" class="font-mono text-red-600">RM 0.00</span>
                                </div>
                            </div>
                        </div>

                        <!-- Terms & Conditions Footer -->
                        <div class="border-t border-gray-200 pt-3 text-[9px] text-gray-500 space-y-1">
                            <p class="font-bold text-gray-700">Terms & Conditions:</p>
                            <ol class="list-decimal list-inside space-y-0.5 leading-tight">
                                <li>Hardware warranty covers manufacturing defects under standard operating conditions. Water ingress & physical drop damage void warranty.</li>
                                <li>Devices uncollected past 30 days post-repair completion notice may be subject to holding fees or recycling.</li>
                                <li>Please retain and present this digital invoice or receipt for device pickup and warranty verification.</li>
                            </ol>
                            <div class="pt-3 text-center text-gray-400 text-[8px] uppercase tracking-widest font-semibold border-t border-gray-100 mt-2">
                                *** Thank You For Choosing SS Dynamic Fix - Quality Hardware Engineering ***
                            </div>
                        </div>
                    </div>

                </div>
            </div>
        </section>

    </main>

    <!-- Notification Toast Modal -->
    <div id="toast" class="fixed bottom-5 right-5 bg-brandGray border border-red-500/50 text-white px-4 py-3 rounded-xl shadow-2xl transition-all duration-300 transform translate-y-20 opacity-0 z-50 flex items-center gap-3">
        <i id="toastIcon" class="fa-solid fa-circle-info text-brandRed text-lg"></i>
        <span id="toastMsg" class="text-xs font-semibold">Notification Message</span>
    </div>

    <!-- Core Business Logic -->
    <script>
        let itemIndex = 0;

        document.addEventListener('DOMContentLoaded', () => {
            initInvoice();
        });

        function initInvoice() {
            regenerateID();
            const today = new Date();
            const formattedDate = today.toLocaleDateString('en-GB', {
                day: '2-digit', month: '2-digit', year: 'numeric'
            });
            document.getElementById('prevDate').innerText = `Date: ${formattedDate}`;

            // Seed default service row
            document.getElementById('itemsContainer').innerHTML = '';
            addItemRow('LCD Screen Replacement & Internal Cleaning', 1, 150.00);
            
            updatePreview();
        }

        function regenerateID() {
            const dateStr = new Date().toISOString().slice(2,10).replace(/-/g,'');
            const randomID = 'INV-' + dateStr + '-' + Math.floor(1000 + Math.random() * 9000);
            document.getElementById('invNo').value = randomID;
            updatePreview();
        }

        function resetForm() {
            document.getElementById('invoiceForm').reset();
            initInvoice();
            showToast('Form reset to default state');
        }

        // Dynamic Itemized Row Manager
        function addItemRow(desc = '', qty = 1, price = 0.00) {
            itemIndex++;
            const container = document.getElementById('itemsContainer');
            const row = document.createElement('div');
            row.className = 'item-row grid grid-cols-12 gap-1.5 items-center bg-black/40 p-2 rounded-lg border border-zinc-800 text-xs';
            row.id = `itemRow_${itemIndex}`;
            
            row.innerHTML = `
                <div class="col-span-6">
                    <input type="text" placeholder="Service / Item Description" value="${desc}" oninput="updatePreview()" class="item-desc w-full bg-black/60 border border-zinc-700 text-white rounded px-2 py-1 text-xs focus:border-brandRed focus:outline-none">
                </div>
                <div class="col-span-2">
                    <input type="number" placeholder="Qty" value="${qty}" min="1" oninput="updatePreview()" class="item-qty w-full bg-black/60 border border-zinc-700 text-white rounded px-1.5 py-1 text-xs text-center focus:border-brandRed focus:outline-none">
                </div>
                <div class="col-span-3">
                    <input type="number" placeholder="Price" value="${price.toFixed(2)}" min="0" step="0.01" oninput="updatePreview()" class="item-price w-full bg-black/60 border border-zinc-700 text-white rounded px-2 py-1 text-xs text-right font-mono focus:border-brandRed focus:outline-none">
                </div>
                <div class="col-span-1 text-center">
                    <button type="button" onclick="removeItemRow('${row.id}')" class="text-gray-500 hover:text-red-400 transition">
                        <i class="fa-solid fa-trash-can"></i>
                    </button>
                </div>
            `;
            container.appendChild(row);
            updatePreview();
        }

        function removeItemRow(rowId) {
            const rows = document.querySelectorAll('.item-row');
            if(rows.length <= 1) {
                showToast('At least one line item is required.');
                return;
            }
            document.getElementById(rowId).remove();
            updatePreview();
        }

        // Realtime Preview & Calculation Engine
        function updatePreview() {
            // Document Attributes
            const docType = document.getElementById('docType').value;
            const invNo = document.getElementById('invNo').value;
            const name = document.getElementById('clientName').value || 'Customer Name';
            const phone = document.getElementById('clientPhone').value || '+601XXXXXXXX';
            const email = document.getElementById('clientEmail').value || 'client@example.com';
            
            const deviceType = document.getElementById('deviceType').value;
            const brand = document.getElementById('deviceBrand').value || '-';
            const model = document.getElementById('deviceModel').value || '-';
            const codeModel = document.getElementById('codeModel').value || '-';
            
            const warranty = document.getElementById('warranty').value;
            const payStatus = document.getElementById('paymentStatus').value;
            
            const deposit = parseFloat(document.getElementById('depositAmount').value) || 0;
            const discount = parseFloat(document.getElementById('discountAmount').value) || 0;

            // Compute Line Items
            const itemRows = document.querySelectorAll('.item-row');
            const tbody = document.getElementById('prevItemsBody');
            tbody.innerHTML = '';

            let subtotal = 0;

            itemRows.forEach((row, idx) => {
                const desc = row.querySelector('.item-desc').value || 'Technical Service Item';
                const qty = parseInt(row.querySelector('.item-qty').value) || 1;
                const price = parseFloat(row.querySelector('.item-price').value) || 0;
                const totalLine = qty * price;
                subtotal += totalLine;

                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td class="py-2.5 pr-2 font-mono text-gray-400">${idx + 1}</td>
                    <td class="py-2.5 pr-2 font-medium text-gray-900">${desc}</td>
                    <td class="py-2.5 text-center px-2 font-mono text-gray-700">${qty}</td>
                    <td class="py-2.5 text-right px-2 font-mono text-gray-700">RM ${price.toFixed(2)}</td>
                    <td class="py-2.5 text-right font-mono font-bold text-gray-900">RM ${totalLine.toFixed(2)}</td>
                `;
                tbody.appendChild(tr);
            });

            const grandTotal = Math.max(0, subtotal - discount);
            const balance = grandTotal - deposit;

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
            document.getElementById('prevWarranty').innerText = warranty;

            // Financial Binding
            document.getElementById('summarySubtotal').innerText = `RM ${subtotal.toFixed(2)}`;
            document.getElementById('summaryDiscount').innerText = `- RM ${discount.toFixed(2)}`;
            document.getElementById('summaryDeposit').innerText = `- RM ${deposit.toFixed(2)}`;
            document.getElementById('summaryBalance').innerText = `RM ${balance.toFixed(2)}`;

            // Badge Display Logic
            const badge = document.getElementById('prevBadge');
            if (payStatus === 'PAID') {
                badge.className = 'inline-block px-3 py-1 rounded-full text-[10px] font-black bg-emerald-100 text-emerald-800 border border-emerald-300';
                badge.innerText = 'PAID IN FULL';
            } else if (payStatus === 'PARTIAL') {
                badge.className = 'inline-block px-3 py-1 rounded-full text-[10px] font-black bg-amber-100 text-amber-800 border border-amber-300';
                badge.innerText = `DEPOSIT (RM ${deposit.toFixed(2)})`;
            } else {
                badge.className = 'inline-block px-3 py-1 rounded-full text-[10px] font-black bg-red-100 text-red-700 border border-red-300';
                badge.innerText = 'UNPAID';
            }
        }

        // PDF Generation Helper
        function generatePDFBlob() {
            const element = document.getElementById('invoiceCanvas');
            const invNo = document.getElementById('invNo').value;
            const clientName = (document.getElementById('clientName').value || 'Customer').replace(/\s+/g, '_');

            const opt = {
                margin:       0,
                filename:     `${invNo}_${clientName}.pdf`,
                image:        { type: 'jpeg', quality: 0.98 },
                html2canvas:  { scale: 3, useCORS: true, logging: false },
                jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
            };

            return html2pdf().set(opt).from(element);
        }

        // Download Local PDF
        function downloadPDF() {
            showToast('Compiling high-resolution PDF...');
            generatePDFBlob().save().then(() => {
                showToast('PDF downloaded successfully!');
            });
        }

        // Automated WhatsApp + PDF Dispatch Logic
        async function handleWhatsAppDispatch() {
            const rawPhone = document.getElementById('clientPhone').value;
            if (!rawPhone || rawPhone.trim() === '') {
                alert('Please enter a valid WhatsApp/Phone number first.');
                document.getElementById('clientPhone').focus();
                return;
            }

            // Sanitize phone number to international format
            let phone = rawPhone.replace(/[^0-9]/g, '');
            if (phone.startsWith('0')) {
                phone = '6' + phone;
            }

            const docType = document.getElementById('docType').value;
            const invNo = document.getElementById('invNo').value;
            const name = document.getElementById('clientName').value || 'Valued Customer';
            const brand = document.getElementById('deviceBrand').value || '';
            const model = document.getElementById('deviceModel').value || '';
            const warranty = document.getElementById('warranty').value;
            const status = document.getElementById('paymentStatus').value;

            // Calculate current summary
            const subtotalText = document.getElementById('summarySubtotal').innerText;
            const balanceText = document.getElementById('summaryBalance').innerText;

            const clientNameClean = name.replace(/\s+/g, '_');
            const pdfFileName = `${invNo}_${clientNameClean}.pdf`;

            // Text Payload Template
            let message = `*SS DYNAMIC FIX - OFFICIAL ${docType}*\n`;
            message += `----------------------------------------\n`;
            message += `*Ref No:* #${invNo}\n`;
            message += `*Customer Name:* ${name}\n`;
            message += `*Device Specs:* ${brand} ${model}\n`;
            message += `*Warranty Period:* ${warranty}\n`;
            message += `----------------------------------------\n`;
            message += `*Subtotal Amount:* ${subtotalText}\n`;
            message += `*Balance Due:* ${balanceText}\n`;
            message += `*Payment Status:* ${status}\n`;
            message += `----------------------------------------\n`;
            message += `Thank you for trusting SS Dynamic Fix! Attached is your official PDF document.\n`;
            message += `For inquiries, call/WA: +601151453147`;

            showToast('Preparing document for WhatsApp...');

            try {
                // Try Web Share API (Works natively on Mobile Browsers)
                const pdfWorker = generatePDFBlob();
                const pdfBlob = await pdfWorker.output('blob');
                const file = new File([pdfBlob], pdfFileName, { type: 'application/pdf' });

                if (navigator.canShare && navigator.canShare({ files: [file] })) {
                    await navigator.share({
                        files: [file],
                        title: `${docType} #${invNo}`,
                        text: message,
                    });
                    showToast('PDF shared to WhatsApp!');
                    return;
                }
            } catch (err) {
                console.log('Native Web Share skipped/unsupported, utilizing web fallback.', err);
            }

            // Desktop / Web Fallback Flow
            // 1. Download PDF automatically
            generatePDFBlob().save();

            // 2. Copy summary text to clipboard
            navigator.clipboard.writeText(message);

            // 3. Launch WhatsApp Web with pre-populated message
            const url = `https://wa.me/${phone}?text=${encodeURIComponent(message)}`;
            window.open(url, '_blank');

            alert(`✅ PDF Saved to your device!\n\n📋 Invoice message copied to clipboard.\n\n📱 Opening WhatsApp with ${phone}... Simply attach the newly downloaded PDF file!`);
        }

        // Notification Helper
        function showToast(message) {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toastMsg');
            toastMsg.innerText = message;
            
            toast.classList.remove('translate-y-20', 'opacity-0');
            toast.classList.add('translate-y-0', 'opacity-100');

            setTimeout(() => {
                toast.classList.remove('translate-y-0', 'opacity-100');
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3500);
        }
    </script>
</body>
