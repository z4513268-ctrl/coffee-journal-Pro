# coffee-journal-Pro
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>咖啡手记 · 完整移动端</title>
    <style>
        /* ===== 全局重置 ===== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "Helvetica Neue", sans-serif;
            background: #1a1410;
            height: 100vh;
            height: 100dvh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            color: #f0e3d8;
            user-select: none;
        }

        /* ===== 主内容区域 ===== */
        .main-content {
            flex: 1;
            padding: 16px 16px 100px;
            overflow-y: auto;
            scroll-behavior: smooth;
            position: relative;
        }

        /* ===== 面板系统 ===== */
        .panel {
            display: none;
            animation: fadeSlide 0.3s cubic-bezier(0.32, 0.72, 0, 1);
        }
        .panel.active {
            display: block;
        }

        @keyframes fadeSlide {
            from {
                opacity: 0;
                transform: translateY(12px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* ===== 页面头部 ===== */
        .page-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 4px 0 18px;
        }
        .page-header h1 {
            font-size: 24px;
            font-weight: 700;
            background: linear-gradient(135deg, #f5e3d4, #c89a78);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        .page-header .date {
            font-size: 13px;
            color: #8a7365;
            background: #2c1e16;
            padding: 6px 14px;
            border-radius: 20px;
            border: 1px solid #3d2b20;
            flex-shrink: 0;
            -webkit-text-fill-color: #8a7365;
        }

        /* ===== 卡片组件 ===== */
        .mock-card {
            background: linear-gradient(145deg, #2c1e16, #221811);
            border-radius: 16px;
            padding: 16px 16px;
            margin-bottom: 12px;
            border: 1px solid rgba(255, 215, 180, 0.05);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
            display: flex;
            align-items: center;
            gap: 14px;
            transition: transform 0.15s ease;
        }
        .mock-card:active {
            transform: scale(0.98);
        }
        .mock-card .avatar {
            width: 48px;
            height: 48px;
            border-radius: 12px;
            background: #4d3426;
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 22px;
        }
        .mock-card .info {
            flex: 1;
            min-width: 0;
        }
        .mock-card .info .title {
            font-size: 15px;
            font-weight: 600;
            color: #f0e3d8;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .mock-card .info .desc {
            font-size: 13px;
            color: #a68979;
            margin-top: 2px;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .mock-card .badge {
            background: #4d3426;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 11px;
            color: #cbb09b;
            border: 1px solid #6b5343;
            flex-shrink: 0;
        }
        .mock-card .badge.highlight {
            color: #c89a78;
            border-color: #c89a78;
        }

        /* ===== 面板专属样式 ===== */

        /* 人格面板 - 进度环 */
        .personality-ring {
            text-align: center;
            padding: 20px 0 30px;
        }
        .personality-ring .big-icon {
            font-size: 72px;
            display: block;
            margin-bottom: 12px;
        }
        .personality-ring .title {
            font-size: 20px;
            font-weight: 600;
            color: #f0e3d8;
        }
        .personality-ring .sub {
            font-size: 14px;
            color: #8a7365;
            margin-top: 4px;
        }
        .progress-track {
            width: 80%;
            max-width: 280px;
            height: 6px;
            background: #2c1e16;
            border-radius: 6px;
            margin: 16px auto 0;
            overflow: hidden;
        }
        .progress-track .bar {
            height: 100%;
            background: linear-gradient(90deg, #c89a78, #f0d5c0);
            border-radius: 6px;
            transition: width 0.6s ease;
        }

        /* 同好面板 - 匹配卡片特殊样式 */
        .match-tag {
            background: rgba(200, 154, 120, 0.12);
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 11px;
            color: #c89a78;
            border: 1px solid rgba(200, 154, 120, 0.2);
            flex-shrink: 0;
        }

        /* 报告面板 - 统计块 */
        .stat-grid {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 10px;
            margin-bottom: 16px;
        }
        .stat-item {
            background: #2c1e16;
            border-radius: 14px;
            padding: 16px 8px;
            text-align: center;
            border: 1px solid #3d2b20;
        }
        .stat-item .num {
            font-size: 22px;
            font-weight: 700;
            color: #f0e3d8;
        }
        .stat-item .label {
            font-size: 11px;
            color: #8a7365;
            margin-top: 4px;
        }

        .insight-card {
            background: #2c1e16;
            border-radius: 14px;
            padding: 16px 18px;
            border: 1px solid #3d2b20;
            margin-bottom: 12px;
        }
        .insight-card .row {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px solid rgba(255, 215, 180, 0.04);
        }
        .insight-card .row:last-child {
            border-bottom: none;
        }
        .insight-card .row .key {
            color: #8a7365;
            font-size: 14px;
        }
        .insight-card .row .value {
            color: #f0e3d8;
            font-size: 14px;
            font-weight: 500;
        }

        .bottom-hint {
            text-align: center;
            color: #4d382b;
            font-size: 12px;
            padding: 16px 0 8px;
            letter-spacing: 0.5px;
        }

        /* ===== 底部导航栏 ===== */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            height: 82px;
            padding-bottom: env(safe-area-inset-bottom, 12px);
            background: rgba(26, 20, 16, 0.75);
            backdrop-filter: blur(24px) saturate(180%);
            -webkit-backdrop-filter: blur(24px) saturate(180%);
            border-top: 0.5px solid rgba(255, 215, 180, 0.08);
            display: flex;
            align-items: flex-start;
            justify-content: space-around;
            z-index: 100;
            box-shadow: 0 -10px 40px rgba(0, 0, 0, 0.6);
        }

        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: flex-end;
            padding-top: 10px;
            flex: 1;
            height: 100%;
            cursor: pointer;
            transition: all 0.15s ease;
            position: relative;
            border: none;
            background: transparent;
            color: #6b5546;
            font-family: inherit;
            font-size: 10px;
            letter-spacing: 0.3px;
            gap: 1px;
            -webkit-touch-callout: none;
        }
        .nav-item .icon {
            font-size: 24px;
            line-height: 1.2;
            transition: transform 0.2s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        .nav-item .label {
            font-size: 10px;
            font-weight: 500;
            opacity: 0.7;
        }
        .nav-item.active {
            color: #f0d5c0;
        }
        .nav-item.active .icon {
            transform: scale(1.05);
        }
        .nav-item:active {
            opacity: 0.5;
        }

        .nav-item .dot {
            width: 4px;
            height: 4px;
            border-radius: 50%;
            background: #c89a78;
            margin-top: 2px;
            opacity: 0;
            transition: opacity 0.2s;
        }
        .nav-item.active .dot {
            opacity: 1;
        }

        /* ===== FAB 凸起按钮 ===== */
        .nav-item.fab {
            position: relative;
            top: -18px;
            flex: 0 0 68px;
            height: 68px;
            background: linear-gradient(145deg, #c89a78, #a87b5e);
            border-radius: 50%;
            box-shadow: 0 8px 30px rgba(200, 154, 120, 0.35), inset 0 1px 0 rgba(255, 255, 255, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 0;
            border: none;
            color: #fff;
            transition: all 0.2s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        .nav-item.fab .icon {
            font-size: 32px;
            line-height: 1;
            filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.2));
        }
        .nav-item.fab .label {
            display: none;
        }
        .nav-item.fab:active {
            transform: scale(0.92);
            box-shadow: 0 4px 15px rgba(200, 154, 120, 0.2);
        }
        .nav-item.fab::before {
            content: '';
            position: absolute;
            inset: -6px;
            border-radius: 50%;
            background: radial-gradient(circle at center, rgba(200, 154, 120, 0.25), transparent 70%);
            z-index: -1;
            animation: fabPulse 3s ease-in-out infinite;
        }
        @keyframes fabPulse {
            0%, 100% { transform: scale(1); opacity: 0.5; }
            50% { transform: scale(1.15); opacity: 0.15; }
        }

        /* ===== 记录浮层 ===== */
        .record-overlay {
            position: fixed;
            inset: 0;
            background: rgba(10, 6, 4, 0.92);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            z-index: 200;
            display: none;
            flex-direction: column;
            justify-content: flex-end;
            animation: slideUp 0.35s cubic-bezier(0.32, 0.72, 0, 1);
        }
        .record-overlay.open {
            display: flex;
        }
        @keyframes slideUp {
            from { transform: translateY(100%); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }

        .record-sheet {
            background: linear-gradient(180deg, #2c1e16, #1a1410);
            border-radius: 32px 32px 0 0;
            padding: 20px 20px 34px;
            max-height: 85vh;
            overflow-y: auto;
            border-top: 1px solid rgba(255, 215, 180, 0.06);
        }
        .record-sheet .handle {
            width: 40px;
            height: 4px;
            background: #4d382b;
            border-radius: 4px;
            margin: 0 auto 18px;
        }
        .record-sheet h2 {
            font-size: 22px;
            font-weight: 600;
            color: #f5e3d4;
        }
        .record-sheet .sub {
            font-size: 14px;
            color: #8a7365;
            margin-bottom: 20px;
        }

        .mock-wheel {
            width: 200px;
            height: 200px;
            border-radius: 50%;
            background: radial-gradient(circle at 40% 35%, #4d3426, #1a100c);
            margin: 0 auto 20px;
            box-shadow: inset 0 0 60px rgba(0, 0, 0, 0.6), 0 10px 40px rgba(0, 0, 0, 0.4);
            display: flex;
            align-items: center;
            justify-content: center;
            border: 1px solid #3d2b20;
            position: relative;
        }
        .mock-wheel .dot {
            width: 16px;
            height: 16px;
            background: #f0d5c0;
            border-radius: 50%;
            box-shadow: 0 0 30px rgba(240, 213, 192, 0.3);
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }
        .mock-wheel .axis-label {
            position: absolute;
            color: rgba(200, 180, 165, 0.25);
            font-size: 12px;
            font-weight: 500;
        }
        .mock-wheel .axis-label.t { top: 12px; left: 50%; transform: translateX(-50%); }
        .mock-wheel .axis-label.b { bottom: 12px; left: 50%; transform: translateX(-50%); }
        .mock-wheel .axis-label.l { left: 12px; top: 50%; transform: translateY(-50%); }
        .mock-wheel .axis-label.r { right: 12px; top: 50%; transform: translateY(-50%); }

        .tag-scroll {
            display: flex;
            gap: 10px;
            overflow-x: auto;
            padding: 4px 0 18px;
            scrollbar-width: none;
            -webkit-overflow-scrolling: touch;
        }
        .tag-scroll::-webkit-scrollbar {
            display: none;
        }
        .tag-scroll .tag {
            background: #35261d;
            padding: 8px 20px;
            border-radius: 30px;
            color: #cbb09b;
            font-size: 14px;
            border: 1px solid #4d382b;
            white-space: nowrap;
            flex-shrink: 0;
            transition: all 0.15s;
        }
        .tag-scroll .tag:active {
            background: #4d382b;
            transform: scale(0.94);
        }
        .tag-scroll .tag.active {
            background: #4d3426;
            border-color: #c89a78;
            color: #f0d5c0;
        }

        .record-actions {
            display: flex;
            gap: 12px;
            margin-top: 4px;
        }
        .record-actions .btn {
            flex: 1;
            padding: 16px;
            border-radius: 16px;
            border: none;
            font-size: 17px;
            font-weight: 600;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.15s;
        }
        .record-actions .btn-cancel {
            background: #2c1e16;
            color: #8a7365;
            border: 1px solid #3d2b20;
        }
        .record-actions .btn-cancel:active {
            transform: scale(0.96);
            background: #3d2b20;
        }
        .record-actions .btn-done {
            background: linear-gradient(145deg, #c89a78, #a87b5e);
            color: #fff;
            box-shadow: 0 6px 24px rgba(200, 154, 120, 0.25);
        }
        .record-actions .btn-done:active {
            transform: scale(0.96);
            box-shadow: 0 3px 12px rgba(200, 154, 120, 0.15);
        }
        .record-actions .btn-done.success {
            background: #4CAF50;
            box-shadow: 0 6px 24px rgba(76, 175, 80, 0.3);
        }
    </style>
</head>
<body>

    <!-- ========================================================= -->
    <!-- 主内容区域 -->
    <!-- ========================================================= -->
    <div class="main-content" id="mainContent">

        <!-- ===== 面板：首页 ===== -->
        <div class="panel active" id="panel-home">
            <div class="page-header">
                <h1>☕ 今日心境</h1>
                <span class="date">6月21日</span>
            </div>

            <div class="mock-card">
                <div class="avatar">🌺</div>
                <div class="info">
                    <div class="title">埃塞·花魁 日晒</div>
                    <div class="desc">明亮果酸 · 花香 · 茶感</div>
                </div>
                <span class="badge">5.0 ⭐</span>
            </div>

            <div class="mock-card">
                <div class="avatar">🍫</div>
                <div class="info">
                    <div class="title">哥伦比亚·蕙兰</div>
                    <div class="desc">黑巧 · 坚果 · 醇厚</div>
                </div>
                <span class="badge">4.5 ⭐</span>
            </div>

            <div class="mock-card">
                <div class="avatar">🍇</div>
                <div class="info">
                    <div class="title">肯尼亚·AA 水洗</div>
                    <div class="desc">莓果 · 红酒韵 · 明亮</div>
                </div>
                <span class="badge">4.8 ⭐</span>
            </div>

            <div class="mock-card">
                <div class="avatar">🌰</div>
                <div class="info">
                    <div class="title">印尼·曼特宁</div>
                    <div class="desc">草本 · 醇厚 · 低酸</div>
                </div>
                <span class="badge">4.2 ⭐</span>
            </div>

            <div class="mock-card">
                <div class="avatar">🌹</div>
                <div class="info">
                    <div class="title">瑰夏·绿标</div>
                    <div class="desc">茉莉花香 · 柑橘 · 蜂蜜</div>
                </div>
                <span class="badge">4.9 ⭐</span>
            </div>

            <div class="bottom-hint">— 已经到底了 —</div>
        </div>

        <!-- ===== 面板：人格 ===== -->
        <div class="panel" id="panel-personality">
            <div class="page-header">
                <h1>🧠 咖啡人格</h1>
                <span class="date">进化中</span>
            </div>

            <div class="personality-ring">
                <span class="big-icon">🌱</span>
                <div class="title">风味探险家 · 萌芽期</div>
                <div class="sub">记录满 10 杯解锁完整人格报告</div>
                <div class="progress-track">
                    <div class="bar" style="width:50%;"></div>
                </div>
                <div style="margin-top:8px; font-size:13px; color:#8a7365;">进度 5 / 10 杯</div>
            </div>

            <div style="background:#2c1e16; border-radius:14px; padding:16px; border:1px solid #3d2b20; margin-bottom:12px;">
                <div style="display:flex; justify-content:space-between; align-items:center;">
                    <span style="color:#8a7365; font-size:14px;">已解锁标签</span>
                    <span style="color:#c89a78; font-size:13px;">+3 杯解锁新标签</span>
                </div>
                <div style="display:flex; flex-wrap:wrap; gap:8px; margin-top:12px;">
                    <span style="background:#4d3426; padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid #6b5343; color:#f0e3d8;">🌸 花香</span>
                    <span style="background:#4d3426; padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid #6b5343; color:#f0e3d8;">🍊 果酸</span>
                    <span style="background:#4d3426; padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid #6b5343; color:#8a7365;">⬜ 待解锁</span>
                    <span style="background:#4d3426; padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid #6b5343; color:#8a7365;">⬜ 待解锁</span>
                </div>
            </div>

            <div style="background:linear-gradient(135deg, #2c1e16, #1f1510); border-radius:14px; padding:16px; border:1px solid #3d2b20; text-align:center;">
                <span style="font-size:28px;">🔮</span>
                <p style="font-size:14px; color:#8a7365; margin-top:4px;">每 10 杯进化一次 · 下一个形态：<span style="color:#f0e3d8;">「风味收藏家」</span></p>
            </div>

            <div class="bottom-hint">— 持续记录，解锁更多 —</div>
        </div>

        <!-- ===== 面板：同好 ===== -->
        <div class="panel" id="panel-social">
            <div class="page-header">
                <h1>🌊 同好漂流瓶</h1>
                <span class="date">今日匹配</span>
            </div>

            <div style="background:linear-gradient(135deg, #2c1e16, #1f1510); border-radius:14px; padding:14px 16px; border:1px solid #3d2b20; margin-bottom:16px; display:flex; align-items:center; gap:12px;">
                <span style="font-size:28px;">🧩</span>
                <div>
                    <div style="font-size:14px; color:#f0e3d8; font-weight:500;">基于风味相似度匹配</div>
                    <div style="font-size:12px; color:#8a7365;">找到 3 位口味相近的咖友</div>
                </div>
            </div>

            <div class="mock-card">
                <div class="avatar">🌸</div>
                <div class="info">
                    <div class="title">@咖啡探险家</div>
                    <div class="desc">🏆 共同偏好：花香 · 果酸</div>
                </div>
                <span class="match-tag">92% 匹配</span>
            </div>

            <div class="mock-card">
                <div class="avatar">🌿</div>
                <div class="info">
                    <div class="title">@风味猎人</div>
                    <div class="desc">🏆 共同偏好：发酵感 · 醇厚</div>
                </div>
                <span class="match-tag">87% 匹配</span>
            </div>

            <div class="mock-card">
                <div class="avatar">🍃</div>
                <div class="info">
                    <div class="title">@手冲星人</div>
                    <div class="desc">🏆 共同偏好：茶感 · 干净</div>
                </div>
                <span class="match-tag">81% 匹配</span>
            </div>

            <div style="text-align:center; padding:12px 0 4px;">
                <span style="background:#4d3426; padding:10px 28px; border-radius:30px; color:#c89a78; font-size:14px; border:1px solid #6b5343; display:inline-block;">📨 发送「隔空共饮」邀请</span>
            </div>

            <div class="bottom-hint">— 基于 8 维风味向量匹配 —</div>
        </div>

        <!-- ===== 面板：报告 ===== -->
        <div class="panel" id="panel-report">
            <div class="page-header">
                <h1>📊 风味报告</h1>
                <span class="date">6 月趋势</span>
            </div>

            <div class="stat-grid">
                <div class="stat-item">
                    <div class="num">12</div>
                    <div class="label">本月杯数</div>
                </div>
                <div class="stat-item">
                    <div class="num">6</div>
                    <div class="label">探索产地</div>
                </div>
                <div class="stat-item">
                    <div class="num">4.7</div>
                    <div class="label">平均评分</div>
                </div>
            </div>

            <div class="insight-card">
                <div class="row">
                    <span class="key">🌍 最爱产区</span>
                    <span class="value">埃塞俄比亚</span>
                </div>
                <div class="row">
                    <span class="key">🏷️ 风味标签</span>
                    <span class="value">花香 · 果酸 · 茶感</span>
                </div>
                <div class="row">
                    <span class="key">🔥 热门处理法</span>
                    <span class="value">日晒 (60%)</span>
                </div>
                <div class="row">
                    <span class="key">📈 本月趋势</span>
                    <span class="value" style="color:#4CAF50;">↑ 偏好向浅烘偏移</span>
                </div>
            </div>

            <div style="background:#2c1e16; border-radius:14px; padding:16px; border:1px solid #3d2b20; text-align:center;">
                <span style="font-size:32px;">📅</span>
                <p style="font-size:14px; color:#8a7365; margin-top:4px;">完整年度报告 · 每月 1 号解锁</p>
                <div style="width:100%; height:4px; background:#1f1510; border-radius:4px; margin-top:12px; overflow:hidden;">
                    <div style="width:45%; height:100%; background:linear-gradient(90deg,#c89a78,#f0d5c0); border-radius:4px;"></div>
                </div>
                <div style="display:flex; justify-content:space-between; font-size:11px; color:#4d382b; margin-top:4px;">
                    <span>1月</span>
                    <span>6月</span>
                    <span>12月</span>
                </div>
            </div>

            <div class="bottom-hint">— 数据每日更新 —</div>
        </div>
    </div>

    <!-- ========================================================= -->
    <!-- 底部导航栏 -->
    <!-- ========================================================= -->
    <nav class="bottom-nav" id="bottomNav">
        <button class="nav-item active" data-tab="home" aria-label="首页">
            <span class="icon">🏠</span>
            <span class="label">首页</span>
            <span class="dot"></span>
        </button>

        <button class="nav-item" data-tab="personality" aria-label="咖啡人格">
            <span class="icon">🧠</span>
            <span class="label">人格</span>
            <span class="dot"></span>
        </button>

        <button class="nav-item fab" id="fabButton" aria-label="记录咖啡">
            <span class="icon">☕</span>
            <span class="label">记录</span>
        </button>

        <button class="nav-item" data-tab="social" aria-label="同好">
            <span class="icon">🌊</span>
            <span class="label">同好</span>
            <span class="dot"></span>
        </button>

        <button class="nav-item" data-tab="report" aria-label="报告">
            <span class="icon">📊</span>
            <span class="label">报告</span>
            <span class="dot"></span>
        </button>
    </nav>

    <!-- ========================================================= -->
    <!-- 记录浮层 -->
    <!-- ========================================================= -->
    <div class="record-overlay" id="recordOverlay">
        <div class="record-sheet">
            <div class="handle"></div>

            <h2>☕ 冲煮手记</h2>
            <div class="sub">滑动轮盘 · 找到你的风味</div>

            <!-- 模拟风味轮盘 -->
            <div class="mock-wheel">
                <span class="axis-label t">干净</span>
                <span class="axis-label b">醇厚</span>
                <span class="axis-label l">酸</span>
                <span class="axis-label r">苦</span>
                <div class="dot"></div>
            </div>

            <!-- 快捷标签（横向滚动） -->
            <div class="tag-scroll">
                <span class="tag active">浅烘</span>
                <span class="tag">中烘</span>
                <span class="tag">深烘</span>
                <span class="tag">水洗</span>
                <span class="tag">日晒</span>
                <span class="tag">蜜处理</span>
                <span class="tag">自烘豆</span>
                <span class="tag">商业豆</span>
            </div>

            <!-- 操作按钮 -->
            <div class="record-actions">
                <button class="btn btn-cancel" id="cancelRecord">取消</button>
                <button class="btn btn-done" id="doneRecord">☕ 完成记录</button>
            </div>
        </div>
    </div>

    <!-- ========================================================= -->
    <!-- JavaScript -->
    <!-- ========================================================= -->
    <script>
        (function() {
            'use strict';

            // ---- DOM 引用 ----
            const navItems = document.querySelectorAll('.nav-item:not(.fab)');
            const panels = {
                home: document.getElementById('panel-home'),
                personality: document.getElementById('panel-personality'),
                social: document.getElementById('panel-social'),
                report: document.getElementById('panel-report')
            };
            const fab = document.getElementById('fabButton');
            const overlay = document.getElementById('recordOverlay');
            const cancelBtn = document.getElementById('cancelRecord');
            const doneBtn = document.getElementById('doneRecord');
            const mainContent = document.getElementById('mainContent');

            // ---- Tab 切换逻辑 ----
            function switchTab(tabName) {
                // 1. 更新导航高亮
                navItems.forEach(item => {
                    item.classList.remove('active');
                    if (item.getAttribute('data-tab') === tabName) {
                        item.classList.add('active');
                    }
                });

                // 2. 切换面板
                Object.keys(panels).forEach(key => {
                    panels[key].classList.remove('active');
                });
                if (panels[tabName]) {
                    panels[tabName].classList.add('active');
                }

                // 3. 滚动到顶部
                mainContent.scrollTop = 0;

                // 4. 触感反馈
                if (navigator.vibrate) navigator.vibrate(5);
            }

            // ---- 绑定导航点击 ----
            navItems.forEach(item => {
                item.addEventListener('click', function() {
                    const tab = this.getAttribute('data-tab');
                    if (tab) switchTab(tab);
                });
            });

            // ---- FAB 弹出记录浮层 ----
            fab.addEventListener('click', function(e) {
                e.preventDefault();
                overlay.classList.add('open');
                if (navigator.vibrate) navigator.vibrate(15);
                mainContent.style.transition = 'transform 0.3s ease';
                mainContent.style.transform = 'scale(0.96)';
            });

            // ---- 关闭浮层 ----
            function closeOverlay() {
                overlay.classList.remove('open');
                mainContent.style.transform = 'scale(1)';
                if (navigator.vibrate) navigator.vibrate(8);

                // 重置完成按钮状态
                const btn = doneBtn;
                btn.textContent = '☕ 完成记录';
                btn.className = 'btn btn-done';
            }

            cancelBtn.addEventListener('click', closeOverlay);

            // 点击背景关闭
            overlay.addEventListener('click', function(e) {
                if (e.target === overlay) closeOverlay();
            });

            // ---- 完成记录（模拟保存） ----
            doneBtn.addEventListener('click', function() {
                if (this.classList.contains('success')) return;

                if (navigator.vibrate) navigator.vibrate([15, 50, 15]);

                this.textContent = '✅ 已记录！';
                this.className = 'btn btn-done success';

                setTimeout(() => {
                    closeOverlay();
                    // 重置按钮状态（延迟重置，避免闪烁）
                    setTimeout(() => {
                        this.textContent = '☕ 完成记录';
                        this.className = 'btn btn-done';
                    }, 300);
                }, 700);
            });

            // ---- 向下滑动关闭浮层（触屏手势） ----
            let startY = 0;
            overlay.addEventListener('touchstart', function(e) {
                const touch = e.touches[0];
                startY = touch.clientY;
            }, { passive: true });

            overlay.addEventListener('touchmove', function(e) {
                const touch = e.touches[0];
                const deltaY = touch.clientY - startY;
                if (deltaY > 60 && overlay.classList.contains('open')) {
                    closeOverlay();
                }
            }, { passive: true });

            // ---- 键盘 ESC 关闭（PC调试用） ----
            document.addEventListener('keydown', function(e) {
                if (e.key === 'Escape' && overlay.classList.contains('open')) {
                    closeOverlay();
                }
            });

            // ---- 快捷标签点击切换（仅视觉演示） ----
            document.querySelectorAll('.tag-scroll .tag').forEach(tag => {
                tag.addEventListener('click', function() {
                    document.querySelectorAll('.tag-scroll .tag').forEach(t => t.classList.remove('active'));
                    this.classList.add('active');
                    if (navigator.vibrate) navigator.vibrate(5);
                });
            });

            console.log('☕ 咖啡手记 · 完整移动端已启动！');
            console.log('📱 点击底部 Tab 切换 4 个面板');
            console.log('➕ 点击中间 ☕ 打开记录浮层');

        })();
    </script>

</body>
</html>
