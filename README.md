
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>기본동사100 스피드퀴즈</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            -webkit-user-select: none; -moz-user-select: none; -ms-user-select: none; user-select: none;
            -webkit-touch-callout: none; -webkit-tap-highlight-color: transparent;
            word-break: keep-all; background-color: #f8fafc; font-family: system-ui, -apple-system, sans-serif;
        }
        .card { perspective: 1000px; }
        .card-inner {
            position: relative; width: 100%; height: 320px;
            transition: transform 0.6s; transform-style: preserve-3d; cursor: pointer;
        }
        .card.flipped .card-inner { transform: rotateY(180deg); }
        .card-front, .card-back {
            position: absolute; width: 100%; height: 100%;
            backface-visibility: hidden; display: flex; flex-direction: column;
            align-items: center; justify-content: center; border-radius: 1.5rem;
            padding: 2rem; box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
        }
        .card-back { transform: rotateY(180deg); }
        .active-tab { background-color: #4f46e5 !important; color: white !important; }
        .star-checked { color: #fbbf24 !important; fill: #fbbf24 !important; }
        .no-scrollbar::-webkit-scrollbar { display: none; }
    </style>
</head>
<body oncontextmenu="return false">

    <div class="max-w-md mx-auto px-4 py-6">
        <header class="text-center mb-6">
            <h1 class="text-2xl font-black text-indigo-600 tracking-tight italic">기본동사100 스피드퀴즈</h1>
        </header>

        <div class="flex space-x-2 mb-6 overflow-x-auto pb-2 no-scrollbar font-bold text-sm">
            <button onclick="selectWeek(9)" id="tab-9" class="week-tab flex-shrink-0 px-5 py-2 bg-white rounded-full shadow-sm border border-slate-100 active-tab">Week 9</button>
            <button onclick="selectWeek(10)" id="tab-10" class="week-tab flex-shrink-0 px-5 py-2 bg-white rounded-full shadow-sm border border-slate-100">Week 10</button>
            <button onclick="selectWeek(11)" id="tab-11" class="week-tab flex-shrink-0 px-5 py-2 bg-white rounded-full shadow-sm border border-slate-100">Week 11</button>
            <button onclick="selectWeek(12)" id="tab-12" class="week-tab flex-shrink-0 px-5 py-2 bg-white rounded-full shadow-sm border border-slate-100">Week 12</button>
            <button onclick="selectWeek(13)" id="tab-13" class="week-tab flex-shrink-0 px-5 py-2 bg-white rounded-full shadow-sm border border-slate-100">Week 13</button>
            <button onclick="selectWeek(14)" id="tab-14" class="week-tab flex-shrink-0 px-5 py-2 bg-white rounded-full shadow-sm border border-slate-100">Week 14</button>
            <button onclick="selectWeek('9-14')" id="tab-9-14" class="week-tab flex-shrink-0 px-5 py-2 bg-white rounded-full shadow-sm border border-slate-100">Week 9-14 누적</button>
        </div>

        <div id="setup-screen" class="bg-white rounded-3xl p-8 shadow-xl border border-slate-100 text-center">
            <div id="selected-week-title" class="text-indigo-500 font-black text-xl mb-6 italic tracking-widest uppercase">WEEK 9</div>
            <div class="space-y-4">
                <button onclick="startQuiz('mild')" class="w-full py-5 bg-emerald-500 text-white rounded-2xl font-black shadow-lg shadow-emerald-100 active:scale-95 transition">순한맛 시작</button>
                <button onclick="startQuiz('spicy')" class="w-full py-5 bg-orange-500 text-white rounded-2xl font-black shadow-lg shadow-orange-100 active:scale-95 transition">매운맛 시작</button>
            </div>
            <div class="mt-8 p-4 bg-slate-50 rounded-xl text-[11px] text-slate-400 text-left leading-relaxed">
                <p>• <strong>순한맛</strong>: 대표예제 + Model examples</p>
                <p>• <strong>매운맛</strong>: 모든 출처 랜덤 10문항</p>
                <p class="mt-2 text-rose-400 font-bold">※ 모바일 Tip: 옆면 무음 스위치를 해제하세요!</p>
            </div>
        </div>

        <div id="quiz-screen" class="hidden">
            <div class="flex justify-between items-center mb-6 px-2">
                <span id="progress-text" class="px-3 py-1 bg-indigo-100 text-indigo-600 rounded-full text-xs font-bold">1 / 10</span>
                <button onclick="location.reload()" class="text-slate-400 font-bold text-xs uppercase">Exit ✕</button>
            </div>

            <div id="card-container" class="card" onclick="flipCard()">
                <div class="card-inner">
                    <div class="card-front bg-white border border-slate-100">
                        <span class="text-indigo-400 font-black text-[10px] tracking-widest mb-6 uppercase">Korean</span>
                        <p id="q-korean" class="text-xl font-bold leading-snug px-4 text-center"></p>
                        <p class="mt-10 text-slate-300 text-xs font-bold animate-pulse">카드를 터치하여 정답 확인</p>
                    </div>
                    <div class="card-back bg-indigo-600 text-white">
                        <button id="star-btn" onclick="toggleStar(event)" class="absolute top-6 right-6 text-white/40 transition-all">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-9 w-9" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5">
                                <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14l-5-4.87 6.91-1.01L12 2z"/>
                            </svg>
                        </button>
                        <span class="text-indigo-200 font-black text-[10px] tracking-widest mb-6 uppercase">English</span>
                        <p id="q-english" class="text-xl font-black leading-snug px-4 mb-8 text-center"></p>
                        <button onclick="event.stopPropagation(); playTTS();" class="w-20 h-20 bg-white/20 rounded-full flex items-center justify-center active:scale-90 transition shadow-inner">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                            </svg>
                        </button>
                        <div id="q-info" class="absolute bottom-8 text-[10px] text-indigo-300 font-bold uppercase tracking-widest italic"></div>
                    </div>
                </div>
            </div>
            <button id="next-btn" onclick="nextQuestion()" class="w-full mt-10 py-5 bg-indigo-600 text-white rounded-2xl font-black shadow-xl hidden active:scale-95 transition">다음 문제로 NEXT</button>
        </div>

        <div id="result-screen" class="hidden bg-white rounded-3xl p-6 shadow-xl border border-slate-100">
            <h2 class="text-center font-black text-xl text-slate-800 mb-8 tracking-tighter">퀴즈 완료 리포트</h2>
            <div id="starred-list" class="space-y-4 max-h-[400px] overflow-y-auto pr-2 custom-scroll"></div>
            <button onclick="location.reload()" class="w-full mt-10 py-5 bg-slate-900 text-white rounded-2xl font-bold active:scale-95 transition">메인으로 돌아가기</button>
        </div>
    </div>

    <script>
        const allData = [
            // ================== Week 9 데이터 ==================
            {w:9, d:41, s:"대표", k:"무슨 이유에서인지 화면이 어두워졌어요.", e:"The screen went dark for some reason."},
            {w:9, d:41, s:"교재1", k:"빵은 냉장 보관하면 오히려 더 빨리 굳어져 맛이 없어져요.", e:"Bread will actually go stale faster if you refrigerate it."},
            {w:9, d:41, s:"교재1", k:"오늘 아침에 버스 정류장에서 버스를 기다리다가 발이 감각이 없어졌어요.", e:"My feet went numb* waiting at the bus stop this morning."},
            {w:9, d:41, s:"교재1", k:"핸드폰 배터리가 나갔어. 충전기 있니?", e:"My phone went dead. Do you have a charger?"},
            {w:9, d:41, s:"교재1", k:"다저스가 월드 시리즈 우승을 했을 때 그는 이성을 잃었다.", e:"He went crazy when the Dodgers won the World Series."},
            {w:9, d:41, s:"교재1", k:"우리 아버지는 30대에 대머리가 되셨다.", e:"My father went bald in his thirties."},
            {w:9, d:41, s:"교재2", k:"우유가 다 떨어진 거야?", e:"Are we out of milk?"},
            {w:9, d:41, s:"교재2", k:"응, 상했더라고. 그래서 오늘 아침에 버려야만 했어.", e:"Yeah, it went bad, so I had to throw it away this morning."},
            {w:9, d:41, s:"교재2", k:"저 사람들 뭐 하고 있는 거야? 위험해 보여.", e:"What are those people doing? It looks dangerous."},
            {w:9, d:41, s:"교재2", k:"그게 바로 지난달에 화제가 된 틱톡 챌린지야.", e:"It’s that TikTok challenge that went viral last month."},
            {w:9, d:41, s:"교재2", k:"요즘은 사람들이 유명해지려고 공원까지 가는 거야? 슬프다.", e:"People go to parks to go viral these days? That’s sad."},
            {w:9, d:41, s:"교재2", k:"누가 아니래. 사람들이 그냥 맑은 공기 쐬러 공원에 갔던 때가 그리워.", e:"So true. I miss when people just went to parks to get some fresh air."},
            {w:9, d:41, s:"교재3", k:"요즘은 항상 여기저기 새롭게 쑤시고 아픈 데가 생겨. 우리 이제 늙어가네.", e:"I have new aches and pains all the time these days. We’re getting old."},
            {w:9, d:41, s:"교재3", k:"완전 공감해.", e:"So true."},
            {w:9, d:41, s:"교재3", k:"겨울에는 아침에 출근 준비하는 게 너무 힘들어.", e:"Getting ready for work in the morning is such a pain during winter."},
            {w:9, d:41, s:"교재3", k:"누가 아니래. 아침엔 정말 어둡기도 하고.", e:"That’s so true. It’s really dark, too."},
            {w:9, d:41, s:"교재3", k:"내가 어릴 때는 한국이 이렇게까지 덥지는 않았는데.", e:"Korea wasn’t this hot when I was a kid."},
            {w:9, d:41, s:"교재3", k:"내 말이!", e:"That’s so true!"},

            {w:9, d:42, s:"대표", k:"음식물 쓰레기는 이 노란 봉투에 넣으면 돼요.", e:"Food trash goes in this yellow bag."},
            {w:9, d:42, s:"교재1", k:"이 꽃들은 여기에 두면 되나요?", e:"Should these flowers go here?"},
            {w:9, d:42, s:"교재1", k:"한국 국적인 분들은 이 줄에 서세요.", e:"Korean citizens go in this line."},
            {w:9, d:42, s:"교재1", k:"죄송한데, 이 경비 보고서들을 다 끝내고 나서 어디에 두면 되나요?", e:"Excuse me, where should these expense reports go when I’m done?"},
            {w:9, d:42, s:"교재1", k:"보험 서류들은 파란색 폴더에 넣으면 됩니다.", e:"Insurance papers go in the blue folder."},
            {w:9, d:42, s:"교재1", k:"포크는 접시 왼쪽에 놓습니다.", e:"The forks go to the left of the plates."},
            {w:9, d:42, s:"교재2", k:"이 덤벨 여기 두면 되나요?", e:"Do these dumbbells go here?"},
            {w:9, d:42, s:"교재2", k:"가벼운 덤벨은 맨 위에 두면 됩니다.", e:"The lighter dumbbells go on the top rack."},
            {w:9, d:42, s:"교재2", k:"내가 설거지하지 말라 그랬잖아.", e:"I told you not to do the dishes."},
            {w:9, d:42, s:"교재2", k:"내가 설거지 좋아한다고 했잖아.", e:"And I told you— I like doing the dishes."},
            {w:9, d:42, s:"교재2", k:"좋아, 그럼 내가 우리가 볼 영화를 골라 볼게.", e:"OK, well, I’ll pick a movie for us to watch."},
            {w:9, d:42, s:"교재2", k:"좋은 생각이야. 그나저나 이 와인 잔은 이 찬장에 두면 되는 거야?", e:"Good idea. By the way, do these wine glasses go in this cupboard?"},
            {w:9, d:42, s:"교재3", k:"그는 바구니에서 빨간 사과를 골랐다. (여러 개 중에 손 가는 대로, 본능적으로 골라서 집어 드는 느낌)", e:"He picked a red apple from the basket."},
            {w:9, d:42, s:"교재3", k:"네가 흥미를 느끼는 주제를 골라. (여러 개 주제 중에 끌리는 것을 뽑는 느낌)", e:"Pick a topic that interests you."},
            {w:9, d:42, s:"교재3", k:"민트 초콜릿과 팥 둘 중에 뭘 선택해야 할지 모르겠어. (두 개의 선택지를 놓고 신중하게 고민하는 느낌)", e:"I can’t choose between mint chocolate and red bean."},
            {w:9, d:42, s:"교재3", k:"결국에는 수학 공부를 선택했습니다. (고심 끝에 내린 결정의 느낌)", e:"I eventually chose to study math."},
            {w:9, d:42, s:"교재3", k:"제프가 500명의 지원자 중에서 장학금 수혜자로 뽑혔다. (격식을 갖춘 공식적인 상황에서 ‘선발된’ 느낌)", e:"Jeff was selected for the scholarship out of 500 applicants."},
            {w:9, d:42, s:"교재3", k:"이용자는 결제 전에 반드시 배송 옵션을 선택해야 합니다. (온라인 쇼핑몰 등에서 자주 등장하는 문장으로 격식을 갖춘 문어체의 느낌)", e:"The user must select a delivery option before checkout."},

            {w:9, d:43, s:"대표", k:"역까지 태워 줄까요?", e:"Do you want a ride to the station?"},
            {w:9, d:43, s:"교재1", k:"저녁으로 피자 먹고 싶어요.", e:"I want pizza for dinner."},
            {w:9, d:43, s:"교재1", k:"이 케이크 한 조각 먹을래?", e:"You want a piece of this cake?"},
            {w:9, d:43, s:"교재1", k:"애들이 아이스크림 먹고 싶다네.", e:"They want ice cream."},
            {w:9, d:43, s:"교재1", k:"새 차를 사야 해서.", e:"We want a new car."},
            {w:9, d:43, s:"교재1", k:"나 저 로봇 청소기 사고 싶어.", e:"I want that robotic vacuum."},
            {w:9, d:43, s:"교재2", k:"죄송한데 창가 자리를 원해서, 예약할 때 말씀드렸는데요.", e:"Excuse me, we wanted a table by the window and mentioned that when I made a reservation."},
            {w:9, d:43, s:"교재2", k:"네, 그 점은 죄송합니다. 그런데 저희가 창가 자리는 예약을 안 받아서요.", e:"Yes, we’re sorry about that, but we don’t take reservations for those tables."},
            {w:9, d:43, s:"교재2", k:"헬스장에 있는 필라테스 수업 신청했어.", e:"I signed up for a Pilates class at the gym."},
            {w:9, d:43, s:"교재2", k:"정말? 나도 새로운 강사의 수업을 한번 들어 보고 싶었거든.", e:"Really? I have been wanting to try the new instructor’s class."},
            {w:9, d:43, s:"교재2", k:"진짜 편안한 성격에다가, 실력도 있어.", e:"She’s super chill and knows her stuff."},
            {w:9, d:43, s:"교재2", k:"그거면 됐지! 자리 예약해야겠다.", e:"That’s it, then! I’m booking a spot."},
            {w:9, d:43, s:"교재3", k:"우리 엄마가 너를 만나 보고 싶어 하셔. (몇 달 전부터 만나 보고 싶어 했으며 지금도 그렇다는 것을 강조)", e:"My mom has been wanting to meet you."},
            {w:9, d:43, s:"교재3", k:"궁 바로 옆에 있는 그 새로 생긴 카페에 가 보고 싶어. (그 카페가 생긴 후로 꼭 한번 가 보고 싶다는 마음이 이어져 왔다는 것을 강조)", e:"I’ve been wanting to try that new café next to the palace."},

            {w:9, d:44, s:"대표", k:"홈페이지에는 그 제품 재고가 없다고 나와 있어요.", e:"The website says the item is out of stock."},
            {w:9, d:44, s:"교재1", k:"건강 검진 결과를 보니까 운동을 좀 더 해야 한다고 나와 있군.", e:"My health check results say I need to exercise more."},
            {w:9, d:44, s:"교재1", k:"앱에 비밀번호를 바꿔야 한다고 나와 있네.", e:"The app says I need to update my password."},
            {w:9, d:44, s:"교재1", k:"메시지 보니까 네 결제가 승인되지 않았다고 나와.", e:"The message says your payment didn’t go through."},
            {w:9, d:44, s:"교재1", k:"포장에 이탈리아 산이라고 적혀 있어.", e:"The package says it was made in Italy."},
            {w:9, d:44, s:"교재1", k:"책 뒷면에 뭐라고 적혀 있어?", e:"What does it say on the back of the book?"},
            {w:9, d:44, s:"교재2", k:"얼마나 더 가야 해?", e:"How much longer do we have?"},
            {w:9, d:44, s:"교재2", k:"내비게이션에는 한 10분 후 도착이라고 나오네.", e:"The GPS says we’ll be arriving in, like, 10 minutes."},
            {w:9, d:44, s:"교재2", k:"네가 마음에 들어 했던 핸드폰 케이스 찾았어?", e:"Did you find the phone case you liked?"},
            {w:9, d:44, s:"교재2", k:"응, 근데 홈페이지에는 품절로 나와.", e:"Yeah, but the website says it’s out of stock."},
            {w:9, d:44, s:"교재2", k:"하아, 또?", e:"Ugh, again?"},
            {w:9, d:44, s:"교재2", k:"아래쪽에 보니, 다음 주에 재입고 예정이라고 나와 있네.", e:"Further down, it says they should have it back in stock next week."},
            {w:9, d:44, s:"교재3", k:"이번 주말쯤이면 날씨가 갤 것 같아.", e:"The weather should clear up by this weekend."},
            {w:9, d:44, s:"교재3", k:"(주문한) 음식이 곧 배달될 것 같은데.", e:"The food should be here any minute."},
            {w:9, d:44, s:"교재3", k:"그 시간쯤에는 차가 그렇게 많지 않을 거예요.", e:"There shouldn’t be any traffic around that time."},
            {w:9, d:44, s:"교재3", k:"집까지 가는 데 그렇게 오래 안 걸릴 것 같아요.", e:"It shouldn’t take me long to get home."},
            {w:9, d:44, s:"교재3", k:"회의를 내일 오후로 조정할 수 있을까요?", e:"Do you think I can reschedule the meeting for tomorrow afternoon?"},
            {w:9, d:44, s:"교재3", k:"물론이죠, 큰 문제 없을 것 같습니다.", e:"Sure, that shouldn’t be a problem."},
            {w:9, d:44, s:"교재3", k:"소프트웨어 업데이트하는 데 얼마나 걸릴까요?", e:"How long will it take to update the software?"},
            {w:9, d:44, s:"교재3", k:"그렇게 오래 안 걸릴 거예요. 몇 분이면 됩니다.", e:"It shouldn’t take long; just a few minutes or so."},

            {w:9, d:45, s:"대표", k:"제가 문까지 바래다드릴게요.", e:"Let me walk you out."},
            {w:9, d:45, s:"교재1", k:"제가 한잔 살게요.", e:"Let me buy you a drink."},
            {w:9, d:45, s:"교재1", k:"내가 같이 가 줄게.", e:"Let me go with you."},
            {w:9, d:45, s:"교재1", k:"한번 볼게요.", e:"Let me take a look."},
            {w:9, d:45, s:"교재1", k:"내가 대신 그 박스 옮겨 줄게.", e:"Let me carry that box for you."},
            {w:9, d:45, s:"교재1", k:"제가 와인 한 잔 더 따라 드릴게요.", e:"Let me pour you another glass of wine."},
            {w:9, d:45, s:"교재2", k:"나 혼자서 이 모든 걸 차로 옮길 수 있을 것 같아.", e:"I think I can carry it all to the car by myself."},
            {w:9, d:45, s:"교재2", k:"알았어, 그럼 내가 문 열어서 잡고 있을게.", e:"OK, well, let me get the door for you."},
            {w:9, d:45, s:"교재2", k:"(강원도 여행 후 서울로 가려는 상황) 다시 서울로 가려면 5시간은 걸릴 듯해.", e:"It’s going to take five hours to get back to the city."},
            {w:9, d:45, s:"교재2", k:"돌아가는 길이 항상 더 막히지.", e:"Traffic is always worse going back."},
            {w:9, d:45, s:"교재2", k:"맞아. 지금 출발하는 게 낫겠다.", e:"Yeah, true. We better get going."},
            {w:9, d:45, s:"교재2", k:"운전은 내가 할게. 당신은 쉬어.", e:"Let me drive. You can rest."},
            {w:9, d:45, s:"교재3", k:"집으로 가 봐야 해.", e:"I need to head home."},
            {w:9, d:45, s:"교재3", k:"집으로 가 봐야 할 시간이야.", e:"It’s time to head home."},
            {w:9, d:45, s:"교재3", k:"늦었다. 오늘은 이쯤에서 마무리하자.", e:"It’s getting late. Let’s call it a night."},
            {w:9, d:45, s:"교재3", k:"아내에게서 전화가 와. 이제 가 봐야겠다.", e:"My wife is calling. I better get going."},
            {w:9, d:45, s:"교재3", k:"그래, 오늘은 여기까지 하자고.", e:"Yeah, let’s call it a night."},

            // ================== Week 10 데이터 ==================
            {w:10, d:46, s:"대표", k:"제 얘기 좀 마저 하게 해 주시겠어요?", e:"Could you let me finish my sentence, please?"},
            {w:10, d:46, s:"교재1", k:"상사가 재택근무를 못하게 해요.", e:"My boss refuses to let me work from home."},
            {w:10, d:46, s:"교재1", k:"저는 학생들이 실수를 해도 중간에 끊지 않습니다.", e:"I let my students go on even when they make mistakes."},
            {w:10, d:46, s:"교재1", k:"아내가 내 친구들을 집에 못 오게 해.", e:"My wife doesn’t let my friends come over to the house."},
            {w:10, d:46, s:"교재1", k:"그냥 그녀가 하고 싶은 대로 하도록 놔 둬.", e:"Just let her do what she wants."},
            {w:10, d:46, s:"교재1", k:"키 줘 봐. 내가 운전할게.", e:"Give me your keys. Let me drive."},
            {w:10, d:46, s:"교재2", k:"네가 자랄 때 너희 부모님한테 특이한 규칙 같은 거 있었어?", e:"Did your parents have any weird rules growing up?"},
            {w:10, d:46, s:"교재2", k:"음, 아빠가 밤 11시까지는 무조건 집에 들어오게 했지. 근데 다 그렇지 않나.", e:"Well, my dad would never let me stay out after 11 p.m., but that’s pretty typical, I guess."},
            {w:10, d:46, s:"교재2", k:"어젯밤 그 양고기 케밥집은 뭔가 현지 분위기가 물씬 나더라.", e:"That lamb kebab place last night had such a local vibe."},
            {w:10, d:46, s:"교재2", k:"맞아! 근데, 우리더러 나가라고 할 줄 알았는데.", e:"I know! I thought they were going to kick us out, though."},
            {w:10, d:46, s:"교재2", k:"영업시간 끝났는데도 우릴 그냥 있게 해 줬지.", e:"He let us stay, even though it was past their closing time."},
            {w:10, d:46, s:"교재2", k:"우리가 계속 거기서 돈을 쓰고 있었으니까 그런 거지. 우리한테 고마워해야 해!", e:"That’s because we kept spending money there. He should thank us!"},
            {w:10, d:46, s:"교재3", k:"그 호텔은 산 전망이 멋지다.", e:"The hotel has a great view of the mountains."},
            {w:10, d:46, s:"교재3", k:"이 동네가 예전에는 전통적인 분위기가 났었다. 지금은 너무 관광지 같다.", e:"This neighborhood used to have more of a traditional vibe. It feels more touristy now."},
            {w:10, d:46, s:"교재3", k:"반포에 있는 그 아파트는 한강이 보이고 편의 시설도 굉장히 좋다.", e:"That apartment in Banpo has a view of the Hangang River and really good amenities."},
            {w:10, d:46, s:"교재3", k:"이 핸드폰은 1080p 화질의 전면 카메라가 있다.", e:"This phone has a 1080p front camera."},
            {w:10, d:46, s:"교재3", k:"길 건너 헬스장에는 스텝밀(천국의 계단)이 세 개 있다.", e:"The gym across the street has three stepmills."},

            {w:10, d:47, s:"대표", k:"전 악플에 굴하지 않을 거예요.", e:"I can’t let nasty comments discourage me."},
            {w:10, d:47, s:"교재1", k:"일 때문에 너무 스트레스 받지 마. 새로운 취미를 찾아 보자.", e:"Don’t let work get to you. Let’s find a new hobby."},
            {w:10, d:47, s:"교재1", k:"그런 산불이 또다시 일어나게 놔 둘 수는 없다.", e:"We can’t let another wildfire like that happen again."},
            {w:10, d:47, s:"교재1", k:"헤어졌다고 공부까지 망치면 안 되지.", e:"Don’t let the breakup mess with your studies."},
            {w:10, d:47, s:"교재1", k:"너무 낙담하지 마.", e:"Don’t let it get you down."},
            {w:10, d:47, s:"교재1", k:"결정을 할 때 감정에 휘둘려서는 안 된다.", e:"You shouldn’t let emotions influence your decisions."},
            {w:10, d:47, s:"교재2", k:"앤드루, 왜 소개팅 더 안 해?", e:"Andrew, why aren’t you going on more dates?"},
            {w:10, d:47, s:"교재2", k:"너무 절박하게 굴 필요는 없으니까. 진정한 사랑이 어딘가에 있을 거야. 그냥 자연스럽게 사랑이 찾아오게 두고 싶어.", e:"I don’t need to act desperate. True love is out there. I just want to let it happen, naturally."},
            {w:10, d:47, s:"교재2", k:"그 자리에 정말 지원하고 싶은데, 면접이 전부 영어로 진행돼.", e:"I really want to apply for that job, but the interviews are all in English."},
            {w:10, d:47, s:"교재2", k:"힘들겠지만, 그렇다고 포기하진 마.", e:"That sounds tough, but don’t let that stop you."},
            {w:10, d:47, s:"교재2", k:"실수를 해서 준비 안 된 사람처럼 보일까 봐 걱정이야.", e:"I’m just afraid I’ll mess up and look unprepared."},
            {w:10, d:47, s:"교재2", k:"영어에 대한 두려움 때문에 발목 잡히면 안 돼.", e:"You can’t let your fear of English hold you back."},
            {w:10, d:47, s:"교재3", k:"하루 종일 장학금 신청하느라 시간을 다 보냈다.", e:"I spent all day applying for scholarships."},
            {w:10, d:47, s:"교재3", k:"구직 활동도 신물이 난다. 비슷한 걸 계속 반복해야 하니.", e:"I hate applying for jobs. It’s so repetitive."},
            {w:10, d:47, s:"교재3", k:"그 해외 영업직에 지원할지 고민 중이다.", e:"I’m considering applying for the overseas sales position."},
            {w:10, d:47, s:"교재3", k:"유튜브에 지원을 했는데 아직 답이 없다.", e:"I applied to YouTube, but I haven’t heard anything back, yet."},
            {w:10, d:47, s:"교재3", k:"동부 쪽 학교에는 지원한 거야?", e:"Have you applied to any schools on the East Coast?"},
            {w:10, d:47, s:"교재3", k:"세 군데 학교에 원서를 냈는데, 모두 합격했다.", e:"I applied to three schools, and I got accepted to all of them."},

            {w:10, d:48, s:"대표", k:"이 노래 진짜 소름 돋네요.", e:"This song gives me chills."},
            {w:10, d:48, s:"교재1", k:"네가 응원을 해 주어서 자신감이 생겼어.", e:"Your encouragement gave me confidence."},
            {w:10, d:48, s:"교재1", k:"아침에 마시는 커피는 하루를 버틸 수 있는 힘을 준다.", e:"Coffee in the morning gives me the energy to get through my day."},
            {w:10, d:48, s:"교재1", k:"그 영화를 보고 난 후 삶에 대한 새로운 시각을 갖게 되었다.", e:"That movie gave me some new perspective on life."},
            {w:10, d:48, s:"교재1", k:"그 친구가 사과를 해서 마음이 풀렸다.", e:"His apology gave me closure."},
            {w:10, d:48, s:"교재1", k:"비가 오는 날에는 늘 기분이 꿀꿀해진다.", e:"Rainy days always give me the blues."},
            {w:10, d:48, s:"교재2", k:"회의가 5시로 미뤄졌어.", e:"The meeting got pushed back to 5."},
            {w:10, d:48, s:"교재2", k:"아, 잘됐다! 그럼 커피 사러 갈 시간 충분할 듯.", e:"Oh, perfect! That will give me enough time to grab some coffee."},
            {w:10, d:48, s:"교재2", k:"지호야, 너 샌프란시스코 자리에 지원해 봐.", e:"Hey, Jiho, you should apply for the position in San Francisco."},
            {w:10, d:48, s:"교재2", k:"진심이야? 난 안 될 것 같은데.", e:"You think so? I don’t think I’d get it."},
            {w:10, d:48, s:"교재2", k:"나는 그렇게 생각 안 해. 네 영어 실력이면 경쟁력 있어.", e:"I disagree. Your English skills give you an edge."},
            {w:10, d:48, s:"교재2", k:"흠, 네 말이 맞을지도. 생각해 볼게.", e:"Hmm, maybe you’re right. I’ll think about it."},
            {w:10, d:48, s:"교재3", k:"저 여자분은 너한테 관심 있는 것 같지 않은데.", e:"I don’t think she’s into you."},
            {w:10, d:48, s:"교재3", k:"그 사람이 그런 뜻으로 한 말은 아닐 거야.", e:"I don’t think he meant it that way."},
            {w:10, d:48, s:"교재3", k:"(동호회 회원들에게 하는 말) 얘들아, 이번 주는 아무래도 못 갈 것 같아.", e:"Guys, I don’t think I can make it this week."},
            {w:10, d:48, s:"교재3", k:"(남녀 사이에서 하는 말) 우리 이제 그만 봐야 할 듯해.", e:"I don’t think we should see each other anymore."},
            {w:10, d:48, s:"교재3", k:"(서울에 사는 외국인이 하는 말) 아무래도 서울의 습기 많은 날씨에는 적응하지 못할 듯해.", e:"I don’t think I’ll ever get used to the humid weather in Seoul."},

            {w:10, d:49, s:"대표", k:"프로젝트 현황을 짧게 업데이트해 드리겠습니다.", e:"Let me give you a quick update on the project."},
            {w:10, d:49, s:"교재1", k:"제 번호 드릴게요.", e:"Let me give you my number."},
            {w:10, d:49, s:"교재1", k:"집에 도착하면 전화 줘.", e:"Give me a call when you get home."},
            {w:10, d:49, s:"교재1", k:"(누나와의 전화 통화 상황에서) 나 대신 누나가 그(조카)에게 뽀뽀해 줘.", e:"Give him a kiss for me."},
            {w:10, d:49, s:"교재1", k:"어서 네 자신을 칭찬해 주렴.", e:"Go ahead and give yourself a pat on the back."},
            {w:10, d:49, s:"교재1", k:"(고민할 시간을) 며칠만 더 주세요.", e:"Just give me a few more days."},
            {w:10, d:49, s:"교재2", k:"(집에서 택시를 부른 상황) 택시가 오고 있어.", e:"The taxi is on its way."},
            {w:10, d:49, s:"교재2", k:"알았어, 잠시만. 금방 내려갈게.", e:"OK, give me a sec. I’ll meet you down there."},
            {w:10, d:49, s:"교재2", k:"(룸메이트에게) 친구야, 미리 알려 주자면, 오늘 밤에 집에 몇 명 놀러 와.", e:"Hey, just to give you a heads-up— a couple people are coming over tonight."},
            {w:10, d:49, s:"교재2", k:"그래, 알려 줘서 고마워.", e:"Cool, thanks for letting me know."},
            {w:10, d:49, s:"교재2", k:"안 시끄럽게 하도록 노력할게.", e:"We’ll try to keep the noise down."},
            {w:10, d:49, s:"교재2", k:"괜찮아. 어차피 나 오늘 밤에 늦게까지 밖에 있을 거야.", e:"No worries. I’ll be out late tonight anyway."},
            {w:10, d:49, s:"교재3", k:"확인차 물어보는 건데, 우리 신촌 메가박스에서 보는 거 맞지?", e:"Just to be clear, we’re meeting at the Sinchon Megabox, correct?"},
            {w:10, d:49, s:"교재3", k:"확인차 연락드립니다. 이번 주 회의는 9시가 아니고 8시 반입니다.", e:"Just to remind you, this week’s meeting is at 8:30, not 9:00."},
            {w:10, d:49, s:"교재3", k:"(상대방 집으로 이사하려는 상황) 알려드리자면, 저희 집은 구매자가 생겼고 이달 말까지 선생님 댁으로 이사하길 바라고 있어요.", e:"Just to update you, we found a buyer, and we hope to move into your place by the end of the month."},
            {w:10, d:49, s:"교재3", k:"확인차 다시 말하는 건데, 우리 7번 출구에서 만나기로 한 거 맞지?", e:"Just to confirm, we’re meeting at exit 7, right?"},
            {w:10, d:49, s:"교재3", k:"너한테 알려 주자면, 나 금요일이 여기 마지막 날이야.", e:"Just to let you know, Friday will be my last day."},

            {w:10, d:50, s:"대표", k:"오믈렛 팬은 당신이 가져요.", e:"You can keep the omelet pan."},
            {w:10, d:50, s:"교재1", k:"(자신의 셔츠를 빌려 간 친구에게 하는 말) 그 셔츠 어차피 나한테 안 맞아. 그냥 너 가져.", e:"That shirt doesn’t fit me anyway. You can keep it."},
            {w:10, d:50, s:"교재1", k:"난 책상 안에 늘 간식을 넣어 둔다.", e:"I always keep some snacks in my desk."},
            {w:10, d:50, s:"교재1", k:"나는 담배는 안 피우는데 라이터는 늘 지니고 다닌다.", e:"I don’t smoke, but I always keep a lighter on me."},
            {w:10, d:50, s:"교재1", k:"(집에 온 친구가 하는 말) 숟가락은 어디에 있어?", e:"Where do you keep your spoons?"},
            {w:10, d:50, s:"교재1", k:"나는 1년 넘은 이메일은 보관하지 않는다.", e:"I don’t keep emails that are more than a year old."},
            {w:10, d:50, s:"교재2", k:"내 차에 너 재킷 두고 갔더라. 내가 차 돌려서 가져다줄 수 있어.", e:"You left your jacket in my car. I can turn around and bring it to you."},
            {w:10, d:50, s:"교재2", k:"아니야, 괜찮아. 다음에 볼 때까지 그냥 가지고 있어.", e:"No, no, that’s OK. Just keep it until I see you next time."},
            {w:10, d:50, s:"교재2", k:"드디어 봄이다. 이제 겨울 코트는 더 이상 필요 없어.", e:"It’s finally spring. No more winter coats."},
            {w:10, d:50, s:"교재2", k:"맞아. 나 여행용 가방 꺼내 와야겠어.", e:"True. I’ll have to get my luggage out."},
            {w:10, d:50, s:"교재2", k:"왜? 여행 가는 거야?", e:"Why? Are you going on a trip?"},
            {w:10, d:50, s:"교재2", k:"아니, 옷장이 너무 작아서 겨울 코트를 여행용 가방에 두거든.", e:"No, I keep my winter coats in my luggage because my closet is too small."},
            {w:10, d:50, s:"교재3", k:"돌아보지 마. 전 여자 친구인 듯해.", e:"Don’t turn around. I think that’s my ex."},
            {w:10, d:50, s:"교재3", k:"내 이름 부르는 소리가 들려서 뒤를 돌아봤지.", e:"I turned around when I heard my name."},
            {w:10, d:50, s:"교재3", k:"택시 기사에게 차를 돌려달라고 했어. 여권을 깜박했거든.", e:"I told the taxi to turn around because I forgot my passport."},
            {w:10, d:50, s:"교재3", k:"조종사가 비행기 기수를 돌렸다.", e:"The pilot turned the plane around."},
            {w:10, d:50, s:"교재3", k:"이번에 취임하는 대통령이 경제를 살릴 수 있기를 바란다.", e:"I hope the incoming president can turn the economy around."},
            {w:10, d:50, s:"교재3", k:"수지의 학점이 확 좋아졌다.", e:"Suzy’s grades have really turned around."},

            // ================== Week 11 데이터 ==================
            {w:11, d:51, s:"대표", k:"이 웹사이트에서 계속 에러 메시지가 떠요.", e:"This website keeps giving me an error message."},
            {w:11, d:51, s:"교재1", k:"내가 계속 너를 브라이언이라고 하네. 미안해, 라이언.", e:"I keep calling you Brian. Sorry about that, Ryan."},
            {w:11, d:51, s:"교재1", k:"여자 친구가 계속 캠핑 데려가 달라고 조르네요.", e:"My girlfriend keeps asking me to take her camping."},
            {w:11, d:51, s:"교재1", k:"이 웹사이트가 계속 다운돼요.", e:"This website keeps crashing."},
            {w:11, d:51, s:"교재1", k:"당신의 개가 저희 집 정원을 계속 엉망으로 만들어요.", e:"Your dog keeps messing up my garden."},
            {w:11, d:51, s:"교재1", k:"이 고객분이 가격 인상에 대해 같은 불만을 계속 제기하고 있습니다.", e:"This customer keeps making the same complaint about the price increases."},
            {w:11, d:51, s:"교재2", k:"모든 것의 가격이 계속 올라.", e:"The price of everything keeps going up."},
            {w:11, d:51, s:"교재2", k:"맞아. 요즘 식료품에 훨씬 많은 돈을 써.", e:"I know. I spend so much more on groceries these days."},
            {w:11, d:51, s:"교재2", k:"이 집 커피 너무 맛없어.", e:"The coffee here is terrible."},
            {w:11, d:51, s:"교재2", k:"맞아.", e:"That’s true."},
            {w:11, d:51, s:"교재2", k:"근데 왜 우리는 매일 점심 먹고 이 집에 계속 오는 거지?", e:"Why do we keep coming back here after lunch every day?"},
            {w:11, d:51, s:"교재2", k:"실은 내가 바리스타에게 반했거든. 언젠가 그녀에게 꼭 데이트 신청하려고 해.", e:"It’s actually because I have a crush on the barista. I swear I’m going to ask her out someday."},
            {w:11, d:51, s:"교재3", k:"어릴 때 3학년 담임 선생님한테 완전 반했잖아.", e:"I had a major crush on my 3rd grade teacher as a kid."},
            {w:11, d:51, s:"교재3", k:"그녀는 눈이 높다.", e:"No one is good enough for her."},
            {w:11, d:51, s:"교재3", k:"그녀는 콧대가 세다.", e:"She thinks she’s too good for everyone."},
            {w:11, d:51, s:"교재3", k:"누구 만나는 사람 있니?", e:"Are you seeing anyone?"},
            {w:11, d:51, s:"교재3", k:"지금 만나는 사람 없어.", e:"I am not seeing anyone at the moment."},
            {w:11, d:51, s:"교재3", k:"그녀는 만나는 사람이 있는 것 같아.", e:"I think she is taken."},
            {w:11, d:51, s:"교재3", k:"카페에서 일할 때 남자들이 늘 데이트 신청을 했었지.", e:"Guys used to always ask me out when I was working at a café."},

            {w:11, d:52, s:"대표", k:"와, 욕실을 깨끗하게 관리하는 비결이 뭐예요?", e:"Wow, how do you keep your bathroom so clean?"},
            {w:11, d:52, s:"교재1", k:"나 머리 짧게 하고 다닐까?", e:"Should I keep my hair short?"},
            {w:11, d:52, s:"교재1", k:"제가 통화하는 동안 조용히 해 주세요.", e:"Please keep it quiet while I’m on the phone."},
            {w:11, d:52, s:"교재1", k:"오늘 밤에 내 차를 써도 돼. 근데 이번엔 제발 깨끗하게 써.", e:"You can use my car tonight, but please keep it clean this time."},
            {w:11, d:52, s:"교재1", k:"회의 관련해서 새로운 소식 계속 알려 드리겠습니다.", e:"I’ll keep you up-to-date regarding our meeting."},
            {w:11, d:52, s:"교재1", k:"말할 게 있는데, 간단하게 할게.", e:"I have something to share, but I’ll keep it brief."},
            {w:11, d:52, s:"교재2", k:"제리가 52세라고? 30대인 줄 알았어.", e:"Jerry is 52 years old? I thought he was in his 30s."},
            {w:11, d:52, s:"교재2", k:"응, 정말 몸매 관리를 제대로 하는 분이지.", e:"Yeah, he really keeps himself fit."},
            {w:11, d:52, s:"교재2", k:"톰, 오늘 좀 신경 쓰이는 일 있어 보이는데. 괜찮아?", e:"You look a little stressed out today, Tom. Are you OK?"},
            {w:11, d:52, s:"교재2", k:"음, 방금 아내가 임신한 걸 알게 되었어. 아직 얼떨떨해.", e:"Well, I just found out my wife is pregnant. I’m still a little shocked."},
            {w:11, d:52, s:"교재2", k:"와, 축하해! 너희가 그동안 노력해 온 거 알고 있어.", e:"Wow, congratulations! I know you guys have been trying for a while."},
            {w:11, d:52, s:"교재2", k:"응, 고마워. 그런데 당분간은 비밀로 해 줄 수 있을까?", e:"Yes, thank you, but could you please keep it private for now?"},
            {w:11, d:52, s:"교재3", k:"여기서 잠시만 기다려 줄래? 금방 돌아올게.", e:"Can you wait here for a little while? I’ll be right back."},
            {w:11, d:52, s:"교재3", k:"영화관에 간 지 꽤 됐네요.", e:"It’s been a while since I went to the movie theater."},
            {w:11, d:52, s:"교재3", k:"저녁 먹으러 파주까지 가자고? 지금?", e:"You want to go to Paju for dinner? Now?"},
            {w:11, d:52, s:"교재3", k:"응, 가는 데 좀 걸릴 수도 있긴 한데, 날 믿어 봐, 운전해서 갈 만한 가치가 있어.", e:"Yeah, it might take a while to get there, but trust me, it’s worth the drive."},
            {w:11, d:52, s:"교재3", k:"달리기하다가 발목이 접질린 지가 꽤 되었는데, 아직도 아프다.", e:"A while ago, I was running and I tweaked my ankle. It’s still tender."},
            {w:11, d:52, s:"교재3", k:"제나랑 헤어진 지가 한참 됐는데, 아직 못 잊겠어.", e:"It’s been quite a while since I broke up with Jenna, but I’m still not over her."},

            {w:11, d:53, s:"대표", k:"내일 첫 비행기로 출국합니다.", e:"I’m leaving on the first flight out tomorrow."},
            {w:11, d:53, s:"교재1", k:"방금 버스 타고 출발했으니까, 곧 집에 도착할 거야.", e:"I just left on the bus, so I’ll be home soon."},
            {w:11, d:53, s:"교재1", k:"가브리엘이 몇 분 전에 친구들과 웃으면서 가던데요.", e:"Garbrielle left laughing with her friends a few minutes ago."},
            {w:11, d:53, s:"교재1", k:"기차가 곧 출발하려고 해. 서둘러!", e:"The train is about to leave. Hurry up!"},
            {w:11, d:53, s:"교재1", k:"벌써 일어나려고?", e:"Are you leaving already?"},
            {w:11, d:53, s:"교재1", k:"내가 태어난 직후 아버지가 가족을 떠나 버렸다.", e:"My dad left shortly after I was born."},
            {w:11, d:53, s:"교재2", k:"추가로 이력서 보시는 거예요?", e:"Looking through more job applications?"},
            {w:11, d:53, s:"교재2", k:"네, 인턴 한 명이 더 나갔어요. 누구도 몇 주 이상을 붙들어 둘 수가 없네요.", e:"Yeah, another intern left. I can’t get any of them to stay longer than a couple weeks."},
            {w:11, d:53, s:"교재2", k:"이런, 보니까 제가 탈 버스가 3분 전에 막 떠났네요.", e:"Oh, no, it looks like my bus just left three minutes ago."},
            {w:11, d:53, s:"교재2", k:"아이고, 다음 버스는 언제 와요?", e:"Shoot, when does the next one come?"},
            {w:11, d:53, s:"교재2", k:"30분 있다가요. 이왕 이렇게 된 거 한 잔 더 하는 게 낫겠네요.", e:"30 minutes. We might as well have another drink."},
            {w:11, d:53, s:"교재2", k:"저야 좋죠. 제가 살게요.", e:"Sounds good to me. I’ll buy."},
            {w:11, d:53, s:"교재3", k:"그는 서류상으로 좋아 보인다.", e:"He looks good on paper."},
            {w:11, d:53, s:"교재3", k:"괜찮은 지원자가 몇 있다.", e:"We have a few good candidates."},
            {w:11, d:53, s:"교재3", k:"면접이 몇 개 잡혀 있다.", e:"I have a couple interviews lined up."},
            {w:11, d:53, s:"교재3", k:"자네가 이 일을 하기에는 스펙이 너무 좋다는 점이 문제야.", e:"The problem is you’re overqualified for this position."},
            {w:11, d:53, s:"교재3", k:"새 직장이 정해질 때까지는 사표 쓰면 안 돼.", e:"Don’t leave your current job until you have a new job lined up."},
            {w:11, d:53, s:"교재3", k:"지금은 다음 직장 알아보고 있어요.", e:"I’m in between jobs at the moment."},

            {w:11, d:54, s:"대표", k:"배고프면 먹으라고 거기 피자 좀 놔 두었어요.", e:"I left you some pizza over there if you’re hungry."},
            {w:11, d:54, s:"교재1", k:"너를 위해 여분의 키를 문 아래에 두었어.", e:"I left an extra key under the door for you."},
            {w:11, d:54, s:"교재1", k:"사람들이 유튜브 영상에 부정적인 의견을 남기면 정말 거슬린다.", e:"It really bothers me when people leave negative comments on YouTube."},
            {w:11, d:54, s:"교재1", k:"택시에 지갑을 두고 내렸다.", e:"I left my wallet in the taxi."},
            {w:11, d:54, s:"교재1", k:"그 이웃이 문에 아주 무례한 쪽지를 붙여 두었다.", e:"The neighbor left a rude note on the door."},
            {w:11, d:54, s:"교재1", k:"택시 기사님에게 별 5개짜리 후기를 남겼어. 정말 훌륭하셨어.", e:"I left my taxi driver a 5-star review. He was exceptional."},
            {w:11, d:54, s:"교재2", k:"머리 스타일이 그렇게 마음에 안 들면 후기를 안 좋게 남겨 버려.", e:"If you hate your haircut so much, leave them a bad review."},
            {w:11, d:54, s:"교재2", k:"안 돼. 그가 무조건 나인 줄 알 거야.", e:"No way. He’ll definitely know it was me."},
            {w:11, d:54, s:"교재2", k:"지난주에 KTX 열차에 내 백팩을 두고 내렸지 뭐야.", e:"I left my backpack on the KTX train last week."},
            {w:11, d:54, s:"교재2", k:"정말? 찾았어?", e:"Really? Did you get it back?"},
            {w:11, d:54, s:"교재2", k:"응, 전화했더니 다행히도 자정쯤에 찾아올 수 있었어.", e:"Yeah, I called them, and I was able to pick it up around midnight, thankfully."},
            {w:11, d:54, s:"교재2", k:"지난달에도 택시에 지갑 두고 내리지 않았어? 이제 그러지 좀 마!", e:"Didn’t you leave your wallet in a taxi last month, too? Stop doing that!"},
            {w:11, d:54, s:"교재3", k:"한동안 핸드폰을 못 찾았는데, 실수로 이불 속에 두고 나왔더라고.", e:"I couldn’t find my phone for a while. I had accidentally left it under the blanket."},
            {w:11, d:54, s:"교재3", k:"젖은 수건은 바닥에 두지 마.", e:"Don’t leave your wet towels on the floor."},
            {w:11, d:54, s:"교재3", k:"이 컵 여기 두면 되나?", e:"Should I leave this cup here?"},
            {w:11, d:54, s:"교재3", k:"나는 보통 수건을 세면대 위쪽에 둔다.", e:"I keep the towels above the sink."},
            {w:11, d:54, s:"교재3", k:"나는 차 키를 서랍 안에 둔다.", e:"I keep my car keys in the drawer."},

            {w:11, d:55, s:"대표", k:"문을 열어 둘까요?", e:"Do you want me to leave the door open?"},
            {w:11, d:55, s:"교재1", k:"남편이 자기 방에 설거지 안 한 그릇들을 쌓아 둔 채로 그냥 뒀더라고요.", e:"My husband left all his dirty dishes piled up in his room."},
            {w:11, d:55, s:"교재1", k:"아내가 차를 시동을 안 끈 채 차고에 둔 거 있죠.", e:"My wife left her car running in the garage."},
            {w:11, d:55, s:"교재1", k:"우리는 냉장고를 비워 둔 채로 휴가를 떠났어요.", e:"We left the fridge empty and went on our vacation."},
            {w:11, d:55, s:"교재1", k:"그 회의 때문에 모두가 낙담했다.", e:"The meeting left everyone discouraged."},
            {w:11, d:55, s:"교재1", k:"그 경기로 관중들이 매우 들떴다.", e:"The game left the crowd thrilled."},
            {w:11, d:55, s:"교재2", k:"빨래 더미를 전부 젖은 채로 세탁기에 그대로 뒀더라.", e:"You left a load of laundry all wet in the washing machine."},
            {w:11, d:55, s:"교재2", k:"아, 깜박했어. 미안. 아침에 다시 그 옷들을 세탁할게.", e:"Oh, I forgot. I’m sorry. I’ll rewash those clothes in the morning."},
            {w:11, d:55, s:"교재2", k:"책상이 왜 온통 젖은 거야?", e:"Why is your desk all wet?"},
            {w:11, d:55, s:"교재2", k:"외출해 있는 동안에 비가 오기 시작했거든.", e:"It started raining while I was out."},
            {w:11, d:55, s:"교재2", k:"또 창문을 열어 뒀구나, 그렇지?", e:"You left the window open again, didn’t you?"},
            {w:11, d:55, s:"교재2", k:"응, 나갈 때는 날씨가 좋았거든! 왜 내가 집에 없으면 늘 비가 오는 거야?", e:"Well, it was nice when I left! Why does it always rain when I’m not at home?"},
            {w:11, d:55, s:"교재3", k:"오늘 춥다, 그렇지?", e:"It’s cold today, isn’t it?"},
            {w:11, d:55, s:"교재3", k:"너 고기 안 먹지, 그렇지?", e:"You don’t eat meat, do you?"},
            {w:11, d:55, s:"교재3", k:"너 야구 팬이지, 그렇지?", e:"You’re a baseball fan, aren’t you?"},
            {w:11, d:55, s:"교재3", k:"맞아. 근데 왜 묻는 거야?", e:"I am. Why do you ask?"},
            {w:11, d:55, s:"교재3", k:"내일 네 생일 맞지, 그렇지?", e:"Tomorrow’s your birthday, isn’t it?"},
            {w:11, d:55, s:"교재3", k:"응, 어떻게 알았어?", e:"Yeah, how did you know?"},
            {w:11, d:55, s:"교재3", k:"비가 올 것 같은 냄새가 나네, 그렇지?", e:"It smells like it’s going to rain, doesn’t it?"},
            {w:11, d:55, s:"교재3", k:"난 아무 냄새도 안 나는데.", e:"I don’t smell anything."},

            // ================== Week 12 데이터 ==================
            {w:12, d:56, s:"대표", k:"닉을 데려가도 되나요?", e:"Do you mind if I bring Nick with me?"},
            {w:12, d:56, s:"교재1", k:"지갑을 안 가져왔네요. 나중에 돈 드려도 되나요?", e:"I didn’t bring my wallet. Can I pay you back later?"},
            {w:12, d:56, s:"교재1", k:"이 쿠키 정말 맛있어. 같이 먹게 사무실에 가져갈게.", e:"These cookies are great. I’ll bring them to the office to share."},
            {w:12, d:56, s:"교재1", k:"너 주려고 커피 가져왔어.", e:"I brought you a coffee."},
            {w:12, d:56, s:"교재1", k:"담요 가져다줄게. 추워 보이네.", e:"Let me bring you a blanket—you look cold."},
            {w:12, d:56, s:"교재1", k:"(베트남에 가는 친구에게 하는 말) 말린 망고 좀 사다 줄 수 있어?", e:"Could you bring me some dried mangoes from Vietnam?"},
            {w:12, d:56, s:"교재2", k:"미나가 오늘 또 개를 회사에 데리고 왔어.", e:"Mina brought her dog to work again today."},
            {w:12, d:56, s:"교재2", k:"나도 봤어. 거의 30분마다 일어나서 개를 데리고 나가네.", e:"I saw that. She gets up to take it outside, like, every 30 minutes."},
            {w:12, d:56, s:"교재2", k:"(싱가포르에 사는 친구에게) 싱가포르 여행 짐 싸고 있어.", e:"I’m packing for my trip to Singapore."},
            {w:12, d:56, s:"교재2", k:"재미있겠다. 노트북 가져올 거야?", e:"That’s fun. Are you going to bring your laptop?"},
            {w:12, d:56, s:"교재2", k:"잘 모르겠어. 무겁긴 한데 한가할 때 일을 좀 할 수도 있어서.", e:"I’m not sure. It’s heavy, but I might want to do some work in my downtime."},
            {w:12, d:56, s:"교재2", k:"그냥 가져와. 싱가포르에서는 걸어 다닐 일이 많지 않을 테니까.", e:"Just bring it. You won’t have to walk around too much in Singapore."},
            {w:12, d:56, s:"교재3", k:"처음에 서울에는 어떻게 오게 된 거예요?", e:"What initially brought you to Seoul?"},
            {w:12, d:56, s:"교재3", k:"우리 동네는 어쩐 일로 온 거야?", e:"What brings you over to my neighborhood?"},
            {w:12, d:56, s:"교재3", k:"한국 음식을 너무 좋아해서 다시 한국에 오게 되었습니다.", e:"My love for Korean food brought me back to Seoul."},
            {w:12, d:56, s:"교재3", k:"켈리를 뽑아서 너무 다행이야.", e:"I’m so glad we hired Kelly."},
            {w:12, d:56, s:"교재3", k:"켈리 덕분에 회사 분위기가 훨씬 활기 넘치지.", e:"She brings a lot of enthusiasm to the company."},
            {w:12, d:56, s:"교재3", k:"샐리와 보비 때문에 사무실이 늘 시끄럽다.", e:"Sally and Bobby bring a lot of drama to the office."},
            {w:12, d:56, s:"교재3", k:"그 친구는 회사에 전혀 도움이 안 돼. 왜 뽑은 거야?", e:"He doesn’t bring much to the table. Why did they hire him?"},
            {w:12, d:56, s:"교재3", k:"나는 커피 한 잔만으로도 행복해진다.", e:"A good cup of coffee brings me joy."},
            {w:12, d:56, s:"교재3", k:"그녀의 말이 내게 위안이 되었다.", e:"Her words brought me comfort."},

            {w:12, d:57, s:"대표", k:"이번 주말에 부산 내려가요.", e:"I’m heading down to Busan this weekend."},
            {w:12, d:57, s:"교재1", k:"우리 지금 공항으로 가.", e:"We’re heading to the airport now."},
            {w:12, d:57, s:"교재1", k:"북쪽으로 이동해서 길이 어디로 이어져 있는지 봅시다.", e:"Let’s head north and see where the road takes us."},
            {w:12, d:57, s:"교재1", k:"(카카오톡 메시지) 나 지금 나가. 5분 늦을지도 몰라.", e:"I’m heading out now. I might be five minutes late."},
            {w:12, d:57, s:"교재1", k:"퇴근하고 너희 집 쪽으로 데리러 갈게.", e:"I’ll head over to your place and pick you up after work."},
            {w:12, d:57, s:"교재1", k:"(파주 캠핑장에 가기로 한 상황) 베니랑 샐리가 퇴근하고 그쪽으로 이동할 거야.", e:"Benny and Sally are going to head up after they get off work."},
            {w:12, d:57, s:"교재2", k:"다음 달에 일본에 갈까 싶어.", e:"I’m thinking of heading over to Japan next month."},
            {w:12, d:57, s:"교재2", k:"멋지다! 일 때문에 가는 거야, 아니면 그냥 휴가차 가는 거야?", e:"That sounds awesome! Are you going for work or just a vacation?"},
            {w:12, d:57, s:"교재2", k:"시간이 꽤 늦었네. 택시 불러서 집에 가야 할 것 같아.", e:"It’s getting pretty late. I guess I should call a taxi and head home."},
            {w:12, d:57, s:"교재2", k:"정말? 그 먼 수원까지 택시로 가면 엄청 비쌀 텐데.", e:"Are you sure? Taking a taxi all the way back to Suwon would be pretty expensive."},
            {w:12, d:57, s:"교재2", k:"어, 네 말이 맞네. 수원까지 그렇게 멀다는 생각을 못 했네.", e:"Yeah, you’re right. I didn’t realize how far it actually is."},
            {w:12, d:57, s:"교재2", k:"그냥 편하게 우리 집에서 자고 가.", e:"Feel free to just crash on my couch."},
            {w:12, d:57, s:"교재3", k:"퇴근 후 곧장 집으로 갔다.", e:"I went straight home after work."},
            {w:12, d:57, s:"교재3", k:"퇴근 후 (회식 장소가 아닌) 집으로 바로 갔다.", e:"I headed straight home after work."},
            {w:12, d:57, s:"교재3", k:"사무실로 복귀하기 전에 커피 한 잔 사려고 했는데, 안 되겠다. 줄 좀 봐.", e:"I wanted to grab a coffee before heading back to work, but I don’t think I can. Look at that line."},
            {w:12, d:57, s:"교재3", k:"(아내가 남편에게 보내는 메시지) 가게 좀 들렀다가 집에 갈 거야.", e:"I’ll head home after I stop by the store."},
            {w:12, d:57, s:"교재3", k:"(교수가 학생에게 보내는 메시지) 사무실에 있을 테니 수업 끝나고 시간 되면 오세요.", e:"I’ll be in my office if you want to head over after class."},

            {w:12, d:58, s:"대표", k:"파티에 우리랑 같이 가는 거지요?", e:"Are you coming with us to the party?"},
            {w:12, d:58, s:"교재1", k:"내가 따라가도 되나요?", e:"Mind if I come along?"},
            {w:12, d:58, s:"교재1", k:"마트에 내가 같이 가 줄까?", e:"Do you want me to come with you to the grocery store?"},
            {w:12, d:58, s:"교재1", k:"내가 가서 그 문제 한번 살펴봐 줄까요?", e:"Do you want me to come over and take a look at the problem?"},
            {w:12, d:58, s:"교재1", k:"아내분도 워크숍에 가시나요?", e:"Is your wife coming to the workshop, too?"},
            {w:12, d:58, s:"교재1", k:"오늘 밤 친구들이 (집에) 오기로 했다.", e:"My friends are coming over tonight."},
            {w:12, d:58, s:"교재2", k:"(퇴근하며 동료에게) 우리는 퇴근해요. 오늘 야근할 거예요?", e:"We’re leaving. Are you going to stay late tonight?"},
            {w:12, d:58, s:"교재2", k:"아뇨, 저도 같이 가요.", e:"No, I’ll come with you."},
            {w:12, d:58, s:"교재2", k:"나랑 내 여동생이 다음 주 금요일에 부산에 가.", e:"My sister and I are coming to Busan next Friday."},
            {w:12, d:58, s:"교재2", k:"와, 잘됐다. 오래 있을 거야?", e:"Oh, that’s great. Are you guys staying long?"},
            {w:12, d:58, s:"교재2", k:"주말까지만 있어. 해운대 해변에 가 보고 싶대.", e:"Just the weekend. She wants to check out Haeundae Beach."},
            {w:12, d:58, s:"교재2", k:"너희 (해운대에) 언제 가는지 알려 줘. 너희 온다니 정말 기대된다.", e:"Let me know when you go. I’m really excited about you guys coming."},
            {w:12, d:58, s:"교재3", k:"매일 아침 수영하니 몸이 탄탄하게 유지된다.", e:"Swimming every morning keeps me fit."},
            {w:12, d:58, s:"교재3", k:"나는 여행을 즐긴다.", e:"I enjoy traveling."},
            {w:12, d:58, s:"교재3", k:"나는 외국어 배우는 것에 관심이 없다.", e:"I’m not interested in learning a foreign language."},
            {w:12, d:58, s:"교재3", k:"내가 가장 좋아하는 취미는 그림 그리기이다.", e:"My favorite hobby is drawing."},
            {w:12, d:58, s:"교재3", k:"그가 우리와 함께 저녁 식사 하러 온다는 게 너무 기쁘다.", e:"I’m really excited about him joining us for dinner."},
            {w:12, d:58, s:"교재3", k:"LA 다저스의 월드 시리즈 우승 가능성은 꽤 높다.", e:"The chances of the LA Dodgers winning the World Series are pretty good."},

            {w:12, d:59, s:"대표", k:"그쪽에서 저를 대기자 명단에 올려 줬어요.", e:"They put me on the waiting list."},
            {w:12, d:59, s:"교재1", k:"네 가방을 여기에다 두자.", e:"Let’s put your bag over here."},
            {w:12, d:59, s:"교재1", k:"소파는 방 저쪽에 놓읍시다.", e:"Let’s put the sofa on that side of the room."},
            {w:12, d:59, s:"교재1", k:"우유를 다시 냉장고에 넣는 거 잊지 마.", e:"Don’t forget to put the milk back in the fridge."},
            {w:12, d:59, s:"교재1", k:"나는 항상 저녁 8시 이후에는 핸드폰을 ‘방해 금지’ 모드로 설정한다.", e:"I always put my phone on “Do Not Disturb” after 8 p.m."},
            {w:12, d:59, s:"교재1", k:"신청서에 성함 기재하는 거 잊지 마세요.", e:"Don’t forget to put your name on the signup sheet."},
            {w:12, d:59, s:"교재2", k:"아직 은행 직원이랑 통화 중이야?", e:"Are you still on the phone with the bank?"},
            {w:12, d:59, s:"교재2", k:"응, 20분 전에 나를 대기 상태로 두었어. 이건 말도 안 돼.", e:"Yes, they put me on hold like 20 minutes ago. This is ridiculous."},
            {w:12, d:59, s:"교재2", k:"저기, 잭, 이거 전자레인지에 넣어도 될까?", e:"Hey, Jack, do you think I can put this in the microwave?"},
            {w:12, d:59, s:"교재2", k:"아랫부분을 확인해 봐. 플라스틱에는 보통 전자레인지에 사용해도 되는지 안 되는지에 대한 표시가 있거든.", e:"Check the bottom. Plastic usually has some kind of symbol that says if it’s microwave safe or not."},
            {w:12, d:59, s:"교재2", k:"아, 알겠어. 전자레인지 돌리면 안 될 것 같네.", e:"Oh, I see. It appears that I cannot."},
            {w:12, d:59, s:"교재2", k:"내가 너 살렸다!", e:"I just saved your life!"},
            {w:12, d:59, s:"교재3", k:"공항에 마중 좀 나와 줄 수 있나요?", e:"Could you pick me up from the airport?"},
            {w:12, d:59, s:"교재3", k:"혹시 공항에 마중 좀 나와 줄 수 있을까요?", e:"Do you think you could pick me up from the airport?"},
            {w:12, d:59, s:"교재3", k:"(원어민에게 회화 수업을 받는 한국인 학생의 말) 이번 주에 두세 번 더 수업할 수 있을까요?", e:"Do you think we could have a couple extra sessions this week?"},
            {w:12, d:59, s:"교재3", k:"혹시 오전에 저 공항까지 좀 데려다주실 수 있을까요?", e:"Do you think you could get me to the airport in the morning?"},
            {w:12, d:59, s:"교재3", k:"(번역가인 아내를 둔 지인에게 하는 말) 아내분이 이거 번역 좀 해 주실 수 있을지요?", e:"Do you think your wife could translate this for me?"},
            {w:12, d:59, s:"교재3", k:"웹사이트 만드는 것 좀 도와줄 수 있을까요?", e:"Do you think you could help me with my website?"},

            {w:12, d:60, s:"대표", k:"악플은 늘 제 기분을 안 좋게 합니다.", e:"Nasty comments always put me in a bad mood."},
            {w:12, d:60, s:"교재1", k:"그 전화를 받았더니 기분이 좋아졌다.", e:"That phone call put me in a good mood."},
            {w:12, d:60, s:"교재1", k:"이 음악을 들으면 늘 잠이 온다.", e:"This music always puts me to sleep."},
            {w:12, d:60, s:"교재1", k:"그는 나를 편안하게 해 준다.", e:"He puts me at ease."},
            {w:12, d:60, s:"교재1", k:"한국 여름은 뭔가 기분을 안 좋게 만든다.", e:"Something about Korean summer puts me in a bad mood."},
            {w:12, d:60, s:"교재1", k:"(산 정상에서 있었던 일화 중) 갑자기 내린 비 때문에 곤란한 상황에 처하게 되었다.", e:"The sudden rain put us in a bad situation."},
            {w:12, d:60, s:"교재2", k:"올해 어머니가 심근 경색을 겪은 뒤라, 건강 검진 결과를 보니 정말 마음이 놓여.", e:"After my mom’s heart attack this year, seeing my health report really put me at ease."},
            {w:12, d:60, s:"교재2", k:"무슨 말인지 알지. 중요한 검사 결과 기다리는 건 너무 초조하니까.", e:"I know what you mean. Waiting for the results of an important test makes me anxious."},
            {w:12, d:60, s:"교재2", k:"(줌 회의 상황에서) 내 화면에서는 왜 사람들 얼굴이 자꾸 바뀌는 거지?", e:"Why do the people’s faces keep changing on my screen?"},
            {w:12, d:60, s:"교재2", k:"줌 통화에서는 말하는 사람이 화면에 뜨거든.", e:"On a Zoom call, it puts the speaker on display."},
            {w:12, d:60, s:"교재2", k:"아, 알겠다. 그래서 말하는 사람이 큰 창에 나오는구나.", e:"Oh, I see. So the person speaking is put in the large window."},
            {w:12, d:60, s:"교재2", k:"그렇지! 익숙해질 거야.", e:"Exactly! You’ll get used to it."},
            {w:12, d:60, s:"교재3", k:"너 기분 좋은 거 맞지?", e:"You’re in a good mood, aren’t you?"},
            {w:12, d:60, s:"교재3", k:"그렇게 보이니? 그럼 그럴지도.", e:"Do I seem like it? Maybe I am."},
            {w:12, d:60, s:"교재3", k:"그 친구 영 힘이 없어 보여.", e:"He seems down."},
            {w:12, d:60, s:"교재3", k:"최근에 아빠랑 이야기해 봤어?", e:"Have you talked to Dad lately?"},
            {w:12, d:60, s:"교재3", k:"아니, 왜?", e:"No, why?"},
            {w:12, d:60, s:"교재3", k:"전화 통화할 때 무척 예민해 보이시더라.", e:"He seemed really grumpy over the phone."},
            {w:12, d:60, s:"교재3", k:"그렇구나. 내일 들러서 기분 좀 풀어 드려야겠다.", e:"I see. I’ll stop by tomorrow and try to cheer him up."},
            {w:12, d:60, s:"교재3", k:"그녀는 늘 여유가 있어 보여.", e:"She’s always chill."},

            // ================== Week 13 데이터 ==================
            {w:13, d:61, s:"대표", k:"이 양복 (촉감이) 비싼 느낌이에요.", e:"This suit feels expensive."},
            {w:13, d:61, s:"교재1", k:"그 방은 아늑하고 따뜻한 느낌이다.", e:"The room feels cozy and warm."},
            {w:13, d:61, s:"교재1", k:"다시 고향에 오니 기분이 이상하다.", e:"It feels weird to be back in my hometown."},
            {w:13, d:61, s:"교재1", k:"배가 더부룩해요.", e:"My stomach feels bloated."},
            {w:13, d:61, s:"교재1", k:"어제 전 여자 친구를 우연히 마주쳤는데 겸연쩍었어.", e:"Running into my ex yesterday felt awkward."},
            {w:13, d:61, s:"교재1", k:"목에 뭔가 걸린 것 같아.", e:"Something feels stuck in my throat."},
            {w:13, d:61, s:"교재2", k:"(지하철에서 여자 친구에게) 왜 자리를 바꾸려 하는 거야?", e:"Why do you want to change seats?"},
            {w:13, d:61, s:"교재2", k:"맞은편에 앉은 남자 뭔가 좀 이상해서. 계속 나를 이상하게 쳐다보잖아.", e:"Something feels off about the guy across from us. He keeps staring at me weird."},
            {w:13, d:61, s:"교재2", k:"그녀랑 그냥 헤어져 버려.", e:"Just break up with her already."},
            {w:13, d:61, s:"교재2", k:"그래야 하는데, 토요일이나 되어야 얼굴을 보니∙∙∙.", e:"I need to, but I don’t see her until Saturday…"},
            {w:13, d:61, s:"교재2", k:"뭘 그때까지 기다려? 그냥 문자 해. 난 늘 그렇게 헤어지는데.", e:"Why wait? Just text her. That’s what I always do."},
            {w:13, d:61, s:"교재2", k:"안 돼, 카카오톡으로 헤어지는 건 뭔가 아니다 싶어.", e:"No, it doesn’t feel right to break up with someone on KakaoTalk."},
            {w:13, d:61, s:"교재3", k:"밖이 꽤 추운 것 같은데∙∙∙ 우리 코트 챙겨야 하는 거 아니야?", e:"It feels pretty cold outside... shouldn’t we take our coats?"},
            {w:13, d:61, s:"교재3", k:"다른 사람들은 모두 따뜻하다고 하는데 나는 춥다.", e:"I feel cold even when everyone else says it’s warm."},
            {w:13, d:61, s:"교재3", k:"직장을 잃는다는 건 정말 끔찍한 일이지. (객관적인 사실이자 일반적으로 그렇다는 말)", e:"It feels awful to lose a job."},
            {w:13, d:61, s:"교재3", k:"직장을 잃어서 너무 괴로워. (자신의 감정을 표현한 말)", e:"I feel awful because I lost my job."},

            {w:13, d:62, s:"대표", k:"당신이 벌써 서른이 되었다니 믿기지가 않는군요.", e:"I can’t believe you’ve already turned 30."},
            {w:13, d:62, s:"교재1", k:"하늘이 갑자기 어두워졌다.", e:"The sky turned dark."},
            {w:13, d:62, s:"교재1", k:"그 소식을 듣자 그의 얼굴이 창백해졌다.", e:"His face turned pale when he heard the news."},
            {w:13, d:62, s:"교재1", k:"우유가 상해 버렸다.", e:"The milk has turned sour."},
            {w:13, d:62, s:"교재1", k:"대화 분위기가 갑자기 어색해졌다.", e:"The conversation suddenly turned awkward."},
            {w:13, d:62, s:"교재1", k:"발표 후 군중들이 폭력적으로 변했다.", e:"The crowd turned violent after the announcement."},
            {w:13, d:62, s:"교재2", k:"마흔이 되었을 때 어떤 느낌이었나요?", e:"How did you feel when you turned 40?"},
            {w:13, d:62, s:"교재2", k:"특별한 느낌은 없었어요. 나이는 그냥 숫자에 불과하죠. 굳이 말하자면, (오히려) 저는 아직도 스물두 살인 것 같은 기분이에요.", e:"I didn’t feel anything. Age is just a number. If anything, I’d say I still feel like I’m 22."},
            {w:13, d:62, s:"교재2", k:"그 시위에 관한 뉴스 봤어?", e:"Did you see the news about the protest?"},
            {w:13, d:62, s:"교재2", k:"응, 나 거기 있었어. 상황이 갑자기 너무 혼란스러워져서 빠져나왔지.", e:"Yeah, I was there. It turned chaotic, and I got out of there."},
            {w:13, d:62, s:"교재2", k:"아, 안 다쳐서 다행이다.", e:"Whoa, I’m glad you didn’t get hurt."},
            {w:13, d:62, s:"교재2", k:"그러게, 다시는 안 갈 거야. 그런 시위들은 자주 폭력적으로 변하거든.", e:"I know, I won’t go again. Those events often turn violent."},
            {w:13, d:62, s:"교재3", k:"(서울시에서 공간을 새롭게 변경한 경우) 서울시는 1990년대에 그 하수처리장을 공원으로 만들었다.", e:"They turned the sewage treatment plant into a park back in the 1990s."},
            {w:13, d:62, s:"교재3", k:"(백화점 리모델링 상황) 백화점 측에서 1층을 카페 공간으로 만드느라 바쁘다.", e:"They’re busy turning the ground floor into a cafeteria."},
            {w:13, d:62, s:"교재3", k:"내 방을 녹음실로 바꾸었다.", e:"I turned my room into a recording studio."},
            {w:13, d:62, s:"교재3", k:"(시에서 건물을 다른 용도로 변경한 경우) 시에서 오래된 교회를 카페로 만들었다.", e:"They turned that old church into a café."},
            {w:13, d:62, s:"교재3", k:"원래 여기 야구 경기장이었던 것 알아?", e:"Did you know this used to be a baseball stadium?"},
            {w:13, d:62, s:"교재3", k:"정말? 언제 그걸 아파트 단지로 만든 건데?", e:"Really? When did they turn it into an apartment complex?"},

            {w:13, d:63, s:"대표", k:"선풍기 좀 저쪽으로 옮겨도 될까요?", e:"Is it OK if I move the fan away from me?"},
            {w:13, d:63, s:"교재1", k:"(공연장에서) 우리는 앞쪽, 무대 근처로 이동했다.", e:"We moved up to the front, near the stage."},
            {w:13, d:63, s:"교재1", k:"(운전을 하는 상황) 저 사람이 좀 비켜 줘야 하는데.", e:"This guy needs to move out of the way."},
            {w:13, d:63, s:"교재1", k:"유럽 사람들은 너무 천천히 다닌다.", e:"People move so slow in Europe."},
            {w:13, d:63, s:"교재1", k:"구급차가 지나가자 차들이 옆으로 비켰다.", e:"The cars moved over as the ambulance came through."},
            {w:13, d:63, s:"교재1", k:"내 책들을 이 방으로 옮겼다.", e:"I moved my books into this room."},
            {w:13, d:63, s:"교재2", k:"요즘 잠들기가 어려워.", e:"I can’t get to sleep these days."},
            {w:13, d:63, s:"교재2", k:"침대를 창문에서 멀리 좀 옮겨 보는 건 어때? (창문을 통해) 가로등 불빛이 너무 많이 들어오니까.", e:"Maybe move your bed away from the window. Too much streetlight comes in there."},
            {w:13, d:63, s:"교재2", k:"이 호떡집 인기가 장난이 아니야.", e:"This hotteok place is super popular."},
            {w:13, d:63, s:"교재2", k:"응, 줄이 진짜 기네. 그런데 맛이 있어야 할 텐데.", e:"Yeah, the line is super long, but I hope it’s good."},
            {w:13, d:63, s:"교재2", k:"가는 사람들이 별로 안 보여.", e:"I don’t see many people leaving."},
            {w:13, d:63, s:"교재2", k:"맞아. 줄이 전혀 줄어들지 않아.", e:"You’re right. The line isn’t moving at all."},
            {w:13, d:63, s:"교재3", k:"제가 처음 시애틀에 왔을 때 친구 사귀는 게 어려웠어요.", e:"When I first moved to Seattle, I had trouble finding friends."},
            {w:13, d:63, s:"교재3", k:"우리는 맨체스터로 이사 오고는 맨체스터 시티 팬이 되었답니다.", e:"When we moved to Manchester, we became Manchester City fans."},
            {w:13, d:63, s:"교재3", k:"제가 한국으로 이사 왔을 때 자연에 완전히 반했지요.", e:"When I moved to Korea, I fell in love with all the nature."},
            {w:13, d:63, s:"교재3", k:"창원으로 이사 갈 생각이 있어요. 거기 지인들이 많아서요.", e:"I’m thinking of moving to Changwon, because I know a lot of people down there."},
            {w:13, d:63, s:"교재3", k:"고향을 떠나 새로운 곳으로 간다는 건 많은 용기가 필요해요.", e:"Leaving your hometown and moving to a new place takes a lot of courage."},

            {w:13, d:64, s:"대표", k:"이 선물 가게는 수녀님들이 운영하고 있어요.", e:"The gift shop is run by nuns."},
            {w:13, d:64, s:"교재1", k:"한강은 도시 한복판을 관통한다.", e:"The Hangang River runs through the middle of the city."},
            {w:13, d:64, s:"교재1", k:"신설 지하철 노선은 과천을 통과할 것이다.", e:"The new subway line will run through Gwacheon."},
            {w:13, d:64, s:"교재1", k:"우리 학교는 일주일 내내 수업을 한다.", e:"My school runs classes seven days a week."},
            {w:13, d:64, s:"교재1", k:"그 축제는 이달 말까지 계속된다.", e:"The festival runs through the end of the month."},
            {w:13, d:64, s:"교재1", k:"내 동생은 치킨 가게를 운영한다.", e:"My brother runs a chicken restaurant."},
            {w:13, d:64, s:"교재2", k:"음식도 그저 그렇고 서비스는 엉망이네.", e:"The food isn’t that good and the service is terrible."},
            {w:13, d:64, s:"교재2", k:"내 말이. 사장이 누굴까?", e:"Seriously. Who runs this place?"},
            {w:13, d:64, s:"교재2", k:"(뉴욕 여행 중 친구에게) 우리 이제 슬슬 호텔로 돌아가야 하지 않을까? 늦었어.", e:"Shouldn’t we get back to the hotel soon? It’s late."},
            {w:13, d:64, s:"교재2", k:"괜찮아. 지하철 타면 되지.", e:"That doesn’t matter. We can take the subway."},
            {w:13, d:64, s:"교재2", k:"근데 지하철 끊기면 어떡해?", e:"But what if the subway closes?"},
            {w:13, d:64, s:"교재2", k:"걱정 마. 여기는 지하철이 24시간 내내 운행하거든.", e:"Calm down. The subway here runs 24/7."},
            {w:13, d:64, s:"교재3", k:"분위기는 마음에 드는데 커피가 너무 별로야.", e:"I like the vibe, but the coffee is terrible."},
            {w:13, d:64, s:"교재3", k:"그 여자 노래 들어 봤어? 형편없더라.", e:"Have you heard her sing? She’s awful."},
            {w:13, d:64, s:"교재3", k:"오늘 아침에 두통이 너무 심했어.", e:"I had an awful headache this morning."},
            {w:13, d:64, s:"교재3", k:"오늘 아침 교통 상황 정말 죽음이더라.", e:"The traffic was horrible this morning."},
            {w:13, d:64, s:"교재3", k:"연휴 때 아픈 건 정말 최악이야.", e:"It sucks being sick during the holidays."},
            {w:13, d:64, s:"교재3", k:"5월인데 날씨가 여전히 안 좋네. 대체 봄은 언제 시작하는 거야?", e:"It’s May, but the weather still sucks. When will spring start?"},

            {w:13, d:65, s:"대표", k:"침은 한 번도 안 맞아 봤어요.", e:"I’ve never tried acupuncture before."},
            {w:13, d:65, s:"교재1", k:"어젯밤에 새로 알게 된 닭고기 레시피를 시도해 봤는데, 요리가 너무 (맛있게) 잘됐다.", e:"I tried a new chicken recipe last night, and it came out great."},
            {w:13, d:65, s:"교재1", k:"여행할 때는 늘 현지 음식을 먹어 보려고 한다.", e:"I always try local food when I travel."},
            {w:13, d:65, s:"교재1", k:"엄마가 어젯밤에 처음으로 초밥을 드셔 보셨는데 정말 싫어하셨다.", e:"My mom tried sushi for the first time last night and hated it."},
            {w:13, d:65, s:"교재1", k:"머리가 빠지는 것 같다. 아무래도 다른 샴푸를 써 봐야 하나 싶다.", e:"I feel like I’m losing my hair. Maybe I should try a different shampoo."},
            {w:13, d:65, s:"교재1", k:"(데이트 상황) 이번 주말에는 새로운 곳에 가 보자.", e:"We should try somewhere new this weekend."},
            {w:13, d:65, s:"교재2", k:"등산 정말 좋았어. 기분이 너무 좋다.", e:"That was a great hike. I feel awesome."},
            {w:13, d:65, s:"교재2", k:"나도. 다음번에는 상급자 코스에 도전해 보자.", e:"Me too. Let’s try the advanced trail next time."},
            {w:13, d:65, s:"교재2", k:"여보, 은행 앱에 접속이 안 돼. 비밀번호가 뭐야?", e:"Honey, I can’t log in to our bank app. What’s the password?"},
            {w:13, d:65, s:"교재2", k:"우리 결혼기념일.", e:"It’s our wedding anniversary."},
            {w:13, d:65, s:"교재2", k:"그걸로 해 봤는데, 한 번 더 실패하면 잠겨 버릴 거야.", e:"I tried that, but if I fail one more time, it’s going to lock me out."},
            {w:13, d:65, s:"교재2", k:"음, 그 말은 당신이 우리 기념일을 까먹었다는 뜻이네.", e:"Hmm, well, that means you forgot our anniversary."},
            {w:13, d:65, s:"교재3", k:"안 가 봤던 데 가 보자.", e:"Let’s go somewhere new."},
            {w:13, d:65, s:"교재3", k:"강남 근처 아무 데서나 만나자.", e:"Let’s meet anywhere near Gangnam."},
            {w:13, d:65, s:"교재3", k:"역에서 가까운 데서 만나자.", e:"Let’s meet somewhere close to the station."},
            {w:13, d:65, s:"교재3", k:"오늘 밤엔 고급스러운 식당에 가서 먹고 싶네.", e:"I feel like eating somewhere fancy tonight."},
            {w:13, d:65, s:"교재3", k:"(이번 여행 중에) 재미있는 데는 아무 데도 안 갔어.", e:"We didn’t go anywhere exciting."},
            {w:13, d:65, s:"교재3", k:"네 생일 때 특별한 곳에 가는 거야?(생일에 어디 좋은 데 가?)", e:"Are you going anywhere special for your birthday?"},

            // ================== Week 14 데이터 ==================
            // Day 66
            {w:14, d:66, s:"대표", k:"너무 더워서 견디기가 힘들어요.", e:"It’s so hot I can’t stand it."},
            {w:14, d:66, s:"교재1", k:"줄 서서 기다리는 건 견디기 너무 힘들다.", e:"I can’t stand waiting in line."},
            {w:14, d:66, s:"교재1", k:"당신이 매일 그 여자랑 같이 일하는 걸 어떻게 참는지 모르겠어요.", e:"I don’t know how you stand working with her every day."},
            {w:14, d:66, s:"교재1", k:"매일 출근하는 데 두 시간이나 걸리는 걸 어떻게 견뎌?", e:"How do you stand a two-hour commute to work every day?"},
            {w:14, d:66, s:"교재1", k:"차들이 끼어들면 정말 참을 수가 없다.", e:"I can’t stand it when cars cut me off."},
            {w:14, d:66, s:"교재1", k:"생선회 냄새를 견디기가 힘들다.", e:"I can’t stand the smell of raw fish."},
            {w:14, d:66, s:"교재2", k:"휴우, 아침에 지하철이 너무 붐벼서 정말 힘들어.", e:"Ugh, I can’t stand being crammed on the subway in the morning."},
            {w:14, d:66, s:"교재2", k:"나도 그래! 그래서 혼잡 시간을 피하려고 조금 일찍 나서려고 해.", e:"Same here! That’s why I try to leave a bit earlier—to avoid the rush."},
            {w:14, d:66, s:"교재2", k:"장 보는 거 너무 힘들어.", e:"I really can’t stand grocery shopping."},
            {w:14, d:66, s:"교재2", k:"쿠팡 같은 것을 이용해서 온라인으로 장 볼 수도 있잖아.", e:"Maybe you could do online shopping using something like Coupang."},
            {w:14, d:66, s:"교재2", k:"응, 근데 (마트에 가면) 신선한 채소를 직접 고르는 재미가 있거든.", e:"Yeah, but I like picking out fresh vegetables myself."},
            {w:14, d:66, s:"교재2", k:"일리 있는 말이다. 그럼 무거운 것만 온라인에서 주문하면 되겠네!", e:"Fair enough, but at least you can order the heavy stuff online!"},
            {w:14, d:66, s:"교재3", k:"난 매운 음식은 좀 별로라서 김치는 사양할게.", e:"I don’t really like spicy food, so I’ll pass on the kimchi."},
            {w:14, d:66, s:"교재3", k:"뭐 그럴 수도 있겠네. 모두가 김치를 좋아하는 건 아니지.", e:"Fair enough. Not everyone enjoys it."},
            {w:14, d:66, s:"교재3", k:"오늘 밤에는 못 나가. 내일 아침 일찍 회의가 있어.", e:"I can’t go out tonight. I have an early meeting tomorrow."},
            {w:14, d:66, s:"교재3", k:"이해해. 일이 우선이지.", e:"Fair enough. Work comes first."},
            {w:14, d:66, s:"교재3", k:"당신 초밥 먹고 싶은 거 아는데, 난 오늘 파스타가 당겨.", e:"I know you wanted sushi, but I’m craving pasta today."},
            {w:14, d:66, s:"교재3", k:"이해해. 초밥은 다음에 먹지 뭐.", e:"Fair enough. We can get sushi next time."},
            {w:14, d:66, s:"교재3", k:"이 마케팅 계획은 이 제품에 잘 맞지 않는 것 같습니다.", e:"I don’t think this marketing plan suits the product."},
            {w:14, d:66, s:"교재3", k:"그 말도 일리가 있네요. 다른 접근 방식을 생각해 둔 거라도 있어요?", e:"Fair enough. Did you have a different approach in mind?"},

            // Day 67
            {w:14, d:67, s:"대표", k:"제가 제일 아끼는 컵 손잡이가 부러졌어요.", e:"The handle on my favorite cup broke."},
            {w:14, d:67, s:"교재1", k:"내 재킷의 지퍼가 고장 났다.", e:"The zipper on my jacket broke."},
            {w:14, d:67, s:"교재1", k:"내가 앉았을 때 의자가 부러졌다.", e:"The chair broke when I sat on it."},
            {w:14, d:67, s:"교재1", k:"열쇠를 차 안에 두고 문을 잠그는 바람에 유리창을 깨야만 했다.", e:"I locked my keys in the car and I had to break the window."},
            {w:14, d:67, s:"교재1", k:"(손톱 손질을 하다가) 손톱이 부러졌다.", e:"I broke my nail."},
            {w:14, d:67, s:"교재1", k:"설거지하다가 실수로 유리잔을 깨뜨렸다.", e:"I accidentally broke a glass while washing dishes."},
            {w:14, d:67, s:"교재2", k:"완전 엉망이네! 무슨 일이야?", e:"What a mess! What happened?"},
            {w:14, d:67, s:"교재2", k:"테이블이 부러져서 커피가 쏟아졌어.", e:"The table broke and spilled my coffee."},
            {w:14, d:67, s:"교재2", k:"밖이 엄청 추워.", e:"It’s freezing outside."},
            {w:14, d:67, s:"교재2", k:"알아, 근데 왜 그렇게 재킷 지퍼를 안 잠그고 있는 거니?", e:"I know, so why do you have your jacket open like that?"},
            {w:14, d:67, s:"교재2", k:"아, 아침에 지퍼가 고장 났어. 그런데 이게 내가 가진 재킷 중에 제일 따뜻해.", e:"Oh, I broke the zipper this morning, and it’s the warmest jacket I own."},
            {w:14, d:67, s:"교재2", k:"음, 새거 사야겠구나.", e:"Well, it’s time to buy a new one."},
            {w:14, d:67, s:"교재3", k:"너한테 이런 말 하기는 좀 그렇긴 한데… 그 여자분 너한테 관심 없는 게 맞아.", e:"I hate to break it to you, but I’m pretty sure she’s not interested in you."},
            {w:14, d:67, s:"교재3", k:"이제는 제가 그만둔다는 걸 이야기해야 될 때인 것 같아요.", e:"I think it’s time I break the news to everyone about me quitting."},
            {w:14, d:67, s:"교재3", k:"그래요, 빠를수록 좋아요.", e:"Yeah, the sooner, the better."},
            {w:14, d:67, s:"교재3", k:"저는 고쳐야 할 나쁜 습관이 많습니다.", e:"I have a lot of bad habits I need to break."},
            {w:14, d:67, s:"교재3", k:"그녀는 핫도그 먹기 대회에서 세계 신기록을 수립했다.", e:"She broke the world record at the hot dog eating contest."},

            // Day 68
            {w:14, d:68, s:"대표", k:"찬물을 마시면 이가 아파요.", e:"My tooth hurts when I drink cold water."},
            {w:14, d:68, s:"교재1", k:"우리 강아지가 어제 고양이를 쫓다가 다리를 다쳤다.", e:"My dog hurt its leg yesterday chasing a cat."},
            {w:14, d:68, s:"교재1", k:"나는 넘어지면서 팔꿈치를 다쳤다.", e:"I hurt my elbow when I fell down."},
            {w:14, d:68, s:"교재1", k:"그는 축구를 하다가 무릎을 다쳤다.", e:"He hurt his knee while playing soccer."},
            {w:14, d:68, s:"교재1", k:"팔꿈치 아직 아파요?", e:"Does your elbow still hurt?"},
            {w:14, d:68, s:"교재1", k:"시도해서 손해 볼 건 없다.", e:"It doesn’t hurt to try."},
            {w:14, d:68, s:"교재2", k:"(짐을 옮겨 주는 동료에게) 꽤 무거워 보여요. 다치면 안 돼요.", e:"That looks pretty heavy. Don’t hurt yourself now."},
            {w:14, d:68, s:"교재2", k:"제 걱정은 하지 마세요. 이 정도는 할 수 있어요.", e:"Don’t worry about me. I can handle this."},
            {w:14, d:68, s:"교재2", k:"지난주에도 이가 아프다고 하지 않았어?", e:"Didn’t you say your tooth hurt last week, too?"},
            {w:14, d:68, s:"교재2", k:"응, 뭔가 단것을 먹으면 이가 너무 아파.", e:"Yeah, it really hurts when I eat anything sweet."},
            {w:14, d:68, s:"교재2", k:"그거 확실히 충치 같은데.", e:"That sounds like a cavity for sure."},
            {w:14, d:68, s:"교재2", k:"통증이 있다가 없다가 그래. 그래서 더 신경이 쓰여.", e:"And the pain comes and goes, which makes it even more annoying."},
            {w:14, d:68, s:"교재3", k:"이번 달에 틀림없이 보너스가 나온다.", e:"We’re getting a bonus this month for sure."},
            {w:14, d:68, s:"교재3", k:"그 사람 이사 가는지 확실히는 모르겠지만, 그렇게 들은 것 같네요.", e:"I don’t know for sure if he’s moving, but that’s what I heard."},
            {w:14, d:68, s:"교재3", k:"그가 꽃병을 깬 사람이야. 확실해. 내가 그 장면을 봤어.", e:"He’s the one who broke the vase, for sure. I saw it happen."},
            {w:14, d:68, s:"교재3", k:"내가 먹어 본 피자 중에 최고였어.", e:"That was the best pizza I’ve ever had."},
            {w:14, d:68, s:"교재3", k:"누가 아니래!", e:"For sure!"},
            {w:14, d:68, s:"교재3", k:"내년에는 꼭 유럽에 가고 싶어.", e:"I want to visit Europe next year, for sure."},
            {w:14, d:68, s:"교재3", k:"이번 주말에는 무조건 쉴 거야.", e:"I’m taking a break this weekend, for sure."},

            // Day 69
            {w:14, d:69, s:"대표", k:"보니까 점심 때 아무것도 안 먹더군요.", e:"I noticed you never eat anything at lunch."},
            {w:14, d:69, s:"교재1", k:"보니까 천장에 금이 가 있었다.", e:"I noticed a crack in the ceiling."},
            {w:14, d:69, s:"교재1", k:"너 아는지 모르겠는데, 셔츠 단추가 풀렸어.", e:"I don’t know if you’ve noticed, but your shirt is unbuttoned."},
            {w:14, d:69, s:"교재1", k:"그 친구 행동이 바뀐 거 눈치챘어?", e:"Have you noticed any changes in his behavior?"},
            {w:14, d:69, s:"교재1", k:"그 사람이 나한테 마음이 있는 줄 전혀 몰랐다.", e:"I didn’t notice he had feelings for me."},
            {w:14, d:69, s:"교재1", k:"너 흰머리가 그렇게 많은 줄 몰랐네.", e:"I didn’t notice how much gray hair you have."},
            {w:14, d:69, s:"교재2", k:"파운데이션 바꾼 뒤로 얼굴에 뭐가 계속 나.", e:"My face has been breaking out* ever since I changed foundations."},
            {w:14, d:69, s:"교재2", k:"정말? 나는 모르겠던데. 얼굴 상태가 좋아 보이는 것 같은데.", e:"Really? I didn’t notice. I think your face looks fine."},
            {w:14, d:69, s:"교재2", k:"방금 (차에) 흠집을 발견했어요. 광고에 이 부분이 언급되어 있었나요?", e:"I just noticed a dent. Was this mentioned in the ad?"},
            {w:14, d:69, s:"교재2", k:"음, 언급 안 한 것 같습니다. 흠집이 어디에 있나요?", e:"Hmm, I don’t think so. Where is it?"},
            {w:14, d:69, s:"교재2", k:"후면 범퍼에요. 꽤나 눈에 잘 띄는데요.", e:"On the rear bumper. It’s pretty noticeable."},
            {w:14, d:69, s:"교재2", k:"오, 지금 보니까 그렇군요. 아직 구매 의사가 있으시면, 가격 낮춰 드릴게요.", e:"Oh, I see it now. I can lower the price, if you’re still interested."},
            {w:14, d:69, s:"교재3", k:"부엌에서 타는 냄새가 나는 걸 알아차렸다.", e:"I noticed a burning smell coming from the kitchen."},
            {w:14, d:69, s:"교재3", k:"제프가 점심 먹고 나서 절대 양치 안 하는 거 눈치챘어?", e:"Did you notice that Jeff never brushes his teeth after lunch?"},
            {w:14, d:69, s:"교재3", k:"부엌에서 연기 냄새가 났는데, 가만히 생각해 보니 오븐을 두 시간째 켜 두었던 것이다.", e:"After I smelled smoke from the kitchen, I realized I had left the oven on for two hours."},
            {w:14, d:69, s:"교재3", k:"사무실에 있던 키 큰 남자가 사장이라는 걸 알게 됐다.", e:"I realized that the tall man in the office was the president."},
            {w:14, d:69, s:"교재3", k:"캘빈클라인 향수 산 거야? 나 그 향수 냄새 알거든.", e:"Did you buy Calvin Klein cologne? I recognize the smell."},
            {w:14, d:69, s:"교재3", k:"바에서 어떤 남자가 나한테 다가왔는데, 누군지 전혀 모르겠더라고.", e:"A guy approached me at the bar, but I didn’t recognize him at all."},

            // Day 70
            {w:14, d:70, s:"대표", k:"내일까지 답변 주시기 바랍니다.", e:"I expect an answer by tomorrow."},
            {w:14, d:70, s:"교재1", k:"회사에서 연봉을 인상해 줄 줄은 생각도 못했는데, 인상되었다.", e:"I didn’t expect a raise at work, but I got one."},
            {w:14, d:70, s:"교재1", k:"캘리포니아에 눈이 올 줄은 몰랐다.", e:"I didn’t expect snow in California."},
            {w:14, d:70, s:"교재1", k:"낯선 사람에게서 그런 호의는 예상하지 못했다.", e:"I didn’t expect such kindness from a stranger."},
            {w:14, d:70, s:"교재1", k:"집에 가야겠어. 베이비시터는 내가 오늘 밤 10시 전에 집에 들어오는 것으로 알고 있거든.", e:"It’s time for me to go. My babysitter is expecting me home before 10 tonight."},
            {w:14, d:70, s:"교재1", k:"내 작품이 박물관에 전시되었을 때 가족과 친구가 엄청 많이 올 줄 알았어요.", e:"I expected a big crowd of family and friends when my art was on display at the museum."},
            {w:14, d:70, s:"교재2", k:"그러니까, 임신했다고 말했는데도 제프가 아무 말을 안 했다고?", e:"You mean, Jeff didn’t say anything after you told him you were pregnant?"},
            {w:14, d:70, s:"교재2", k:"응, 밤새 아무 말이 없었어. 그런 반응을 보일 줄은 전혀 예상하지 못했는데∙∙∙.", e:"Yeah, he was silent all night. I didn’t expect that reaction from him…"},
            {w:14, d:70, s:"교재2", k:"그래서, 당신에게 정규직을 제안드리고 싶습니다. 어떠세요?", e:"So, we want to offer you a full-time position. What do you think?"},
            {w:14, d:70, s:"교재2", k:"와, 제안 정말 감사합니다. 조금 생각해 볼 시간을 주시겠어요?", e:"Wow, I really appreciate the offer. Could I take a little time to consider it?"},
            {w:14, d:70, s:"교재2", k:"물론이죠. 하지만 내일까지 답을 주셔야 합니다. 최대한 빨리 채용을 해야 해서요.", e:"Of course, but I expect an answer from you by tomorrow, because we need to hire someone ASAP."},
            {w:14, d:70, s:"교재2", k:"알겠습니다. 오전에 전화 드리겠습니다.", e:"Understood. I’ll call you in the morning."},
            {w:14, d:70, s:"교재3", k:"이 제안서 내일 오후 5시까지 제출해야 해요.", e:"We need to submit this proposal by 5 p.m. tomorrow."},
            {w:14, d:70, s:"교재3", k:"알겠습니다. 제가 처리하겠습니다.", e:"Understood. I’ll take care of it."},
            {w:14, d:70, s:"교재3", k:"나 오후 3시 비행기로 도착하니까, 네가 3시 30분쯤에 데리러 오면 좋을 듯해.", e:"I fly in at 3 p.m., so if you could pick me up around 3:30, that’d be great."},
            {w:14, d:70, s:"교재3", k:"알겠어. 그리로 갈게.", e:"Got it. I’ll be there."},
            {w:14, d:70, s:"교재3", k:"(자동차 판매원에게) 질문을 너무 많이 드려서 죄송하지만, 이건 저희에게 고액의 구매거든요.", e:"I apologize for asking so many questions, but this is a big purchase for us."},
            {w:14, d:70, s:"교재3", k:"무슨 말씀이신지 잘 알겠습니다, 선생님. 성심성의껏 모든 질문에 답변드릴게요.", e:"Point taken, sir. I’m happy to answer any of your questions."}
        ];

        // 고유 ID 부여
        allData.forEach((item, index) => item.id = index + 1);

        const sourceMap = { "대표": "대표예제", "교재1": "Model examples", "교재2": "Small talk", "교재3": "Further studies" };
        let selectedWeek = 9; // 기본 9주차 세팅
        let quizPool = [];
        let currentIndex = 0;
        let starredIds = new Set();

        // 탭 선택 로직
        function selectWeek(w) {
            selectedWeek = w;
            document.querySelectorAll('.week-tab').forEach(t => t.classList.remove('active-tab'));
            document.getElementById(`tab-${w}`).classList.add('active-tab');

            const titleEl = document.getElementById('selected-week-title');
            if (w === '9-14') {
                titleEl.innerText = 'WEEK 9-14 누적';
            } else {
                titleEl.innerText = `WEEK ${w}`;
            }
        }

        // 모바일 오디오 깨우기
        function unlockAudio() {
            if ('speechSynthesis' in window) {
                const msg = new SpeechSynthesisUtterance("Quiz Start");
                msg.volume = 0;
                msg.rate = 10;
                window.speechSynthesis.speak(msg);
                window.speechSynthesis.cancel(); 
            }
        }

        // 퀴즈 시작 (누적 처리 포함)
        function startQuiz(mode) {
            unlockAudio();

            let weekData = [];
            if (selectedWeek === '9-14') {
                // 누적 탭일 경우 9 ~ 14주차 모두 합산
                weekData = allData.filter(item => item.w >= 9 && item.w <= 14);
            } else {
                // 개별 주차
                weekData = allData.filter(item => item.w === selectedWeek);
            }
            
            let filtered = (mode === 'mild') 
                ? weekData.filter(item => item.s === '대표' || item.s === '교재1') 
                : [...weekData];

            quizPool = filtered.sort(() => Math.random() - 0.5).slice(0, 10);
            currentIndex = 0;
            starredIds.clear();
            
            document.getElementById('setup-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.remove('hidden');
            showQuestion();
        }

        function showQuestion() {
            const item = quizPool[currentIndex];
            const card = document.getElementById('card-container');
            const star = document.getElementById('star-btn');
            
            card.classList.remove('flipped');
            star.classList.remove('star-checked');
            document.getElementById('next-btn').classList.add('hidden');
            
            document.getElementById('q-korean').innerText = item.k;
            document.getElementById('q-english').innerText = item.e;
            document.getElementById('q-info').innerText = `Day ${String(item.d).padStart(3, '0')} - ${sourceMap[item.s]}`;
            document.getElementById('progress-text').innerText = `${currentIndex + 1} / ${quizPool.length}`;
        }

        function flipCard() {
            document.getElementById('card-container').classList.add('flipped');
            document.getElementById('next-btn').classList.remove('hidden');
        }

        function toggleStar(e) {
            e.stopPropagation();
            const item = quizPool[currentIndex];
            const starBtn = document.getElementById('star-btn');
            if (starredIds.has(item.id)) {
                starredIds.delete(item.id);
                starBtn.classList.remove('star-checked');
            } else {
                starredIds.add(item.id);
                starBtn.classList.add('star-checked');
            }
        }

        function playTTS() {
            const text = document.getElementById('q-english').innerText;
            if ('speechSynthesis' in window) {
                window.speechSynthesis.cancel();
                const msg = new SpeechSynthesisUtterance();
                msg.text = text;
                msg.lang = 'en-US';
                msg.rate = 0.9;
                
                let voices = window.speechSynthesis.getVoices();
                if (voices.length > 0) {
                    msg.voice = voices.find(v => v.lang.includes('en-US')) || voices[0];
                }
                window.speechSynthesis.speak(msg);
            }
        }

        function nextQuestion() {
            currentIndex++;
            if (currentIndex < quizPool.length) showQuestion();
            else showResults();
        }

        function showResults() {
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');
            const listEl = document.getElementById('starred-list');
            listEl.innerHTML = '<p class="text-[10px] text-slate-400 font-black mb-4 uppercase tracking-widest text-center">⭐ 다시 학습할 문장</p>';
            
            const starredItems = quizPool.filter(item => starredIds.has(item.id));
            if (starredItems.length === 0) {
                listEl.innerHTML += '<p class="text-sm text-slate-400 py-12 text-center font-bold">체크한 문장이 없네요!</p>';
            } else {
                starredItems.forEach(item => {
                    const div = document.createElement('div');
                    div.className = "p-5 bg-indigo-50 rounded-2xl border border-indigo-100 mb-3";
                    div.innerHTML = `
                        <div class="flex justify-between items-start mb-2">
                            <span class="text-[9px] bg-indigo-500 text-white px-1.5 py-0.5 rounded font-black tracking-tighter">DAY ${item.d}</span>
                            <span class="text-[9px] text-indigo-400 font-bold italic">${sourceMap[item.s]}</span>
                        </div>
                        <p class="text-xs text-slate-500 mb-1 leading-relaxed">${item.k}</p>
                        <p class="text-sm font-black text-indigo-900 leading-snug">${item.e}</p>
                    `;
                    listEl.appendChild(div);
                });
            }
        }

        // 보안: 우클릭/복사 방지
        document.addEventListener('contextmenu', e => e.preventDefault());
        document.addEventListener('keydown', e => {
            if (e.ctrlKey && (e.key === 'c' || e.key === 'u' || e.key === 's')) e.preventDefault();
            if (e.key === 'F12') e.preventDefault();
        });
    </script>
</body>
</html>
