[laika_bookstore_website (1).html](https://github.com/user-attachments/files/32793678/laika_bookstore_website.1.html)
<!DOCTYPE html>
<html lang="ko" class="scroll-smooth">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>LAIKA BOOKSTORE | 우주 개 라이카 동화책 전문 서점</title>

  <!-- Tailwind CSS -->
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            space: {
              950: '#070a14',
              900: '#0e1326',
              850: '#131a33',
              800: '#1a2342',
              700: '#25325c',
              600: '#34457a'
            },
            star: {
              300: '#fde047',
              400: '#fbbf24',
              500: '#f59e0b',
              600: '#d97706'
            }
          },
          fontFamily: {
            serif: ['Gowun Batang', 'Georgia', 'serif'],
            sans: ['Noto Sans KR', 'Inter', 'sans-serif']
          }
        }
      }
    }
  </script>

  <!-- Fonts & Icons -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Gowun+Batang:wght@400;700&family=Noto+Sans+KR:wght@300;400;500;700&display=swap" rel="stylesheet">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css">

  <style>
    ::-webkit-scrollbar { width: 8px; }
    ::-webkit-scrollbar-track { background: #070a14; }
    ::-webkit-scrollbar-thumb { background: #25325c; border-radius: 4px; }
    ::-webkit-scrollbar-thumb:hover { background: #f59e0b; }

    .star-bg {
      background-color: #070a14;
      background-image: 
        radial-gradient(1.5px 1.5px at 25px 35px, #ffffff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 50px 80px, #f59e0b, rgba(0,0,0,0)),
        radial-gradient(1px 1px at 120px 40px, #ffffff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 180px 140px, #fbbf24, rgba(0,0,0,0)),
        radial-gradient(1.5px 1.5px at 260px 210px, #ffffff, rgba(0,0,0,0));
      background-repeat: repeat;
      background-size: 320px 320px;
    }

    .glass-nav {
      background: rgba(14, 19, 38, 0.88);
      backdrop-filter: blur(16px);
      -webkit-backdrop-filter: blur(16px);
    }

    .glass-card {
      background: rgba(26, 35, 66, 0.65);
      backdrop-filter: blur(10px);
      border: 1px solid rgba(255, 255, 255, 0.08);
    }

    .glass-card:hover {
      border-color: rgba(245, 158, 11, 0.4);
    }

    .glow-gold {
      box-shadow: 0 0 25px rgba(245, 158, 11, 0.25);
    }

    /* Modal Animation */
    .modal-enter { opacity: 0; transform: scale(0.95); }
    .modal-enter-active { opacity: 1; transform: scale(1); transition: opacity 200ms, transform 200ms; }
    .modal-exit { opacity: 1; transform: scale(1); }
    .modal-exit-active { opacity: 0; transform: scale(0.95); transition: opacity 150ms, transform 150ms; }

    /* Toast Notification */
    #toastContainer {
      position: fixed;
      bottom: 24px;
      right: 24px;
      z-index: 100;
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .toast {
      animation: slideIn 0.3s cubic-bezier(0.16, 1, 0.3, 1), fadeOut 0.3s 2.7s forwards;
    }

    @keyframes slideIn {
      from { transform: translateY(100%); opacity: 0; }
      to { transform: translateY(0); opacity: 1; }
    }
    @keyframes fadeOut {
      from { opacity: 1; }
      to { opacity: 0; }
    }
  </style>
</head>
<body class="star-bg text-slate-100 font-sans min-h-screen flex flex-col antialiased selection:bg-star-500 selection:text-space-950">

  <!-- Toast Notification Container -->
  <div id="toastContainer" class="pointer-events-none"></div>

  <!-- Header Navigation -->
  <header class="sticky top-0 z-40 glass-nav border-b border-slate-800/80">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-20 flex items-center justify-between gap-4">
      
      <!-- Logo -->
      <a href="#" class="flex items-center gap-3 group shrink-0">
        <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-star-600 via-star-500 to-amber-200 flex items-center justify-center text-space-950 shadow-lg shadow-star-500/20 group-hover:scale-105 transition-transform duration-300">
          <i class="fa-solid fa-dog text-xl"></i>
        </div>
        <div class="flex flex-col">
          <span class="font-serif text-2xl font-bold tracking-tight text-white group-hover:text-star-400 transition-colors">LAIKA</span>
          <span class="text-[10px] tracking-widest text-star-400 uppercase font-semibold">Space Dog Bookstore</span>
        </div>
      </a>

      <!-- Search Bar -->
      <div class="hidden md:flex flex-1 max-w-md mx-6 relative">
        <input type="text" id="searchInput" placeholder="동화책 제목, 저자, 키워드 검색..." 
          class="w-full bg-space-900/90 border border-slate-700/80 rounded-full py-2.5 pl-11 pr-10 text-sm text-slate-200 placeholder-slate-400 focus:outline-none focus:border-star-500 focus:ring-1 focus:ring-star-500 transition-all">
        <i class="fa-solid fa-magnifying-glass absolute left-4 top-1/2 -translate-y-1/2 text-slate-400 text-sm"></i>
        <button id="clearSearchBtn" class="hidden absolute right-3 top-1/2 -translate-y-1/2 text-slate-400 hover:text-white text-xs p-1">
          <i class="fa-solid fa-xmark"></i>
        </button>
      </div>

      <!-- Actions -->
      <nav class="flex items-center gap-4 sm:gap-6">
        <div class="hidden lg:flex items-center gap-6 text-sm font-medium text-slate-300">
          <a href="#curation" class="hover:text-star-400 transition-colors">도서 목록</a>
          <a href="#spotlight" class="hover:text-star-400 transition-colors">이달의 픽</a>
          <a href="#reviews" class="hover:text-star-400 transition-colors">독자 후기</a>
          <a href="#guestbook" class="hover:text-star-400 transition-colors">우주 방명록</a>
        </div>

        <div class="flex items-center gap-3 border-l border-slate-700/60 pl-4 sm:pl-6">
          <!-- Wishlist Button -->
          <button id="wishlistBtn" class="relative p-2.5 text-slate-300 hover:text-star-400 hover:bg-space-800 rounded-full transition-all" title="위시리스트">
            <i class="fa-regular fa-heart text-xl"></i>
            <span id="wishBadge" class="absolute top-1 right-1 w-4 h-4 bg-rose-500 text-white text-[10px] font-bold rounded-full flex items-center justify-center hidden">0</span>
          </button>

          <!-- Cart Button -->
          <button id="cartBtn" class="relative flex items-center gap-2 bg-star-500 hover:bg-star-400 text-space-950 font-bold px-4 py-2 rounded-full shadow-md shadow-star-500/10 transition-all hover:scale-105 active:scale-95">
            <i class="fa-solid fa-bag-shopping"></i>
            <span class="hidden sm:inline text-sm">장바구니</span>
            <span id="cartBadge" class="bg-space-950 text-star-400 text-xs font-bold px-2 py-0.5 rounded-full">0</span>
          </button>
        </div>
      </nav>

    </div>
  </header>

  <!-- Mobile Search Bar -->
  <div class="md:hidden px-4 py-3 bg-space-900 border-b border-slate-800">
    <div class="relative">
      <input type="text" id="mobileSearchInput" placeholder="동화책, 저자 검색..." 
        class="w-full bg-space-950 border border-slate-700 rounded-full py-2 pl-10 pr-4 text-sm text-slate-200 placeholder-slate-400 focus:outline-none focus:border-star-500">
      <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400 text-sm"></i>
    </div>
  </div>

  <!-- Hero Section -->
  <section class="relative py-16 sm:py-24 lg:py-28 overflow-hidden">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10 text-center">
      
      <div class="inline-flex items-center gap-2 px-4 py-1.5 rounded-full bg-star-500/10 border border-star-500/30 text-star-400 text-xs sm:text-sm font-medium mb-6">
        <i class="fa-solid fa-star text-xs animate-pulse"></i>
        <span>우주 개 라이카를 기억하는 감성 동화 전문 서점</span>
      </div>

      <h1 class="font-serif text-3xl sm:text-5xl lg:text-6xl font-bold text-white tracking-tight leading-tight mb-6">
        차가운 우주를 건너온<br>
        <span class="text-transparent bg-clip-text bg-gradient-to-r from-star-400 via-amber-200 to-amber-400">가장 따뜻한 별의 이야기</span>
      </h1>

      <p class="max-w-2xl mx-auto text-base sm:text-lg text-slate-300 font-light leading-relaxed mb-10">
        1957년 우주로 떠난 최초의 동물 생명체 라이카. 역사의 한 페이지 속에 묵묵히 빛나는 용감한 작은 영웅의 이야기를 동화책과 그림책으로 소장해보세요.
      </p>

      <div class="flex flex-wrap justify-center gap-4">
        <a href="#curation" class="bg-star-500 hover:bg-star-400 text-space-950 font-bold px-8 py-3.5 rounded-full shadow-lg shadow-star-500/20 hover:shadow-star-500/40 transition-all hover:-translate-y-0.5">
          도서 둘러보기 <i class="fa-solid fa-arrow-right ml-2 text-sm"></i>
        </a>
        <a href="#spotlight" class="bg-space-800/80 hover:bg-space-700 text-slate-200 border border-slate-700 px-8 py-3.5 rounded-full font-medium transition-all hover:-translate-y-0.5">
          이달의 추천 도서
        </a>
      </div>

      <!-- Banner Features -->
      <div class="mt-16 grid grid-cols-2 md:grid-cols-4 gap-4 max-w-4xl mx-auto text-left">
        <div class="glass-card p-4 rounded-2xl flex items-center gap-3">
          <div class="w-10 h-10 rounded-xl bg-star-500/10 border border-star-500/20 flex items-center justify-center text-star-400 shrink-0">
            <i class="fa-solid fa-book-bookmark text-lg"></i>
          </div>
          <div>
            <div class="text-xs text-slate-400">엄선된 큐레이션</div>
            <div class="text-sm font-bold text-slate-200">라이카 전문 도서</div>
          </div>
        </div>

        <div class="glass-card p-4 rounded-2xl flex items-center gap-3">
          <div class="w-10 h-10 rounded-xl bg-star-500/10 border border-star-500/20 flex items-center justify-center text-star-400 shrink-0">
            <i class="fa-solid fa-gift text-lg"></i>
          </div>
          <div>
            <div class="text-xs text-slate-400">전 구성원 혜택</div>
            <div class="text-sm font-bold text-slate-200">라이카 기념 엽서 증정</div>
          </div>
        </div>

        <div class="glass-card p-4 rounded-2xl flex items-center gap-3">
          <div class="w-10 h-10 rounded-xl bg-star-500/10 border border-star-500/20 flex items-center justify-center text-star-400 shrink-0">
            <i class="fa-solid fa-truck-fast text-lg"></i>
          </div>
          <div>
            <div class="text-xs text-slate-400">안심 신속 배송</div>
            <div class="text-sm font-bold text-slate-200">무료 배송 혜택</div>
          </div>
        </div>

        <div class="glass-card p-4 rounded-2xl flex items-center gap-3">
          <div class="w-10 h-10 rounded-xl bg-star-500/10 border border-star-500/20 flex items-center justify-center text-star-400 shrink-0">
            <i class="fa-solid fa-heart-circle-check text-lg"></i>
          </div>
          <div>
            <div class="text-xs text-slate-400">독자 평점</div>
            <div class="text-sm font-bold text-slate-200">★ 4.9/5.0 만족도</div>
          </div>
        </div>
      </div>

    </div>
  </section>

  <!-- Book Curation Section -->
  <section id="curation" class="py-16 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
    
    <!-- Section Header -->
    <div class="flex flex-col md:flex-row md:items-end justify-between mb-8 gap-4">
      <div>
        <span class="text-star-400 text-xs font-bold tracking-widest uppercase mb-2 block">BOOK CURATION</span>
        <h2 class="font-serif text-3xl sm:text-4xl font-bold text-white">라이카 서점 큐레이션 목록</h2>
      </div>

      <!-- Sorting Selector -->
      <div class="flex items-center gap-3">
        <label for="sortSelect" class="text-xs text-slate-400 shrink-0">정렬:</label>
        <select id="sortSelect" class="bg-space-900 border border-slate-700 text-xs sm:text-sm text-slate-200 rounded-lg px-3 py-2 focus:outline-none focus:border-star-500 cursor-pointer">
          <option value="recommended">추천순</option>
          <option value="popular">인기순</option>
          <option value="priceLow">낮은 가격순</option>
          <option value="priceHigh">높은 가격순</option>
          <option value="rating">평점 높은순</option>
        </select>
      </div>
    </div>

    <!-- Category Tabs -->
    <div class="flex items-center gap-2 overflow-x-auto pb-4 mb-8 no-scrollbar" id="categoryTabs">
      <!-- Generated via JS -->
    </div>

    <!-- Results Count Indicator -->
    <div class="flex justify-between items-center mb-6 text-xs text-slate-400">
      <span>총 <strong id="resultCount" class="text-star-400 font-bold">0</strong>권의 도서</span>
      <span id="activeCategoryName" class="text-slate-300">전체 도서</span>
    </div>

    <!-- Book Grid -->
    <div id="bookGrid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
      <!-- Books Injected by JS -->
    </div>
  </section>

  <!-- Monthly Spotlight Section -->
  <section id="spotlight" class="py-20 bg-space-900/70 border-y border-slate-800">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      
      <div class="glass-card rounded-3xl p-8 lg:p-12 relative overflow-hidden">
        <div class="absolute -right-20 -bottom-20 w-96 h-96 bg-star-500/10 rounded-full blur-3xl pointer-events-none"></div>

        <div class="grid grid-cols-1 lg:grid-cols-12 gap-10 items-center relative z-10">
          
          <!-- Book Art Illustration View -->
          <div class="lg:col-span-5 flex justify-center">
            <div class="relative w-64 sm:w-72 aspect-[3/4] rounded-2xl bg-gradient-to-tr from-space-950 via-space-800 to-indigo-950 p-6 shadow-2xl border border-star-500/40 flex flex-col justify-between group hover:rotate-1 transition-transform duration-500">
              <div class="flex justify-between items-start">
                <span class="bg-star-500 text-space-950 text-xs font-bold px-2.5 py-1 rounded-md">이달의 추천 도서</span>
                <i class="fa-solid fa-moon text-star-400 text-xl"></i>
              </div>

              <div class="my-auto text-center py-6">
                <div class="w-24 h-24 mx-auto mb-4 rounded-full bg-space-900/90 border border-star-500/40 flex items-center justify-center text-star-400 shadow-inner">
                  <i class="fa-solid fa-user-astronaut text-4xl"></i>
                </div>
                <h3 class="font-serif text-2xl font-bold text-white mb-1">우주로 간 작은 별, 라이카</h3>
                <p class="text-xs text-amber-200/80">어린이 대표 그림책</p>
              </div>

              <div class="flex justify-between items-end text-[11px] text-slate-400 border-t border-slate-800 pt-3">
                <span>저자: 라이카 기획팀</span>
                <span>우주동화 출판사</span>
              </div>
            </div>
          </div>

          <!-- Spotlight Info -->
          <div class="lg:col-span-7 flex flex-col items-start">
            <div class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-star-500/20 text-star-400 text-xs font-semibold mb-4">
              <i class="fa-solid fa-award"></i> 2026 올해의 감성 아동 도서 우수작
            </div>

            <h2 class="font-serif text-3xl sm:text-4xl font-bold text-white mb-4 leading-tight">
              『우주로 간 작은 별, 라이카』
            </h2>

            <p class="text-slate-300 text-base mb-6 leading-relaxed font-light">
              스푸트니크 2호를 타고 차가운 우주로 떠난 용감한 강아지 라이카. 어두운 밤하늘 속에서 아름다운 별자리가 되어 사람들의 마음에 위로를 건네는 잔잔하고 감동적인 이야기를 만나보세요.
            </p>

            <div class="w-full bg-space-950/80 border-l-4 border-star-500 p-4 rounded-r-xl mb-8">
              <p class="font-serif text-amber-200/90 text-sm sm:text-base italic">
                "밤하늘의 가장 반짝이는 별을 볼 때마다 라이카의 안녕을 기도하게 됩니다."
              </p>
            </div>

            <div class="flex flex-wrap gap-4 w-full sm:w-auto">
              <button onclick="openBookModal(1)" class="flex-1 sm:flex-initial bg-star-500 hover:bg-star-400 text-space-950 font-bold px-6 py-3 rounded-xl transition-all flex items-center justify-center gap-2">
                <i class="fa-solid fa-book-open"></i> 상세 및 미리보기
              </button>
              <button onclick="addToCart(1)" class="flex-1 sm:flex-initial bg-space-800 hover:bg-space-700 border border-slate-700 text-slate-200 font-semibold px-6 py-3 rounded-xl transition-all flex items-center justify-center gap-2">
                <i class="fa-solid fa-cart-plus"></i> 14,400원 장바구니 담기
              </button>
            </div>
          </div>

        </div>

      </div>

    </div>
  </section>

  <!-- Reader Reviews Section -->
  <section id="reviews" class="py-16 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
    <div class="text-center mb-12">
      <span class="text-star-400 text-xs font-bold tracking-widest uppercase mb-2 block">REVIEWS & STORIES</span>
      <h2 class="font-serif text-3xl font-bold text-white mb-3">독자 후기 & 별빛 평점</h2>
      <p class="text-slate-400 text-sm max-w-lg mx-auto">라이카 동화책을 먼저 접한 학부모님들과 어린이 독자들의 따뜻한 소감입니다.</p>
    </div>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-6" id="reviewsGrid">
      <!-- Reviews injected via JS -->
    </div>
  </section>

  <!-- Interactive Guestbook Section -->
  <section id="guestbook" class="py-16 bg-space-900/50 border-t border-slate-800">
    <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="glass-card p-8 sm:p-10 rounded-3xl border border-slate-800 text-center">
        <div class="w-14 h-14 mx-auto mb-4 rounded-2xl bg-star-500/10 border border-star-500/30 flex items-center justify-center text-star-400 text-2xl">
          <i class="fa-solid fa-paper-plane"></i>
        </div>
        
        <h2 class="font-serif text-2xl sm:text-3xl font-bold text-white mb-2">우주 개 라이카에게 남기는 한 줄 메시지</h2>
        <p class="text-slate-400 text-sm mb-8">별이 된 라이카에게 여러분의 따뜻한 안부 인사를 보내주세요.</p>

        <!-- Message Form -->
        <form id="guestbookForm" class="flex flex-col sm:flex-row gap-3 mb-8">
          <input type="text" id="guestNameInput" placeholder="이름 (닉네임)" required
            class="sm:w-1/3 bg-space-950 border border-slate-700 rounded-xl px-4 py-3 text-sm text-slate-200 placeholder-slate-500 focus:outline-none focus:border-star-500">
          <input type="text" id="guestMsgInput" placeholder="라이카에게 전할 메시지를 작성해 주세요..." required
            class="flex-1 bg-space-950 border border-slate-700 rounded-xl px-4 py-3 text-sm text-slate-200 placeholder-slate-500 focus:outline-none focus:border-star-500">
          <button type="submit" class="bg-star-500 hover:bg-star-400 text-space-950 font-bold px-6 py-3 rounded-xl transition-colors shrink-0">
            보내기
          </button>
        </form>

        <!-- Message List -->
        <div id="guestbookList" class="space-y-3 text-left max-h-60 overflow-y-auto pr-2">
          <!-- Injected via JS -->
        </div>
      </div>
    </div>
  </section>

  <!-- Footer -->
  <footer class="mt-auto bg-space-950 border-t border-slate-800 py-10 text-slate-400 text-xs">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col md:flex-row justify-between items-center gap-6">
      <div class="flex items-center gap-3">
        <div class="w-8 h-8 rounded-lg bg-star-500 flex items-center justify-center text-space-950 font-bold">L</div>
        <div>
          <div class="font-serif font-bold text-slate-200 text-sm">LAIKA BOOKSTORE</div>
          <div class="text-slate-500">우주 개 라이카 동화책 전문 서점 프로젝트</div>
        </div>
      </div>
      <div class="text-slate-500 text-center md:text-right">
        본 사이트는 라이카 실화를 동화로 기록하는 프로젝트 서점 시안입니다.<br>
        © 2026 LAIKA BOOKSTORE. All rights reserved.
      </div>
    </div>
  </footer>

  <!-- ==================== MODALS ==================== -->

  <!-- Book Detail & Preview Modal -->
  <div id="bookModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-space-950/80 backdrop-blur-md hidden">
    <div class="glass-card max-w-2xl w-full rounded-2xl border border-slate-700 p-6 sm:p-8 relative max-h-[90vh] overflow-y-auto">
      <button id="closeBookModalBtn" class="absolute top-4 right-4 p-2 text-slate-400 hover:text-white rounded-lg hover:bg-space-800 transition-colors">
        <i class="fa-solid fa-xmark text-xl"></i>
      </button>

      <div id="bookModalContent">
        <!-- Dynamic Book Details -->
      </div>
    </div>
  </div>

  <!-- Cart Drawer -->
  <div id="cartDrawer" class="fixed inset-0 z-50 pointer-events-none opacity-0 transition-opacity duration-300">
    <div id="cartOverlay" class="absolute inset-0 bg-space-950/80 backdrop-blur-sm pointer-events-auto"></div>

    <div class="fixed inset-y-0 right-0 max-w-full flex pl-10 pointer-events-auto">
      <div id="cartPanel" class="w-screen max-w-md bg-space-900 border-l border-slate-800 p-6 flex flex-col justify-between shadow-2xl translate-x-full transition-transform duration-300">
        
        <!-- Header -->
        <div>
          <div class="flex items-center justify-between pb-4 border-b border-slate-800">
            <h3 class="font-serif text-xl font-bold text-white flex items-center gap-2">
              <i class="fa-solid fa-bag-shopping text-star-400"></i> 장바구니
            </h3>
            <button id="closeCartBtn" class="p-2 text-slate-400 hover:text-white rounded-lg hover:bg-space-800">
              <i class="fa-solid fa-xmark text-lg"></i>
            </button>
          </div>

          <!-- Items List -->
          <div id="cartItemsList" class="py-6 space-y-4 max-h-[calc(100vh-280px)] overflow-y-auto">
            <!-- Dynamic Cart Items -->
          </div>
        </div>

        <!-- Footer -->
        <div class="border-t border-slate-800 pt-6 space-y-4">
          <div class="flex justify-between items-center text-slate-300">
            <span>총 결제 예정 금액</span>
            <span id="cartTotalPrice" class="font-bold text-xl text-star-400">0원</span>
          </div>

          <p class="text-[11px] text-slate-500 text-center">
            * 본 서점은 전시용 프로젝트로 실제 결제가 이루어지지 않습니다.
          </p>

          <button id="checkoutBtn" class="w-full bg-star-500 hover:bg-star-400 text-space-950 font-bold py-3.5 rounded-xl shadow-lg transition-all flex items-center justify-center gap-2">
            주문 진행하기 <i class="fa-solid fa-arrow-right text-sm"></i>
          </button>
        </div>

      </div>
    </div>
  </div>

  <!-- Wishlist Modal -->
  <div id="wishlistModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-space-950/80 backdrop-blur-md hidden">
    <div class="glass-card max-w-lg w-full rounded-2xl border border-slate-700 p-6 sm:p-8 relative max-h-[80vh] overflow-y-auto">
      <div class="flex justify-between items-center mb-6 pb-3 border-b border-slate-800">
        <h3 class="font-serif text-xl font-bold text-white flex items-center gap-2">
          <i class="fa-solid fa-heart text-rose-500"></i> 내가 찜한 도서
        </h3>
        <button id="closeWishlistModalBtn" class="p-1.5 text-slate-400 hover:text-white">
          <i class="fa-solid fa-xmark text-xl"></i>
        </button>
      </div>

      <div id="wishlistModalContent" class="space-y-3">
        <!-- Wishlist items -->
      </div>
    </div>
  </div>

  <!-- Checkout Demo Modal -->
  <div id="checkoutModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-space-950/85 backdrop-blur-md hidden">
    <div class="glass-card max-w-md w-full rounded-2xl border border-slate-700 p-6 sm:p-8 text-center">
      <div class="w-16 h-16 bg-star-500/20 text-star-400 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl">
        <i class="fa-solid fa-rocket"></i>
      </div>
      <h3 class="font-serif text-2xl font-bold text-white mb-2">주문 체험 완료!</h3>
      <p class="text-slate-300 text-sm mb-6 leading-relaxed">
        우주 개 라이카 서점 프로젝트에 함께해주셔서 감사합니다.<br>라이카의 따뜻한 이야기가 여러분의 밤하늘을 환히 밝혀주길 바랍니다.
      </p>
      <button id="confirmCheckoutBtn" class="w-full bg-star-500 hover:bg-star-400 text-space-950 font-bold py-3 rounded-xl">
        확인
      </button>
    </div>
  </div>

  <!-- JavaScript Logic -->
  <script>
    
    // Master Book Data
    const booksData = [
      {
        id: 1,
        title: "우주로 간 작은 별, 라이카",
        author: "라이카 프로젝트 팀",
        publisher: "우주동화",
        category: "children",
        categoryName: "유아·어린이 동화",
        ageGroup: "5~8세",
        price: 14400,
        originalPrice: 16000,
        rating: 4.9,
        reviewsCount: 128,
        popularity: 98,
        badge: "BEST",
        icon: "fa-user-astronaut",
        bgGradient: "from-amber-900/40 via-space-900 to-indigo-950",
        summary: "1957년 밤하늘로 떠난 강아지 라이카가 온 우주의 가장 따뜻한 별이 되는 감동 그림책입니다.",
        previewPages: [
          "지구의 자그마한 골목길에서 라이카는 밤하늘 별들을 바라보며 꿈을 꾸었습니다.",
          "스푸트니크 2호의 문이 열리고, 라이카는 용감하게 우주로 향하는 첫 발을 내딛었습니다.",
          "차가운 우주 대신, 아이들의 순수한 마음 속에서 라이카는 영원히 행복하게 달립니다."
        ]
      },
      {
        id: 2,
        title: "별에서 온 신호: 라이카의 은하수 교신",
        author: "김은별 작가",
        publisher: "은하수북스",
        category: "graphic",
        categoryName: "그래픽 노블",
        ageGroup: "초등 전학년",
        price: 16200,
        originalPrice: 18000,
        rating: 4.8,
        reviewsCount: 94,
        popularity: 85,
        badge: "RECOMMEND",
        icon: "fa-star",
        bgGradient: "from-indigo-950 via-space-900 to-purple-950",
        summary: "우주 과학 소년과 라이카 별자리가 나누는 비밀스러운 은하수 교신을 다룬 서사 그래픽 노블.",
        previewPages: [
          "망원경 너머로 북쪽 하늘을 관찰하던 날, 은은한 황금빛 신호가 도착했습니다.",
          "'안녕, 지구의 친구야! 나는 밤하늘의 은하수 위에서 잘 지내고 있어.'"
        ]
      },
      {
        id: 3,
        title: "어린이를 위한 우주 탐사 역사와 라이카",
        author: "우주과학연구회",
        publisher: "미래탐구",
        category: "science",
        categoryName: "우주·과학 교양",
        ageGroup: "초등 고학년",
        price: 13500,
        originalPrice: 15000,
        rating: 4.7,
        reviewsCount: 62,
        popularity: 70,
        badge: null,
        icon: "fa-shuttle-space",
        bgGradient: "from-blue-950 via-space-900 to-slate-900",
        summary: "인류의 우주 도전 역사 속 라이카와 동물 영웅들의 이야기를 알기 쉽게 설명한 과학 교육서.",
        previewPages: [
          "인류가 달에 도달하기 전, 수많은 동물 친구들이 우주길을 먼저 밝혀주었습니다.",
          "라이카의 용기 있는 걸음은 인류 우주 탐사의 가장 중요한 첫 열쇠가 되었습니다."
        ]
      },
      {
        id: 4,
        title: "라이카의 별빛 드로잉북 & 엽서세트",
        author: "디자인 라카",
        publisher: "스페이스아트",
        category: "goods",
        categoryName: "아트북·굿즈",
        ageGroup: "전연령",
        price: 11000,
        originalPrice: 12500,
        rating: 5.0,
        reviewsCount: 45,
        popularity: 92,
        badge: "HOT",
        icon: "fa-paintbrush",
        bgGradient: "from-purple-950 via-space-900 to-pink-950",
        summary: "독자가 직접 나만의 라이카 별자리를 채색하고 편지를 쓸 수 있는 감성 드로잉북 세트.",
        previewPages: [
          "나만의 밤하늘을 라이카 일러스트 스티커와 물감으로 예쁘게 채워보세요.",
          "소중한 사람에게 라이카의 안부를 담아 별빛 엽서를 보내보세요."
        ]
      },
      {
        id: 5,
        title: "달빛 아래 라이카의 꿈",
        author: "이달님 글/그림",
        publisher: "달빛아틀리에",
        category: "children",
        categoryName: "유아·어린이 동화",
        ageGroup: "4~7세",
        price: 12800,
        originalPrice: 14000,
        rating: 4.9,
        reviewsCount: 88,
        popularity: 88,
        badge: "NEW",
        icon: "fa-moon",
        bgGradient: "from-amber-950 via-space-900 to-indigo-900",
        summary: "우주선에 오르기 전 밤마다 달빛을 보며 꾸었던 라이카의 수줍은 소원 이야기.",
        previewPages: [
          "달님이 라이카를 포근히 감싸 안으며 말했어요.",
          "'너는 언제나 세상에서 가장 빛나는 별이 될 거 간단다.'"
        ]
      },
      {
        id: 6,
        title: "우주 비행사 라이카의 스페이스 아카이브",
        author: "스푸트니크 기록팀",
        publisher: "히스토리북스",
        category: "science",
        categoryName: "우주·과학 교양",
        ageGroup: "청소년·일반",
        price: 18000,
        originalPrice: 20000,
        rating: 4.8,
        reviewsCount: 51,
        popularity: 76,
        badge: null,
        icon: "fa-box-archive",
        bgGradient: "from-slate-900 via-space-900 to-blue-950",
        summary: "1957년 당시의 사진 자료와 기록을 온전히 담아낸 아카이빙 양장 양식 서적.",
        previewPages: [
          "희귀 사진으로 만나는 라이카의 우주 훈련 일지와 스푸트니크 2호의 내부 구조."
        ]
      }
    ];

    const categories = [
      { id: 'all', name: '전체 도서' },
      { id: 'children', name: '유아·어린이 동화' },
      { id: 'graphic', name: '그래픽 노블' },
      { id: 'science', name: '우주·과학 교양' },
      { id: 'goods', name: '아트북·굿즈' }
    ];

    const initialReviews = [
      {
        id: 1,
        author: "이지은 학부모님",
        bookTitle: "우주로 간 작은 별, 라이카",
        rating: 5,
        content: "아이에게 우주에 대해 설명해주다가 울컥했습니다. 그림체가 따뜻하고 내용이 너무 감동적입니다.",
        upvotes: 24
      },
      {
        id: 2,
        author: "박민우 (초등 4학년)",
        bookTitle: "별에서 온 신호: 라이카의 은하수 교신",
        rating: 5,
        content: "밤하늘 볼 때마다 북극성 옆에 라이카 별이 있는 것 같아서 기분이 좋아요!",
        upvotes: 18
      },
      {
        id: 3,
        author: "최현수 독자님",
        bookTitle: "라이카의 별빛 드로잉북",
        rating: 5,
        content: "엽서 세트 일러스트 퀄리티가 대박입니다. 소장용으로 사길 정말 잘했네요.",
        upvotes: 12
      }
    ];

    let initialGuestbook = [
      { name: "민지", msg: "라이카야, 우주에서도 밤하늘 보면서 항상 행복해야 해!", date: "방금 전" },
      { name: "우주소년", msg: "지구의 많은 친구들이 너를 기억하고 있어.", date: "10분 전" }
    ];

    // App State
    let currentCategory = 'all';
    let searchQuery = '';
    let sortOption = 'recommended';
    let cart = [];
    let wishlist = [];

    document.addEventListener('DOMContentLoaded', () => {
      renderCategoryTabs();
      renderBooks();
      renderReviews();
      renderGuestbook();

      // Search Listeners
      const searchInput = document.getElementById('searchInput');
      const mobileSearchInput = document.getElementById('mobileSearchInput');
      const clearSearchBtn = document.getElementById('clearSearchBtn');

      searchInput.addEventListener('input', (e) => {
        searchQuery = e.target.value.trim().toLowerCase();
        mobileSearchInput.value = e.target.value;
        toggleClearButton();
        renderBooks();
      });

      mobileSearchInput.addEventListener('input', (e) => {
        searchQuery = e.target.value.trim().toLowerCase();
        searchInput.value = e.target.value;
        toggleClearButton();
        renderBooks();
      });

      clearSearchBtn.addEventListener('click', () => {
        searchQuery = '';
        searchInput.value = '';
        mobileSearchInput.value = '';
        toggleClearButton();
        renderBooks();
      });

      function toggleClearButton() {
        if (searchQuery.length > 0) {
          clearSearchBtn.classList.remove('hidden');
        } else {
          clearSearchBtn.classList.add('hidden');
        }
      }

      // Sort Selector Listener
      document.getElementById('sortSelect').addEventListener('change', (e) => {
        sortOption = e.target.value;
        renderBooks();
      });

      // Header Buttons
      document.getElementById('cartBtn').addEventListener('click', openCartDrawer);
      document.getElementById('closeCartBtn').addEventListener('click', closeCartDrawer);
      document.getElementById('cartOverlay').addEventListener('click', closeCartDrawer);

      document.getElementById('wishlistBtn').addEventListener('click', openWishlistModal);
      document.getElementById('closeWishlistModalBtn').addEventListener('click', closeWishlistModal);

      document.getElementById('closeBookModalBtn').addEventListener('click', closeBookModal);
      document.getElementById('checkoutBtn').addEventListener('click', openCheckoutModal);
      document.getElementById('confirmCheckoutBtn').addEventListener('click', closeCheckoutModal);

      // Guestbook Form
      document.getElementById('guestbookForm').addEventListener('submit', (e) => {
        e.preventDefault();
        const nameInput = document.getElementById('guestNameInput');
        const msgInput = document.getElementById('guestMsgInput');
        
        if (nameInput.value.trim() && msgInput.value.trim()) {
          initialGuestbook.unshift({
            name: nameInput.value.trim(),
            msg: msgInput.value.trim(),
            date: "방금 전"
          });
          nameInput.value = '';
          msgInput.value = '';
          renderGuestbook();
          showToast("우주 메시지가 성공적으로 전송되었습니다!");
        }
      });
    });


    // Render Category Tabs
    function renderCategoryTabs() {
      const container = document.getElementById('categoryTabs');
      container.innerHTML = categories.map(cat => `
        <button onclick="setCategory('${cat.id}')" 
          class="px-4 py-2 rounded-full text-xs sm:text-sm font-medium transition-all whitespace-nowrap border ${
            currentCategory === cat.id 
              ? 'bg-star-500 text-space-950 border-star-500 font-bold shadow-md shadow-star-500/20' 
              : 'bg-space-900/80 text-slate-300 border-slate-700/80 hover:border-slate-500'
          }">
          ${cat.name}
        </button>
      `).join('');
    }

    function setCategory(catId) {
      currentCategory = catId;
      const catObj = categories.find(c => c.id === catId);
      document.getElementById('activeCategoryName').textContent = catObj ? catObj.name : '전체 도서';
      renderCategoryTabs();
      renderBooks();
    }

    // Filter & Render Books
    function renderBooks() {
      const container = document.getElementById('bookGrid');

      let filtered = booksData.filter(book => {
        const matchesCat = currentCategory === 'all' || book.category === currentCategory;
        const matchesQuery = book.title.toLowerCase().includes(searchQuery) ||
                             book.author.toLowerCase().includes(searchQuery) ||
                             book.summary.toLowerCase().includes(searchQuery);
        return matchesCat && matchesQuery;
      });

      // Sorting
      if (sortOption === 'popular') filtered.sort((a, b) => b.popularity - a.popularity);
      if (sortOption === 'priceLow') filtered.sort((a, b) => a.price - b.price);
      if (sortOption === 'priceHigh') filtered.sort((a, b) => b.price - a.price);
      if (sortOption === 'rating') filtered.sort((a, b) => b.rating - a.rating);

      document.getElementById('resultCount').textContent = filtered.length;

      if (filtered.length === 0) {
        container.innerHTML = `
          <div class="col-span-full text-center py-16 text-slate-400 glass-card rounded-2xl">
            <i class="fa-solid fa-satellite text-4xl mb-3 text-slate-600 block"></i>
            검색 결과와 일치하는 도서가 없습니다.
          </div>
        `;
        return;
      }

      container.innerHTML = filtered.map(book => {
        const isWish = wishlist.includes(book.id);
        return `
          <div class="glass-card rounded-2xl p-5 border border-slate-800 flex flex-col justify-between group transition-all duration-300 hover:-translate-y-1">
            
            <div>
              <!-- Book Cover Art -->
              <div class="relative w-full aspect-[3/4] rounded-xl bg-gradient-to-b ${book.bgGradient} p-4 border border-slate-700/50 flex flex-col justify-between mb-4 shadow-inner overflow-hidden">
                <div class="flex justify-between items-start z-10">
                  <span class="bg-space-950/80 text-star-400 text-[10px] font-bold px-2 py-0.5 rounded border border-star-500/20">
                    ${book.ageGroup}
                  </span>
                  <button onclick="toggleWishlist(${book.id})" class="p-1.5 rounded-full bg-space-950/60 text-slate-300 hover:text-rose-400 transition-colors">
                    <i class="${isWish ? 'fa-solid fa-heart text-rose-500' : 'fa-regular fa-heart'}"></i>
                  </button>
                </div>

                <div class="text-center my-auto py-4 z-10">
                  <i class="fa-solid ${book.icon} text-4xl text-star-400/90 mb-3 group-hover:scale-110 transition-transform"></i>
                  <div class="font-serif font-bold text-white text-base leading-snug line-clamp-2">${book.title}</div>
                </div>

                <div class="text-[10px] text-slate-400 text-center border-t border-slate-800/80 pt-2 z-10">
                  ${book.author} · ${book.publisher}
                </div>

                ${book.badge ? `<span class="absolute -right-6 top-3 bg-star-500 text-space-950 font-bold text-[9px] px-6 py-0.5 rotate-45 shadow">${book.badge}</span>` : ''}
              </div>

              <!-- Meta Info -->
              <div class="flex items-center gap-1.5 text-xs text-star-400 font-semibold mb-1">
                <i class="fa-solid fa-star"></i>
                <span>${book.rating}</span>
                <span class="text-slate-500 font-normal">(${book.reviewsCount})</span>
              </div>

              <h3 class="font-bold text-slate-100 text-base mb-1 line-clamp-1">${book.title}</h3>
              <p class="text-xs text-slate-400 mb-4 line-clamp-2 font-light">${book.summary}</p>
            </div>

            <!-- Price & Actions -->
            <div>
              <div class="flex items-baseline gap-2 mb-3">
                <span class="font-bold text-lg text-amber-200">${book.price.toLocaleString()}원</span>
                <span class="text-xs text-slate-500 line-through">${book.originalPrice.toLocaleString()}원</span>
              </div>

              <div class="grid grid-cols-2 gap-2">
                <button onclick="openBookModal(${book.id})" class="text-xs bg-space-800 hover:bg-space-700 text-slate-300 font-medium py-2 rounded-lg border border-slate-700 transition-colors flex items-center justify-center gap-1">
                  <i class="fa-solid fa-eye text-[10px]"></i> 상세보기
                </button>
                <button onclick="addToCart(${book.id})" class="text-xs bg-star-500 hover:bg-star-400 text-space-950 font-bold py-2 rounded-lg transition-colors flex items-center justify-center gap-1">
                  <i class="fa-solid fa-cart-plus"></i> 담기
                </button>
              </div>
            </div>

          </div>
        `;
      }).join('');
    }

    // Wishlist Logic
    function toggleWishlist(bookId) {
      if (wishlist.includes(bookId)) {
        wishlist = wishlist.filter(id => id !== bookId);
        showToast("위시리스트에서 제외되었습니다.");
      } else {
        wishlist.push(bookId);
        showToast("위시리스트에 추가되었습니다! ❤️");
      }
      updateWishlistUI();
      renderBooks();
    }

    function updateWishlistUI() {
      const badge = document.getElementById('wishBadge');
      if (wishlist.length > 0) {
        badge.textContent = wishlist.length;
        badge.classList.remove('hidden');
      } else {
        badge.classList.add('hidden');
      }
    }

    function openWishlistModal() {
      const modal = document.getElementById('wishlistModal');
      const content = document.getElementById('wishlistModalContent');

      if (wishlist.length === 0) {
        content.innerHTML = `
          <div class="text-center py-8 text-slate-400">
            <i class="fa-regular fa-heart text-3xl mb-2 block text-slate-600"></i>
            찜한 도서가 없습니다.
          </div>
        `;
      } else {
        const wishBooks = booksData.filter(b => wishlist.includes(b.id));
        content.innerHTML = wishBooks.map(book => `
          <div class="flex items-center justify-between gap-3 bg-space-950 p-3 rounded-xl border border-slate-800">
            <div>
              <h4 class="text-sm font-bold text-slate-200">${book.title}</h4>
              <span class="text-xs text-star-400 font-semibold">${book.price.toLocaleString()}원</span>
            </div>
            <div class="flex gap-2">
              <button onclick="addToCart(${book.id}); closeWishlistModal();" class="text-xs bg-star-500 text-space-950 font-bold px-3 py-1.5 rounded-lg">
                담기
              </button>
              <button onclick="toggleWishlist(${book.id}); openWishlistModal();" class="text-xs bg-space-800 text-slate-400 px-2 py-1.5 rounded-lg">
                <i class="fa-solid fa-trash"></i>
              </button>
            </div>
          </div>
        `).join('');
      }

      modal.classList.remove('hidden');
    }

    function closeWishlistModal() {
      document.getElementById('wishlistModal').classList.add('hidden');
    }

    // Cart Logic
    function addToCart(bookId) {
      const existing = cart.find(item => item.id === bookId);
      if (existing) {
        existing.quantity += 1;
      } else {
        const book = booksData.find(b => b.id === bookId);
        cart.push({ ...book, quantity: 1 });
      }
      updateCartUI();
      showToast("장바구니에 도서가 담겼습니다! 🛒");
    }

    function updateQuantity(bookId, delta) {
      const item = cart.find(i => i.id === bookId);
      if (item) {
        item.quantity += delta;
        if (item.quantity <= 0) {
          cart = cart.filter(i => i.id !== bookId);
        }
      }
      updateCartUI();
    }

    function updateCartUI() {
      const badge = document.getElementById('cartBadge');
      const container = document.getElementById('cartItemsList');
      const totalEl = document.getElementById('cartTotalPrice');

      const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
      const totalPrice = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);

      badge.textContent = totalItems;
      totalEl.textContent = `${totalPrice.toLocaleString()}원`;

      if (cart.length === 0) {
        container.innerHTML = `
          <div class="text-center py-12 text-slate-500">
            <i class="fa-solid fa-basket-shopping text-3xl mb-3 block text-slate-700"></i>
            장바구니가 비어 있습니다.
          </div>
        `;
        return;
      }

      container.innerHTML = cart.map(item => `
        <div class="flex items-center justify-between gap-3 bg-space-950 p-3.5 rounded-xl border border-slate-800">
          <div class="flex-1 min-w-0">
            <h4 class="text-sm font-bold text-slate-200 truncate">${item.title}</h4>
            <div class="text-xs text-star-400 font-semibold">${item.price.toLocaleString()}원</div>
          </div>

          <div class="flex items-center gap-2 bg-space-900 border border-slate-700 rounded-lg px-2 py-1">
            <button onclick="updateQuantity(${item.id}, -1)" class="text-slate-400 hover:text-white px-1.5 font-bold">-</button>
            <span class="text-xs font-bold text-slate-200">${item.quantity}</span>
            <button onclick="updateQuantity(${item.id}, 1)" class="text-slate-400 hover:text-white px-1.5 font-bold">+</button>
          </div>
        </div>
      `).join('');
    }

    function openCartDrawer() {
      const drawer = document.getElementById('cartDrawer');
      const panel = document.getElementById('cartPanel');
      drawer.classList.remove('pointer-events-none', 'opacity-0');
      drawer.classList.add('opacity-100');
      panel.classList.remove('translate-x-full');
    }

    function closeCartDrawer() {
      const drawer = document.getElementById('cartDrawer');
      const panel = document.getElementById('cartPanel');
      panel.classList.add('translate-x-full');
      setTimeout(() => {
        drawer.classList.add('pointer-events-none', 'opacity-0');
        drawer.classList.remove('opacity-100');
      }, 300);
    }

    // Book Detail Modal
    function openBookModal(bookId) {
      const book = booksData.find(b => b.id === bookId);
      if (!book) return;

      const content = document.getElementById('bookModalContent');
      content.innerHTML = `
        <div class="flex flex-col md:flex-row gap-6 mb-6">
          <div class="w-full md:w-1/3 aspect-[3/4] rounded-xl bg-gradient-to-b ${book.bgGradient} p-4 border border-slate-700 flex flex-col justify-between text-center shrink-0">
            <span class="text-[10px] bg-space-950/80 text-star-400 font-bold px-2 py-0.5 rounded self-start">${book.ageGroup}</span>
            <i class="fa-solid ${book.icon} text-5xl text-star-400 my-auto"></i>
            <div class="text-xs text-slate-400">${book.author}</div>
          </div>

          <div class="flex-1">
            <span class="text-xs text-star-400 font-bold uppercase tracking-wider">${book.categoryName}</span>
            <h3 class="font-serif text-2xl font-bold text-white mt-1 mb-2">${book.title}</h3>
            
            <div class="flex items-center gap-2 text-xs text-slate-400 mb-4">
              <span class="text-star-400 font-bold">★ ${book.rating}</span>
              <span>·</span>
              <span>리뷰 ${book.reviewsCount}개</span>
              <span>·</span>
              <span>출판사: ${book.publisher}</span>
            </div>

            <p class="text-slate-300 text-sm font-light leading-relaxed mb-6">${book.summary}</p>

            <div class="flex items-baseline gap-2 mb-6">
              <span class="text-2xl font-bold text-amber-200">${book.price.toLocaleString()}원</span>
              <span class="text-xs text-slate-500 line-through">${book.originalPrice.toLocaleString()}원</span>
            </div>

            <div class="flex gap-3">
              <button onclick="addToCart(${book.id})" class="flex-1 bg-star-500 hover:bg-star-400 text-space-950 font-bold py-3 rounded-xl transition-all flex items-center justify-center gap-2">
                <i class="fa-solid fa-cart-plus"></i> 장바구니 담기
              </button>
            </div>
          </div>
        </div>

        <div class="bg-space-950 p-5 rounded-xl border border-slate-800">
          <h4 class="text-xs font-bold text-star-400 uppercase tracking-widest mb-3"><i class="fa-solid fa-book-open mr-1.5"></i> 책 속 한 구절 미리보기</h4>
          <div class="space-y-3">
            ${book.previewPages.map(page => `
              <p class="font-serif text-slate-300 text-sm leading-relaxed italic border-l-2 border-star-500/50 pl-3">
                "${page}"
              </p>
            `).join('')}
          </div>
        </div>
      `;

      document.getElementById('bookModal').classList.remove('hidden');
    }

    function closeBookModal() {
      document.getElementById('bookModal').classList.add('hidden');
    }

    // Checkout Modal
    function openCheckoutModal() {
      if (cart.length === 0) {
        showToast("장바구니가 비어 있습니다.");
        return;
      }
      closeCartDrawer();
      document.getElementById('checkoutModal').classList.remove('hidden');
    }

    function closeCheckoutModal() {
      cart = [];
      updateCartUI();
      document.getElementById('checkoutModal').classList.add('hidden');
      showToast("주문이 정상 접수되었습니다! ✨");
    }

    // Render Reviews
    function renderReviews() {
      const container = document.getElementById('reviewsGrid');
      container.innerHTML = initialReviews.map(r => `
        <div class="glass-card p-6 rounded-2xl border border-slate-800 flex flex-col justify-between">
          <div>
            <div class="flex text-star-400 text-xs gap-1 mb-3">
              ${'<i class="fa-solid fa-star"></i>'.repeat(r.rating)}
            </div>
            <p class="text-slate-300 text-sm font-light leading-relaxed mb-4">"${r.content}"</p>
          </div>
          <div class="pt-4 border-t border-slate-800/80 flex justify-between items-center text-xs">
            <div>
              <div class="font-bold text-slate-200">${r.author}</div>
              <div class="text-slate-500 text-[11px]">${r.bookTitle}</div>
            </div>
            <button onclick="upvoteReview(${r.id}, this)" class="text-slate-400 hover:text-star-400 bg-space-950 px-2.5 py-1 rounded-lg border border-slate-800">
              <i class="fa-regular fa-thumbs-up mr-1"></i> <span>${r.upvotes}</span>
            </button>
          </div>
        </div>
      `).join('');
    }

    function upvoteReview(id, btn) {
      const review = initialReviews.find(r => r.id === id);
      if (review) {
        review.upvotes += 1;
        btn.querySelector('span').textContent = review.upvotes;
        showToast("후기를 추천했습니다.");
      }
    }

    // Render Guestbook
    function renderGuestbook() {
      const container = document.getElementById('guestbookList');
      container.innerHTML = initialGuestbook.map(g => `
        <div class="bg-space-950/80 p-3.5 rounded-xl border border-slate-800 flex justify-between items-start">
          <div>
            <span class="text-xs font-bold text-star-400 mr-2">${g.name}</span>
            <span class="text-xs text-slate-300 font-light">${g.msg}</span>
          </div>
          <span class="text-[10px] text-slate-500 shrink-0 ml-2">${g.date}</span>
        </div>
      `).join('');
    }

    // Toast Notification Utility
    function showToast(message) {
      const container = document.getElementById('toastContainer');
      const toast = document.createElement('div');
      toast.className = 'toast bg-star-500 text-space-950 font-bold px-5 py-3 rounded-xl shadow-xl flex items-center gap-2 text-sm';
      toast.innerHTML = `<i class="fa-solid fa-circle-check text-base"></i> ${message}`;

      container.appendChild(toast);

      setTimeout(() => {
        toast.remove();
      }, 3000);
    }
  </script>
</body>
</html>
