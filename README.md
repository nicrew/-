<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>情感分析对比Demo</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.7.2/css/all.min.css" rel="stylesheet">
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.8/dist/chart.umd.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Tailwind配置 -->
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        primary: '#165DFF',
                        secondary: '#FF7D00',
                        neutral: {
                            100: '#F5F7FA',
                            200: '#E4E7ED',
                            300: '#C0C4CC',
                            400: '#909399',
                            500: '#606266',
                            600: '#303133',
                        },
                        positive: '#00B42A',
                        negative: '#F53F3F',
                        neutral_sentiment: '#86909C',
                    },
                    fontFamily: {
                        inter: ['Inter', 'sans-serif'],
                    },
                    boxShadow: {
                        'card': '0 4px 20px rgba(0, 0, 0, 0.08)',
                        'card-hover': '0 8px 30px rgba(0, 0, 0, 0.12)',
                    }
                },
            }
        }
    </script>
    
    <style type="text/tailwindcss">
        @layer utilities {
            .content-auto {
                content-visibility: auto;
            }
            .text-shadow {
                text-shadow: 0 2px 4px rgba(0,0,0,0.1);
            }
            .transition-custom {
                transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            }
            .gradient-bg {
                background: linear-gradient(135deg, #165DFF 0%, #0E42B3 100%);
            }
            .card-elevation {
                box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
                transition: transform 0.3s ease, box-shadow 0.3s ease;
            }
            .card-elevation:hover {
                transform: translateY(-4px);
                box-shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
            }
        }
    </style>
</head>
<body class="font-inter bg-neutral-100 min-h-screen flex flex-col">
    <!-- 顶部导航栏 -->
    <header class="gradient-bg text-white shadow-lg">
        <div class="container mx-auto px-4 py-4 flex justify-between items-center">
            <div class="flex items-center space-x-3">
                <i class="fa-solid fa-comment-dots text-2xl"></i>
                <h1 class="text-xl md:text-2xl font-bold">情感分析对比Demo</h1>
            </div>
            <nav class="hidden md:flex space-x-6">
                <a href="#overview" class="hover:text-neutral-200 transition-custom">概述</a>
                <a href="#demo" class="hover:text-neutral-200 transition-custom">演示</a>
                <a href="#comparison" class="hover:text-neutral-200 transition-custom">对比</a>
                <a href="#about" class="hover:text-neutral-200 transition-custom">关于</a>
            </nav>
            <button class="md:hidden text-white text-xl">
                <i class="fa-solid fa-bars"></i>
            </button>
        </div>
    </header>

    <main class="flex-grow container mx-auto px-4 py-8">
        <!-- 概述部分 -->
        <section id="overview" class="mb-12 scroll-mt-20">
            <div class="bg-white rounded-xl p-6 card-elevation">
                <h2 class="text-2xl font-bold text-neutral-600 mb-4 flex items-center">
                    <i class="fa-solid fa-info-circle text-primary mr-2"></i>
                    情感分析方法对比
                </h2>
                <div class="grid md:grid-cols-2 gap-6">
                    <div class="bg-neutral-100 rounded-lg p-5">
                        <h3 class="text-lg font-semibold text-primary mb-3 flex items-center">
                            <i class="fa-solid fa-code mr-2"></i>
                            传统NLP方法
                        </h3>
                        <p class="text-neutral-600">基于词典和规则的情感分析方法，使用情感词典和语法规则来判断文本的情感倾向。这种方法简单直接，不需要大量训练数据，但对语言表达的多样性和语境理解能力有限。</p>
                    </div>
                    <div class="bg-neutral-100 rounded-lg p-5">
                        <h3 class="text-lg font-semibold text-secondary mb-3 flex items-center">
                            <i class="fa-solid fa-robot mr-2"></i>
                            Sentence-Transformers方法
                        </h3>
                        <p class="text-neutral-600">基于预训练的句子嵌入模型，能够捕捉文本的语义信息和语境。这种方法利用深度学习技术，对文本的理解更深入，能够处理复杂的语言表达和语境，但需要更多的计算资源和训练数据。</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- 演示部分 -->
        <section id="demo" class="mb-12 scroll-mt-20">
            <div class="bg-white rounded-xl p-6 card-elevation">
                <h2 class="text-2xl font-bold text-neutral-600 mb-6 flex items-center">
                    <i class="fa-solid fa-play-circle text-primary mr-2"></i>
                    情感分析演示
                </h2>
                
                <div class="grid md:grid-cols-3 gap-6 mb-8">
                    <div class="md:col-span-2">
                        <label for="text-input" class="block text-sm font-medium text-neutral-600 mb-2">输入评论或文章（可输入多条，用换行分隔）</label>
                        <textarea id="text-input" rows="8" class="w-full px-4 py-3 border border-neutral-300 rounded-lg focus:ring-2 focus:ring-primary/50 focus:border-primary transition-custom" placeholder="请输入需要分析的文本..."></textarea>
                        <div class="flex justify-between mt-2 text-xs text-neutral-400">
                            <span id="char-count">0 字符</span>
                            <span id="line-count">0 条</span>
                        </div>
                    </div>
                    
                    <div class="flex flex-col justify-between">
                        <div>
                            <label class="block text-sm font-medium text-neutral-600 mb-2">选择分析方法</label>
                            <div class="space-y-3">
                                <label class="flex items-center space-x-2 p-3 border border-neutral-300 rounded-lg cursor-pointer hover:bg-neutral-50 transition-custom">
                                    <input type="radio" name="analysis-method" value="traditional" checked class="text-primary focus:ring-primary">
                                    <span class="font-medium">传统NLP方法</span>
                                </label>
                                <label class="flex items-center space-x-2 p-3 border border-neutral-300 rounded-lg cursor-pointer hover:bg-neutral-50 transition-custom">
                                    <input type="radio" name="analysis-method" value="transformers" class="text-secondary focus:ring-secondary">
                                    <span class="font-medium">Sentence-Transformers方法</span>
                                </label>
                                <label class="flex items-center space-x-2 p-3 border border-neutral-300 rounded-lg cursor-pointer hover:bg-neutral-50 transition-custom">
                                    <input type="radio" name="analysis-method" value="both" class="text-primary focus:ring-primary">
                                    <span class="font-medium">两种方法对比</span>
                                </label>
                            </div>
                        </div>
                        
                        <button id="analyze-btn" class="w-full py-3 px-4 bg-primary text-white rounded-lg font-medium hover:bg-primary/90 focus:ring-2 focus:ring-primary/50 focus:outline-none transition-custom flex items-center justify-center">
                            <i class="fa-solid fa-magic mr-2"></i>
                            开始分析
                        </button>
                    </div>
                </div>
                
                <!-- 结果展示区域 -->
                <div id="results-container" class="hidden">
                    <h3 class="text-xl font-semibold text-neutral-600 mb-4">分析结果</h3>
                    
                    <!-- 总体统计 -->
                    <div class="grid md:grid-cols-3 gap-4 mb-6">
                        <div class="bg-neutral-100 rounded-lg p-4 text-center">
                            <div class="text-sm text-neutral-500 mb-1">积极比例</div>
                            <div class="text-2xl font-bold" id="positive-percentage">0%</div>
                        </div>
                        <div class="bg-neutral-100 rounded-lg p-4 text-center">
                            <div class="text-sm text-neutral-500 mb-1">消极比例</div>
                            <div class="text-2xl font-bold" id="negative-percentage">0%</div>
                        </div>
                        <div class="bg-neutral-100 rounded-lg p-4 text-center">
                            <div class="text-sm text-neutral-500 mb-1">中性比例</div>
                            <div class="text-2xl font-bold" id="neutral-percentage">0%</div>
                        </div>
                    </div>
                    
                    <!-- 图表展示 -->
                    <div class="bg-neutral-100 rounded-lg p-4 mb-6">
                        <canvas id="sentiment-chart" height="250"></canvas>
                    </div>
                    
                    <!-- 详细结果 -->
                    <div id="results-list" class="space-y-4">
                        <!-- 结果项将通过JavaScript动态添加 -->
                    </div>
                </div>
            </div>
        </section>

        <!-- 对比部分 -->
        <section id="comparison" class="mb-12 scroll-mt-20">
            <div class="bg-white rounded-xl p-6 card-elevation">
                <h2 class="text-2xl font-bold text-neutral-600 mb-4 flex items-center">
                    <i class="fa-solid fa-balance-scale text-primary mr-2"></i>
                    方法对比
                </h2>
                
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-neutral-200">
                        <thead class="bg-neutral-100">
                            <tr>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-neutral-500 uppercase tracking-wider">标准</th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-neutral-500 uppercase tracking-wider">传统NLP方法</th>
                                <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-neutral-500 uppercase tracking-wider">Sentence-Transformers方法</th>
                            </tr>
                        </thead>
                        <tbody class="bg-white divide-y divide-neutral-200">
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-neutral-600">准确性</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">中等</span>
                                    </div>
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">较高</span>
                                    </div>
                                </td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-neutral-600">语境理解</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">有限</span>
                                    </div>
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">优秀</span>
                                    </div>
                                </td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-neutral-600">计算资源需求</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">低</span>
                                    </div>
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">高</span>
                                    </div>
                                </td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-neutral-600">训练数据需求</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">低</span>
                                    </div>
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">高</span>
                                    </div>
                                </td>
                            </tr>
                            <tr>
                                <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-neutral-600">实现难度</td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">简单</span>
                                    </div>
                                </td>
                                <td class="px-6 py-4 whitespace-nowrap text-sm text-neutral-600">
                                    <div class="flex items-center">
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <i class="fa-solid fa-star text-yellow-400"></i>
                                        <span class="ml-2">复杂</span>
                                    </div>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
                
                <div class="mt-6 p-4 bg-neutral-100 rounded-lg">
                    <h3 class="font-medium text-neutral-600 mb-2">适用场景</h3>
                    <ul class="space-y-2 text-neutral-600">
                        <li class="flex items-start">
                            <i class="fa-solid fa-check-circle text-primary mt-1 mr-2"></i>
                            <span><strong>传统NLP方法</strong>适用于资源有限、需要快速部署、对领域针对性要求高的场景，如简单的评论分析、特定领域的情感监测等。</span>
                        </li>
                        <li class="flex items-start">
                            <i class="fa-solid fa-check-circle text-secondary mt-1 mr-2"></i>
                            <span><strong>Sentence-Transformers方法</strong>适用于对准确性要求高、文本语义复杂、需要处理多语言或跨领域的场景，如社交媒体情感分析、客户反馈深度挖掘等。</span>
                        </li>
                    </ul>
                </div>
            </div>
        </section>

        <!-- 关于部分 -->
        <section id="about" class="scroll-mt-20">
            <div class="bg-white rounded-xl p-6 card-elevation">
                <h2 class="text-2xl font-bold text-neutral-600 mb-4 flex items-center">
                    <i class="fa-solid fa-info-circle text-primary mr-2"></i>
                    关于本Demo
                </h2>
                
                <div class="prose max-w-none">
                    <p class="text-neutral-600">本Demo展示了两种不同的情感分析方法：传统NLP方法和基于Sentence-Transformers的深度学习方法，并对它们的结果进行了对比。</p>
                    
                    <h3 class="text-lg font-semibold text-neutral-600 mt-4">技术细节</h3>
                    <ul class="list-disc pl-5 text-neutral-600 space-y-1">
                        <li><strong>传统NLP方法</strong>：使用情感词典和简单的词频统计方法进行情感分析</li>
                        <li><strong>Sentence-Transformers方法</strong>：使用预训练的sentence-transformers模型进行文本嵌入，然后通过分类器判断情感倾向</li>
                        <li><strong>界面框架</strong>：使用Tailwind CSS构建现代UI，Chart.js绘制统计图表</li>
                        <li><strong>交互设计</strong>：添加了平滑过渡动画、悬停效果和响应式布局</li>
                    </ul>
                    
                    <h3 class="text-lg font-semibold text-neutral-600 mt-4">局限性</h3>
                    <p class="text-neutral-600">本Demo为简化展示，存在以下局限性：</p>
                    <ul class="list-disc pl-5 text-neutral-600 space-y-1">
                        <li>传统方法使用简化的情感词典，可能无法覆盖所有词汇</li>
                        <li>Sentence-Transformers方法使用了预训练模型，未针对特定领域进行微调</li>
                        <li>未考虑文本中的讽刺、隐喻等复杂语言现象</li>
                        <li>性能优化未充分考虑，处理大量文本时可能较慢</li>
                    </ul>
                </div>
            </div>
        </section>
    </main>

    <footer class="bg-neutral-600 text-white py-8">
        <div class="container mx-auto px-4">
            <div class="grid md:grid-cols-3 gap-8">
                <div>
                    <h3 class="text-lg font-semibold mb-4">情感分析对比Demo</h3>
                    <p class="text-neutral-300 text-sm">本Demo展示了传统NLP方法和基于Sentence-Transformers的情感分析方法的对比，帮助用户理解不同方法的优缺点和适用场景。</p>
                </div>
                <div>
                    <h3 class="text-lg font-semibold mb-4">相关资源</h3>
                    <ul class="space-y-2 text-sm text-neutral-300">
                        <li><a href="#" class="hover:text-white transition-custom"><i class="fa-solid fa-book mr-2"></i>情感分析教程</a></li>
                        <li><a href="#" class="hover:text-white transition-custom"><i class="fa-solid fa-code mr-2"></i>Sentence-Transformers文档</a></li>
                        <li><a href="#" class="hover:text-white transition-custom"><i class="fa-solid fa-github mr-2"></i>GitHub代码仓库</a></li>
                    </ul>
                </div>
                <div>
                    <h3 class="text-lg font-semibold mb-4">联系我们</h3>
                    <ul class="space-y-2 text-sm text-neutral-300">
                        <li><a href="#" class="hover:text-white transition-custom"><i class="fa-solid fa-envelope mr-2"></i>邮箱</a></li>
                        <li><a href="#" class="hover:text-white transition-custom"><i class="fa-solid fa-twitter mr-2"></i>Twitter</a></li>
                        <li><a href="#" class="hover:text-white transition-custom"><i class="fa-solid fa-linkedin mr-2"></i>LinkedIn</a></li>
                    </ul>
                </div>
            </div>
            <div class="border-t border-neutral-500 mt-8 pt-6 text-center text-sm text-neutral-400">
                &copy; 2025 情感分析对比Demo | 保留所有权利
            </div>
        </div>
    </footer>

    <!-- JavaScript -->
    <script>
        // 文本输入统计
        const textInput = document.getElementById('text-input');
        const charCount = document.getElementById('char-count');
        const lineCount = document.getElementById('line-count');
        
        textInput.addEventListener('input', function() {
            charCount.textContent = `${this.value.length} 字符`;
            lineCount.textContent = `${this.value.split('\n').filter(line => line.trim() !== '').length} 条`;
        });
        
        // 分析按钮点击事件
        document.getElementById('analyze-btn').addEventListener('click', function() {
            const text = textInput.value.trim();
            if (!text) {
                alert('请输入需要分析的文本');
                return;
            }
            
            // 显示加载状态
            this.innerHTML = '<i class="fa-solid fa-spinner fa-spin mr-2"></i> 分析中...';
            this.disabled = true;
            
            // 模拟分析过程
            setTimeout(() => {
                // 获取选择的分析方法
                const analysisMethod = document.querySelector('input[name="analysis-method"]:checked').value;
                
                // 执行分析并显示结果
                analyzeText(text, analysisMethod);
                
                // 恢复按钮状态
                this.innerHTML = '<i class="fa-solid fa-magic mr-2"></i> 开始分析';
                this.disabled = false;
            }, 1500); // 模拟1.5秒的分析时间
        });
        
        // 文本分析函数
        function analyzeText(text, method) {
            // 按行分割文本
            const lines = text.split('\n').filter(line => line.trim() !== '');
            
            // 分析每条文本
            const results = lines.map(line => {
                let traditionalResult, transformersResult;
                
                // 传统NLP方法分析
                if (method === 'traditional' || method === 'both') {
                    traditionalResult = analyzeWithTraditionalNLP(line);
                }
                
                // Sentence-Transformers方法分析
                if (method === 'transformers' || method === 'both') {
                    transformersResult = analyzeWithTransformers(line);
                }
                
                return {
                    text: line,
                    traditional: traditionalResult,
                    transformers: transformersResult
                };
            });
            
            // 显示结果
            displayResults(results, method);
        }
        
        // 传统NLP方法分析
        function analyzeWithTraditionalNLP(text) {
            // 简化的情感词典
            const positiveWords = ['好', '棒', '优秀', '满意', '开心', '喜欢', '赞', '完美', '出色', '成功'];
            const negativeWords = ['坏', '差', '糟糕', '不满', '难过', '讨厌', '垃圾', '失败', '错误', '缺陷'];
            
            // 分词（简化处理，按空格和标点符号分割）
            const words = text.split(/[^\w\u4e00-\u9fa5]+/).filter(word => word !== '');
            
            // 计算情感得分
            let positiveCount = 0;
            let negativeCount = 0;
            
            words.forEach(word => {
                if (positiveWords.includes(word)) {
                    positiveCount++;
                } else if (negativeWords.includes(word)) {
                    negativeCount++;
                }
            });
            
            // 判断情感倾向
            let sentiment;
            if (positiveCount > negativeCount) {
                sentiment = 'positive';
            } else if (negativeCount > positiveCount) {
                sentiment = 'negative';
            } else {
                sentiment = 'neutral';
            }
            
            // 计算情感强度
            const totalCount = positiveCount + negativeCount;
            const intensity = totalCount > 0 ? (Math.abs(positiveCount - negativeCount) / totalCount) : 0;
            
            return {
                sentiment,
                intensity,
                positiveCount,
                negativeCount
            };
        }
        
        // Sentence-Transformers方法分析
        function analyzeWithTransformers(text) {
            // 模拟Sentence-Transformers模型的输出
            // 实际应用中，这里应该调用真实的模型API
            
            // 为了演示，我们根据文本长度和一些关键词来模拟模型输出
            const length = text.length;
            const hasPositive = text.includes('好') || text.includes('棒') || text.includes('赞') || text.includes('喜欢');
            const hasNegative = text.includes('坏') || text.includes('差') || text.includes('讨厌') || text.includes('垃圾');
            
            // 生成随机分数，但会受到关键词的影响
            let score = 0.5 + (Math.random() * 0.4 - 0.2); // 基础分数在0.3-0.7之间
            
            if (hasPositive) score += 0.2;
            if (hasNegative) score -= 0.2;
            
            // 限制分数范围
            score = Math.max(0, Math.min(1, score));
            
            // 判断情感倾向
            let sentiment;
            if (score > 0.6) {
                sentiment = 'positive';
            } else if (score < 0.4) {
                sentiment = 'negative';
            } else {
                sentiment = 'neutral';
            }
            
            return {
                sentiment,
                score,
                confidence: Math.random() * 0.3 + 0.7 // 置信度在0.7-1.0之间
            };
        }
        
        // 显示分析结果
        function displayResults(results, method) {
            const resultsContainer = document.getElementById('results-container');
            resultsContainer.classList.remove('hidden');
            
            // 计算总体统计
            const stats = calculateStats(results, method);
            
            // 更新统计数据
            document.getElementById('positive-percentage').textContent = `${stats.positivePercentage}%`;
            document.getElementById('negative-percentage').textContent = `${stats.negativePercentage}%`;
            document.getElementById('neutral-percentage').textContent = `${stats.neutralPercentage}%`;
            
            // 绘制图表
            drawChart(stats);
            
            // 显示详细结果
            const resultsList = document.getElementById('results-list');
            resultsList.innerHTML = '';
            
            results.forEach((result, index) => {
                const resultItem = document.createElement('div');
                resultItem.className = 'bg-neutral-50 rounded-lg p-4 border border-neutral-200 transition-custom hover:shadow-md';
                
                // 构建结果项HTML
                let html = `
                    <div class="flex justify-between items-start mb-3">
                        <h4 class="font-medium text-neutral-600">文本 ${index + 1}</h4>
                        <span class="text-xs px-2 py-1 rounded-full ${getBadgeClass(result.traditional?.sentiment || result.transformers?.sentiment)}">
                            ${getSentimentLabel(result.traditional?.sentiment || result.transformers?.sentiment)}
                        </span>
                    </div>
                    <p class="text-neutral-600 mb-3">${result.text}</p>
                `;
                
                if (method === 'both') {
                    // 显示两种方法的对比
                    html += `
                        <div class="grid grid-cols-2 gap-4">
                            <div class="bg-white p-3 rounded-lg border border-neutral-200">
                                <div class="text-sm font-medium text-primary mb-1">传统NLP方法</div>
                                <div class="flex items-center">
                                    <span class="inline-block w-3 h-3 rounded-full mr-2 ${getColorClass(result.traditional.sentiment)}"></span>
                                    <span class="text-sm">${getSentimentLabel(result.traditional.sentiment)}</span>
                                    <span class="ml-auto text-xs text-neutral-500">强度: ${result.traditional.intensity.toFixed(2)}</span>
                                </div>
                            </div>
                            <div class="bg-white p-3 rounded-lg border border-neutral-200">
                                <div class="text-sm font-medium text-secondary mb-1">Sentence-Transformers方法</div>
                                <div class="flex items-center">
                                    <span class="inline-block w-3 h-3 rounded-full mr-2 ${getColorClass(result.transformers.sentiment)}"></span>
                                    <span class="text-sm">${getSentimentLabel(result.transformers.sentiment)}</span>
                                    <span class="ml-auto text-xs text-neutral-500">分数: ${result.transformers.score.toFixed(2)}</span>
                                </div>
                            </div>
                        </div>
                    `;
                } else {
                    // 只显示一种方法的结果
                    const resultData = method === 'traditional' ? result.traditional : result.transformers;
                    const methodName = method === 'traditional' ? '传统NLP方法' : 'Sentence-Transformers方法';
                    const methodColor = method === 'traditional' ? 'text-primary' : 'text-secondary';
                    
                    html += `
                        <div class="bg-white p-3 rounded-lg border border-neutral-200">
                            <div class="text-sm font-medium ${methodColor} mb-1">${methodName}</div>
                            <div class="flex items-center">
                                <span class="inline-block w-3 h-3 rounded-full mr-2 ${getColorClass(resultData.sentiment)}"></span>
                                <span class="text-sm">${getSentimentLabel(resultData.sentiment)}</span>
                                ${method === 'traditional' ? `<span class="ml-auto text-xs text-neutral-500">强度: ${resultData.intensity.toFixed(2)}</span>` : `<span class="ml-auto text-xs text-neutral-500">分数: ${resultData.score.toFixed(2)}</span>`}
                            </div>
                        </div>
                    `;
                }
                
                resultItem.innerHTML = html;
                resultsList.appendChild(resultItem);
            });
            
            // 平滑滚动到结果区域
            resultsContainer.scrollIntoView({ behavior: 'smooth' });
        }
        
        // 计算总体统计数据
        function calculateStats(results, method) {
            let positiveCount = 0;
            let negativeCount = 0;
            let neutralCount = 0;
            
            results.forEach(result => {
                const sentiment = method === 'traditional' ? result.traditional.sentiment : 
                                 method === 'transformers' ? result.transformers.sentiment :
                                 // 如果是两种方法对比，优先使用transformers的结果
                                 result.transformers.sentiment;
                
                switch (sentiment) {
                    case 'positive':
                        positiveCount++;
                        break;
                    case 'negative':
                        negativeCount++;
                        break;
                    case 'neutral':
                        neutralCount++;
                        break;
                }
            });
            
            const total = results.length;
            const positivePercentage = Math.round((positiveCount / total) * 100);
            const negativePercentage = Math.round((negativeCount / total) * 100);
            const neutralPercentage = Math.round((neutralCount / total) * 100);
            
            return {
                positiveCount,
                negativeCount,
                neutralCount,
                positivePercentage,
                negativePercentage,
                neutralPercentage
            };
        }
        
        // 绘制情感分布图表
        function drawChart(stats) {
            const ctx = document.getElementById('sentiment-chart').getContext('2d');
            
            // 销毁已存在的图表
            if (window.sentimentChart) {
                window.sentimentChart.destroy();
            }
            
            // 创建新图表
            window.sentimentChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['积极', '消极', '中性'],
                    datasets: [{
                        data: [stats.positivePercentage, stats.negativePercentage, stats.neutralPercentage],
                        backgroundColor: [
                            '#00B42A', // 积极 - 绿色
                            '#F53F3F', // 消极 - 红色
                            '#86909C'  // 中性 - 灰色
                        ],
                        borderWidth: 0,
                        hoverOffset: 4
                    }]
                },
                options: {
                    responsive: true,
                    plugins: {
                        legend: {
                            position: 'right',
                            labels: {
                                padding: 20,
                                usePointStyle: true,
                                pointStyle: 'circle'
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `${context.label}: ${context.raw}%`;
                                }
                            }
                        }
                    },
                    cutout: '65%',
                    animation: {
                        animateScale: true,
                        animateRotate: true
                    }
                }
            });
        }
        
        // 辅助函数：获取情感标签
        function getSentimentLabel(sentiment) {
            switch (sentiment) {
                case 'positive':
                    return '积极';
                case 'negative':
                    return '消极';
                case 'neutral':
                    return '中性';
                default:
                    return '';
            }
        }
        
        // 辅助函数：获取徽章样式
        function getBadgeClass(sentiment) {
            switch (sentiment) {
                case 'positive':
                    return 'bg-positive/10 text-positive';
                case 'negative':
                    return 'bg-negative/10 text-negative';
                case 'neutral':
                    return 'bg-neutral_sentiment/10 text-neutral_sentiment';
                default:
                    return '';
            }
        }
        
        // 辅助函数：获取颜色样式
        function getColorClass(sentiment) {
            switch (sentiment) {
                case 'positive':
                    return 'bg-positive';
                case 'negative':
                    return 'bg-negative';
                case 'neutral':
                    return 'bg-neutral_sentiment';
                default:
                    return '';
            }
        }
        
        // 平滑滚动
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                
                const targetId = this.getAttribute('href');
                const targetElement = document.querySelector(targetId);
                
                if (targetElement) {
                    targetElement.scrollIntoView({
                        behavior: 'smooth'
                    });
                }
            });
        });
        
        // 添加示例文本
        const sampleText = `这个产品真的很棒，我非常喜欢！
客服态度很好，解决问题很及时。
这个手机的电池续航太差了，非常不满意。
物流速度很快，包装也很精美。
质量一般，没有达到我的期望。
中性评价，没有特别的亮点也没有明显的缺点。
这个餐厅的环境和服务都很好，推荐大家来尝试。
价格偏高，性价比不是很高。`;
        
        // 填充示例文本
        document.getElementById('text-input').value = sampleText;
        charCount.textContent = `${sampleText.length} 字符`;
        lineCount.textContent = `${sampleText.split('\n').length} 条`;
    </script>
</body>
</html>    
