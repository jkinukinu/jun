# jun<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>神経症状の連関モデル</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chosen Palette: Calm Neutrals -->
    <!-- Application Structure Plan: A narrative, scroll-based SPA. It starts with a central interactive human diagram for exploration, followed by a scroll-animated "domino effect" flowchart to summarize the causal chain, and concludes with a tabbed section for detailed text. This structure transforms a linear document into an engaging, multi-faceted learning tool, allowing both quick overviews and deep dives. -->
    <!-- Visualization & Content Choices: Report Info: Brain-body connections -> Goal: Organize/Relate -> Viz: Interactive diagram (HTML/CSS) -> Interaction: Click hotspots to show text -> Justification: Visually intuitive -> Method: JS. Report Info: Causal chain summary -> Goal: Explain Process -> Viz: Animated vertical flowchart -> Interaction: Scroll-reveal -> Justification: Emphasizes step-by-step nature -> Method: JS (Intersection Observer). Report Info: Detailed explanations -> Goal: Inform -> Viz: Tabbed content area -> Interaction: Click tabs -> Justification: Manages text density -> Method: JS. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Hiragino Kaku Gothic ProN', 'メイリオ', Meiryo, sans-serif;
            background-color: #FDFBF8;
            color: #333;
        }
        .hotspot {
            transition: all 0.3s ease;
            cursor: pointer;
        }
        .hotspot:hover, .hotspot.active {
            transform: scale(1.1);
            filter: drop-shadow(0 0 10px rgba(45, 212, 191, 0.7));
        }
        .content-panel {
            transition: opacity 0.5s ease, transform 0.5s ease;
        }
        .domino-step {
            transition: opacity 0.6s ease-in-out, transform 0.6s ease-in-out;
            opacity: 0;
            transform: translateY(20px);
        }
        .domino-step.visible {
            opacity: 1;
            transform: translateY(0);
        }
        .domino-line {
            height: 0;
            transition: height 0.5s ease-in-out 0.3s;
        }
        .domino-step.visible + .domino-line {
            height: 3rem;
        }
        .tab-button.active {
            border-color: #0d9488;
            background-color: #ccfbf1;
            color: #0d9488;
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
        }
    </style>
