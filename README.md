# SS-DYNAMIC-FIX-INVOICE-AND-RECEIPT
SS DYNAMIC FIX INVOICE AND RECEIPT
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SS DYNAMIC FIX | Expert Invoice & Receipt Generator</title>
    <!-- Tailwind CSS Engine -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Vector Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Export Libraries: SheetJS (Excel), jsPDF & AutoTable (PDF) -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.29/jspdf.plugin.autotable.min.js"></script>

    <style>
        body {
            background-color: #0d0d0d;
            background-image: 
                linear-gradient(to right, rgba(239, 68, 68, 0.05) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(239, 68, 68, 0.05) 1px, transparent 1px);
            background-size: 24px 24px;
        }
        .border-glow-red {
            box-shadow: 0 0 15px rgba(220, 38, 38, 0.25);
        }
        .invoice-preview-shadow {
            box-shadow: 0 10px 30px rgba(0,0,0,0.8);
        }
    </style>
</head>
<body class="text-neutral-100 min-h-screen font-sans antialiased">

    <!-- MASTER HEADER -->
    <header class="bg-neutral-950 border-b-4 border-red-600 sticky top-0 z-50 shadow-xl">
        <div class="max-w-7xl mx-auto px-6 py-4 flex flex-col md:flex-row items-center justify-between gap-4">
            <div class="flex items-center gap-4">
                <div class="bg-red-600 text-black p-3 rounded-lg text-2xl font-black shadow-lg shadow-red-600/30">
                    <i class="fa-solid fa-file-invoice-dollar animate-pulse"></i>
                </div>
                <div>
                    <h1 class="text-2xl font-black tracking-wider text-white">SS DYNAMIC FIX</h1>
                    <p class="text-xs font-bold text-red-500 tracking-widest uppercase">Phone & Laptop Repairing Lab • Invoice & Receipt Generator</p>
                </div>
            </div>
            
            <!-- Dynamic Sync Status Indicator -->
            <div class="flex items-center gap-3 bg-neutral-900 border border-neutral-800 px-4 py-2 rounded-full">
                <span class="relative flex h-3 w-3">
                    <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-green-400 opacity-75"></span>
                    <span class="relative inline-flex rounded-full h-3 w-3 bg-green-500"></span>
                </span>
                <span id="syncStatus" class="text-xs font-bold text-neutral-300 uppercase">Auto Sync Active</span>
            </div>
        </div>
    </header>

    <!-- MAIN DASHBOARD CONTENT -->
    <main class="max-w-7xl mx-auto px-4 py-8 grid grid-cols-1 lg:grid-cols-12 gap-8">
        
        <!-- FORM CONTROL ENGINE (LEFT COLUMN - 5 COLS) -->
        <section class="lg:col-span-5 bg-neutral-950 border border-neutral-800 rounded-xl p-6 border-glow-red h-fit">
            <div class="flex items-center justify-between border-b border-neutral-800 pb-4 mb-6">
                <h2 class="text-lg font-black uppercase text-white tracking-tight">
                    <i class="fa-solid fa-pen-to-square text-red-600 mr-2"></i>Invoice Details Input
                </h2>
                <button type="button" onclick="generateNewInvoiceNumber()" class="text-[10px] bg-red-600/20 text-red-400 font-extrabold px-2.5 py-1 rounded border border-red-600/40 uppercase hover:bg-red-600 hover:text-white transition-colors">
                    <i class="fa-solid fa-rotate mr-1"></i> New Inv #
                </button>
            </div>

            <form id="invoiceForm" class="space-y-4" onsubmit="handleFormSubmission(event)">
                
                <!-- DOCUMENT METADATA -->
                <div class="grid grid-cols-2 gap-3 bg-neutral-900/50 p-3 rounded-lg border border-neutral-900">
                    <div>
                        <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Doc Type</label>
                        <select id="docType" onchange="updateLivePreview()" class="w-full bg-neutral-900 border border-neutral-700 rounded p-2 text-white text-xs font-bold focus:outline-none focus:border-red-600">
                            <option value="INVOICE">OFFICIAL INVOICE</option>
                            <option value="RECEIPT">PAYMENT RECEIPT</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Invoice / Receipt No</label>
                        <input type="text" id="invNo" readonly class="w-full bg-neutral-950 border border-neutral-800 rounded p-2 text-red-500 font-mono text-xs font-bold focus:outline-none">
                    </div>
                </div>

                <!-- CLIENT INFORMATION -->
                <div class="bg-neutral-900/50 p-4 rounded-lg border border-neutral-900 space-y-3">
                    <h3 class="text-xs font-black uppercase tracking-wider text-neutral-400 border-b border-neutral-800 pb-2">
                        <i class="fa-solid fa-user-gear text-red-500 mr-1"></i> 1. Customer Information
                    </h3>
                    <div>
                        <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Customer Name</label>
                        <input type="text" id="custName" placeholder="e.g., Nik Nabihah" required oninput="updateLivePreview()" class="w-full bg-neutral-900 border border-neutral-700 rounded p-2 text-white text-xs focus:outline-none focus:border-red-600">
                    </div>
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Phone / WhatsApp</label>
                            <input type="text" id="custPhone" placeholder="60123456789" required oninput="updateLivePreview()" class="w-full bg-neutral-900 border border-neutral-700 rounded p-2 text-white text-xs focus:outline-none focus:border-red-600">
                        </div>
                        <div>
                            <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Email Address</label>
                            <input type="email" id="custEmail" placeholder="client@domain.com" oninput="updateLivePreview()" class="w-full bg-neutral-900 border border-neutral-700 rounded p-2 text-white text-xs focus:outline-none focus:border-red-600">
                        </div>
                    </div>
                </div>

                <!-- DEVICE HARDWARE METRICS -->
                <div class="bg-neutral-900/50 p-4 rounded-lg border border-neutral-900 space-y-3">
                    <h3 class="text-xs font-black uppercase tracking-wider text-neutral-400 border-b border-neutral-800 pb-2">
                        <i class="fa-solid fa-laptop-medical text-red-500 mr-1"></i> 2. Device Identification
                    </h3>
                    <div>
                        <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Brand Name</label>
                        <input type="text" id="brand" placeholder="e.g., Apple / ASUS / Samsung" required oninput="updateLivePreview()" class="w-full bg-neutral-900 border border-neutral-700 rounded p-2 text-white text-xs focus:outline-none focus:border-red-600">
                    </div>
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Model Name</label>
                            <input type="text" id="model" placeholder="e.g., iPhone 15 Pro / ROG Strix" required oninput="updateLivePreview()" class="w-full bg-neutral-900 border border-neutral-700 rounded p-2 text-white text-xs focus:outline-none focus:border-red-600">
                        </div>
                        <div>
                            <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Model Code / Serial</label>
                            <input type="text" id="codeModel" placeholder="e.g., A3106 / G513" required oninput="updateLivePreview()" class="w-full bg-neutral-900 border border-neutral-700 rounded p-2 text-white text-xs focus:outline-none focus:border-red-600">
                        </div>
                    </div>
                </div>

                <!-- SERVICE DETAILS & WARRANTY -->
                <div class="bg-neutral-900/50 p-4 rounded-lg border border-neutral-900 space-y-3">
                    <h3 class="text-xs font-black uppercase tracking-wider text-neutral-400 border-b border-neutral-800 pb-2">
                        <i class="fa-solid fa-screwdriver-wrench text-red-500 mr-1"></i> 3. Service Scope & Warranty
                    </h3>
                    <div>
                        <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Repair Description</label>
                        <textarea id="description" rows="2" placeholder="e.g., Motherboard IC replacement, LCD assembly swap, thermal repaste" required oninput="updateLivePreview()" class="w-full bg-neutral-900 border border-neutral-700 rounded p-2 text-white text-xs focus:outline-none focus:border-red-600"></textarea>
                    </div>

                    <!-- WARRANTY SELECTOR (7, 14, 30, 60, 90 DAYS) -->
                    <div>
                        <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Warranty Term Period</label>
                        <div class="grid grid-cols-5 gap-1.5">
                            <button type="button" onclick="selectWarranty(7)" id="war-7" class="war-btn bg-neutral-800 hover:bg-red-600 text-white font-bold py-1.5 text-xs rounded border border-neutral-700 transition-colors">7 Days</button>
                            <button type="button" onclick="selectWarranty(14)" id="war-14" class="war-btn bg-neutral-800 hover:bg-red-600 text-white font-bold py-1.5 text-xs rounded border border-neutral-700 transition-colors">14 Days</button>
                            <button type="button" onclick="selectWarranty(30)" id="war-30" class="war-btn bg-red-600 text-white font-bold py-1.5 text-xs rounded border border-red-500 transition-colors">30 Days</button>
                            <button type="button" onclick="selectWarranty(60)" id="war-60" class="war-btn bg-neutral-800 hover:bg-red-600 text-white font-bold py-1.5 text-xs rounded border border-neutral-700 transition-colors">60 Days</button>
                            <button type="button" onclick="selectWarranty(90)" id="war-90" class="war-btn bg-neutral-800 hover:bg-red-600 text-white font-bold py-1.5 text-xs rounded border border-neutral-700 transition-colors">90 Days</button>
                        </div>
                    </div>

                    <!-- AMOUNT (RM) -->
                    <div>
                        <label class="block text-[11px] font-bold text-neutral-400 uppercase mb-1">Total Amount Charged (RM)</label>
                        <div class="relative">
                            <span class="absolute left-3 top-2 text-xs font-bold text-red-500">RM</span>
                            <input type="number" id="amountRm" step="0.01" placeholder="0.00" required oninput="updateLivePreview()" class="w-full bg-neutral-900 border border-neutral-700 rounded p-2 pl-10 text-white font-mono font-bold text-sm focus:outline-none focus:border-red-600">
                        </div>
                    </div>
                </div>

                <button type="submit" class="w-full bg-red-600 hover:bg-red-700 text-white font-black text-xs uppercase tracking-widest py-3 rounded-lg shadow-lg shadow-red-600/30 transition-all active:scale-[0.99]">
                    <i class="fa-solid fa-floppy-disk mr-1.5"></i> Commit Entry & Auto Sync to Ledger
                </button>
            </form>
        </section>

        <!-- LIVE DISPLAY ENGINE & AUTO DISPATCH (RIGHT COLUMN - 7 COLS) -->
        <section class="lg:col-span-7 flex flex-col gap-6">
            
            <!-- WHATSAPP DISPATCH & EXPORT CONTROLS -->
            <div class="bg-neutral-950 border border-neutral-800 rounded-xl p-4 border-glow-red flex flex-wrap items-center justify-between gap-4">
                <div>
                    <h3 class="font-bold text-xs text-white uppercase tracking-wider">High Power Automation Control Hub</h3>
                    <p class="text-[11px] text-neutral-400">Instantly generate documents and send to client WhatsApp</p>
                </div>
                
                <div class="flex flex-wrap gap-2">
                    <!-- WhatsApp Auto Sent Button -->
                    <button type="button" onclick="sendWhatsAppInvoice()" class="bg-emerald-600 hover:bg-emerald-700 text-white text-xs font-extrabold uppercase px-3 py-2 rounded-lg flex items-center gap-1.5 shadow-md shadow-emerald-600/20 transition-colors">
                        <i class="fa-brands fa-whatsapp text-sm"></i> Auto WhatsApp
                    </button>
                    <!-- Exports -->
                    <button type="button" onclick="exportToExcel()" class="bg-neutral-800 hover:bg-neutral-700 text-white text-xs font-bold uppercase px-3 py-2 rounded-lg border border-neutral-700 flex items-center gap-1 transition-colors">
                        <i class="fa-solid fa-file-excel text-emerald-500"></i> Sheet
                    </button>
                    <button type="button" onclick="exportToWord()" class="bg-neutral-800 hover:bg-neutral-700 text-white text-xs font-bold uppercase px-3 py-2 rounded-lg border border-neutral-700 flex items-center gap-1 transition-colors">
                        <i class="fa-solid fa-file-word text-blue-500"></i> Word
                    </button>
                    <button type="button" onclick="exportToPDF()" class="bg-neutral-800 hover:bg-neutral-700 text-white text-xs font-bold uppercase px-3 py-2 rounded-lg border border-neutral-700 flex items-center gap-1 transition-colors">
                        <i class="fa-solid fa-file-pdf text-red-500"></i> PDF
                    </button>
                </div>
            </div>

            <!-- DYNAMIC LIVE DOCUMENT PREVIEW CANVAS -->
            <div id="invoiceCanvas" class="bg-white text-neutral-900 rounded-xl p-8 invoice-preview-shadow font-sans relative overflow-hidden min-h-[500px]">
                
                <!-- TOP HEADER -->
                <div class="flex justify-between items-start border-b-2 border-red-600 pb-6">
                    <div>
                        <h2 class="text-3xl font-black text-black tracking-tighter">SS DYNAMIC FIX</h2>
                        <p class="text-[10px] font-bold text-red-600 tracking-widest uppercase mt-0.5">PHONE AND LAPTOP REPAIR SERVICE MASTER LAB</p>
                        <p class="text-xs text-neutral-600 mt-2 font-medium">Alor Gajah, Melaka, Malaysia<br>Hotline / WhatsApp: +60 12-345 6789</p>
                    </div>
                    <div class="text-right">
                        <span id="prevDocType" class="inline-block bg-red-600 text-white text-xs font-black uppercase px-3 py-1 rounded tracking-wider mb-2">OFFICIAL INVOICE</span>
                        <div class="text-xs text-neutral-600 font-bold" id="prevInvNo">INV-2026-0001</div>
                        <div class="text-xs text-neutral-500 mt-1" id="prevDate">Date: 2026-08-03</div>
                    </div>
                </div>

                <!-- CLIENT & DEVICE INFO GRID -->
                <div class="grid grid-cols-2 gap-6 my-6 bg-neutral-50 p-4 rounded-lg border border-neutral-200">
                    <div>
                        <span class="text-[10px] font-bold text-neutral-400 uppercase tracking-wider block">CUSTOMER DETAILS</span>
                        <div class="text-sm font-black text-neutral-900 mt-1" id="prevCustName">Client Name</div>
                        <div class="text-xs text-neutral-600 mt-0.5" id="prevCustPhone">Phone: -</div>
                        <div class="text-xs text-neutral-600" id="prevCustEmail">Email: -</div>
                    </div>
                    <div>
                        <span class="text-[10px] font-bold text-neutral-400 uppercase tracking-wider block">TARGET HARDWARE SPECIFICATION</span>
                        <div class="text-sm font-black text-red-600 mt-1"><span id="prevBrand">Brand</span> <span id="prevModel">Model</span></div>
                        <div class="text-xs text-neutral-700 font-mono font-bold mt-0.5">Code Model: <span id="prevCodeModel">-</span></div>
                    </div>
                </div>

                <!-- SERVICE ITEMS TABLE -->
                <div class="my-6">
                    <table class="w-full text-left text-xs border-collapse">
                        <thead>
                            <tr class="bg-neutral-900 text-white uppercase text-[10px] tracking-wider">
                                <th class="p-3 rounded-l">Service Scope / Repair Description</th>
                                <th class="p-3 text-center">Warranty</th>
                                <th class="p-3 text-right rounded-r">Amount</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr class="border-b border-neutral-200">
                                <td class="p-3 text-neutral-800 font-medium" id="prevDescription">No repair description provided yet.</td>
                                <td class="p-3 text-center font-bold text-red-600 whitespace-nowrap"><span id="prevWarranty">30</span> Days</td>
                                <td class="p-3 text-right font-mono font-bold text-neutral-900 whitespace-nowrap">RM <span id="prevAmount">0.00</span></td>
                            </tr>
                        </tbody>
                    </table>
                </div>

                <!-- GRAND TOTAL BAR -->
                <div class="flex justify-between items-center border-t-2 border-neutral-200 pt-4 mt-8">
                    <div class="text-xs text-neutral-500">
                        <p class="font-bold text-neutral-700">Terms & Conditions:</p>
                        <p class="text-[10px] text-neutral-500">Warranty covers replaced hardware parts only. Physical/Liquid damage voids warranty.</p>
                    </div>
                    <div class="text-right">
                        <span class="text-xs text-neutral-500 uppercase font-bold tracking-wider block">Total Payable</span>
                        <span class="text-2xl font-black text-red-600 font-mono">RM <span id="prevTotal">0.00</span></span>
                    </div>
                </div>

                <!-- FOOTER AUTHORIZATION -->
                <div class="mt-12 flex justify-between items-end text-neutral-400 text-[10px] border-t border-neutral-100 pt-4">
                    <div>SS DYNAMIC FIX &bull; Master Repair Lab</div>
                    <div class="text-center border-t border-neutral-300 px-6 pt-1 text-neutral-600 font-bold">Authorized Technician Signature</div>
                </div>
            </div>

            <!-- AUTO SYNCED SYSTEM LEDGER GRID -->
            <div class="bg-neutral-950 border border-neutral-800 rounded-xl overflow-hidden flex-1 border-glow-red">
                <div class="p-4 bg-neutral-900/50 border-b border-neutral-800 flex justify-between items-center">
                    <span class="text-xs font-black uppercase tracking-widest 
