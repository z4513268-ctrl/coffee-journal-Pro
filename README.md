<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>咖啡手记 · 完整版</title>
    <style>
        /* ===== 全局重置 & CSS变量 ===== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --bg-primary: #1a1410;
            --bg-secondary: #2c1e16;
            --glow-color: rgba(200, 150, 110, 0.15);
            --accent-1: #c89a78;
            --accent-2: #a87b5e;
            --text-primary: #f0e3d8;
            --text-secondary: #a68979;
            --card-border: rgba(255, 215, 180, 0.05);
            --nav-blur: rgba(26, 20, 16, 0.75);
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "Helvetica Neue", sans-serif;
            background: var(--bg-primary);
            height: 100vh;
            height: 100dvh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            color: var(--text-primary);
            user-select: none;
            transition: background 0.8s cubic-bezier(0.32, 0.72, 0, 1);
        }

        /* ===== 氛围光晕 ===== */
        .ambient-glow {
            position: fixed;
            top: -30%;
            left: -30%;
            width: 160%;
            height: 160%;
            pointer-events: none;
            z-index: 0;
            transition: background 0.6s cubic-bezier(0.32, 0.72, 0, 1);
            border-radius: 50%;
            filter: blur(60px);
            opacity: 0.5;
        }
        .main-content,
        .bottom-nav,
        .record-overlay {
            position: relative;
            z-index: 1;
        }

        /* ===== 主内容 ===== */
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
            background: linear-gradient(135deg, var(--text-primary), var(--accent-1));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            transition: background 0.6s ease;
        }
        .page-header .date {
            font-size: 13px;
            color: var(--text-secondary);
            background: var(--bg-secondary);
            padding: 6px 14px;
            border-radius: 20px;
            border: 1px solid var(--card-border);
            flex-shrink: 0;
            -webkit-text-fill-color: var(--text-secondary);
            transition: all 0.6s ease;
        }

        /* ===== 卡片 ===== */
        .mock-card {
            background: linear-gradient(145deg, var(--bg-secondary), rgba(34, 24, 17, 0.8));
            border-radius: 16px;
            padding: 16px 16px;
            margin-bottom: 12px;
            border: 1px solid var(--card-border);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
            display: flex;
            align-items: center;
            gap: 14px;
            transition: all 0.6s ease;
        }
        .mock-card:active {
            transform: scale(0.98);
        }
        .mock-card .avatar {
            width: 48px;
            height: 48px;
            border-radius: 12px;
            background: var(--bg-secondary);
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 22px;
            transition: background 0.6s ease;
        }
        .mock-card .info .title {
            font-size: 15px;
            font-weight: 600;
            color: var(--text-primary);
        }
        .mock-card .info .desc {
            font-size: 13px;
            color: var(--text-secondary);
            margin-top: 2px;
        }
        .mock-card .badge {
            background: var(--bg-secondary);
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 11px;
            color: var(--text-secondary);
            border: 1px solid var(--card-border);
            flex-shrink: 0;
            transition: all 0.6s ease;
        }
        .bottom-hint {
            text-align: center;
            color: var(--text-secondary);
            opacity: 0.3;
            font-size: 12px;
            padding: 16px 0 8px;
            letter-spacing: 0.5px;
            transition: color 0.6s ease;
        }

        /* ===== 今日宜喝卡片（命理） ===== */
        .fortune-summary {
            background: linear-gradient(145deg, var(--bg-secondary), rgba(31, 21, 16, 0.9));
            border-radius: 16px;
            padding: 16px 18px;
            border: 1px solid rgba(200, 154, 120, 0.08);
            margin-bottom: 16px;
            display: flex;
            align-items: center;
            gap: 14px;
            transition: all 0.6s ease;
        }
        .fortune-summary .icon {
            font-size: 32px;
        }
        .fortune-summary .info .label {
            font-size: 12px;
            color: var(--text-secondary);
        }
        .fortune-summary .info .value {
            font-size: 16px;
            font-weight: 600;
            color: var(--text-primary);
        }
        .fortune-summary .match-badge {
            margin-left: auto;
            background: rgba(200, 154, 120, 0.12);
            padding: 4px 14px;
            border-radius: 20px;
            color: var(--accent-1);
            font-size: 13px;
            border: 1px solid rgba(200, 154, 120, 0.1);
            transition: all 0.6s ease;
        }

        /* ===== 附近推荐 ===== */
        .shop-card {
            background: linear-gradient(145deg, var(--bg-secondary), rgba(34, 24, 17, 0.8));
            border-radius: 16px;
            padding: 16px;
            margin-bottom: 12px;
            border: 1px solid var(--card-border);
            display: flex;
            gap: 14px;
            transition: all 0.6s ease;
        }
        .shop-card:active {
            transform: scale(0.98);
        }
        .shop-card .thumb {
            width: 60px;
            height: 60px;
            border-radius: 12px;
            background: var(--bg-secondary);
            flex-shrink: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            transition: background 0.6s ease;
        }
        .shop-card .info {
            flex: 1;
            min-width: 0;
        }
        .shop-card .info .name {
            font-size: 15px;
            font-weight: 600;
            color: var(--text-primary);
        }
        .shop-card .info .address {
            font-size: 12px;
            color: var(--text-secondary);
            margin-top: 2px;
        }
        .shop-card .info .tags {
            display: flex;
            gap: 6px;
            margin-top: 6px;
            flex-wrap: wrap;
        }
        .shop-card .info .tags span {
            font-size: 10px;
            padding: 2px 12px;
            border-radius: 12px;
            background: var(--bg-secondary);
            border: 1px solid var(--card-border);
            color: var(--text-secondary);
            transition: all 0.6s ease;
        }
        .shop-card .info .tags .highlight {
            border-color: var(--accent-1);
            color: var(--accent-1);
            background: rgba(200, 154, 120, 0.06);
        }
        .shop-card .reason {
            margin-top: 8px;
            padding: 8px 14px;
            background: rgba(200, 154, 120, 0.04);
            border-radius: 10px;
            border-left: 2px solid var(--accent-1);
            font-size: 12px;
            color: var(--text-secondary);
            line-height: 1.5;
            transition: all 0.6s ease;
        }
        .shop-card .right {
            display: flex;
            flex-direction: column;
            align-items: flex-end;
            justify-content: space-between;
            flex-shrink: 0;
        }
        .shop-card .right .score {
            font-size: 16px;
            font-weight: 700;
            color: var(--text-primary);
        }
        .shop-card .right .score small {
            font-size: 11px;
            font-weight: 400;
            color: var(--text-secondary);
        }
        .shop-card .right .match-pct {
            font-size: 12px;
            color: var(--accent-1);
            background: rgba(200, 154, 120, 0.06);
            padding: 2px 12px;
            border-radius: 12px;
            border: 1px solid rgba(200, 154, 120, 0.1);
            transition: all 0.6s ease;
        }
        .section-title {
            font-size: 14px;
            font-weight: 500;
            color: var(--text-secondary);
            margin: 16px 0 12px;
            letter-spacing: 0.5px;
            transition: color 0.6s ease;
        }
        .refresh-btn {
            width: 100%;
            padding: 14px;
            border-radius: 14px;
            border: 1px solid var(--card-border);
            background: transparent;
            color: var(--text-secondary);
            font-size: 15px;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.2s;
            margin-top: 8px;
        }
        .refresh-btn:active {
            background: var(--bg-secondary);
            transform: scale(0.98);
        }

        /* ===== 底部导航 ===== */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            height: 82px;
            padding-bottom: env(safe-area-inset-bottom, 12px);
            background: var(--nav-blur);
            backdrop-filter: blur(24px) saturate(180%);
            -webkit-backdrop-filter: blur(24px) saturate(180%);
            border-top: 0.5px solid var(--card-border);
            display: flex;
            align-items: flex-start;
            justify-content: space-around;
            z-index: 100;
            box-shadow: 0 -10px 40px rgba(0, 0, 0, 0.6);
            transition: all 0.6s ease;
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
            color: var(--text-secondary);
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
            color: var(--accent-1);
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
            background: var(--accent-1);
            margin-top: 2px;
            opacity: 0;
            transition: opacity 0.2s, background 0.6s ease;
        }
        .nav-item.active .dot {
            opacity: 1;
        }

        /* FAB */
        .nav-item.fab {
            position: relative;
            top: -18px;
            flex: 0 0 68px;
            height: 68px;
            background: linear-gradient(145deg, var(--accent-1), var(--accent-2));
            border-radius: 50%;
            box-shadow: 0 8px 30px rgba(200, 154, 120, 0.35), inset 0 1px 0 rgba(255, 255, 255, 0.2);
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 0;
            border: none;
            color: #fff;
            transition: all 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
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
        }
        .nav-item.fab::before {
            content: '';
            position: absolute;
            inset: -6px;
            border-radius: 50%;
            background: radial-gradient(circle at center, var(--accent-1), transparent 70%);
            z-index: -1;
            animation: fabPulse 3s ease-in-out infinite;
            transition: background 0.6s ease;
        }
        @keyframes fabPulse {
            0%,
            100% {
                transform: scale(1);
                opacity: 0.5;
            }
            50% {
                transform: scale(1.15);
                opacity: 0.15;
            }
        }

        /* ===== 记录浮层 ===== */
        .record-overlay {
            position: fixed;
            inset: 0;
            background: rgba(10, 6, 4, 0.88);
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
            from {
                transform: translateY(100%);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }
        .record-sheet {
            background: linear-gradient(180deg, var(--bg-secondary), var(--bg-primary));
            border-radius: 32px 32px 0 0;
            padding: 20px 20px 34px;
            max-height: 88vh;
            overflow-y: auto;
            border-top: 1px solid var(--card-border);
            transition: all 0.6s ease;
        }
        .record-sheet .handle {
            width: 40px;
            height: 4px;
            background: var(--card-border);
            border-radius: 4px;
            margin: 0 auto 18px;
            transition: background 0.6s ease;
        }
        .record-sheet h2 {
            font-size: 22px;
            font-weight: 600;
            color: var(--text-primary);
            transition: color 0.6s ease;
        }
        .record-sheet .sub {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 16px;
            transition: color 0.6s ease;
        }

        /* 轮盘 */
        .wheel-container {
            display: flex;
            justify-content: center;
            margin: 0 auto 16px;
            touch-action: none;
        }
        .wheel-container canvas {
            width: 260px;
            height: 260px;
            border-radius: 50%;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.6), inset 0 0 60px rgba(0, 0, 0, 0.3);
            touch-action: none;
            cursor: grab;
            background: #2a1d15;
            display: block;
        }
        .wheel-container canvas:active {
            cursor: grabbing;
        }

        .flavor-display {
            text-align: center;
            height: 56px;
            margin-bottom: 12px;
        }
        .flavor-display .main {
            font-size: 24px;
            font-weight: 600;
            color: var(--text-primary);
            letter-spacing: 1px;
            transition: color 0.3s ease;
        }
        .flavor-display .sub {
            font-size: 13px;
            color: var(--text-secondary);
            margin-top: 2px;
            transition: color 0.3s ease;
        }

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
            background: var(--bg-secondary);
            padding: 8px 20px;
            border-radius: 30px;
            color: var(--text-secondary);
            font-size: 14px;
            border: 1px solid var(--card-border);
            white-space: nowrap;
            flex-shrink: 0;
            transition: all 0.3s ease;
        }
        .tag-scroll .tag:active {
            transform: scale(0.94);
        }
        .tag-scroll .tag.active {
            background: var(--bg-secondary);
            border-color: var(--accent-1);
            color: var(--text-primary);
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
            transition: all 0.3s ease;
        }
        .record-actions .btn-cancel {
            background: var(--bg-secondary);
            color: var(--text-secondary);
            border: 1px solid var(--card-border);
        }
        .record-actions .btn-cancel:active {
            transform: scale(0.96);
            opacity: 0.7;
        }
        .record-actions .btn-done {
            background: linear-gradient(145deg, var(--accent-1), var(--accent-2));
            color: #fff;
            box-shadow: 0 6px 24px rgba(200, 154, 120, 0.25);
        }
        .record-actions .btn-done:active {
            transform: scale(0.96);
        }
        .record-actions .btn-done.success {
            background: #4CAF50;
            box-shadow: 0 6px 24px rgba(76, 175, 80, 0.3);
        }

        /* ===== 命理弹窗（精简内嵌） ===== */
        .fortune-modal {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(10, 6, 4, 0.9);
            backdrop-filter: blur(16px);
            z-index: 300;
            align-items: center;
            justify-content: center;
            padding: 24px;
            animation: fadeIn 0.4s ease;
        }
        .fortune-modal.open {
            display: flex;
        }
        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: scale(0.96);
            }
            to {
                opacity: 1;
                transform: scale(1);
            }
        }
        .fortune-modal .card {
            max-width: 400px;
            width: 100%;
            background: linear-gradient(160deg, #2c1e16, #1a1410);
            border-radius: 32px;
            padding: 32px 24px 28px;
            border: 1px solid rgba(255, 215, 180, 0.06);
            box-shadow: 0 30px 80px rgba(0, 0, 0, 0.8);
            max-height: 90vh;
            overflow-y: auto;
        }
        .fortune-modal .card .compass {
            width: 72px;
            height: 72px;
            margin: 0 auto 16px;
            position: relative;
        }
        .fortune-modal .card .compass svg {
            width: 100%;
            height: 100%;
            animation: spin 20s linear infinite;
        }
        @keyframes spin {
            0% {
                transform: rotate(0deg);
            }
            100% {
                transform: rotate(360deg);
            }
        }
        .fortune-modal .card .compass .label {
            position: absolute;
            inset: 0;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: 300;
            color: var(--accent-1);
        }
        .fortune-modal .card .modal-title {
            text-align: center;
            font-size: 20px;
            font-weight: 600;
            background: linear-gradient(135deg, #f5e3d4, #c89a78);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        .fortune-modal .card .modal-sub {
            text-align: center;
            font-size: 13px;
            color: #8a7365;
            margin-bottom: 20px;
        }
        .fortune-modal .card .input-group {
            display: flex;
            gap: 8px;
            margin-bottom: 16px;
            flex-wrap: wrap;
        }
        .fortune-modal .card .input-group select {
            flex: 1;
            min-width: 70px;
            padding: 12px 12px;
            border-radius: 12px;
            background: #2c1e16;
            border: 1px solid #3d2b20;
            color: #f0e3d8;
            font-size: 14px;
            font-family: inherit;
            appearance: none;
            -webkit-appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%238a7365' stroke-width='1.5' fill='none'/%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 12px center;
        }
        .fortune-modal .card .input-group select:focus {
            outline: none;
            border-color: #c89a78;
        }
        .fortune-modal .card .btn-generate {
            width: 100%;
            padding: 14px;
            border-radius: 14px;
            border: none;
            background: linear-gradient(145deg, #c89a78, #a87b5e);
            color: #fff;
            font-size: 16px;
            font-weight: 600;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 6px 24px rgba(200, 154, 120, 0.2);
        }
        .fortune-modal .card .btn-generate:active {
            transform: scale(0.96);
        }
        .fortune-modal .card .result {
            margin-top: 20px;
            padding: 18px 16px;
            border-radius: 16px;
            background: rgba(200, 154, 120, 0.04);
            border: 1px solid rgba(200, 154, 120, 0.06);
            display: none;
        }
        .fortune-modal .card .result.visible {
            display: block;
        }
        .fortune-modal .card .result .tag {
            display: inline-block;
            background: rgba(200, 154, 120, 0.12);
            padding: 3px 14px;
            border-radius: 16px;
            font-size: 11px;
            color: #c89a78;
            border: 1px solid rgba(200, 154, 120, 0.1);
            margin-bottom: 8px;
        }
        .fortune-modal .card .result .main {
            font-size: 22px;
            font-weight: 700;
            color: #f5e3d4;
        }
        .fortune-modal .card .result .sub {
            font-size: 14px;
            color: #a68979;
            margin-bottom: 10px;
        }
        .fortune-modal .card .result .divider {
            height: 1px;
            background: linear-gradient(90deg, transparent, rgba(200, 154, 120, 0.12), transparent);
            margin: 12px 0;
        }
        .fortune-modal .card .result .row {
            display: flex;
            justify-content: space-between;
            padding: 4px 0;
            font-size: 13px;
        }
        .fortune-modal .card .result .row .label {
            color: #8a7365;
        }
        .fortune-modal .card .result .row .value {
            color: #f0e3d8;
            font-weight: 500;
        }
        .fortune-modal .card .result .mindful {
            margin-top: 12px;
            padding: 12px 14px;
            background: rgba(200, 154, 120, 0.04);
            border-radius: 10px;
            border-left: 2px solid #c89a78;
            font-size: 14px;
            line-height: 1.6;
            color: #d5c4b8;
            font-style: italic;
        }
        .fortune-modal .card .result .mindful::before {
            content: '🧘 ';
            font-style: normal;
        }
        .fortune-modal .card .close-btn {
            width: 100%;
            margin-top: 16px;
            padding: 12px;
            border-radius: 14px;
            border: 1px solid #3d2b20;
            background: transparent;
            color: #8a7365;
            font-size: 15px;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.2s;
        }
        .fortune-modal .card .close-btn:active {
            background: #2c1e16;
            transform: scale(0.96);
        }
    </style>
</head>
<body>

    <!-- ===== 氛围光晕 ===== -->
    <div class="ambient-glow" id="ambientGlow"></div>

    <!-- ========================================================= -->
    <!-- 主内容 -->
    <!-- ========================================================= -->
    <div class="main-content" id="mainContent">

        <!-- ===== 面板：首页 ===== -->
        <div class="panel active" id="panel-home">
            <div class="page-header">
                <h1>☕ 今日心境</h1>
                <span class="date" id="todayDate">6月21日</span>
            </div>

            <!-- 今日宜喝摘要 -->
            <div class="fortune-summary" id="fortuneSummary" style="cursor:pointer;">
                <span class="icon">🔮</span>
                <div class="info">
                    <div class="label">今日宜喝</div>
                    <div class="value" id="summaryBean">埃塞俄比亚·花魁</div>
                </div>
                <span class="match-badge" id="summaryBadge">✨ 查看命理</span>
            </div>

            <!-- 时间线记录 -->
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
            <div class="bottom-hint">— 已经到底了 —</div>
        </div>

        <!-- ===== 面板：人格 ===== -->
        <div class="panel" id="panel-personality">
            <div class="page-header">
                <h1>🧠 咖啡人格</h1>
                <span class="date">进化中</span>
            </div>
            <div style="text-align:center; padding:20px 0 30px;">
                <span style="font-size:72px; display:block; margin-bottom:12px;">🌱</span>
                <div style="font-size:20px; font-weight:600; color:var(--text-primary);">风味探险家 · 萌芽期</div>
                <div style="font-size:14px; color:var(--text-secondary); margin-top:4px;">记录满 10 杯解锁完整人格报告</div>
                <div style="width:80%; max-width:280px; height:6px; background:var(--bg-secondary); border-radius:6px; margin:16px auto 0; overflow:hidden;">
                    <div style="width:50%; height:100%; background:linear-gradient(90deg, var(--accent-1), var(--accent-2)); border-radius:6px; transition:all 0.6s ease;"></div>
                </div>
                <div style="margin-top:8px; font-size:13px; color:var(--text-secondary);">进度 5 / 10 杯</div>
            </div>
            <div style="background:var(--bg-secondary); border-radius:14px; padding:16px; border:1px solid var(--card-border); margin-bottom:12px; transition:all 0.6s ease;">
                <div style="display:flex; justify-content:space-between; align-items:center;">
                    <span style="color:var(--text-secondary); font-size:14px;">已解锁标签</span>
                    <span style="color:var(--accent-1); font-size:13px;">+3 杯解锁新标签</span>
                </div>
                <div style="display:flex; flex-wrap:wrap; gap:8px; margin-top:12px;">
                    <span style="background:var(--bg-secondary); padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid var(--accent-1); color:var(--text-primary);">🌸 花香</span>
                    <span style="background:var(--bg-secondary); padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid var(--accent-1); color:var(--text-primary);">🍊 果酸</span>
                    <span style="background:var(--bg-secondary); padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid var(--card-border); color:var(--text-secondary);">⬜ 待解锁</span>
                    <span style="background:var(--bg-secondary); padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid var(--card-border); color:var(--text-secondary);">⬜ 待解锁</span>
                </div>
            </div>
            <div class="bottom-hint">— 持续记录，解锁更多 —</div>
        </div>

        <!-- ===== 面板：同好 ===== -->
        <div class="panel" id="panel-social">
            <div class="page-header">
                <h1>🌊 同好漂流瓶</h1>
                <span class="date">今日匹配</span>
            </div>
            <div style="background:linear-gradient(135deg, var(--bg-secondary), rgba(31,21,16,0.8)); border-radius:14px; padding:14px 16px; border:1px solid var(--card-border); margin-bottom:16px; display:flex; align-items:center; gap:12px; transition:all 0.6s ease;">
                <span style="font-size:28px;">🧩</span>
                <div>
                    <div style="font-size:14px; color:var(--text-primary); font-weight:500;">基于风味相似度匹配</div>
                    <div style="font-size:12px; color:var(--text-secondary);">找到 3 位口味相近的咖友</div>
                </div>
            </div>
            <div class="mock-card">
                <div class="avatar">🌸</div>
                <div class="info">
                    <div class="title">@咖啡探险家</div>
                    <div class="desc">🏆 共同偏好：花香 · 果酸</div>
                </div>
                <span style="background:rgba(200,154,120,0.12); padding:4px 12px; border-radius:20px; font-size:11px; color:var(--accent-1); border:1px solid rgba(200,154,120,0.2); flex-shrink:0; transition:all 0.6s ease;">92% 匹配</span>
            </div>
            <div class="mock-card">
                <div class="avatar">🌿</div>
                <div class="info">
                    <div class="title">@风味猎人</div>
                    <div class="desc">🏆 共同偏好：发酵感 · 醇厚</div>
                </div>
                <span style="background:rgba(200,154,120,0.12); padding:4px 12px; border-radius:20px; font-size:11px; color:var(--accent-1); border:1px solid rgba(200,154,120,0.2); flex-shrink:0; transition:all 0.6s ease;">87% 匹配</span>
            </div>
            <div class="mock-card">
                <div class="avatar">🍃</div>
                <div class="info">
                    <div class="title">@手冲星人</div>
                    <div class="desc">🏆 共同偏好：茶感 · 干净</div>
                </div>
                <span style="background:rgba(200,154,120,0.12); padding:4px 12px; border-radius:20px; font-size:11px; color:var(--accent-1); border:1px solid rgba(200,154,120,0.2); flex-shrink:0; transition:all 0.6s ease;">81% 匹配</span>
            </div>
            <div style="text-align:center; padding:12px 0 4px;">
                <span style="background:var(--bg-secondary); padding:10px 28px; border-radius:30px; color:var(--accent-1); font-size:14px; border:1px solid var(--card-border); display:inline-block; transition:all 0.6s ease;">📨 发送「隔空共饮」邀请</span>
            </div>
            <div class="bottom-hint">— 基于 8 维风味向量匹配 —</div>
        </div>

        <!-- ===== 面板：报告 ===== -->
        <div class="panel" id="panel-report">
            <div class="page-header">
                <h1>📊 风味报告</h1>
                <span class="date">6 月趋势</span>
            </div>
            <div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:10px; margin-bottom:16px;">
                <div style="background:var(--bg-secondary); border-radius:14px; padding:16px 8px; text-align:center; border:1px solid var(--card-border); transition:all 0.6s ease;">
                    <div style="font-size:22px; font-weight:700; color:var(--text-primary);">12</div>
                    <div style="font-size:11px; color:var(--text-secondary); margin-top:4px;">本月杯数</div>
                </div>
                <div style="background:var(--bg-secondary); border-radius:14px; padding:16px 8px; text-align:center; border:1px solid var(--card-border); transition:all 0.6s ease;">
                    <div style="font-size:22px; font-weight:700; color:var(--text-primary);">6</div>
                    <div style="font-size:11px; color:var(--text-secondary); margin-top:4px;">探索产地</div>
                </div>
                <div style="background:var(--bg-secondary); border-radius:14px; padding:16px 8px; text-align:center; border:1px solid var(--card-border); transition:all 0.6s ease;">
                    <div style="font-size:22px; font-weight:700; color:var(--text-primary);">4.7</div>
                    <div style="font-size:11px; color:var(--text-secondary); margin-top:4px;">平均评分</div>
                </div>
            </div>
            <div style="background:var(--bg-secondary); border-radius:14px; padding:16px 18px; border:1px solid var(--card-border); margin-bottom:12px; transition:all 0.6s ease;">
                <div style="display:flex; justify-content:space-between; padding:8px 0; border-bottom:1px solid var(--card-border);">
                    <span style="color:var(--text-secondary); font-size:14px;">🌍 最爱产区</span>
                    <span style="color:var(--text-primary); font-size:14px; font-weight:500;">埃塞俄比亚</span>
                </div>
                <div style="display:flex; justify-content:space-between; padding:8px 0; border-bottom:1px solid var(--card-border);">
                    <span style="color:var(--text-secondary); font-size:14px;">🏷️ 风味标签</span>
                    <span style="color:var(--text-primary); font-size:14px; font-weight:500;">花香 · 果酸 · 茶感</span>
                </div>
                <div style="display:flex; justify-content:space-between; padding:8px 0;">
                    <span style="color:var(--text-secondary); font-size:14px;">📈 本月趋势</span>
                    <span style="color:#4CAF50; font-size:14px; font-weight:500;">↑ 偏好向浅烘偏移</span>
                </div>
            </div>

            <!-- 附近推荐区域（内嵌在报告页，也可作为独立模块） -->
            <div class="section-title">📍 附近 · 风味匹配</div>
            <div class="shop-card">
                <div class="thumb">☕</div>
                <div class="info">
                    <div class="name">叁舍咖啡 · 手冲专门店</div>
                    <div class="address">📍 天河路 228 号 · 步行 320m</div>
                    <div class="tags">
                        <span class="highlight">🏷️ 推荐：花魁</span>
                        <span>浅烘</span>
                        <span>手冲</span>
                        <span>⭐ 4.7</span>
                    </div>
                    <div class="reason">💡 大众点评：“花魁手冲花香炸裂，与你今日命理高度匹配。”</div>
                </div>
                <div class="right">
                    <div class="score">4.7 <small>/5</small></div>
                    <span class="match-pct">🔥 94%</span>
                </div>
            </div>
            <div class="shop-card">
                <div class="thumb">🌸</div>
                <div class="info">
                    <div class="name">瑰夏·花园咖啡</div>
                    <div class="address">📍 花城大道 89 号 · 步行 480m</div>
                    <div class="tags">
                        <span class="highlight">🏷️ 推荐：瑰夏</span>
                        <span>中浅烘</span>
                        <span>SOE</span>
                        <span>⭐ 4.5</span>
                    </div>
                    <div class="reason">💡 小红书：“冰冲瑰夏柑橘蜂蜜味绝了。”</div>
                </div>
                <div class="right">
                    <div class="score">4.5 <small>/5</small></div>
                    <span class="match-pct">✨ 87%</span>
                </div>
            </div>
            <button class="refresh-btn" id="refreshBtn">🔄 刷新附近推荐</button>
            <div style="text-align:center; color:var(--text-secondary); opacity:0.3; font-size:11px; padding:12px 0 4px; transition:color 0.6s ease;">— 数据基于大众点评 · 小红书 AI 汇总 —</div>
            <div class="bottom-hint">— 数据每日更新 —</div>
        </div>
    </div>

    <!-- ========================================================= -->
    <!-- 底部导航 -->
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
            <div class="sub">单指滑动轮盘 · 找到你的风味</div>
            <div class="wheel-container">
                <canvas id="wheelCanvas" width="400" height="400"></canvas>
            </div>
            <div class="flavor-display">
                <div class="main" id="flavorMain">明亮果酸</div>
                <div class="sub" id="flavorSub">酸 · 清爽</div>
            </div>
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
            <div class="record-actions">
                <button class="btn btn-cancel" id="cancelRecord">取消</button>
                <button class="btn btn-done" id="doneRecord">☕ 完成记录</button>
            </div>
        </div>
    </div>

    <!-- ========================================================= -->
    <!-- 命理弹窗 -->
    <!-- ========================================================= -->
    <div class="fortune-modal" id="fortuneModal">
        <div class="card">
            <div class="compass">
                <svg viewBox="0 0 100 100">
                    <circle cx="50" cy="50" r="46" fill="none" stroke="#3d2b20" stroke-width="1"/>
                    <circle cx="50" cy="50" r="38" fill="none" stroke="#4d382b" stroke-width="0.5" stroke-dasharray="4 4"/>
                    <line x1="50" y1="4" x2="50" y2="20" stroke="#c89a78" stroke-width="1.5"/>
                    <line x1="50" y1="80" x2="50" y2="96" stroke="#c89a78" stroke-width="1.5"/>
                    <line x1="4" y1="50" x2="20" y2="50" stroke="#c89a78" stroke-width="1.5"/>
                    <line x1="80" y1="50" x2="96" y2="50" stroke="#c89a78" stroke-width="1.5"/>
                    <text x="50" y="14" text-anchor="middle" fill="#8a7365" font-size="6">北</text>
                    <text x="50" y="94" text-anchor="middle" fill="#8a7365" font-size="6">南</text>
                    <text x="10" y="54" text-anchor="middle" fill="#8a7365" font-size="6">西</text>
                    <text x="90" y="54" text-anchor="middle" fill="#8a7365" font-size="6">东</text>
                </svg>
                <div class="label">☯</div>
            </div>
            <div class="modal-title">今日宜喝 · 命理咖啡</div>
            <div class="modal-sub">输入你的生辰 · 获取今日专属指引</div>
            <div class="input-group">
                <select id="birthYear"></select>
                <select id="birthMonth">
                    <option value="1">1月</option><option value="2">2月</option><option value="3">3月</option>
                    <option value="4">4月</option><option value="5">5月</option><option value="6">6月</option>
                    <option value="7">7月</option><option value="8">8月</option><option value="9">9月</option>
                    <option value="10">10月</option><option value="11">11月</option><option value="12">12月</option>
                </select>
                <select id="birthDay"></select>
            </div>
            <button class="btn-generate" id="generateBtn">🔮 查看今日命理</button>
            <div class="result" id="fortuneResult">
                <span class="tag" id="fTag">✦ 今日宜 · 向上生长</span>
                <div class="main" id="fMain">埃塞俄比亚·花魁</div>
                <div class="sub" id="fSub">明亮果酸 · 花香 · 茶感</div>
                <div class="divider"></div>
                <div class="row"><span class="label">生命数字</span><span class="value" id="fLife">—</span></div>
                <div class="row"><span class="label">五行能量</span><span class="value" id="fElement">—</span></div>
                <div class="row"><span class="label">推荐烘焙度</span><span class="value" id="fRoast">—</span></div>
                <div class="row"><span class="label">饮用温度</span><span class="value" id="fTemp">—</span></div>
                <div class="mindful" id="fMindful">今日宜·向上生长，让花香唤醒你内在的创造力。每一口都是与自己的对话。</div>
            </div>
            <button class="close-btn" id="closeFortune">✕ 关闭</button>
        </div>
    </div>

    <!-- ========================================================= -->
    <!-- JavaScript -->
    <!-- ========================================================= -->
    <script>
        (function() {
            'use strict';

            // =========================================================
            // 一、动态色彩系统
            // =========================================================
            const COLOR_MAP = {
                acid: { low: '#f5e3d4', high: '#e8a87c' },
                sweet: { low: '#c4b5a5', high: '#d4a050' },
                bitter: { low: '#d5c8bc', high: '#4a3428' },
                body: { low: '#e8ddd0', high: '#8b5a3c' },
                fruity: { low: '#b5c0b0', high: '#c46a6a' },
                floral: { low: '#c5b8c0', high: '#d4a0b0' },
                nutty: { low: '#c5b8a8', high: '#5a3d2e' },
                fermented: { low: '#c5c0b8', high: '#7a4a5a' },
            };

            function hexToRgb(hex) {
                const r = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
                return r ? { r: parseInt(r[1], 16), g: parseInt(r[2], 16), b: parseInt(r[3], 16) } : { r: 200, g: 150,
                    b: 110 };
            }

            function rgbToHex(r, g, b) {
                const c = (v) => Math.max(0, Math.min(255, Math.round(v)));
                return '#' + [c(r), c(g), c(b)].map(v => v.toString(16).padStart(2, '0')).join('');
            }

            function posToVector(x, y) {
                const acid = Math.round((1 - x) / 2 * 10);
                const body = Math.round((1 + y) / 2 * 10);
                const fruity = Math.min(10, Math.round(acid * 0.7 + 2));
                const floral = Math.min(10, Math.round(acid * 0.6 + 1));
                const sweet = Math.min(10, Math.round((1 - (x + y) / 2) * 6 + 2));
                const bitter = Math.min(10, Math.round((1 + (x - y) / 2) * 6 + 2));
                const nutty = Math.min(10, Math.round(body * 0.6 + 1));
                const fermented = Math.min(10, Math.round(acid * 0.3 + body * 0.4 + 1));
                return [acid, sweet, bitter, body, fruity, floral, nutty, fermented];
            }

            function vectorToColors(vector) {
                if (!vector || vector.length < 8) {
                    return { primary: '#1a1410', secondary: '#2c1e16', glow: 'rgba(200,150,110,0.15)', accent1: '#c89a78',
                        accent2: '#a87b5e', textPrimary: '#f0e3d8', textSecondary: '#a68979' };
                }
                const dims = ['acid', 'sweet', 'bitter', 'body', 'fruity', 'floral', 'nutty', 'fermented'];
                let mr = 0,
                    mg = 0,
                    mb = 0,
                    tw = 0;
                dims.forEach((key, i) => {
                    const val = Math.max(0, Math.min(10, vector[i] || 5));
                    const w = val / 10;
                    const l = hexToRgb(COLOR_MAP[key].low);
                    const h = hexToRgb(COLOR_MAP[key].high);
                    const r = l.r + (h.r - l.r) * w;
                    const g = l.g + (h.g - l.g) * w;
                    const b = l.b + (h.b - l.b) * w;
                    const inf = (val / 10) * 0.8 + 0.2;
                    mr += r * inf;
                    mg += g * inf;
                    mb += b * inf;
                    tw += inf;
                });
                mr = Math.round(mr / tw);
                mg = Math.round(mg / tw);
                mb = Math.round(mb / tw);
                const primary = rgbToHex(mr - 30, mg - 25, mb - 20);
                const secondary = rgbToHex(mr + 20, mg + 25, mb + 30);
                const accent1 = rgbToHex(Math.min(255, mr + 40), Math.min(255, mg - 10), Math.min(255, mb - 30));
                const accent2 = rgbToHex(Math.min(255, mr - 20), Math.min(255, mg + 20), Math.min(255, mb + 10));
                const glow = `rgba(${mr}, ${mg}, ${mb}, 0.15)`;
                const br = (mr * 299 + mg * 587 + mb * 114) / 1000;
                const textPrimary = br > 160 ? '#1a1410' : '#f0e3d8';
                const textSecondary = br > 160 ? '#4a3a30' : '#a68979';
                return { primary, secondary, glow, accent1, accent2, textPrimary, textSecondary };
            }

            function applyColors(colors) {
                const root = document.documentElement;
                root.style.setProperty('--bg-primary', colors.primary);
                root.style.setProperty('--bg-secondary', colors.secondary);
                root.style.setProperty('--glow-color', colors.glow);
                root.style.setProperty('--accent-1', colors.accent1);
                root.style.setProperty('--accent-2', colors.accent2);
                root.style.setProperty('--text-primary', colors.textPrimary);
                root.style.setProperty('--text-secondary', colors.textSecondary);
                const glow = document.getElementById('ambientGlow');
                glow.style.background = `radial-gradient(circle at 40% 30%, ${colors.glow}, transparent 70%)`;
                const navBg = hexToRgb(colors.primary);
                root.style.setProperty('--nav-blur', `rgba(${navBg.r}, ${navBg.g}, ${navBg.b}, 0.75)`);
                document.body.style.background = colors.primary;
            }

            // =========================================================
            // 二、风味轮盘
            // =========================================================
            const canvas = document.getElementById('wheelCanvas');
            const ctx = canvas.getContext('2d');
            const flavorMain = document.getElementById('flavorMain');
            const flavorSub = document.getElementById('flavorSub');
            const size = 400,
                center = size / 2,
                radius = 170;
            const flavorMap = [
                { x: -1.0, y: -1.0, main: '明亮果酸', sub: '酸 · 清爽' },
                { x: -0.7, y: -0.3, main: '柑橘酸甜', sub: '酸 · 平衡' },
                { x: -0.3, y: -0.8, main: '青柠清新', sub: '酸 · 明亮' },
                { x: 0.0, y: -1.0, main: '茶感清透', sub: '干净 · 柔和' },
                { x: 0.5, y: -0.7, main: '焦糖甜感', sub: '甜 · 圆润' },
                { x: 1.0, y: -0.5, main: '黑巧浓郁', sub: '苦 · 醇厚' },
                { x: 0.8, y: 0.3, main: '坚果可可', sub: '醇厚 · 扎实' },
                { x: 0.5, y: 0.8, main: '奶油触感', sub: '醇厚 · 顺滑' },
                { x: 0.0, y: 0.9, main: '蜂蜜甜醇', sub: '甜 · 饱满' },
                { x: -0.5, y: 0.6, main: '花香轻盈', sub: '花香 · 优雅' },
                { x: -0.8, y: 0.2, main: '莓果发酵', sub: '果酸 · 酒韵' },
                { x: -0.2, y: -0.2, main: '平衡圆润', sub: '均衡 · 日常' },
                { x: 0.9, y: -0.9, main: '强烈烟熏', sub: '苦 · 厚重' },
                { x: -0.9, y: 0.9, main: '玫瑰蜜桃', sub: '花香 · 甜感' },
            ];
            let posX = 0.0,
                posY = -0.15,
                isDragging = false;

            function findClosestFlavor(x, y) {
                let best = flavorMap[0],
                    bestDist = Infinity;
                for (const f of flavorMap) {
                    const d = (f.x - x) ** 2 + (f.y - y) ** 2;
                    if (d < bestDist) { bestDist = d;
                        best = f; }
                }
                return best;
            }

            function drawWheel(x, y) {
                ctx.clearRect(0, 0, size, size);
                const grad = ctx.createRadialGradient(center, center, 20, center, center, radius);
                grad.addColorStop(0, '#4d3426');
                grad.addColorStop(0.7, '#2a1d15');
                grad.addColorStop(1, '#1a100c');
                ctx.beginPath();
                ctx.arc(center, center, radius, 0, Math.PI * 2);
                ctx.fillStyle = grad;
                ctx.fill();
                ctx.strokeStyle = 'rgba(255,215,180,0.05)';
                ctx.lineWidth = 1;
                for (let i = 0; i < 8; i++) {
                    const a = (i / 8) * Math.PI * 2;
                    ctx.beginPath();
                    ctx.moveTo(center, center);
                    ctx.lineTo(center + Math.cos(a) * radius, center + Math.sin(a) * radius);
                    ctx.stroke();
                }
                for (let r = 40; r < radius; r += 40) {
                    ctx.beginPath();
                    ctx.arc(center, center, r, 0, Math.PI * 2);
                    ctx.stroke();
                }
                ctx.fillStyle = 'rgba(200,180,165,0.25)';
                ctx.font = '12px -apple-system, sans-serif';
                ctx.textAlign = 'center';
                ctx.textBaseline = 'middle';
                ctx.fillText('酸', center - radius + 22, center);
                ctx.fillText('苦', center + radius - 22, center);
                ctx.fillText('干净', center, center - radius + 22);
                ctx.fillText('醇厚', center, center + radius - 22);
                const cx = center + x * (radius - 20);
                const cy = center + y * (radius - 20);
                const grd = ctx.createRadialGradient(cx, cy, 4, cx, cy, 30);
                grd.addColorStop(0, 'rgba(235, 190, 160, 0.4)');
                grd.addColorStop(1, 'rgba(235, 190, 160, 0)');
                ctx.beginPath();
                ctx.arc(cx, cy, 30, 0, Math.PI * 2);
                ctx.fillStyle = grd;
                ctx.fill();
                ctx.beginPath();
                ctx.arc(cx, cy, 14, 0, Math.PI * 2);
                ctx.fillStyle = '#f0d5c0';
                ctx.shadowColor = 'rgba(240, 213, 192, 0.4)';
                ctx.shadowBlur = 24;
                ctx.fill();
                ctx.shadowBlur = 0;
                ctx.beginPath();
                ctx.arc(cx - 4, cy - 4, 5, 0, Math.PI * 2);
                ctx.fillStyle = 'rgba(255,255,255,0.25)';
                ctx.fill();
                ctx.beginPath();
                ctx.arc(cx, cy, 4, 0, Math.PI * 2);
                ctx.fillStyle = '#fff';
                ctx.fill();
            }

            function updateUI(x, y) {
                const cx = Math.max(-1, Math.min(1, x));
                const cy = Math.max(-1, Math.min(1, y));
                posX = cx;
                posY = cy;
                const flavor = findClosestFlavor(cx, cy);
                flavorMain.textContent = flavor.main;
                flavorSub.textContent = flavor.sub;
                const colors = vectorToColors(posToVector(cx, cy));
                applyColors(colors);
                drawWheel(cx, cy);
                if (isDragging && navigator.vibrate) navigator.vibrate(5);
            }

            function getPosFromEvent(e) {
                const rect = canvas.getBoundingClientRect();
                const scX = canvas.width / rect.width,
                    scY = canvas.height / rect.height;
                let cx, cy;
                if (e.touches) { cx = e.touches[0].clientX;
                    cy = e.touches[0].clientY;
                    e.preventDefault(); } else { cx = e.clientX;
                    cy = e.clientY; }
                const dx = ((cx - rect.left) * scX - center) / (radius - 20);
                const dy = ((cy - rect.top) * scY - center) / (radius - 20);
                return { x: Math.max(-1, Math.min(1, dx)), y: Math.max(-1, Math.min(1, dy)) };
            }

            function onStart(e) { isDragging = true; const p = getPosFromEvent(e);
                updateUI(p.x, p.y); }

            function onMove(e) { if (!isDragging) return; const p = getPosFromEvent(e);
                updateUI(p.x, p.y);
                e.preventDefault(); }

            function onEnd(e) { isDragging = false; if (navigator.vibrate) navigator.vibrate(8); }
            canvas.addEventListener('mousedown', onStart);
            window.addEventListener('mousemove', onMove);
            window.addEventListener('mouseup', onEnd);
            canvas.addEventListener('touchstart', onStart, { passive: false });
            window.addEventListener('touchmove', onMove, { passive: false });
            window.addEventListener('touchend', onEnd, { passive: false });
            canvas.addEventListener('touchmove', e => e.preventDefault(), { passive: false });
            updateUI(posX, posY);

            // =========================================================
            // 三、命理系统
            // =========================================================
            const LIFE_MAP = {
                1: { flavor: '明亮果酸', bean: '埃塞俄比亚·花魁', sub: '明亮果酸 · 花香 · 茶感', element: '火' },
                2: { flavor: '均衡圆润', bean: '哥伦比亚·蕙兰', sub: '均衡圆润 · 坚果 · 柔和', element: '土' },
                3: { flavor: '花香轻盈', bean: '瑰夏·绿标', sub: '茉莉花香 · 柑橘 · 蜂蜜', element: '木' },
                4: { flavor: '坚果可可', bean: '巴西·黄波旁', sub: '坚果 · 可可 · 醇厚扎实', element: '土' },
                5: { flavor: '发酵酒韵', bean: '肯尼亚·AA', sub: '莓果 · 红酒韵 · 明亮', element: '水' },
                6: { flavor: '蜂蜜甜醇', bean: '危地马拉·安提瓜', sub: '蜂蜜 · 甜醇 · 圆润', element: '金' },
                7: { flavor: '茶感清透', bean: '中国·云南', sub: '茶感 · 清透 · 柔和', element: '水' },
                8: { flavor: '黑巧浓郁', bean: '印尼·曼特宁', sub: '黑巧 · 浓郁 · 低酸', element: '金' },
                9: { flavor: '复合层次', bean: '拼配豆 · 深藏', sub: '复合层次 · 深邃 · 多变', element: '火' },
            };
            const ROAST_MAP = {
                '木': { roast: '浅烘', temp: '温热 60-65°C', word: '向上生长 · 拥抱可能' },
                '火': { roast: '中浅烘', temp: '稍热 65-70°C', word: '热情绽放 · 活在当下' },
                '土': { roast: '中烘', temp: '温润 60°C', word: '脚踏实地 · 感恩此刻' },
                '金': { roast: '中深烘', temp: '温热 60°C', word: '坚定锐意 · 专注前行' },
                '水': { roast: '深烘', temp: '稍凉 55-60°C', word: '沉静内省 · 倾听内心' },
            };
            const ELEM_NAMES = { '木': '木·生长', '火': '火·热情', '土': '土·稳定', '金': '金·锐意', '水': '水·沉静' };
            const LIFE_NAMES = ['独立·创新', '平衡·和谐', '创意·表达', '稳定·踏实', '冒险·自由', '关爱·责任', '深思·内省', '力量·成就', '智慧·包容'];

            function getLifeNumber(y, m, d) {
                let s = y + m + d;
                while (s > 9) { s = String(s).split('').reduce((a, b) => a + parseInt(b), 0); }
                return s;
            }

            function getElement(y) {
                const map = { 0: '土', 1: '金', 2: '金', 3: '水', 4: '水', 5: '木', 6: '木', 7: '火', 8: '火', 9: '土' };
                return map[y % 10] || '土';
            }

            function getMindfulness(num, elem) {
                const prefixes = {
                    1: '让' + (LIFE_MAP[num]?.flavor || '风味') + '唤醒你内在的创造力。',
                    2: '以' + (LIFE_MAP[num]?.flavor || '风味') + '平衡今日的喧嚣与宁静。',
                    3: (LIFE_MAP[num]?.flavor || '风味') + '是你今日的表达方式。',
                    4: '在' + (LIFE_MAP[num]?.flavor || '风味') + '中感受稳定与力量。',
                    5: (LIFE_MAP[num]?.flavor || '风味') + '带你探索未知的边界。',
                    6: (LIFE_MAP[num]?.flavor || '风味') + '是今日的温柔陪伴。',
                    7: '在' + (LIFE_MAP[num]?.flavor || '风味') + '中寻找内心的答案。',
                    8: (LIFE_MAP[num]?.flavor || '风味') + '赋予你今日的坚定与决心。',
                    9: (LIFE_MAP[num]?.flavor || '风味') + '连接你与更广阔的智慧。',
                };
                return (prefixes[num] || '让这杯咖啡陪你度过今日。') + ' ' + (ROAST_MAP[elem]?.word || '静心品味') + '。每一口都是与自己的对话。';
            }

            // 填充下拉
            const ySel = document.getElementById('birthYear'),
                dSel = document.getElementById('birthDay');
            for (let y = 2026; y >= 1900; y--) { const o = document.createElement('option');
                o.value = y;
                o.textContent = y + '年'; if (y === 1995) o.selected = true;
                ySel.appendChild(o); }
            for (let d = 1; d <= 31; d++) { const o = document.createElement('option');
                o.value = d;
                o.textContent = d + '日'; if (d === 15) o.selected = true;
                dSel.appendChild(o); }

            function generateFortune() {
                const y = parseInt(document.getElementById('birthYear').value);
                const m = parseInt(document.getElementById('birthMonth').value);
                const d = parseInt(document.getElementById('birthDay').value);
                const num = getLifeNumber(y, m, d);
                const elem = getElement(y);
                const data = LIFE_MAP[num] || LIFE_MAP[5];
                const roast = ROAST_MAP[elem] || ROAST_MAP['土'];
                document.getElementById('fTag').textContent = '✦ ' + roast.word;
                document.getElementById('fMain').textContent = data.bean;
                document.getElementById('fSub').textContent = data.sub;
                document.getElementById('fLife').textContent = num + ' (' + (LIFE_NAMES[num - 1] || '') + ')';
                document.getElementById('fElement').textContent = elem + ' (' + (ELEM_NAMES[elem] || '') + ')';
                document.getElementById('fRoast').textContent = roast.roast;
                document.getElementById('fTemp').textContent = roast.temp;
                document.getElementById('fMindful').textContent = getMindfulness(num, elem);
                document.getElementById('fortuneResult').classList.add('visible');
                // 更新首页摘要
                document.getElementById('summaryBean').textContent = data.bean;
                if (navigator.vibrate) navigator.vibrate(10);
            }

            document.getElementById('generateBtn').addEventListener('click', generateFortune);

            // =========================================================
            // 四、UI 交互控制
            // =========================================================
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
            const fortuneModal = document.getElementById('fortuneModal');
            const closeFortune = document.getElementById('closeFortune');

            function switchTab(tabName) {
                navItems.forEach(item => { item.classList.toggle('active', item.getAttribute('data-tab') === tabName); });
                Object.keys(panels).forEach(k => panels[k].classList.toggle('active', k === tabName));
                mainContent.scrollTop = 0;
                if (navigator.vibrate) navigator.vibrate(5);
            }
            navItems.forEach(item => {
                item.addEventListener('click', function() { const t = this.getAttribute('data-tab'); if (t) switchTab(
                        t); });
            });

            // 命理弹窗
            document.getElementById('fortuneSummary').addEventListener('click', function() {
                fortuneModal.classList.add('open');
                generateFortune();
            });
            closeFortune.addEventListener('click', function() { fortuneModal.classList.remove('open'); });
            fortuneModal.addEventListener('click', function(e) { if (e.target === this) this.classList.remove('open'); });

            // 记录浮层
            fab.addEventListener('click', function(e) {
                e.preventDefault();
                overlay.classList.add('open');
                if (navigator.vibrate) navigator.vibrate(15);
                mainContent.style.transition = 'transform 0.3s ease';
                mainContent.style.transform = 'scale(0.96)';
                setTimeout(() => drawWheel(posX, posY), 50);
            });

            function closeOverlay() {
                overlay.classList.remove('open');
                mainContent.style.transform = 'scale(1)';
                if (navigator.vibrate) navigator.vibrate(8);
                const btn = doneBtn;
                btn.textContent = '☕ 完成记录';
                btn.className = 'btn btn-done';
            }
            cancelBtn.addEventListener('click', closeOverlay);
            overlay.addEventListener('click', function(e) { if (e.target === overlay) closeOverlay(); });

            doneBtn.addEventListener('click', function() {
                if (this.classList.contains('success')) return;
                const flavor = findClosestFlavor(posX, posY);
                if (navigator.vibrate) navigator.vibrate([15, 50, 15]);
                this.textContent = '✅ 已记录！';
                this.className = 'btn btn-done success';
                console.log('☕ 记录成功！风味：' + flavor.main + '，坐标：(' + posX.toFixed(2) + ', ' + posY.toFixed(2) +
                    ')');
                setTimeout(() => { closeOverlay();
                    setTimeout(() => { this.textContent = '☕ 完成记录';
                        this.className = 'btn btn-done'; }, 300); }, 700);
            });

            // 下滑关闭浮层
            let startY = 0;
            overlay.addEventListener('touchstart', function(e) { startY = e.touches[0].clientY; }, { passive: true });
            overlay.addEventListener('touchmove', function(e) {
                if (e.touches[0].clientY - startY > 60 && overlay.classList.contains('open')) closeOverlay();
            }, { passive: true });

            document.addEventListener('keydown', function(e) {
                if (e.key === 'Escape') {
                    if (overlay.classList.contains('open')) closeOverlay();
                    if (fortuneModal.classList.contains('open')) fortuneModal.classList.remove('open');
                }
            });

            // 快捷标签
            document.querySelectorAll('.tag-scroll .tag').forEach(tag => {
                tag.addEventListener('click', function() {
                    document.querySelectorAll('.tag-scroll .tag').forEach(t => t.classList.remove('active'));
                    this.classList.add('active');
                    if (navigator.vibrate) navigator.vibrate(5);
                });
            });

            // 附近推荐刷新
            document.getElementById('refreshBtn').addEventListener('click', function() {
                const reasons = [
                    '💡 大众点评：“花魁手冲花香炸裂，与你今日命理高度匹配。”',
                    '💡 小红书：“冰冲瑰夏柑橘蜂蜜味绝了。”',
                    '💡 用户口碑：“深烘曼特宁的奶油坚果香很正。”',
                    '💡 咖啡群推荐：“肯尼亚 AA 莓果调性突出。”',
                    '💡 最新评价：“埃塞豆子新鲜，草莓香气明显。”',
                ];
                const bubbles = document.querySelectorAll('.shop-card .reason');
                bubbles.forEach((b, i) => { b.textContent = reasons[(i + Math.floor(Math.random() * 3)) % reasons
                        .length]; });
                const pcts = document.querySelectorAll('.shop-card .match-pct');
                const vals = ['🔥 96%', '✨ 89%', '🌱 74%'];
                pcts.forEach((p, i) => { if (i < vals.length) p.textContent = vals[i]; });
                if (navigator.vibrate) navigator.vibrate(8);
                const orig = this.textContent;
                this.textContent = '✅ 已刷新';
                setTimeout(() => { this.textContent = orig; }, 1000);
            });

            // 设置日期
            const now = new Date();
            document.getElementById('todayDate').textContent = (now.getMonth() + 1) + '月' + now.getDate() + '日';

            // 初始命理生成
            setTimeout(generateFortune, 100);

            console.log('☕ 咖啡手记 · 完整版已启动！');
            console.log('📱 包含：轮盘色彩系统 · 命理推荐 · 附近匹配 · 多面板切换');

        })();
    </script>

</body>
</html>