</head>
<body class="bg-[#FDFBF8] text-slate-800">

    <div class="max-w-6xl mx-auto p-4 sm:p-6 lg:p-8">
        
        <header class="text-center mb-12">
            <h1 class="text-3xl md:text-4xl font-bold text-teal-800 mb-2">脳神経学的観点から見た症状の連関モデル</h1>
            <p class="text-lg text-slate-600">左下肢の痛みと右臀部の痛みの意外な繋がりを探る</p>
        </header>

        <main>
            <section id="interactive-diagram" class="mb-16">
                <h2 class="text-2xl font-bold text-center mb-2 text-teal-700">インタラクティブ人体図</h2>
                <p class="text-center text-slate-600 mb-8">知りたい部位をクリックすると、関連する解説が表示されます。</p>

                <div class="grid grid-cols-1 md:grid-cols-3 gap-8 items-center">
                    
                    <div class="md:col-span-1 h-full">
                        <div id="content-display" class="bg-white p-6 rounded-lg shadow-lg border border-slate-200 h-full content-panel">
                            <h3 id="content-title" class="text-xl font-bold text-teal-700 mb-4">解説</h3>
                            <p id="content-text" class="text-slate-700 leading-relaxed">左の人体図の各部位をクリックしてください。</p>
                        </div>
                    </div>

                    <div class="md:col-span-2 flex justify-center items-center relative">
                        <div class="w-full max-w-md relative">
                            <div class="absolute top-0 left-1/2 -translate-x-1/2 w-32 h-32 bg-rose-100 rounded-full flex justify-center items-center text-center p-2 hotspot" data-target="cerebrum">
                                <span class="font-bold text-rose-800">右大脳</span>
                            </div>
                            
                            <div class="absolute top-24 left-1/4 -translate-x-1/2 w-20 h-20 bg-sky-100 rounded-full flex justify-center items-center text-center p-1 hotspot" data-target="cerebellum">
                                <span class="font-bold text-sky-800">左小脳</span>
                            </div>

                            <div class="absolute top-24 right-1/4 translate-x-1/2 w-20 h-20 bg-amber-100 rounded-full flex justify-center items-center text-center p-1 hotspot" data-target="canals">
                                <span class="font-bold text-amber-800">右三半規管</span>
                            </div>

                            <div class="w-2/3 h-[450px] bg-slate-200 mx-auto mt-20 rounded-t-full rounded-b-lg relative overflow-hidden">
                                <div class="absolute top-1/2 left-0 w-1/2 h-1/2 bg-sky-200 p-4 hotspot" data-target="left-leg">
                                    <span class="font-bold text-sky-900">左下肢・膝</span>
                                </div>
                                <div class="absolute top-1/2 right-0 w-1/2 h-1/2 bg-rose-200 p-4 text-right hotspot" data-target="right-buttock">
                                    <span class="font-bold text-rose-900">右臀部</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </section>
            
            <section id="domino-effect" class="py-16 bg-white rounded-lg shadow-lg border border-slate-200">
                <h2 class="text-2xl font-bold text-center mb-12 text-teal-700">症状の連鎖反応：ドミノエフェクト</h2>
                <div class="max-w-2xl mx-auto flex flex-col items-center">

                    <div class="domino-step bg-rose-50 border-l-4 border-rose-500 p-4 rounded-r-lg shadow-sm w-full">
                        <p class="font-bold text-rose-800 text-lg">① 右大脳の機能低下（根本原因）</p>
                    </div>
                    <div class="domino-line w-1 bg-slate-300"></div>
                    
                    <div class="domino-step bg-slate-50 border-l-4 border-slate-500 p-4 rounded-r-lg shadow-sm w-full">
                        <p class="font-bold text-slate-800 text-lg">② 左下肢の運動・感覚障害 ＆ 左小脳への入力低下</p>
                    </div>
                    <div class="domino-line w-1 bg-slate-300"></div>

                    <div class="domino-step bg-sky-50 border-l-4 border-sky-500 p-4 rounded-r-lg shadow-sm w-full">
                        <p class="font-bold text-sky-800 text-lg">③ 左小脳 ＆ 右三半規管の機能低下</p>
                    </div>
                    <div class="domino-line w-1 bg-slate-300"></div>
                    
                    <div class="domino-step bg-slate-50 border-l-4 border-slate-500 p-4 rounded-r-lg shadow-sm w-full">
                        <p class="font-bold text-slate-800 text-lg">④ 左半身の協調運動不全とバランス能力の低下</p>
                    </div>
                    <div class="domino-line w-1 bg-slate-300"></div>

                    <div class="domino-step bg-slate-50 border-l-4 border-slate-500 p-4 rounded-r-lg shadow-sm w-full">
                        <p class="font-bold text-slate-800 text-lg">⑤ 全身の不安定性による重心の右側への偏り</p>
                    </div>
                    <div class="domino-line w-1 bg-slate-300"></div>

                    <div class="domino-step bg-rose-50 border-l-4 border-rose-500 p-4 rounded-r-lg shadow-sm w-full">
                        <p class="font-bold text-rose-800 text-lg">⑥ 右臀部の筋肉への慢性的な過負荷</p>
                    </div>
                    <div class="domino-line w-1 bg-slate-300"></div>

                    <div class="domino-step bg-rose-50 border-l-4 border-rose-500 p-4 rounded-r-lg shadow-sm w-full">
                        <p class="font-bold text-rose-800 text-lg">⑦ 右臀部の痛み（代償作用の破綻）</p>
                    </div>
                </div>
            </section>

            <section id="detailed-explanations" class="mt-16">
                <h2 class="text-2xl font-bold text-center mb-8 text-teal-700">詳細解説</h2>
                <div class="w-full max-w-4xl mx-auto">
                    <div class="border-b border-slate-300 flex justify-center space-x-2 md:space-x-4 mb-6">
                        <button class="tab-button active font-semibold py-2 px-4 border-b-2 border-transparent transition" data-tab="1">1. すべての始まり</button>
                        <button class="tab-button font-semibold py-2 px-4 border-b-2 border-transparent transition" data-tab="2">2. 連携不全</button>
                        <button class="tab-button font-semibold py-2 px-4 border-b-2 border-transparent transition" data-tab="3">3. 代償作用の破綻</button>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-lg border border-slate-200">
                        <div id="tab-content-1" class="tab-content active"></div>
                        <div id="tab-content-2" class="tab-content"></div>
                        <div id="tab-content-3" class="tab-content"></div>
                    </div>
                </div>
            </section>
        </main>

        <footer class="mt-16 pt-8 border-t border-slate-300 text-center text-slate-500">
            <p class="font-bold text-red-600">【重要】</p>
            <p class="text-sm">この解説は、提示された情報に基づいた神経学的な考察モデルであり、医学的な診断ではありません。正確な診断と治療のためには、必ず専門家にご相談ください。</p>
        </footer>

    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const contentData = {
                cerebrum: {
                    title: "1. すべての始まり：右大脳の機能低下",
                    text: "中心的な問題です。運動をコントロールする運動野や、感覚を処理する感覚野の機能が低下している状態を想定します。脳の指令は交叉するため、右大脳は主に体の左半身の運動と感覚を支配しています。"
                },
                'left-leg': {
                    title: "右大脳機能低下が引き起こす直接的な症状",
                    text: "①左下肢の運動低下: 右大脳からの指令が弱まり、左足の筋肉がうまく使えなくなります。\n②左膝の痛み（感覚異常）: 感覚情報を正しく処理できず、関節が不安定になったり、脳が痛みを過剰に感じたりする可能性があります。"
                },
                cerebellum: {
                    title: "2. 右大脳から左小脳への影響",
                    text: "大脳の運動計画は、反対側の小脳に送られ動きを調整します。右大脳の機能が低下すると、連携する左小脳への入力も減少し機能低下を招きます。左小脳は左半身の動きの微調整やバランスを担うため、症状が悪化します。"
                },
                canals: {
                    title: "2. 右三半規管の機能低下",
                    text: "頭の回転や傾きを感知しバランスを保つセンサーです。機能低下が起きると、空間認識や平衡感覚が乱れ、体は常に不安定な状態になります。これを補うため無意識に筋肉が緊張します。"
                },
                'right-buttock': {
                    title: "3. 結果として生じる右臀部の痛み",
                    text: "左半身が不安定になると、体は無意識に重心を安定している右側に移します。右足で体を支えるため、骨盤を安定させる右側の臀筋群が常に過剰に緊張し酷使されます。この慢性的な過負荷が痛みを引き起こします。"
                }
            };

            const detailedContent = {
                '1': `
                    <h3 class="text-xl font-bold text-teal-700 mb-4">1. すべての始まり：右大脳の機能低下</h3>
                    <p class="mb-4">まず中心的な問題として「右大脳の機能低下」を据えます。特に、運動をコントロールする<strong>運動野</strong>や、感覚を処理する<strong>感覚野</strong>（頭頂葉）の機能が低下している状態を想定します。</p>
                    <div class="bg-slate-50 p-4 rounded-lg">
                        <p class="mb-2"><strong>右大脳と左半身の関係（交叉支配）:</strong> 脳の指令の大部分は延髄で交叉するため、右大脳は主に体の<strong>左半身</strong>の運動と感覚を支配しています。</p>
                        <p><strong>右大脳機能低下が引き起こす直接的な症状:</strong></p>
                        <ul class="list-disc list-inside ml-4 mt-2 space-y-1">
                            <li><strong>左下肢の運動低下:</strong> 右大脳の運動野からの指令が弱まるため、左足の筋肉をうまく使えなくなります。これにより、足が上がりにくくなったり、力が入らなくなったりします。</li>
                            <li><strong>左膝の痛み（感覚異常）:</strong> 右大脳の感覚野の機能が低下すると、左膝からの感覚情報（位置覚、圧覚など）を正しく処理できなくなります。これにより、関節が不安定になったり、脳が痛みを過剰に感じたり（中枢性疼痛）、あるいは不安定な動きによって二次的に物理的な痛みが生じたりする可能性があります。</li>
                        </ul>
                    </div>
                `,
                '2': `
                    <h3 class="text-xl font-bold text-teal-700 mb-4">2. 大脳と小脳、そして三半規管の連携不全</h3>
                    <p class="mb-4">次に、大脳と連携して働く小脳と三半規管（前庭系）の問題が加わります。</p>
                    <div class="bg-slate-50 p-4 rounded-lg mb-4">
                        <p class="font-semibold mb-2">右大脳から左小脳への影響</p>
                        <p>大脳が運動の「計画」を立てると、その情報は<strong>対側（反対側）の小脳</strong>に送られ、動きがスムーズになるよう「調整」されます。したがって、右大脳の機能が低下すると、連携している<strong>左小脳</strong>への入力も減少し、結果として<strong>左小脳の機能低下</strong>を引き起こす可能性があります。</p>
                        <ul class="list-disc list-inside ml-4 mt-2 space-y-1">
                            <li><strong>左小脳の役割:</strong> 左小脳は<strong>同側（左側）</strong>の体の動きの微調整、バランス、協調運動を担います。</li>
                            <li><strong>左小脳機能低下による症状の悪化:</strong> 左小脳の機能が低下すると、ただでさえ弱っている左下肢の動きがさらにぎこちなくなり、協調性を失います。</li>
                        </ul>
                    </div>
                    <div class="bg-slate-50 p-4 rounded-lg">
                        <p class="font-semibold mb-2">右三半規管の機能低下</p>
                        <p>三半規管は、頭の回転や傾きを感知し、体のバランスを保つための重要なセンサーです。<strong>右三半規管の機能低下</strong>が起きると、空間認識や平衡感覚が乱れ、慢性的なめまい感やふらつきが生じます。体は常に不安定な状態に置かれ、これを補うために無意識に筋肉を緊張させます。</p>
                    </div>
                `,
                '3': `
                    <h3 class="text-xl font-bold text-teal-700 mb-4">3. 結果として生じる「右臀部の痛み」：代償作用の破綻</h3>
                    <p class="mb-4">ここまでの流れによって、あなたの体は<strong>左側全体が著しく不安定</strong>になります。まっすぐ立ったり歩いたりすることが困難になり、左側に倒れないように無意識のうちに体を支えようとします。このとき、体を支えるために最も重要な役割を果たすのが、<strong>安定している側の右半身</strong>です。</p>
                    <ol class="list-decimal list-inside space-y-2 bg-slate-50 p-4 rounded-lg">
                        <li><strong>重心の右側へのシフト:</strong> 体は無意識に重心を安定している右足側に移動させます。</li>
                        <li><strong>右臀部への過剰な負荷:</strong> 右足で体を支えるため、特に骨盤を安定させる役割を持つ<strong>右側の臀筋群（中殿筋など）</strong>や股関節周囲の筋肉が常に過剰に緊張し、酷使される状態になります。</li>
                        <li><strong>痛みの発生:</strong> この慢性的な過負荷が、筋肉の疲労、血行不良などを引き起こし、結果として<strong>右臀部の痛み</strong>として現れます。これは、神経の直接的な支配の問題ではなく、不安定な体を支えるための<strong>代償動作（compensation）</strong>が限界に達した結果生じる痛みと考えられます。</li>
                    </ol>
                `
            };
            
            document.getElementById('tab-content-1').innerHTML = detailedContent['1'];
            document.getElementById('tab-content-2').innerHTML = detailedContent['2'];
            document.getElementById('tab-content-3').innerHTML = detailedContent['3'];

            const hotspots = document.querySelectorAll('.hotspot');
            const contentTitle = document.getElementById('content-title');
            const contentText = document.getElementById('content-text');
            const contentDisplay = document.getElementById('content-display');

            hotspots.forEach(hotspot => {
                hotspot.addEventListener('click', () => {
                    const target = hotspot.dataset.target;
                    const data = contentData[target];

                    hotspots.forEach(h => h.classList.remove('active'));
                    hotspot.classList.add('active');
                    
                    contentDisplay.style.opacity = '0';
                    contentDisplay.style.transform = 'translateY(10px)';

                    setTimeout(() => {
                        contentTitle.textContent = data.title;
                        contentText.innerText = data.text;
                        contentDisplay.style.opacity = '1';
                        contentDisplay.style.transform = 'translateY(0)';
                    }, 300);
                });
            });

            const dominoSteps = document.querySelectorAll('.domino-step');
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        entry.target.classList.add('visible');
                    }
                });
            }, { threshold: 0.5 });

            dominoSteps.forEach(step => {
                observer.observe(step);
            });
            
            const tabButtons = document.querySelectorAll('.tab-button');
            const tabContents = document.querySelectorAll('.tab-content');

            tabButtons.forEach(button => {
                button.addEventListener('click', () => {
                    const tabId = button.dataset.tab;

                    tabButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');

                    tabContents.forEach(content => {
                        if (content.id === `tab-content-${tabId}`) {
                            content.classList.add('active');
                        } else {
                            content.classList.remove('active');
                        }
                    });
                });
            });
        });
    </script>

</body>
</html>
