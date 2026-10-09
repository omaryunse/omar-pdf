<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>بي دي إف فَن — محرر PDF المرح</title>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;800;900&family=Space+Grotesk:wght@600;700&display=swap" rel="stylesheet">
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<script src="https://unpkg.com/pdf-lib@1.17.1/dist/pdf-lib.min.js"></script>
<script>
tailwind.config={theme:{extend:{fontFamily:{cairo:['Cairo','sans-serif'],space:['Space Grotesk','sans-serif']}}}}
</script>
<style>
*{font-family:'Cairo',sans-serif}
body{background:#FFFBF5;overflow-x:hidden}
::-webkit-scrollbar{width:8px;height:8px}
::-webkit-scrollbar-thumb{background:linear-gradient(180deg,#FF4D6D,#7C3AED);border-radius:99px}
.gradient-blob{position:absolute;border-radius:50%;filter:blur(50px);pointer-events:none;opacity:.55}
.tool-btn{width:52px;height:52px;border-radius:16px;display:flex;align-items:center;justify-content:center;transition:.22s cubic-bezier(.34,1.56,.64,1);border:2px solid transparent;background:white;color:#111827;box-shadow:0 4px 12px rgba(0,0,0,.06)}
.tool-btn:hover{transform:translateY(-2px) scale(1.04);box-shadow:0 10px 24px rgba(0,0,0,.12)}
.tool-btn.active{background:linear-gradient(135deg,#FF4D6D 0%,#FF8A5B 50%,#7C3AED 100%);color:white;box-shadow:0 12px 28px rgba(255,77,109,.35);border-color:rgba(255,255,255,.6);transform:scale(1.08)}
.color-dot{width:28px;height:28px;border-radius:50%;cursor:pointer;border:3px solid white;box-shadow:0 2px 8px rgba(0,0,0,.15),0 0 0 2px transparent;transition:.2s}
.color-dot.active{box-shadow:0 0 0 3px #111827,0 6px 16px rgba(0,0,0,.2);transform:scale(1.15)}
.page-wrapper{transform-origin:top center}
.drop-dashed{background-image:url("data:image/svg+xml,%3csvg width='100%25' height='100%25' xmlns='http://www.w3.org/2000/svg'%3e%3crect width='100%25' height='100%25' fill='none' rx='32' ry='32' stroke='%23FF4D6D' stroke-width='4' stroke-dasharray='16%2c 16' stroke-dashoffset='0' stroke-linecap='round'/%3e%3c/svg%3e")}
.shimmer{position:relative;overflow:hidden}
.shimmer:after{content:'';position:absolute;inset:0;background:linear-gradient(90deg,transparent,rgba(255,255,255,.6),transparent);transform:translateX(-100%);animation:shim 2.2s infinite}
@keyframes shim{100%{transform:translateX(100%)}}
@keyframes float{0%,100%{transform:translateY(0) rotate(0)}50%{transform:translateY(-12px) rotate(2deg)}}
.floaty{animation:float 5s ease-in-out infinite}
input[type=range]{accent-color:#FF4D6D}
</style>
</head>
<body class="text-zinc-900 selection:bg-[#FF4D6D]/20">
<!-- blobs -->
<div class="fixed inset-0 pointer-events-none overflow-hidden -z-10">
<div class="gradient-blob w-[600px] h-[600px] bg-gradient-to-br from-[#FF4D6D] to-[#FF8A5B] -top-40 -right-40"></div>
<div class="gradient-blob w-[500px] h-[500px] bg-gradient-to-br from-[#7C3AED] to-[#3B82F6] top-[30%] -left-32"></div>
<div class="gradient-blob w-[400px] h-[400px] bg-gradient-to-br from-[#06FFA5] to-[#06B6D4] bottom-0 right-[20%]"></div>
<div class="gradient-blob w-[300px] h-[300px] bg-[#FFD93D] bottom-[10%] left-[40%]"></div>
</div>

<header class="sticky top-0 z-40 backdrop-blur-2xl bg-white/75 border-b border-zinc-900/[.06]">
<div class="max-w-[1600px] mx-auto px-4 md:px-7 h-[68px] flex items-center justify-between">
<div class="flex items-center gap-3">
<div class="w-11 h-11 rounded-[14px] bg-gradient-to-br from-[#FF4D6D] to-[#FF8A5B] shadow-[0_8px_20px_rgba(255,77,109,.35)] flex items-center justify-center text-white font-black text-[22px] rotate-3">ف</div>
<div class="leading-[.9]">
<h1 class="font-black text-[22px] tracking-tight">بي دي إف <span class="bg-gradient-to-r from-[#FF4D6D] via-[#FF8A5B] to-[#7C3AED] bg-clip-text text-transparent">فَن</span></h1>
<p class="font-space font-bold text-[10px] tracking-[.22em] text-zinc-900/40 mt-[2px]">PDF FUN STUDIO</p>
</div>
<div class="hidden md:flex items-center gap-2 mr-8">
<span class="px-3 py-1 rounded-full bg-zinc-900 text-white text-[11px] font-bold">سريع</span>
<span class="px-3 py-1 rounded-full bg-[#FFD93D] text-zinc-900 text-[11px] font-bold">آمن 100%</span>
<span class="px-3 py-1 rounded-full bg-[#06FFA5] text-zinc-900 text-[11px] font-bold">بدون رفع</span>
</div>
</div>
<div class="flex items-center gap-2">
<button id="newFileBtn" class="h-10 px-4 rounded-full bg-white border border-zinc-900/10 font-bold text-sm hover:bg-zinc-900 hover:text-white transition">ملف جديد</button>
<button id="exportBtn" class="h-11 px-6 rounded-full bg-zinc-900 text-white font-extrabold text-sm shadow-[0_10px_24px_rgba(0,0,0,.18)] hover:shadow-[0_14px_32px_rgba(0,0,0,.25)] hover:-translate-y-[1px] transition flex items-center gap-2">
<span>تصدير</span><span class="w-6 h-6 rounded-full bg-gradient-to-br from-[#FF4D6D] to-[#FF8A5B] flex items-center justify-center text-[12px]">↗</span>
</button>
</div>
</div>
</header>

<!-- EMPTY STATE -->
<div id="emptyState" class="min-h-[calc(100vh-68px)] flex items-center justify-center p-4 md:p-10">
<div class="max-w-[1200px] w-full grid lg:grid-cols-[1.1fr_.9fr] gap-10 items-center">
<div class="relative">
<div class="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-white shadow-[0_8px_24px_rgba(0,0,0,.08)] border border-zinc-900/5 font-bold text-xs mb-6">
<span class="w-2 h-2 rounded-full bg-[#06FFA5] animate-pulse"></span> محرر يعمل داخل المتصفح فقط
</div>
<h2 class="font-black text-[42px] md:text-[64px] leading-[.9] tracking-[-0.03em]">
حرّر ملفات<br>
<span class="relative inline-block"><span class="relative z-10 bg-gradient-to-r from-[#FF4D6D] to-[#7C3AED] bg-clip-text text-transparent">الـ PDF</span><span class="absolute bottom-2 left-0 right-0 h-4 bg-[#FFD93D]/60 -rotate-1 -z-0"></span></span><br>
كأنك ترسم!
</h2>
<p class="mt-5 text-[18px] leading-7 text-zinc-600 font-semibold max-w-[48ch]">أضف نصوص، ارسم، علّم، وقّع، أضف صور، احذف صفحات. كل شيء يحدث على جهازك — لا يتم رفع ملفاتك أبداً.</p>
<div class="mt-8 grid grid-cols-3 gap-3 max-w-[420px]">
<div class="bg-white rounded-[18px] p-4 shadow-[0_8px_24px_rgba(0,0,0,.06)] border border-zinc-900/5">
<div class="w-10 h-10 rounded-xl bg-gradient-to-br from-[#FF4D6D] to-[#FF8A5B] flex items-center justify-center text-white font-bold">T</div>
<p class="mt-3 font-extrabold text-sm">نص حر</p><p class="text-xs text-zinc-500 font-bold">عربي 100%</p>
</div>
<div class="bg-white rounded-[18px] p-4 shadow-[0_8px_24px_rgba(0,0,0,.06)] border border-zinc-900/5 rotate-2">
<div class="w-10 h-10 rounded-xl bg-gradient-to-br from-[#7C3AED] to-[#3B82F6] flex items-center justify-center text-white">✏️</div>
<p class="mt-3 font-extrabold text-sm">رسم يدوي</p><p class="text-xs text-zinc-500 font-bold">بألوان نيون</p>
</div>
<div class="bg-white rounded-[18px] p-4 shadow-[0_8px_24px_rgba(0,0,0,.06)] border border-zinc-900/5 -rotate-2">
<div class="w-10 h-10 rounded-xl bg-gradient-to-br from-[#06FFA5] to-[#06B6D4] flex items-center justify-center">🖼️</div>
<p class="mt-3 font-extrabold text-sm">صور وتوقيع</p><p class="text-xs text-zinc-500 font-bold">اسحب وأفلت</p>
</div>
</div>
</div>

<div class="relative">
<div class="absolute -top-10 -left-10 w-24 h-24 bg-[#FFD93D] rounded-[24px] rotate-12 shadow-xl floaty hidden md:flex items-center justify-center font-black text-2xl">PDF</div>
<div class="absolute -bottom-8 -right-6 w-20 h-20 bg-[#FF4D6D] rounded-full shadow-xl floaty flex items-center justify-center text-white text-2xl" style="animation-delay:.8s">✦</div>

<div id="dropZone" class="drop-dashed relative bg-white rounded-[32px] p-8 md:p-10 shadow-[0_24px_64px_rgba(0,0,0,.12)]">
<div class="text-center">
<div class="mx-auto w-20 h-20 rounded-[22px] bg-gradient-to-br from-zinc-900 to-zinc-700 flex items-center justify-center text-white text-3xl shadow-[0_12px_24px_rgba(0,0,0,.2)]">📄</div>
<h3 class="mt-5 font-black text-2xl">اسحب ملف PDF إلى هنا</h3>
<p class="mt-2 text-sm font-bold text-zinc-500">أو اختر ملف من جهازك — يعمل حتى 200 صفحة</p>
<div class="mt-6 flex flex-col items-center gap-3">
<label class="cursor-pointer h-12 px-8 rounded-full bg-gradient-to-r from-[#FF4D6D] to-[#FF8A5B] text-white font-black shadow-[0_12px_24px_rgba(255,77,109,.35)] hover:shadow-[0_16px_32px_rgba(255,77,109,.45)] hover:-translate-y-[1px] transition flex items-center justify-center">
اختر ملف PDF
<input id="pdfInput" type="file" accept="application/pdf" class="hidden">
</label>
<p class="text-[11px] font-bold tracking-widest text-zinc-400">PDF • خصوصية تامة • لا خوادم</p>
</div>
<div class="mt-8 grid grid-cols-2 gap-3 text-right">
<div class="rounded-2xl bg-[#FFF7ED] border border-orange-100 p-3 flex gap-2 items-start"><span class="w-8 h-8 rounded-full bg-white flex items-center justify-center">⚡</span><div><p class="font-bold text-xs">سريع جداً</p><p class="text-[11px] text-zinc-500 font-semibold">معالجة فورية</p></div></div>
<div class="rounded-2xl bg-[#F5F3FF] border border-violet-100 p-3 flex gap-2 items-start"><span class="w-8 h-8 rounded-full bg-white flex items-center justify-center">🔒</span><div><p class="font-bold text-xs">آمن</p><p class="text-[11px] text-zinc-500 font-semibold">لا يغادر جهازك</p></div></div>
</div>
</div>
</div>
</div>
</div>
</div>

<!-- EDITOR -->
<div id="editor" class="hidden h-[calc(100vh-68px)] overflow-hidden">
<div class="flex h-full">
<!-- tools -->
<aside class="w-[84px] md:w-[92px] bg-white border-l border-zinc-900/5 flex flex-col items-center py-5 gap-2.5 shrink-0 z-20 shadow-[8px_0_24px_rgba(0,0,0,.04)]">
<div class="text-[10px] font-black tracking-widest text-zinc-400 mb-1">الأدوات</div>
<button data-tool="select" class="tool-btn active" title="تحديد"><svg width="22" height="22" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M3 3l7.07 16.97 2.51-7.39 7.39-2.51L3 3z"/><path d="M13 13l6 6"/></svg></button>
<button data-tool="text" class="tool-btn" title="نص"><span class="font-black text-[18px]">T</span></button>
<button data-tool="draw" class="tool-btn" title="قلم"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M17 3a2.85 2.85 0 1 1 4 4L7.5 20.5 2 22l1.5-5.5L17 3z"/></svg></button>
<button data-tool="highlight" class="tool-btn" title="تمييز"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9 11l-4 4v4h4l4-4"/><path d="M14 6l5 5"/></svg></button>
<button data-tool="rect" class="tool-btn" title="مستطيل"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="3"/></svg></button>
<button data-tool="circle" class="tool-btn" title="دائرة"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="9"/></svg></button>
<button data-tool="image" class="tool-btn" title="صورة"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect x="3" y="3" width="18" height="18" rx="3"/><circle cx="9" cy="9" r="2"/><path d="M21 15l-3.5-3.5L3 21"/></svg></button>
<button data-tool="signature" class="tool-btn" title="توقيع"><span class="font-space font-bold text-[13px]">✍︎</span></button>
<button data-tool="eraser" class="tool-btn !bg-zinc-900 !text-white" title="ممحاة"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M20 20H7L3 16C2.5 15.5 2.5 14.7 3 14.2L14.2 3c.5-.5 1.3-.5 1.8 0l4 4c.5.5.5 1.3 0 1.8L9 20"/></svg></button>
<div class="mt-auto flex flex-col items-center gap-2">
<div class="w-10 h-[1px] bg-zinc-200"></div>
<button id="zoomOut" class="w-9 h-9 rounded-full bg-white border border-zinc-200 font-bold">−</button>
<div id="zoomVal" class="text-[11px] font-black">100%</div>
<button id="zoomIn" class="w-9 h-9 rounded-full bg-zinc-900 text-white font-bold">+</button>
</div>
</aside>

<!-- center -->
<div class="flex-1 flex flex-col min-w-0 bg-[#FFF7ED]">
<div id="propsBar" class="h-[64px] shrink-0 bg-white/90 backdrop-blur-xl border-b border-zinc-900/5 flex items-center gap-3 px-4 overflow-x-auto">
<div class="flex items-center gap-2 shrink-0">
<span class="text-[11px] font-black tracking-widest text-zinc-400">اللون</span>
<div id="colorPalette" class="flex items-center gap-2">
<!-- dots injected -->
</div>
</div>
<div class="w-[1px] h-6 bg-zinc-200 shrink-0"></div>
<div id="sizeControl" class="flex items-center gap-2 shrink-0">
<span class="text-[11px] font-black text-zinc-400">الحجم</span>
<input id="sizeSlider" type="range" min="2" max="48" value="6" class="w-24">
<span id="sizeLabel" class="text-xs font-bold w-8">6</span>
</div>
<div class="w-[1px] h-6 bg-zinc-200 shrink-0"></div>
<div id="fontControl" class="flex items-center gap-2 shrink-0">
<span class="text-[11px] font-black text-zinc-400">خط</span>
<input id="fontSizeSlider" type="range" min="12" max="96" value="22" class="w-24">
<span id="fontSizeLabel" class="text-xs font-bold w-8">22</span>
<button id="boldBtn" class="w-8 h-8 rounded-full bg-zinc-900 text-white font-black text-sm">B</button>
</div>
<div class="mr-auto flex items-center gap-2 shrink-0">
<button id="deleteAnnBtn" class="h-9 px-4 rounded-full bg-red-50 text-red-600 font-bold text-xs border border-red-200 hover:bg-red-600 hover:text-white transition">حذف المحدد ⌫</button>
<div class="hidden md:flex items-center gap-1 text-[11px] font-bold text-zinc-400 bg-zinc-50 px-3 py-1.5 rounded-full border"><span>💡</span> اسحب العنصر لتحريكه</div>
</div>
</div>
<div id="editorCenter" class="flex-1 overflow-auto">
<div id="pagesContainer" class="p-6 md:p-10 flex flex-col items-center gap-10 min-h-full"></div>
</div>
</div>

<!-- right -->
<aside class="w-[300px] bg-white border-r border-zinc-900/5 hidden xl:flex flex-col shrink-0">
<div class="p-5 border-b border-zinc-900/5 flex items-center justify-between">
<h3 class="font-black text-[15px]">الصفحات <span id="pageCount" class="ml-2 bg-zinc-900 text-white text-[11px] px-2 py-0.5 rounded-full">0</span></h3>
<div class="flex gap-1">
<button id="addPageBtn" class="h-8 px-3 rounded-full bg-gradient-to-r from-[#FF4D6D] to-[#FF8A5B] text-white font-bold text-xs">+ صفحة</button>
</div>
</div>
<div id="thumbnails" class="flex-1 overflow-auto p-4 space-y-4 bg-[#FFFBF5]"></div>
<div class="p-3 border-t border-zinc-900/5 bg-white">
<div class="rounded-2xl bg-zinc-900 text-white p-3 flex items-center gap-3">
<div class="w-10 h-10 rounded-xl bg-white/10 flex items-center justify-center">✨</div>
<div class="leading-tight"><p class="font-bold text-xs">نصيحة فَن</p><p class="text-[11px] text-white/60 font-semibold">اضغط T ثم انقر في أي مكان لإضافة نص عربي جميل</p></div>
</div>
</div>
</aside>
</div>
</div>

<!-- text input overlay -->
<div id="textInputWrap" class="hidden fixed z-50">
<div class="bg-white rounded-[16px] shadow-[0_20px_60px_rgba(0,0,0,.25)] border border-zinc-900/10 p-3 w-[300px]">
<textarea id="textInput" class="w-full min-h-[80px] p-3 rounded-xl bg-zinc-50 border border-zinc-200 font-bold text-sm focus:outline-none focus:ring-2 focus:ring-[#FF4D6D]/30 resize-none" placeholder="اكتب نصك هنا..."></textarea>
<div class="mt-2 flex justify-between items-center">
<span class="text-[11px] font-bold text-zinc-400">Enter للحفظ • Esc للإلغاء</span>
<div class="flex gap-2">
<button id="cancelTextBtn" class="h-8 px-3 rounded-full bg-zinc-100 font-bold text-xs">إلغاء</button>
<button id="saveTextBtn" class="h-8 px-4 rounded-full bg-zinc-900 text-white font-bold text-xs">حفظ</button>
</div>
</div>
</div>
</div>

<!-- signature modal -->
<div id="signatureModal" class="hidden fixed inset-0 z-[100] bg-zinc-900/60 backdrop-blur-[8px] flex items-center justify-center p-4">
<div class="bg-white rounded-[28px] w-full max-w-[560px] shadow-[0_32px_80px_rgba(0,0,0,.4)] overflow-hidden">
<div class="p-6 pb-3 flex items-center justify-between">
<h3 class="font-black text-xl">ارسم توقيعك ✍️</h3>
<button id="closeSigModal" class="w-9 h-9 rounded-full bg-zinc-100 font-bold">✕</button>
</div>
<div class="px-6">
<div class="rounded-[20px] border-[3px] border-dashed border-zinc-200 bg-[#FFFBF5] overflow-hidden">
<canvas id="sigCanvas" width="800" height="320" class="w-full h-[220px] touch-none cursor-crosshair block"></canvas>
</div>
</div>
<div class="p-6 flex gap-2 justify-between">
<button id="clearSig" class="h-11 px-5 rounded-full bg-zinc-100 font-bold">مسح</button>
<div class="flex gap-2">
<button id="cancelSig" class="h-11 px-5 rounded-full bg-white border border-zinc-200 font-bold">إلغاء</button>
<button id="saveSig" class="h-11 px-7 rounded-full bg-gradient-to-r from-[#FF4D6D] to-[#7C3AED] text-white font-black shadow-[0_10px_24px_rgba(124,58,237,.35)]">حفظ التوقيع</button>
</div>
</div>
</div>
</div>

<input type="file" id="imageInput" accept="image/*" class="hidden">
<div id="toast" class="fixed bottom-6 left-1/2 -translate-x-1/2 z-[200] pointer-events-none opacity-0 transition-all duration-300 translate-y-4">
<div class="bg-zinc-900 text-white px-5 py-3 rounded-full font-bold text-sm shadow-[0_16px_40px_rgba(0,0,0,.3)] flex items-center gap-2"><span id="toastIcon">✨</span><span id="toastMsg">تم</span></div>
</div>

<script>
const pdfjsLib = window['pdfjs-dist/build/pdf'];
pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

const COLORS = ['#111827','#FF4D6D','#7C3AED','#06B6D4','#F59E0B','#10B981','#FFD93D','#FFFFFF'];
let state = {
 pages:[], currentTool:'select', color:'#FF4D6D', size:6, fontSize:22, isBold:false,
 zoom:1, selectedPage:0, dragging:false, dragOff:{x:0,y:0}, isDrawing:false,
 currentPath:[], startPos:null, activeText:{page:0,x:0,y:0,canvasX:0,canvasY:0,editIndex:null},
 sigDrawing:false, renderScale:2
};

const $ = s=>document.querySelector(s);
const pagesContainer = $('#pagesContainer');
const thumbnailsEl = $('#thumbnails');
const colorPalette = $('#colorPalette');
const sizeSlider = $('#sizeSlider'), sizeLabel = $('#sizeLabel');
const fontSizeSlider = $('#fontSizeSlider'), fontSizeLabel = $('#fontSizeLabel');
const boldBtn = $('#boldBtn');
const textWrap = $('#textInputWrap'), textInput = $('#textInput');
const pdfInput = $('#pdfInput'), imageInput = $('#imageInput');
const dropZone = $('#dropZone');
const emptyState = $('#emptyState'), editor = $('#editor');
const pageCountEl = $('#pageCount');
const zoomVal = $('#zoomVal');

function toast(msg, icon='✨'){
 $('#toastMsg').textContent=msg; $('#toastIcon').textContent=icon;
 const t=$('#toast'); t.style.opacity='1'; t.style.transform='translate(-50%,0)';
 setTimeout(()=>{t.style.opacity='0'; t.style.transform='translate(-50%,16px)';},2600);
}

// colors
COLORS.forEach(c=>{
 const d=document.createElement('div'); d.className='color-dot'; d.style.background=c;
 if(c==='#FFFFFF') d.style.border='3px solid #111827';
 if(c===state.color) d.classList.add('active');
 d.onclick=()=>{
   state.color=c;
   document.querySelectorAll('.color-dot').forEach(x=>x.classList.remove('active'));
   d.classList.add('active');
   if(state.pages[state.selectedPage]) redrawPage(state.selectedPage);
 };
 colorPalette.appendChild(d);
});

sizeSlider.oninput=e=>{state.size=+e.target.value; sizeLabel.textContent=state.size;};
fontSizeSlider.oninput=e=>{state.fontSize=+e.target.value; fontSizeLabel.textContent=state.fontSize;};
boldBtn.onclick=()=>{state.isBold=!state.isBold; boldBtn.classList.toggle('!bg-[#FF4D6D]',state.isBold);};

// tools
document.querySelectorAll('.tool-btn').forEach(btn=>{
 btn.onclick=()=>{
   document.querySelectorAll('.tool-btn').forEach(b=>b.classList.remove('active'));
   btn.classList.add('active');
   state.currentTool=btn.dataset.tool;
   if(state.currentTool==='image'){ imageInput.click(); }
   if(state.currentTool==='signature'){ openSigModal(); }
   toast('تم اختيار: '+btn.title,'🖊️');
 };
});

$('#zoomIn').onclick=()=>{state.zoom=Math.min(2,state.zoom+0.1); applyZoom();};
$('#zoomOut').onclick=()=>{state.zoom=Math.max(0.4,state.zoom-0.1); applyZoom();};
function applyZoom(){
 pagesContainer.style.transform=`scale(${state.zoom})`;
 pagesContainer.style.transformOrigin='top center';
 zoomVal.textContent=Math.round(state.zoom*100)+'%';
}

// file handling
pdfInput.onchange=e=>{ if(e.target.files[0]) handlePDF(e.target.files[0]); };
dropZone.addEventListener('dragover',e=>{e.preventDefault(); dropZone.classList.add('!bg-orange-50');});
dropZone.addEventListener('dragleave',()=>dropZone.classList.remove('!bg-orange-50'));
dropZone.addEventListener('drop',e=>{e.preventDefault(); dropZone.classList.remove('!bg-orange-50'); if(e.dataTransfer.files[0]) handlePDF(e.dataTransfer.files[0]);});
$('#newFileBtn').onclick=()=>{ pdfInput.click(); };

async function handlePDF(file){
 if(file.type!=='application/pdf'){ toast('الرجاء اختيار ملف PDF','⚠️'); return; }
 toast('جاري تحميل الملف...','⏳');
 const buf=await file.arrayBuffer();
 const pdf = await pdfjsLib.getDocument({data:buf}).promise;
 state.pages=[]; pagesContainer.innerHTML=''; thumbnailsEl.innerHTML='';
 for(let i=1;i<=pdf.numPages;i++){
   const p=await pdf.getPage(i);
   await createPage(p,i-1);
 }
 emptyState.classList.add('hidden'); editor.classList.remove('hidden');
 pageCountEl.textContent=state.pages.length;
 toast(`تم تحميل ${pdf.numPages} صفحة بنجاح`,'🎉');
}

async function createPage(pdfPage, index){
 const wrapper=document.createElement('div');
 wrapper.className='page-wrapper group relative bg-white rounded-[28px] shadow-[0_24px_64px_rgba(0,0,0,.12)] overflow-hidden border-[6px] border-white';
 wrapper.dataset.index=index;
 const canvCont=document.createElement('div'); canvCont.className='relative';
 const pdfCanvas=document.createElement('canvas'); pdfCanvas.className='pdf-canvas block';
 const annCanvas=document.createElement('canvas'); annCanvas.className='annotation-canvas absolute top-0 left-0 touch-none';
 canvCont.append(pdfCanvas, annCanvas);
 wrapper.appendChild(canvCont);
 const badge=document.createElement('div');
 badge.className='absolute top-4 right-4 bg-zinc-900 text-white text-[11px] font-black px-3 py-1.5 rounded-full shadow-lg flex items-center gap-1';
 badge.innerHTML=`<span>صفحة</span><span>${index+1}</span>`;
 wrapper.appendChild(badge);
 const actions=document.createElement('div');
 actions.className='absolute top-4 left-4 flex gap-2 opacity-0 group-hover:opacity-100 transition';
 actions.innerHTML=`<button class="w-9 h-9 bg-white rounded-full shadow flex items-center justify-center hover:bg-zinc-900 hover:text-white transition font-bold" title="تدوير">↻</button><button class="w-9 h-9 bg-white rounded-full shadow flex items-center justify-center hover:bg-red-500 hover:text-white transition font-bold" title="حذف">✕</button>`;
 wrapper.appendChild(actions);
 actions.children[0].onclick=()=>rotatePage(index);
 actions.children[1].onclick=()=>deletePage(index);
 pagesContainer.appendChild(wrapper);

 const viewport = pdfPage.getViewport({scale:state.renderScale});
 pdfCanvas.width=viewport.width; pdfCanvas.height=viewport.height;
 annCanvas.width=viewport.width; annCanvas.height=viewport.height;
 const displayW = viewport.width/state.renderScale;
 const displayH = viewport.height/state.renderScale;
 pdfCanvas.style.width=displayW+'px'; pdfCanvas.style.height=displayH+'px';
 annCanvas.style.width=displayW+'px'; annCanvas.style.height=displayH+'px';

 const ctx=pdfCanvas.getContext('2d');
 await pdfPage.render({canvasContext:ctx, viewport}).promise;

 const pageObj={pdfPage, pdfCanvas, annCanvas, annCtx:annCanvas.getContext('2d'), annotations:[], rotation:0, selectedIndex:null, wrapper, viewport};
 state.pages.push(pageObj);
 attachCanvasEvents(annCanvas,index);
 createThumbnail(index);
}

function createThumbnail(idx){
 const page=state.pages[idx];
 const thumbWrap=document.createElement('div');
 thumbWrap.className='group relative bg-white rounded-[18px] p-2 shadow-[0_8px_24px_rgba(0,0,0,.06)] border border-zinc-900/5 cursor-pointer hover:shadow-[0_12px_32px_rgba(0,0,0,.12)] transition';
 thumbWrap.innerHTML=`<canvas class="w-full rounded-[10px] bg-zinc-50"></canvas><div class="mt-2 flex items-center justify-between"><span class="text-[11px] font-black">صفحة ${idx+1}</span><span class="w-5 h-5 rounded-full bg-zinc-900 text-white flex items-center justify-center text-[10px] opacity-0 group-hover:opacity-100">↗</span></div>`;
 const c=thumbWrap.querySelector('canvas');
 const w=120; const ratio=page.pdfCanvas.height/page.pdfCanvas.width;
 c.width=w*2; c.height=w*ratio*2; c.style.width=w+'px'; c.style.height=(w*ratio)+'px';
 const tCtx=c.getContext('2d');
 tCtx.drawImage(page.pdfCanvas,0,0,c.width,c.height);
 tCtx.drawImage(page.annCanvas,0,0,c.width,c.height);
 thumbWrap.onclick=()=>{ document.querySelectorAll('.page-wrapper')[idx]?.scrollIntoView({behavior:'smooth',block:'center'}); state.selectedPage=idx; };
 thumbnailsEl.appendChild(thumbWrap);
 page.thumbCanvas=c; page.thumbWrap=thumbWrap;
}
function updateThumbnail(idx){
 const page=state.pages[idx]; if(!page||!page.thumbCanvas) return;
 const c=page.thumbCanvas; const tCtx=c.getContext('2d');
 tCtx.clearRect(0,0,c.width,c.height);
 tCtx.drawImage(page.pdfCanvas,0,0,c.width,c.height);
 tCtx.drawImage(page.annCanvas,0,0,c.width,c.height);
}

function getPos(e, canvas){
 const rect=canvas.getBoundingClientRect();
 const scaleX=canvas.width/rect.width, scaleY=canvas.height/rect.height;
 const clientX = e.touches? e.touches[0].clientX : e.clientX;
 const clientY = e.touches? e.touches[0].clientY : e.clientY;
 return {x:(clientX-rect.left)*scaleX, y:(clientY-rect.top)*scaleY, cssX:(clientX-rect.left), cssY:(clientY-rect.top), rect};
}

function hitTest(pageIdx, pos){
 const page=state.pages[pageIdx]; const ctx=page.annCtx;
 for(let i=page.annotations.length-1;i>=0;i--){
   const a=page.annotations[i];
   if(a.type==='text'){
     ctx.font=`${a.fontWeight||'normal'} ${a.fontSize}px Cairo`;
     const m=ctx.measureText(a.text.split('\n')[0]||'');
     const w=m.width, h=a.fontSize*1.4;
     if(pos.x>=a.x-10 && pos.x<=a.x+w+20 && pos.y>=a.y-h && pos.y<=a.y+10) return i;
   }else if(a.type==='rect'||a.type==='circle'||a.type==='image'){
     if(pos.x>=a.x-8 && pos.x<=a.x+a.w+8 && pos.y>=a.y-8 && pos.y<=a.y+a.h+8) return i;
   }else if(a.type==='draw'||a.type==='highlight'){
     for(const p of a.points){ if(Math.hypot(p.x-pos.x,p.y-pos.y)<18) return i; }
   }
 }
 return null;
}

function attachCanvasEvents(canvas, pageIdx){
 canvas.addEventListener('pointerdown', e=>{
   canvas.setPointerCapture(e.target===canvas?e.pointerId:0);
   const pos=getPos(e,canvas);
   state.selectedPage=pageIdx;
   const page=state.pages[pageIdx];
   if(state.currentTool==='select'){
     const hit=hitTest(pageIdx,pos);
     if(hit!==null){
       page.selectedIndex=hit; state.dragging=true;
       const a=page.annotations[hit];
       if(a.type==='draw'||a.type==='highlight'){ state.dragOff={x:pos.x-a.points[0].x, y:pos.y-a.points[0].y}; }
       else state.dragOff={x:pos.x-a.x, y:pos.y-a.y};
       redrawPage(pageIdx);
     }else{ page.selectedIndex=null; redrawPage(pageIdx); }
     return;
   }
   if(state.currentTool==='eraser'){
     const hit=hitTest(pageIdx,pos); if(hit!==null){ page.annotations.splice(hit,1); redrawPage(pageIdx); toast('تم المسح','🧹'); } return;
   }
   if(state.currentTool==='text'){
     showTextInput(pos,pageIdx,null,e);
     return;
   }
   if(state.currentTool==='draw'||state.currentTool==='highlight'){
     state.isDrawing=true; state.currentPath=[{x:pos.x,y:pos.y}]; return;
   }
   if(state.currentTool==='rect'||state.currentTool==='circle'){
     state.isDrawing=true; state.startPos={x:pos.x,y:pos.y}; state.currentPath=[{x:pos.x,y:pos.y}]; return;
   }
 });
 canvas.addEventListener('pointermove', e=>{
   const pos=getPos(e,canvas); const page=state.pages[pageIdx];
   if(state.dragging && state.currentTool==='select' && page.selectedIndex!==null){
     const a=page.annotations[page.selectedIndex];
     const dx=pos.x-state.dragOff.x, dy=pos.y-state.dragOff.y;
     if(a.type==='draw'||a.type==='highlight'){
       const ox=a.points[0].x, oy=a.points[0].y;
       const mx=dx-ox, my=dy-oy;
       a.points=a.points.map(p=>({x:p.x+mx,y:p.y+my}));
     }else{ a.x=dx; a.y=dy; }
     state.dragOff={x:pos.x-(a.type==='draw'||a.type==='highlight'?a.points[0].x:a.x), y:pos.y-(a.type==='draw'||a.type==='highlight'?a.points[0].y:a.y)};
     redrawPage(pageIdx); return;
   }
   if(!state.isDrawing) return;
   if(state.currentTool==='draw'||state.currentTool==='highlight'){
     state.currentPath.push({x:pos.x,y:pos.y});
     const ctx=page.annCtx;
     ctx.save(); ctx.lineCap='round'; ctx.lineJoin='round';
     ctx.strokeStyle=state.color; ctx.lineWidth=state.currentTool==='highlight'?state.size*3:state.size;
     if(state.currentTool==='highlight') ctx.globalAlpha=0.35;
     ctx.beginPath(); const pts=state.currentPath;
     if(pts.length>1){ ctx.moveTo(pts[pts.length-2].x,pts[pts.length-2].y); ctx.lineTo(pts[pts.length-1].x,pts[pts.length-1].y); ctx.stroke(); }
     ctx.restore();
   }else if(state.currentTool==='rect'||state.currentTool==='circle'){
     redrawPage(pageIdx);
     const ctx=page.annCtx; ctx.save(); ctx.strokeStyle=state.color; ctx.lineWidth=state.size;
     const w=pos.x-state.startPos.x, h=pos.y-state.startPos.y;
     if(state.currentTool==='rect') ctx.strokeRect(state.startPos.x,state.startPos.y,w,h);
     else { ctx.beginPath(); ctx.ellipse(state.startPos.x+w/2, state.startPos.y+h/2, Math.abs(w/2), Math.abs(h/2),0,0,Math.PI*2); ctx.stroke(); }
     ctx.restore();
   }
 });
 const endDraw = async (e)=>{
   if(!state.isDrawing) { state.dragging=false; return; }
   const pos=getPos(e,canvas); const page=state.pages[pageIdx];
   if(state.currentTool==='draw'||state.currentTool==='highlight'){
     if(state.currentPath.length>2){
       page.annotations.push({type:state.currentTool, points:[...state.currentPath], color:state.color, width:state.size});
     }
   }else if(state.currentTool==='rect'){
     const w=pos.x-state.startPos.x, h=pos.y-state.startPos.y;
     if(Math.abs(w)>6 && Math.abs(h)>6) page.annotations.push({type:'rect', x:state.startPos.x, y:state.startPos.y, w,h, color:state.color, width:state.size});
   }else if(state.currentTool==='circle'){
     const w=pos.x-state.startPos.x, h=pos.y-state.startPos.y;
     if(Math.abs(w)>6 && Math.abs(h)>6) page.annotations.push({type:'circle', x:state.startPos.x, y:state.startPos.y, w,h, color:state.color, width:state.size});
   }
   state.isDrawing=false; state.currentPath=[]; state.startPos=null;
   redrawPage(pageIdx);
   state.dragging=false;
 };
 canvas.addEventListener('pointerup', endDraw);
 canvas.addEventListener('pointerleave', ()=>{ state.isDrawing=false; state.dragging=false; });
}

function redrawPage(idx){
 const page=state.pages[idx]; if(!page) return;
 const ctx=page.annCtx; ctx.clearRect(0,0,ctx.canvas.width,ctx.canvas.height);
 page.annotations.forEach((a,i)=>{
   ctx.save();
   if(a.type==='draw'||a.type==='highlight'){
     ctx.lineCap='round'; ctx.lineJoin='round'; ctx.strokeStyle=a.color; ctx.lineWidth=a.type==='highlight'?a.width*3:a.width;
     if(a.type==='highlight') ctx.globalAlpha=0.34;
     ctx.beginPath(); if(a.points.length){ ctx.moveTo(a.points[0].x,a.points[0].y); for(let k=1;k<a.points.length;k++) ctx.lineTo(a.points[k].x,a.points[k].y); }
     ctx.stroke();
   }else if(a.type==='rect'){
     ctx.strokeStyle=a.color; ctx.lineWidth=a.width; ctx.strokeRect(a.x,a.y,a.w,a.h);
   }else if(a.type==='circle'){
     ctx.strokeStyle=a.color; ctx.lineWidth=a.width; ctx.beginPath(); ctx.ellipse(a.x+a.w/2,a.y+a.h/2,Math.abs(a.w/2),Math.abs(a.h/2),0,0,Math.PI*2); ctx.stroke();
   }else if(a.type==='text'){
     ctx.fillStyle=a.color; ctx.font=`${a.fontWeight||'normal'} ${a.fontSize}px Cairo`; ctx.textBaseline='alphabetic';
     const lines=a.text.split('\n'); lines.forEach((ln,j)=>ctx.fillText(ln,a.x,a.y+j*a.fontSize*1.25));
   }else if(a.type==='image'){
     if(a.img && a.img.complete) ctx.drawImage(a.img,a.x,a.y,a.w,a.h);
   }
   ctx.restore();
   if(page.selectedIndex===i){
     ctx.save(); ctx.strokeStyle='#7C3AED'; ctx.lineWidth=2; ctx.setLineDash([10,6]);
     let bx,by,bw,bh;
     if(a.type==='text'){ ctx.font=`${a.fontWeight||'normal'} ${a.fontSize}px Cairo`; const met=ctx.measureText(a.text.split('\n')[0]||''); bx=a.x-6; by=a.y-a.fontSize-6; bw=met.width+12; bh=a.fontSize*1.4+12; }
     else if(a.type==='rect'||a.type==='circle'||a.type==='image'){ bx=a.x-6; by=a.y-6; bw=a.w+12; bh=a.h+12; }
     else{ const xs=a.points.map(p=>p.x), ys=a.points.map(p=>p.y); bx=Math.min(...xs)-12; by=Math.min(...ys)-12; bw=Math.max(...xs)-Math.min(...xs)+24; bh=Math.max(...ys)-Math.min(...ys)+24; }
     ctx.strokeRect(bx,by,bw,bh); ctx.setLineDash([]); ctx.fillStyle='#7C3AED'; [[bx,by],[bx+bw,by],[bx,by+bh],[bx+bw,by+bh]].forEach(([x,y])=>ctx.fillRect(x-5,y-5,10,10)); ctx.restore();
   }
 });
 updateThumbnail(idx);
}

// text input
function showTextInput(pos, pageIdx, editIdx, evt){
 state.activeText={page:pageIdx, x:pos.x, y:pos.y, cssX:pos.cssX, cssY:pos.cssY, editIndex:editIdx, rect:pos.rect};
 textWrap.style.left=(pos.rect.left+pos.cssX)+'px';
 textWrap.style.top=(pos.rect.top+pos.cssY+window.scrollY)+'px';
 textWrap.classList.remove('hidden');
 textInput.value = editIdx!==null? state.pages[pageIdx].annotations[editIdx].text : '';
 textInput.focus();
 if(evt) evt.preventDefault();
}
$('#saveTextBtn').onclick=saveText;
$('#cancelTextBtn').onclick=()=>textWrap.classList.add('hidden');
textInput.addEventListener('keydown',e=>{
 if(e.key==='Enter' && (e.ctrlKey||e.metaKey)){ saveText(); }
 if(e.key==='Escape'){ textWrap.classList.add('hidden'); }
});
function saveText(){
 const t=textInput.value.trim(); if(!t){ textWrap.classList.add('hidden'); return; }
 const at=state.activeText; const page=state.pages[at.page];
 if(at.editIndex!==null){
   page.annotations[at.editIndex].text=t;
 }else{
   page.annotations.push({type:'text', x:at.x, y:at.y, text:t, fontSize:state.fontSize*state.renderScale*0.75, color:state.color, fontWeight:state.isBold?'bold':'normal'});
 }
 textWrap.classList.add('hidden'); redrawPage(at.page); toast('تمت إضافة النص','✍️');
}

// image
imageInput.onchange=async e=>{
 const file=e.target.files[0]; if(!file) return;
 const url=URL.createObjectURL(file); const img=new Image(); img.src=url; await img.decode();
 const pageIdx=state.selectedPage||0; const page=state.pages[pageIdx]; if(!page) return;
 const w=220*state.renderScale, h=(img.height/img.width)*w;
 page.annotations.push({type:'image', x:100*state.renderScale, y:100*state.renderScale, w,h, img});
 redrawPage(pageIdx); toast('تمت إضافة الصورة','🖼️');
 e.target.value='';
};

// signature modal
const sigModal=$('#signatureModal'), sigCanvas=$('#sigCanvas'), sigCtx=sigCanvas.getContext('2d');
function openSigModal(){ sigModal.classList.remove('hidden'); sigCtx.clearRect(0,0,sigCanvas.width,sigCanvas.height); sigCtx.lineWidth=4; sigCtx.lineCap='round'; sigCtx.strokeStyle='#111827'; }
$('#closeSigModal').onclick=$('#cancelSig').onclick=()=>sigModal.classList.add('hidden');
$('#clearSig').onclick=()=>sigCtx.clearRect(0,0,sigCanvas.width,sigCanvas.height);
let sigPath=[];
sigCanvas.addEventListener('pointerdown',e=>{ state.sigDrawing=true; const r=sigCanvas.getBoundingClientRect(); const x=(e.clientX-r.left)*(sigCanvas.width/r.width), y=(e.clientY-r.top)*(sigCanvas.height/r.height); sigPath=[{x,y}]; sigCtx.beginPath(); sigCtx.moveTo(x,y); });
sigCanvas.addEventListener('pointermove',e=>{ if(!state.sigDrawing) return; const r=sigCanvas.getBoundingClientRect(); const x=(e.clientX-r.left)*(sigCanvas.width/r.width), y=(e.clientY-r.top)*(sigCanvas.height/r.height); sigPath.push({x,y}); sigCtx.lineTo(x,y); sigCtx.stroke(); });
sigCanvas.addEventListener('pointerup',()=>{ state.sigDrawing=false; });
$('#saveSig').onclick=async()=>{
 const data=sigCanvas.toDataURL('image/png'); const img=new Image(); img.src=data; await img.decode();
 const pageIdx=state.selectedPage||0; const page=state.pages[pageIdx];
 const w=260*state.renderScale, h=(img.height/img.width)*w;
 page.annotations.push({type:'image', x:80*state.renderScale, y:80*state.renderScale, w,h, img, isSig:true});
 redrawPage(pageIdx); sigModal.classList.add('hidden'); toast('تم حفظ التوقيع','✒️');
};

// page actions
window.rotatePage=async idx=>{
 const page=state.pages[idx]; page.rotation=(page.rotation+90)%360;
 const viewport=page.pdfPage.getViewport({scale:state.renderScale, rotation:page.rotation});
 page.pdfCanvas.width=viewport.width; page.pdfCanvas.height=viewport.height;
 page.annCanvas.width=viewport.width; page.annCanvas.height=viewport.height;
 page.pdfCanvas.style.width=viewport.width/state.renderScale+'px'; page.pdfCanvas.style.height=viewport.height/state.renderScale+'px';
 page.annCanvas.style.width=viewport.width/state.renderScale+'px'; page.annCanvas.style.height=viewport.height/state.renderScale+'px';
 const ctx=page.pdfCanvas.getContext('2d'); await page.pdfPage.render({canvasContext:ctx, viewport}).promise;
 redrawPage(idx);
};
window.deletePage=idx=>{
 if(state.pages.length<=1){ toast('لا يمكن حذف كل الصفحات','⚠️'); return; }
 state.pages[idx].wrapper.remove(); state.pages[idx].thumbWrap.remove();
 state.pages.splice(idx,1);
 document.querySelectorAll('.page-wrapper').forEach((el,i)=>el.dataset.index=i);
 pageCountEl.textContent=state.pages.length;
 toast('تم حذف الصفحة','🗑️');
};
$('#addPageBtn').onclick=()=>{
 const idx=state.pages.length;
 const wrapper=document.createElement('div');
 wrapper.className='page-wrapper relative bg-white rounded-[28px] shadow-[0_24px_64px_rgba(0,0,0,.12)] overflow-hidden border-[6px] border-white';
 const canvCont=document.createElement('div'); canvCont.className='relative';
 const pdfCanvas=document.createElement('canvas'); pdfCanvas.className='pdf-canvas block';
 const annCanvas=document.createElement('canvas'); annCanvas.className='annotation-canvas absolute top-0 left-0';
 canvCont.append(pdfCanvas,annCanvas); wrapper.appendChild(canvCont);
 pagesContainer.appendChild(wrapper);
 const w=794*state.renderScale, h=1123*state.renderScale;
 pdfCanvas.width=w; pdfCanvas.height=h; annCanvas.width=w; annCanvas.height=h;
 pdfCanvas.style.width=w/state.renderScale+'px'; pdfCanvas.style.height=h/state.renderScale+'px';
 annCanvas.style.width=w/state.renderScale+'px'; annCanvas.style.height=h/state.renderScale+'px';
 const ctx=pdfCanvas.getContext('2d'); ctx.fillStyle='white'; ctx.fillRect(0,0,w,h);
 // light grid
 ctx.strokeStyle='#f5f5f5'; ctx.lineWidth=1;
 for(let x=0;x<w;x+=40*state.renderScale){ ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,h); ctx.stroke(); }
 for(let y=0;y<h;y+=40*state.renderScale){ ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(w,y); ctx.stroke(); }
 const pageObj={pdfPage:null, pdfCanvas, annCanvas, annCtx:annCanvas.getContext('2d'), annotations:[], rotation:0, selectedIndex:null, wrapper};
 state.pages.push(pageObj); attachCanvasEvents(annCanvas,idx); createThumbnail(idx);
 pageCountEl.textContent=state.pages.length; toast('تمت إضافة صفحة فارغة','➕');
};

// delete annotation
$('#deleteAnnBtn').onclick=()=>{
 const p=state.pages[state.selectedPage]; if(!p||p.selectedIndex===null) { toast('اختر عنصراً أولاً','⚠️'); return; }
 p.annotations.splice(p.selectedIndex,1); p.selectedIndex=null; redrawPage(state.selectedPage); toast('تم الحذف','🗑️');
};
document.addEventListener('keydown',e=>{
 if(e.key==='Delete'||e.key==='Backspace'){ if(document.activeElement!==textInput){ const p=state.pages[state.selectedPage]; if(p&&p.selectedIndex!==null){ p.annotations.splice(p.selectedIndex,1); p.selectedIndex=null; redrawPage(state.selectedPage);} } }
});

// export
$('#exportBtn').onclick=exportPDF;
async function exportPDF(){
 if(!state.pages.length){ toast('لا يوجد ملف للتصدير','⚠️'); return; }
 toast('جاري تجهيز PDF للتصدير...','⏳');
 const {PDFDocument}=PDFLib; const newPdf=await PDFDocument.create();
 for(let i=0;i<state.pages.length;i++){
   const page=state.pages[i];
   const combined=document.createElement('canvas'); combined.width=page.pdfCanvas.width; combined.height=page.pdfCanvas.height;
   const cCtx=combined.getContext('2d'); cCtx.fillStyle='white'; cCtx.fillRect(0,0,combined.width,combined.height);
   cCtx.drawImage(page.pdfCanvas,0,0); cCtx.drawImage(page.annCanvas,0,0);
   const dataUrl=combined.toDataURL('image/jpeg',0.88);
   const base64=dataUrl.split(',')[1]; const bytes=Uint8Array.from(atob(base64),c=>c.charCodeAt(0));
   const jpg=await newPdf.embedJpg(bytes);
   const pdfPage=newPdf.addPage([combined.width, combined.height]);
   pdfPage.drawImage(jpg,{x:0,y:0,width:combined.width,height:combined.height});
 }
 const pdfBytes=await newPdf.save();
 const blob=new Blob([pdfBytes],{type:'application/pdf'}); const url=URL.createObjectURL(blob);
 const a=document.createElement('a'); a.href=url; a.download=`فن-PDF-${Date.now()}.pdf`; a.click();
 URL.revokeObjectURL(url); toast('تم التصدير بنجاح! 🎉','✅');
}
</script>
</body>
</html>
