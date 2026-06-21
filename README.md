<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>咖啡手记 · 完整版 v5.0</title>
    <style>
        /* ===== 全局重置 ===== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --bg-primary: #1a1410;
            --bg-secondary: #2c1e16;
            --bg-card: rgba(44, 30, 22, 0.85);
            --glow-color: rgba(200, 150, 110, 0.15);
            --accent-1: #c89a78;
            --accent-2: #a87b5e;
            --text-primary: #f0e3d8;
            --text-secondary: #a68979;
            --card-border: rgba(255, 215, 180, 0.06);
            --card-border-light: rgba(255, 215, 180, 0.12);
            --nav-blur: rgba(26, 20, 16, 0.78);
            --nav-border: rgba(255, 215, 180, 0.06);
            --success: #4CAF50;
            --warning: #f0a030;
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
        .record-overlay,
        .fortune-modal,
        .onboarding-overlay,
        .share-modal {
            position: relative;
            z-index: 1;
        }

        .main-content {
            flex: 1;
            padding: 16px 16px 100px;
            overflow-y: auto;
            scroll-behavior: smooth;
            position: relative;
        }

        /* ===== 面板 ===== */
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
            transition: all 0.6s ease;
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
        .page-header .header-action {
            font-size: 20px;
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 50%;
            transition: background 0.2s;
            background: none;
            border: none;
            color: var(--text-secondary);
        }
        .page-header .header-action:active {
            background: var(--bg-secondary);
        }

        /* ===== 卡片 ===== */
        .mock-card {
            background: var(--bg-card);
            backdrop-filter: blur(4px);
            -webkit-backdrop-filter: blur(4px);
            border-radius: 16px;
            padding: 16px 16px;
            margin-bottom: 12px;
            border: 1px solid var(--card-border);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
            display: flex;
            align-items: center;
            gap: 14px;
            transition: all 0.6s ease;
            cursor: pointer;
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
        .mock-card .info {
            flex: 1;
            min-width: 0;
        }
        .mock-card .info .title {
            font-size: 15px;
            font-weight: 600;
            color: var(--text-primary);
            transition: color 0.3s ease;
        }
        .mock-card .info .desc {
            font-size: 13px;
            color: var(--text-secondary);
            margin-top: 2px;
            transition: color 0.3s ease;
        }
        .mock-card .info .meta {
            font-size: 11px;
            color: var(--text-secondary);
            opacity: 0.6;
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

        /* ===== 空状态 ===== */
        .empty-state {
            text-align: center;
            padding: 60px 20px 40px;
            animation: fadeSlide 0.4s ease;
        }
        .empty-state .icon {
            font-size: 64px;
            display: block;
            margin-bottom: 16px;
            opacity: 0.7;
        }
        .empty-state .title {
            font-size: 20px;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 6px;
        }
        .empty-state .sub {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 20px;
            line-height: 1.6;
        }
        .empty-state .btn-primary {
            padding: 12px 32px;
            border-radius: 30px;
            border: none;
            background: linear-gradient(145deg, var(--accent-1), var(--accent-2));
            color: #fff;
            font-size: 16px;
            font-weight: 600;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 6px 24px rgba(200, 154, 120, 0.2);
        }
        .empty-state .btn-primary:active {
            transform: scale(0.95);
        }
        .empty-state .progress-track {
            width: 60%;
            max-width: 200px;
            height: 6px;
            background: var(--bg-secondary);
            border-radius: 6px;
            margin: 12px auto 0;
            overflow: hidden;
        }
        .empty-state .progress-track .bar {
            height: 100%;
            background: linear-gradient(90deg, var(--accent-1), var(--accent-2));
            border-radius: 6px;
            transition: width 0.6s ease;
        }

        /* ===== 今日宜喝 ===== */
        .fortune-summary {
            background: var(--bg-card);
            backdrop-filter: blur(4px);
            -webkit-backdrop-filter: blur(4px);
            border-radius: 16px;
            padding: 16px 18px;
            border: 1px solid var(--card-border);
            margin-bottom: 16px;
            display: flex;
            align-items: center;
            gap: 14px;
            transition: all 0.6s ease;
            cursor: pointer;
        }
        .fortune-summary:active {
            transform: scale(0.98);
        }
        .fortune-summary .icon {
            font-size: 32px;
        }
        .fortune-summary .info {
            flex: 1;
        }
        .fortune-summary .info .label {
            font-size: 12px;
            color: var(--text-secondary);
            transition: color 0.3s ease;
        }
        .fortune-summary .info .value {
            font-size: 16px;
            font-weight: 600;
            color: var(--text-primary);
            transition: color 0.3s ease;
        }
        .fortune-summary .match-badge {
            background: rgba(200, 154, 120, 0.12);
            padding: 4px 14px;
            border-radius: 20px;
            color: var(--accent-1);
            font-size: 13px;
            border: 1px solid var(--card-border-light);
            transition: all 0.6s ease;
            flex-shrink: 0;
        }
        .fortune-summary .match-badge.setup {
            color: var(--warning);
            border-color: var(--warning);
            background: rgba(240, 160, 48, 0.1);
        }

        /* ===== 附近推荐 ===== */
        .shop-card {
            background: var(--bg-card);
            backdrop-filter: blur(4px);
            -webkit-backdrop-filter: blur(4px);
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
            font-size: 28px;
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
            transition: color 0.3s ease;
        }
        .shop-card .info .address {
            font-size: 12px;
            color: var(--text-secondary);
            margin-top: 2px;
            transition: color 0.3s ease;
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
            transition: color 0.3s ease;
        }
        .shop-card .right .score small {
            font-size: 11px;
            font-weight: 400;
            color: var(--text-secondary);
            transition: color 0.3s ease;
        }
        .shop-card .right .match-pct {
            font-size: 12px;
            color: var(--accent-1);
            background: rgba(200, 154, 120, 0.06);
            padding: 2px 12px;
            border-radius: 12px;
            border: 1px solid var(--card-border-light);
            transition: all 0.6s ease;
        }
        .section-title {
            font-size: 14px;
            font-weight: 500;
            color: var(--text-secondary);
            margin: 16px 0 12px;
            letter-spacing: 0.5px;
            transition: color 0.6s ease;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .section-title .action-link {
            font-size: 12px;
            color: var(--accent-1);
            cursor: pointer;
            background: none;
            border: none;
            font-family: inherit;
        }
        .section-title .action-link:active {
            opacity: 0.6;
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
            transition: all 0.3s ease;
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
            border-top: 0.5px solid var(--nav-border);
            display: flex;
            align-items: flex-start;
            justify-content: space-around;
            z-index: 100;
            box-shadow: 0 -10px 40px rgba(0, 0, 0, 0.4);
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
            transition: color 0.3s ease;
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
        .nav-item .badge-dot {
            position: absolute;
            top: 6px;
            right: 50%;
            transform: translateX(18px);
            width: 8px;
            height: 8px;
            background: #ff6b6b;
            border-radius: 50%;
            border: 2px solid var(--bg-primary);
        }

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
            background: rgba(10, 6, 4, 0.85);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
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
            background: var(--bg-card);
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
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
        .record-sheet .header-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .record-sheet h2 {
            font-size: 22px;
            font-weight: 600;
            color: var(--text-primary);
            transition: color 0.3s ease;
        }
        .record-sheet .help-btn {
            background: none;
            border: none;
            color: var(--text-secondary);
            font-size: 20px;
            cursor: pointer;
            padding: 4px 8px;
            border-radius: 50%;
            transition: background 0.2s;
        }
        .record-sheet .help-btn:active {
            background: var(--bg-secondary);
        }
        .record-sheet .sub {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 16px;
            transition: color 0.3s ease;
        }

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
            cursor: pointer;
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
            background: var(--success);
            box-shadow: 0 6px 24px rgba(76, 175, 80, 0.3);
        }

        /* ===== 命理弹窗 ===== */
        .fortune-modal {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(10, 6, 4, 0.88);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
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
            max-width: 420px;
            width: 100%;
            background: var(--bg-card);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-radius: 32px;
            padding: 32px 24px 28px;
            border: 1px solid var(--card-border);
            box-shadow: 0 30px 80px rgba(0, 0, 0, 0.8);
            max-height: 92vh;
            overflow-y: auto;
            transition: all 0.6s ease;
        }
        .fortune-modal .card::-webkit-scrollbar {
            width: 3px;
        }
        .fortune-modal .card::-webkit-scrollbar-track {
            background: transparent;
        }
        .fortune-modal .card::-webkit-scrollbar-thumb {
            background: var(--card-border);
            border-radius: 10px;
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
            background: linear-gradient(135deg, var(--text-primary), var(--accent-1));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        .fortune-modal .card .modal-sub {
            text-align: center;
            font-size: 13px;
            color: var(--text-secondary);
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
            background: var(--bg-secondary);
            border: 1px solid var(--card-border);
            color: var(--text-primary);
            font-size: 14px;
            font-family: inherit;
            appearance: none;
            -webkit-appearance: none;
            background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%238a7365' stroke-width='1.5' fill='none'/%3E%3C/svg%3E");
            background-repeat: no-repeat;
            background-position: right 12px center;
            transition: all 0.3s ease;
        }
        .fortune-modal .card .input-group select:focus {
            outline: none;
            border-color: var(--accent-1);
        }
        .fortune-modal .card .input-group select option {
            background: var(--bg-secondary);
            color: var(--text-primary);
        }
        .fortune-modal .card .btn-generate {
            width: 100%;
            padding: 14px;
            border-radius: 14px;
            border: none;
            background: linear-gradient(145deg, var(--accent-1), var(--accent-2));
            color: #fff;
            font-size: 16px;
            font-weight: 600;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s ease;
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
            border: 1px solid var(--card-border);
            display: none;
            transition: all 0.3s ease;
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
            color: var(--accent-1);
            border: 1px solid var(--card-border-light);
            margin-bottom: 8px;
        }
        .fortune-modal .card .result .main {
            font-size: 22px;
            font-weight: 700;
            color: var(--text-primary);
            transition: color 0.3s ease;
        }
        .fortune-modal .card .result .sub {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 10px;
            transition: color 0.3s ease;
        }
        .fortune-modal .card .result .divider {
            height: 1px;
            background: linear-gradient(90deg, transparent, var(--card-border), transparent);
            margin: 12px 0;
        }
        .fortune-modal .card .result .row {
            display: flex;
            justify-content: space-between;
            padding: 4px 0;
            font-size: 13px;
        }
        .fortune-modal .card .result .row .label {
            color: var(--text-secondary);
            transition: color 0.3s ease;
        }
        .fortune-modal .card .result .row .value {
            color: var(--text-primary);
            font-weight: 500;
            transition: color 0.3s ease;
        }
        .fortune-modal .card .result .mindful {
            margin-top: 12px;
            padding: 12px 14px;
            background: rgba(200, 154, 120, 0.04);
            border-radius: 10px;
            border-left: 2px solid var(--accent-1);
            font-size: 14px;
            line-height: 1.6;
            color: var(--text-secondary);
            font-style: italic;
            transition: all 0.3s ease;
        }
        .fortune-modal .card .result .mindful::before {
            content: '🧘 ';
            font-style: normal;
        }

        /* ===== 附近同款推荐（弹窗内） ===== */
        .nearby-shop-list {
            margin-top: 16px;
            padding-top: 16px;
            border-top: 1px solid var(--card-border);
        }
        .nearby-shop-list .nearby-title {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-primary);
            margin-bottom: 12px;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        .nearby-shop-list .nearby-title small {
            font-weight: 400;
            font-size: 12px;
            color: var(--text-secondary);
        }
        .nearby-shop-item {
            background: var(--bg-secondary);
            border-radius: 12px;
            padding: 12px 14px;
            margin-bottom: 10px;
            border: 1px solid var(--card-border);
            display: flex;
            align-items: center;
            gap: 12px;
            transition: all 0.3s ease;
            cursor: pointer;
        }
        .nearby-shop-item:active {
            transform: scale(0.98);
        }
        .nearby-shop-item .shop-icon {
            font-size: 24px;
            flex-shrink: 0;
            width: 40px;
            height: 40px;
            border-radius: 10px;
            background: var(--bg-card);
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .nearby-shop-item .shop-info {
            flex: 1;
            min-width: 0;
        }
        .nearby-shop-item .shop-info .shop-name {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-primary);
        }
        .nearby-shop-item .shop-info .shop-desc {
            font-size: 12px;
            color: var(--text-secondary);
            margin-top: 1px;
        }
        .nearby-shop-item .shop-info .shop-tags {
            display: flex;
            gap: 4px;
            margin-top: 4px;
            flex-wrap: wrap;
        }
        .nearby-shop-item .shop-info .shop-tags span {
            font-size: 10px;
            padding: 1px 10px;
            border-radius: 10px;
            background: rgba(200, 154, 120, 0.08);
            border: 1px solid var(--card-border);
            color: var(--text-secondary);
        }
        .nearby-shop-item .shop-info .shop-tags .highlight-tag {
            border-color: var(--accent-1);
            color: var(--accent-1);
        }
        .nearby-shop-item .shop-distance {
            font-size: 12px;
            color: var(--text-secondary);
            flex-shrink: 0;
            white-space: nowrap;
        }
        .nearby-empty {
            text-align: center;
            color: var(--text-secondary);
            font-size: 13px;
            padding: 16px 0;
            opacity: 0.6;
        }

        .fortune-modal .card .close-btn {
            width: 100%;
            margin-top: 16px;
            padding: 12px;
            border-radius: 14px;
            border: 1px solid var(--card-border);
            background: transparent;
            color: var(--text-secondary);
            font-size: 15px;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .fortune-modal .card .close-btn:active {
            background: var(--bg-secondary);
            transform: scale(0.96);
        }

        /* ===== 新手引导遮罩 ===== */
        .onboarding-overlay {
            position: fixed;
            inset: 0;
            background: rgba(10, 6, 4, 0.75);
            backdrop-filter: blur(6px);
            -webkit-backdrop-filter: blur(6px);
            z-index: 500;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 32px 24px;
            animation: fadeIn 0.4s ease;
        }
        .onboarding-overlay.open {
            display: flex;
        }
        .onboarding-overlay .onboarding-card {
            max-width: 380px;
            width: 100%;
            background: var(--bg-card);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-radius: 32px;
            padding: 36px 28px 28px;
            border: 1px solid var(--card-border);
            text-align: center;
            animation: slideUp 0.5s cubic-bezier(0.32, 0.72, 0, 1);
        }
        .onboarding-overlay .onboarding-card .step-icon {
            font-size: 56px;
            display: block;
            margin-bottom: 12px;
        }
        .onboarding-overlay .onboarding-card .step-title {
            font-size: 22px;
            font-weight: 700;
            color: var(--text-primary);
            margin-bottom: 6px;
        }
        .onboarding-overlay .onboarding-card .step-desc {
            font-size: 14px;
            color: var(--text-secondary);
            line-height: 1.6;
            margin-bottom: 20px;
        }
        .onboarding-overlay .onboarding-card .step-dots {
            display: flex;
            justify-content: center;
            gap: 8px;
            margin-bottom: 20px;
        }
        .onboarding-overlay .onboarding-card .step-dots .dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: var(--card-border);
            transition: all 0.3s ease;
        }
        .onboarding-overlay .onboarding-card .step-dots .dot.active {
            background: var(--accent-1);
            width: 24px;
            border-radius: 4px;
        }
        .onboarding-overlay .onboarding-card .btn-group {
            display: flex;
            gap: 12px;
        }
        .onboarding-overlay .onboarding-card .btn-group .btn-primary {
            flex: 1;
            padding: 14px;
            border-radius: 16px;
            border: none;
            background: linear-gradient(145deg, var(--accent-1), var(--accent-2));
            color: #fff;
            font-size: 16px;
            font-weight: 600;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .onboarding-overlay .onboarding-card .btn-group .btn-primary:active {
            transform: scale(0.96);
        }
        .onboarding-overlay .onboarding-card .btn-group .btn-skip {
            padding: 14px 20px;
            border-radius: 16px;
            border: 1px solid var(--card-border);
            background: transparent;
            color: var(--text-secondary);
            font-size: 14px;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .onboarding-overlay .onboarding-card .btn-group .btn-skip:active {
            background: var(--bg-secondary);
        }

        /* ===== 分享弹窗 ===== */
        .share-modal {
            position: fixed;
            inset: 0;
            background: rgba(10, 6, 4, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            z-index: 400;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 24px;
            animation: fadeIn 0.3s ease;
        }
        .share-modal.open {
            display: flex;
        }
        .share-modal .share-card {
            max-width: 380px;
            width: 100%;
            background: var(--bg-card);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-radius: 24px;
            padding: 28px 24px 24px;
            border: 1px solid var(--card-border);
            text-align: center;
        }
        .share-modal .share-card .preview {
            background: var(--bg-secondary);
            border-radius: 16px;
            padding: 24px 20px;
            margin-bottom: 20px;
            border: 1px solid var(--card-border);
        }
        .share-modal .share-card .preview .icon {
            font-size: 48px;
            display: block;
            margin-bottom: 8px;
        }
        .share-modal .share-card .preview .title {
            font-size: 18px;
            font-weight: 700;
            color: var(--text-primary);
        }
        .share-modal .share-card .preview .sub {
            font-size: 13px;
            color: var(--text-secondary);
            margin-top: 4px;
        }
        .share-modal .share-card .preview .flavor-tags {
            display: flex;
            justify-content: center;
            gap: 8px;
            margin-top: 10px;
            flex-wrap: wrap;
        }
        .share-modal .share-card .preview .flavor-tags span {
            font-size: 11px;
            padding: 4px 14px;
            border-radius: 20px;
            background: var(--bg-card);
            border: 1px solid var(--card-border);
            color: var(--text-secondary);
        }
        .share-modal .share-card .share-channels {
            display: flex;
            gap: 12px;
            justify-content: center;
            flex-wrap: wrap;
        }
        .share-modal .share-card .share-channels button {
            padding: 10px 20px;
            border-radius: 30px;
            border: 1px solid var(--card-border);
            background: var(--bg-secondary);
            color: var(--text-primary);
            font-size: 14px;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .share-modal .share-card .share-channels button:active {
            transform: scale(0.95);
            background: var(--bg-card);
        }
        .share-modal .share-card .share-channels button .emoji {
            font-size: 18px;
        }
        .share-modal .share-card .close-share {
            margin-top: 16px;
            padding: 10px;
            border-radius: 30px;
            border: none;
            background: transparent;
            color: var(--text-secondary);
            font-size: 14px;
            font-family: inherit;
            cursor: pointer;
            width: 100%;
        }
        .share-modal .share-card .close-share:active {
            opacity: 0.6;
        }

        /* ===== 反馈弹窗 ===== */
        .feedback-modal {
            position: fixed;
            inset: 0;
            background: rgba(10, 6, 4, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            z-index: 450;
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 24px;
            animation: fadeIn 0.3s ease;
        }
        .feedback-modal.open {
            display: flex;
        }
        .feedback-modal .feedback-card {
            max-width: 400px;
            width: 100%;
            background: var(--bg-card);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border-radius: 28px;
            padding: 28px 24px 24px;
            border: 1px solid var(--card-border);
        }
        .feedback-modal .feedback-card .fb-title {
            font-size: 20px;
            font-weight: 700;
            color: var(--text-primary);
            text-align: center;
            margin-bottom: 4px;
        }
        .feedback-modal .feedback-card .fb-sub {
            font-size: 13px;
            color: var(--text-secondary);
            text-align: center;
            margin-bottom: 16px;
        }
        .feedback-modal .feedback-card .fb-type {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            margin-bottom: 14px;
            justify-content: center;
        }
        .feedback-modal .feedback-card .fb-type button {
            padding: 6px 16px;
            border-radius: 20px;
            border: 1px solid var(--card-border);
            background: var(--bg-secondary);
            color: var(--text-secondary);
            font-size: 13px;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .feedback-modal .feedback-card .fb-type button:active {
            transform: scale(0.94);
        }
        .feedback-modal .feedback-card .fb-type button.active {
            border-color: var(--accent-1);
            color: var(--accent-1);
            background: rgba(200, 154, 120, 0.06);
        }
        .feedback-modal .feedback-card textarea {
            width: 100%;
            min-height: 100px;
            padding: 14px;
            border-radius: 14px;
            background: var(--bg-secondary);
            border: 1px solid var(--card-border);
            color: var(--text-primary);
            font-size: 14px;
            font-family: inherit;
            resize: vertical;
            transition: border-color 0.3s;
        }
        .feedback-modal .feedback-card textarea:focus {
            outline: none;
            border-color: var(--accent-1);
        }
        .feedback-modal .feedback-card textarea::placeholder {
            color: var(--text-secondary);
            opacity: 0.5;
        }
        .feedback-modal .feedback-card .fb-actions {
            display: flex;
            gap: 12px;
            margin-top: 14px;
        }
        .feedback-modal .feedback-card .fb-actions button {
            flex: 1;
            padding: 14px;
            border-radius: 14px;
            border: none;
            font-size: 16px;
            font-weight: 600;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .feedback-modal .feedback-card .fb-actions .fb-cancel {
            background: var(--bg-secondary);
            color: var(--text-secondary);
            border: 1px solid var(--card-border);
        }
        .feedback-modal .feedback-card .fb-actions .fb-cancel:active {
            transform: scale(0.96);
        }
        .feedback-modal .feedback-card .fb-actions .fb-submit {
            background: linear-gradient(145deg, var(--accent-1), var(--accent-2));
            color: #fff;
            box-shadow: 0 4px 20px rgba(200, 154, 120, 0.2);
        }
        .feedback-modal .feedback-card .fb-actions .fb-submit:active {
            transform: scale(0.96);
        }
        .feedback-modal .feedback-card .fb-success {
            display: none;
            text-align: center;
            padding: 20px 0;
        }
        .feedback-modal .feedback-card .fb-success .icon {
            font-size: 48px;
            display: block;
            margin-bottom: 8px;
        }
        .feedback-modal .feedback-card .fb-success .title {
            font-size: 18px;
            font-weight: 600;
            color: var(--text-primary);
        }
        .feedback-modal .feedback-card .fb-success .sub {
            font-size: 13px;
            color: var(--text-secondary);
            margin-top: 4px;
        }

        /* ===== Toast 通知 ===== */
        .toast {
            position: fixed;
            bottom: 100px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--bg-card);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            padding: 12px 24px;
            border-radius: 16px;
            border: 1px solid var(--card-border);
            color: var(--text-primary);
            font-size: 14px;
            z-index: 600;
            opacity: 0;
            transition: all 0.4s cubic-bezier(0.32, 0.72, 0, 1);
            pointer-events: none;
            box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4);
            max-width: 90%;
        }
        .toast.show {
            opacity: 1;
            transform: translateX(-50%) translateY(-10px);
        }

        /* ===== 设置页简化 ===== */
        .settings-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 14px 16px;
            background: var(--bg-card);
            border-radius: 14px;
            border: 1px solid var(--card-border);
            margin-bottom: 8px;
            transition: all 0.6s ease;
            cursor: pointer;
        }
        .settings-item:active {
            transform: scale(0.98);
        }
        .settings-item .left {
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .settings-item .left .icon {
            font-size: 20px;
        }
        .settings-item .left .label {
            font-size: 15px;
            color: var(--text-primary);
        }
        .settings-item .right {
            color: var(--text-secondary);
            font-size: 14px;
        }
        .settings-item .right .arrow {
            opacity: 0.3;
        }
        .settings-divider {
            height: 1px;
            background: var(--card-border);
            margin: 12px 0;
        }
        .settings-version {
            text-align: center;
            color: var(--text-secondary);
            font-size: 12px;
            opacity: 0.3;
            padding: 16px 0 8px;
        }

        /* ===== 推送授权提示 ===== */
        .push-prompt {
            background: var(--bg-card);
            backdrop-filter: blur(4px);
            -webkit-backdrop-filter: blur(4px);
            border-radius: 16px;
            padding: 16px 18px;
            border: 1px solid var(--card-border);
            margin-bottom: 16px;
            display: none;
            align-items: center;
            gap: 14px;
            transition: all 0.6s ease;
        }
        .push-prompt.visible {
            display: flex;
        }
        .push-prompt .icon {
            font-size: 28px;
        }
        .push-prompt .info {
            flex: 1;
        }
        .push-prompt .info .title {
            font-size: 14px;
            font-weight: 600;
            color: var(--text-primary);
        }
        .push-prompt .info .desc {
            font-size: 12px;
            color: var(--text-secondary);
        }
        .push-prompt .btn-enable {
            padding: 6px 18px;
            border-radius: 20px;
            border: none;
            background: linear-gradient(145deg, var(--accent-1), var(--accent-2));
            color: #fff;
            font-size: 13px;
            font-family: inherit;
            cursor: pointer;
            transition: all 0.3s ease;
            flex-shrink: 0;
        }
        .push-prompt .btn-enable:active {
            transform: scale(0.94);
        }
        .push-prompt .btn-dismiss {
            background: none;
            border: none;
            color: var(--text-secondary);
            font-size: 18px;
            cursor: pointer;
            padding: 4px;
        }
        .push-prompt .btn-dismiss:active {
            opacity: 0.5;
        }
    </style>
</head>
<body>

    <!-- ===== 氛围光晕 ===== -->
    <div class="ambient-glow" id="ambientGlow"></div>

    <!-- ===== Toast 通知 ===== -->
    <div class="toast" id="toast"></div>

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

            <!-- 推送授权提示 -->
            <div class="push-prompt" id="pushPrompt">
                <span class="icon">🔔</span>
                <div class="info">
                    <div class="title">开启每日宜喝提醒</div>
                    <div class="desc">每天早晨 8:00 推送今日专属咖啡指引</div>
                </div>
                <button class="btn-enable" id="enablePush">开启</button>
                <button class="btn-dismiss" id="dismissPush">✕</button>
            </div>

            <!-- 今日宜喝 -->
            <div class="fortune-summary" id="fortuneSummary">
                <span class="icon">🔮</span>
                <div class="info">
                    <div class="label">今日宜喝</div>
                    <div class="value" id="summaryBean">设置生辰，获取专属指引</div>
                </div>
                <span class="match-badge setup" id="summaryBadge">⚙️ 设置</span>
            </div>

            <!-- 记录列表 -->
            <div id="homeContent">
                <!-- 由JS动态渲染 -->
            </div>
        </div>

        <!-- ===== 面板：人格 ===== -->
        <div class="panel" id="panel-personality">
            <div class="page-header">
                <h1>🧠 咖啡人格</h1>
                <span class="date" id="personalityDate">进化中</span>
            </div>
            <div id="personalityContent">
                <!-- 由JS动态渲染 -->
            </div>
        </div>

        <!-- ===== 面板：同好 ===== -->
        <div class="panel" id="panel-social">
            <div class="page-header">
                <h1>🌊 同好漂流瓶</h1>
                <span class="date" id="socialDate">今日匹配</span>
            </div>
            <div id="socialContent">
                <!-- 由JS动态渲染 -->
            </div>
        </div>

        <!-- ===== 面板：报告 ===== -->
        <div class="panel" id="panel-report">
            <div class="page-header">
                <h1>📊 风味报告</h1>
                <span class="date" id="reportDate">6 月趋势</span>
            </div>
            <div id="reportContent">
                <!-- 由JS动态渲染 -->
            </div>
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
            <div class="header-row">
                <h2>☕ 冲煮手记</h2>
                <button class="help-btn" id="helpBtn">❓</button>
            </div>
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
    <!-- 命理弹窗（含附近同款推荐） -->
    <!-- ========================================================= -->
    <div class="fortune-modal" id="fortuneModal">
        <div class="card">
            <div class="compass">
                <svg viewBox="0 0 100 100">
                    <circle cx="50" cy="50" r="46" fill="none" stroke="var(--card-border)" stroke-width="1"/>
                    <circle cx="50" cy="50" r="38" fill="none" stroke="var(--card-border)" stroke-width="0.5" stroke-dasharray="4 4"/>
                    <line x1="50" y1="4" x2="50" y2="20" stroke="var(--accent-1)" stroke-width="1.5"/>
                    <line x1="50" y1="80" x2="50" y2="96" stroke="var(--accent-1)" stroke-width="1.5"/>
                    <line x1="4" y1="50" x2="20" y2="50" stroke="var(--accent-1)" stroke-width="1.5"/>
                    <line x1="80" y1="50" x2="96" y2="50" stroke="var(--accent-1)" stroke-width="1.5"/>
                    <text x="50" y="14" text-anchor="middle" fill="var(--text-secondary)" font-size="6">北</text>
                    <text x="50" y="94" text-anchor="middle" fill="var(--text-secondary)" font-size="6">南</text>
                    <text x="10" y="54" text-anchor="middle" fill="var(--text-secondary)" font-size="6">西</text>
                    <text x="90" y="54" text-anchor="middle" fill="var(--text-secondary)" font-size="6">东</text>
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
            <button class="btn-generate" id="generateBtn">🔮 查看今日解读</button>
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

                <div class="nearby-shop-list" id="nearbyShopList">
                    <div class="nearby-title">
                        📍 附近同款推荐
                        <small>· 根据今日宜喝匹配</small>
                    </div>
                    <div id="nearbyShopsContainer"></div>
                </div>

                <!-- 分享按钮 -->
                <button class="btn-generate" id="shareFortuneBtn" style="margin-top:12px; width:100%;">📤 分享今日宜喝</button>
            </div>
            <button class="close-btn" id="closeFortune">✕ 关闭</button>
        </div>
    </div>

    <!-- ========================================================= -->
    <!-- 新手引导 -->
    <!-- ========================================================= -->
    <div class="onboarding-overlay" id="onboardingOverlay">
        <div class="onboarding-card">
            <span class="step-icon" id="obIcon">☕</span>
            <div class="step-title" id="obTitle">欢迎来到咖啡手记</div>
            <div class="step-desc" id="obDesc">记录每一杯咖啡，发现你的风味人格</div>
            <div class="step-dots" id="obDots">
                <span class="dot active"></span>
                <span class="dot"></span>
                <span class="dot"></span>
                <span class="dot"></span>
            </div>
            <div class="btn-group">
                <button class="btn-skip" id="obSkip">跳过</button>
                <button class="btn-primary" id="obNext">下一步 →</button>
            </div>
        </div>
    </div>

    <!-- ========================================================= -->
    <!-- 分享弹窗 -->
    <!-- ========================================================= -->
    <div class="share-modal" id="shareModal">
        <div class="share-card">
            <div class="preview" id="sharePreview">
                <span class="icon">☕</span>
                <div class="title" id="shareTitle">埃塞俄比亚·花魁</div>
                <div class="sub" id="shareSub">明亮果酸 · 花香 · 茶感</div>
                <div class="flavor-tags" id="shareTags">
                    <span>🔥 今日宜喝</span>
                    <span>✨ 命理推荐</span>
                </div>
            </div>
            <div class="share-channels">
                <button onclick="showToast('📱 已复制分享链接')"><span class="emoji">📋</span> 复制链接</button>
                <button onclick="showToast('📸 已保存到相册')"><span class="emoji">📸</span> 保存图片</button>
                <button onclick="showToast('💬 已分享到微信')"><span class="emoji">💬</span> 微信</button>
            </div>
            <button class="close-share" id="closeShare">✕ 关闭</button>
        </div>
    </div>

    <!-- ========================================================= -->
    <!-- 反馈弹窗 -->
    <!-- ========================================================= -->
    <div class="feedback-modal" id="feedbackModal">
        <div class="feedback-card">
            <div id="fbForm">
                <div class="fb-title">💬 意见反馈</div>
                <div class="fb-sub">你的每一份反馈都在让咖啡手记变得更好</div>
                <div class="fb-type" id="fbTypeGroup">
                    <button data-type="bug">🐛 Bug反馈</button>
                    <button data-type="idea">💡 功能建议</button>
                    <button data-type="content">📝 内容纠错</button>
                    <button data-type="other">❤️ 其他</button>
                </div>
                <textarea id="fbContent" placeholder="请详细描述你的问题或建议...（至少10个字）"></textarea>
                <div class="fb-actions">
                    <button class="fb-cancel" id="fbCancel">取消</button>
                    <button class="fb-submit" id="fbSubmit">提交反馈</button>
                </div>
            </div>
            <div class="fb-success" id="fbSuccess">
                <span class="icon">🎉</span>
                <div class="title">收到你的反馈！</div>
                <div class="sub">我们会认真查看，感谢你的支持 💪</div>
                <button class="btn-generate" style="margin-top:16px; width:100%;" id="fbDone">好的</button>
            </div>
        </div>
    </div>

    <!-- ========================================================= -->
    <!-- JavaScript -->
    <!-- ========================================================= -->
    <script>
        (function() {
            'use strict';

            // =========================================================
            // 工具函数
            // =========================================================
            function hexToRgb(hex) {
                const r = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
                return r ? { r: parseInt(r[1], 16), g: parseInt(r[2], 16), b: parseInt(r[3], 16) } : { r: 200, g: 150,
                    b: 110 };
            }

            function rgbToHex(r, g, b) {
                const c = (v) => Math.max(0, Math.min(255, Math.round(v)));
                return '#' + [c(r), c(g), c(b)].map(v => v.toString(16).padStart(2, '0')).join('');
            }

            function showToast(msg) {
                const t = document.getElementById('toast');
                t.textContent = msg;
                t.classList.add('show');
                clearTimeout(t._timer);
                t._timer = setTimeout(() => t.classList.remove('show'), 2500);
            }

            // =========================================================
            // 一、咖啡豆数据库（50+款）
            // =========================================================
            const COFFEE_BEANS = [
                // 埃塞俄比亚
                { id: 1, name: '花魁', full: '埃塞俄比亚·花魁 G1 日晒', origin: '埃塞俄比亚', region: '古吉·罕贝拉', process: '日晒',
                    roast: '浅烘', vector: [8, 6, 2, 4, 8, 9, 3, 3], desc: '明亮果酸 · 花香 · 茶感' },
                { id: 2, name: '瑰夏', full: '巴拿马·瑰夏 水洗', origin: '巴拿马', region: '波奎特', process: '水洗', roast: '浅烘',
                    vector: [9, 7, 2, 3, 8, 9, 2, 2], desc: '茉莉花香 · 柑橘 · 蜂蜜' },
                { id: 3, name: '耶加雪菲', full: '埃塞俄比亚·耶加雪菲 G2 水洗', origin: '埃塞俄比亚', region: '耶加雪菲', process: '水洗',
                    roast: '浅烘', vector: [7, 6, 2, 4, 7, 7, 3, 3], desc: '柑橘 · 茉莉 · 茶感' },
                { id: 4, name: '西达摩', full: '埃塞俄比亚·西达摩 G1 日晒', origin: '埃塞俄比亚', region: '西达摩', process: '日晒',
                    roast: '中浅烘', vector: [6, 7, 3, 5, 7, 6, 4, 4], desc: '莓果 · 巧克力 · 香料' },
                // 肯尼亚
                { id: 5, name: '肯尼亚 AA', full: '肯尼亚·AA 水洗', origin: '肯尼亚', region: '涅里', process: '水洗', roast: '中烘',
                    vector: [8, 5, 3, 5, 8, 4, 3, 5], desc: '莓果 · 红酒韵 · 明亮' },
                { id: 6, name: '肯尼亚 SL28', full: '肯尼亚·SL28 水洗', origin: '肯尼亚', region: '基安布', process: '水洗',
                    roast: '中浅烘', vector: [7, 6, 3, 5, 7, 5, 3, 4], desc: '黑醋栗 · 乌梅 · 甜感' },
                { id: 7, name: '肯尼亚 SL34', full: '肯尼亚·SL34 水洗', origin: '肯尼亚', region: '穆兰加', process: '水洗',
                    roast: '中烘', vector: [7, 5, 4, 6, 6, 4, 4, 4], desc: '番茄 · 莓果 · 酸质明亮' },
                // 哥伦比亚
                { id: 8, name: '蕙兰', full: '哥伦比亚·蕙兰 水洗', origin: '哥伦比亚', region: '蕙兰', process: '水洗', roast: '中浅烘',
                    vector: [6, 7, 4, 6, 5, 4, 7, 3], desc: '黑巧 · 坚果 · 醇厚' },
                { id: 9, name: '娜玲珑', full: '哥伦比亚·娜玲珑 水洗', origin: '哥伦比亚', region: '娜玲珑', process: '水洗',
                    roast: '中烘', vector: [5, 7, 5, 7, 4, 3, 7, 3], desc: '巧克力 · 焦糖 · 均衡' },
                { id: 10, name: '考卡', full: '哥伦比亚·考卡 水洗', origin: '哥伦比亚', region: '考卡', process: '水洗', roast: '中烘',
                    vector: [6, 6, 4, 6, 5, 4, 6, 3], desc: '坚果 · 巧克力 · 柔和' },
                // 巴西
                { id: 11, name: '黄波旁', full: '巴西·黄波旁 日晒', origin: '巴西', region: '米纳斯吉拉斯', process: '日晒',
                    roast: '中深烘', vector: [4, 7, 6, 7, 3, 2, 8, 3], desc: '坚果 · 可可 · 醇厚扎实' },
                { id: 12, name: '红波旁', full: '巴西·红波旁 日晒', origin: '巴西', region: '圣保罗', process: '日晒', roast: '中烘',
                    vector: [4, 7, 5, 7, 3, 2, 7, 3], desc: '巧克力 · 坚果 · 均衡' },
                { id: 13, name: '卡杜艾', full: '巴西·卡杜艾 日晒', origin: '巴西', region: '巴拉那', process: '日晒', roast: '中烘',
                    vector: [5, 6, 5, 6, 4, 3, 7, 3], desc: '坚果 · 巧克力 · 柔和' },
                // 印尼
                { id: 14, name: '曼特宁', full: '印尼·曼特宁 湿刨', origin: '印尼', region: '苏门答腊', process: '湿刨',
                    roast: '深烘', vector: [3, 7, 8, 9, 2, 1, 9, 4], desc: '草本 · 醇厚 · 低酸' },
                { id: 15, name: '爪哇', full: '印尼·爪哇 水洗', origin: '印尼', region: '爪哇', process: '水洗', roast: '中深烘',
                    vector: [4, 6, 7, 8, 3, 2, 8, 3], desc: '香料 · 草本 · 醇厚' },
                { id: 16, name: '苏拉威西', full: '印尼·苏拉威西 水洗', origin: '印尼', region: '苏拉威西', process: '水洗',
                    roast: '中深烘', vector: [4, 6, 6, 8, 3, 3, 7, 3], desc: '草本 · 巧克力 · 醇厚' },
                // 危地马拉
                { id: 17, name: '安提瓜', full: '危地马拉·安提瓜 水洗', origin: '危地马拉', region: '安提瓜', process: '水洗',
                    roast: '中烘', vector: [6, 7, 5, 7, 4, 3, 7, 3], desc: '蜂蜜 · 甜醇 · 圆润' },
                { id: 18, name: '薇薇特南果', full: '危地马拉·薇薇特南果 水洗', origin: '危地马拉', region: '薇薇特南果', process: '水洗',
                    roast: '中浅烘', vector: [7, 6, 4, 5, 5, 5, 5, 3], desc: '果香 · 巧克力 · 甜感' },
                // 中国云南
                { id: 19, name: '云南卡蒂姆', full: '中国·云南 卡蒂姆 水洗', origin: '中国', region: '云南·保山', process: '水洗',
                    roast: '中烘', vector: [5, 6, 5, 6, 4, 3, 6, 3], desc: '坚果 · 巧克力 · 柔和' },
                { id: 20, name: '云南铁皮卡', full: '中国·云南 铁皮卡 日晒', origin: '中国', region: '云南·普洱', process: '日晒',
                    roast: '中浅烘', vector: [6, 7, 4, 5, 5, 4, 5, 3], desc: '果香 · 甜感 · 茶韵' },
                // 哥斯达黎加
                { id: 21, name: '哥斯达黎加·蜜处理', full: '哥斯达黎加·蜜处理', origin: '哥斯达黎加', region: '塔拉珠', process: '蜜处理',
                    roast: '中烘', vector: [6, 8, 4, 6, 5, 4, 5, 4], desc: '蜂蜜 · 焦糖 · 圆润' },
                // 拼配豆
                { id: 22, name: '经典意式拼配', full: '经典意式拼配 深烘', origin: '多产地拼配', region: '-', process: '拼配',
                    roast: '深烘', vector: [3, 6, 8, 9, 3, 2, 8, 4], desc: '浓郁 · 巧克力 · 醇厚' },
                { id: 23, name: '手冲拼配·花韵', full: '手冲拼配·花韵 中浅烘', origin: '多产地拼配', region: '-', process: '拼配',
                    roast: '中浅烘', vector: [7, 6, 3, 5, 7, 7, 4, 3], desc: '花香 · 果酸 · 层次丰富' },
                { id: 24, name: '季节限定·秋实', full: '季节限定·秋实 中烘', origin: '多产地拼配', region: '-', process: '拼配',
                    roast: '中烘', vector: [5, 7, 5, 7, 5, 4, 7, 4], desc: '坚果 · 甜感 · 温暖' },
                // 更多埃塞
                { id: 25, name: '古吉·罕贝拉', full: '埃塞俄比亚·古吉·罕贝拉 日晒', origin: '埃塞俄比亚', region: '古吉', process: '日晒',
                    roast: '浅烘', vector: [8, 6, 2, 4, 8, 8, 3, 4], desc: '草莓 · 花香 · 酒韵' },
                { id: 26, name: '乌拉嘎', full: '埃塞俄比亚·乌拉嘎 水洗', origin: '埃塞俄比亚', region: '乌拉嘎', process: '水洗',
                    roast: '浅烘', vector: [8, 5, 2, 4, 7, 8, 3, 3], desc: '柑橘 · 茉莉 · 茶感' },
                // 更多哥伦比亚
                { id: 27, name: '哥伦比亚·粉波旁', full: '哥伦比亚·粉波旁 水洗', origin: '哥伦比亚', region: '蕙兰', process: '水洗',
                    roast: '中浅烘', vector: [7, 7, 3, 5, 6, 5, 5, 3], desc: '水果 · 焦糖 · 甜感' },
                // 更多肯尼亚
                { id: 28, name: '肯尼亚·水洗AA', full: '肯尼亚·水洗AA', origin: '肯尼亚', region: '涅里', process: '水洗',
                    roast: '中烘', vector: [8, 5, 3, 5, 7, 4, 3, 5], desc: '黑莓 · 乌梅 · 明亮' },
                // 巴拿马
                { id: 29, name: '巴拿马·瑰夏', full: '巴拿马·瑰夏 日晒', origin: '巴拿马', region: '波奎特', process: '日晒',
                    roast: '浅烘', vector: [9, 7, 2, 3, 9, 9, 2, 2], desc: '玫瑰 · 柑橘 · 蜂蜜' },
                // 洪都拉斯
                { id: 30, name: '洪都拉斯·SHB', full: '洪都拉斯·SHB 水洗', origin: '洪都拉斯', region: '马卡拉', process: '水洗',
                    roast: '中烘', vector: [5, 6, 5, 6, 5, 4, 6, 3], desc: '巧克力 · 坚果 · 均衡' },
            ];

            // =========================================================
            // 二、附近店铺数据
            // =========================================================
            const NEARBY_SHOPS = [
                { id: 1, name: '叁舍咖啡 · 手冲专门店', icon: '☕', address: '天河路 228 号', distance: '320m',
                    beans: ['花魁', '瑰夏', '肯尼亚 AA'], tags: ['浅烘', '手冲', 'SOE'], rating: 4.7,
                comment: '花魁手冲花香炸裂，与你今日命理高度匹配' },
                { id: 2, name: '瑰夏·花园咖啡', icon: '🌸', address: '花城大道 89 号', distance: '480m',
                    beans: ['瑰夏', '花魁', '哥伦比亚'], tags: ['中浅烘', 'SOE', '冰冲'], rating: 4.5,
                comment: '冰冲瑰夏柑橘蜂蜜味绝了' },
                { id: 3, name: '豆仓 · 精品咖啡', icon: '🌰', address: '体育西路 102 号', distance: '650m',
                    beans: ['曼特宁', '巴西', '哥伦比亚'], tags: ['深烘', '意式', '拼配'], rating: 4.3,
                comment: '深烘曼特宁奶油坚果香很正' },
                { id: 4, name: '黑胶咖啡 · 肯尼亚专场', icon: '🍇', address: '黄埔大道 56 号', distance: '820m',
                    beans: ['肯尼亚 AA', '埃塞', '花魁'], tags: ['中烘', '水洗', '单品'], rating: 4.2,
                comment: '肯尼亚AA莓果调性突出，酒韵悠长' },
                { id: 5, name: '微光 · 咖啡实验室', icon: '🔬', address: '珠江新城 花城汇 负一层', distance: '1.1km',
                    beans: ['瑰夏', '埃塞', '秘鲁'], tags: ['浅烘', '手冲', '创意特调'], rating: 4.6,
                comment: '埃塞豆子新鲜，草莓香气明显' },
                { id: 6, name: '雲啡 · 云南精品', icon: '🌿', address: '体育东路 45 号', distance: '950m',
                    beans: ['云南卡蒂姆', '云南铁皮卡'], tags: ['中烘', '水洗', '日晒'], rating: 4.4,
                comment: '云南豆子的花果香和茶韵很独特' },
                { id: 7, name: '果核咖啡 · 烘焙工坊', icon: '🫘', address: '天河城 5 楼', distance: '780m',
                    beans: ['经典意式拼配', '手冲拼配·花韵'], tags: ['深烘', '中浅烘', '拼配'], rating: 4.5,
                comment: '意式浓缩油脂丰富，手冲拼配层次感强' },
            ];

            // =========================================================
            // 三、动态色彩系统
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

            function getColorBrightness(hex) {
                const rgb = hexToRgb(hex);
                return (rgb.r * 299 + rgb.g * 587 + rgb.b * 114) / 1000;
            }

            function getContrastText(hex) {
                return getColorBrightness(hex) > 160 ? '#1a1410' : '#f0e3d8';
            }

            function getBorderColor(hex, opacity) {
                const rgb = hexToRgb(hex);
                return `rgba(${rgb.r}, ${rgb.g}, ${rgb.b}, ${opacity || 0.06})`;
            }

            function getCardBg(hex, opacity) {
                const rgb = hexToRgb(hex);
                return `rgba(${rgb.r}, ${rgb.g}, ${rgb.b}, ${opacity || 0.85})`;
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
                    return {
                        primary: '#1a1410',
                        secondary: '#2c1e16',
                        cardBg: 'rgba(44,30,22,0.85)',
                        glow: 'rgba(200,150,110,0.15)',
                        accent1: '#c89a78',
                        accent2: '#a87b5e',
                        textPrimary: '#f0e3d8',
                        textSecondary: '#a68979',
                        border: 'rgba(255,215,180,0.06)',
                        borderLight: 'rgba(255,215,180,0.12)',
                    };
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

                const primary = rgbToHex(
                    Math.max(10, Math.min(245, mr - 25)),
                    Math.max(10, Math.min(245, mg - 20)),
                    Math.max(10, Math.min(245, mb - 15))
                );
                const secondary = rgbToHex(
                    Math.max(10, Math.min(245, mr + 25)),
                    Math.max(10, Math.min(245, mg + 30)),
                    Math.max(10, Math.min(245, mb + 35))
                );

                const accent1 = rgbToHex(
                    Math.max(10, Math.min(255, mr + 45)),
                    Math.max(10, Math.min(255, mg - 5)),
                    Math.max(10, Math.min(255, mb - 25))
                );
                const accent2 = rgbToHex(
                    Math.max(10, Math.min(255, mr - 15)),
                    Math.max(10, Math.min(255, mg + 25)),
                    Math.max(10, Math.min(255, mb + 15))
                );

                const glow = `rgba(${mr}, ${mg}, ${mb}, 0.15)`;
                const textPrimary = getContrastText(primary);
                const textSecondary = getContrastText(secondary);
                const border = getBorderColor(primary, 0.06);
                const borderLight = getBorderColor(primary, 0.12);
                const cardBg = getCardBg(primary, 0.85);

                return {
                    primary,
                    secondary,
                    cardBg,
                    glow,
                    accent1,
                    accent2,
                    textPrimary,
                    textSecondary,
                    border,
                    borderLight,
                };
            }

            function applyColors(colors) {
                const root = document.documentElement;
                root.style.setProperty('--bg-primary', colors.primary);
                root.style.setProperty('--bg-secondary', colors.secondary);
                root.style.setProperty('--bg-card', colors.cardBg);
                root.style.setProperty('--glow-color', colors.glow);
                root.style.setProperty('--accent-1', colors.accent1);
                root.style.setProperty('--accent-2', colors.accent2);
                root.style.setProperty('--text-primary', colors.textPrimary);
                root.style.setProperty('--text-secondary', colors.textSecondary);
                root.style.setProperty('--card-border', colors.border);
                root.style.setProperty('--card-border-light', colors.borderLight);

                const navRgb = hexToRgb(colors.primary);
                root.style.setProperty('--nav-blur', `rgba(${navRgb.r}, ${navRgb.g}, ${navRgb.b}, 0.78)`);
                root.style.setProperty('--nav-border', colors.border);

                const glow = document.getElementById('ambientGlow');
                glow.style.background = `radial-gradient(circle at 40% 30%, ${colors.glow}, transparent 70%)`;
                document.body.style.background = colors.primary;
            }

            // =========================================================
            // 四、风味轮盘
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
            // 五、命理系统
            // =========================================================
            const LIFE_MAP = {
                1: { flavor: '明亮果酸', bean: '花魁', full: '埃塞俄比亚·花魁 G1 日晒', sub: '明亮果酸 · 花香 · 茶感', element: '火' },
                2: { flavor: '均衡圆润', bean: '蕙兰', full: '哥伦比亚·蕙兰 水洗', sub: '均衡圆润 · 坚果 · 柔和', element: '土' },
                3: { flavor: '花香轻盈', bean: '瑰夏', full: '巴拿马·瑰夏 水洗', sub: '茉莉花香 · 柑橘 · 蜂蜜', element: '木' },
                4: { flavor: '坚果可可', bean: '黄波旁', full: '巴西·黄波旁 日晒', sub: '坚果 · 可可 · 醇厚扎实', element: '土' },
                5: { flavor: '发酵酒韵', bean: '肯尼亚 AA', full: '肯尼亚·AA 水洗', sub: '莓果 · 红酒韵 · 明亮', element: '水' },
                6: { flavor: '蜂蜜甜醇', bean: '安提瓜', full: '危地马拉·安提瓜 水洗', sub: '蜂蜜 · 甜醇 · 圆润', element: '金' },
                7: { flavor: '茶感清透', bean: '云南卡蒂姆', full: '中国·云南 卡蒂姆 水洗', sub: '茶感 · 清透 · 柔和', element: '水' },
                8: { flavor: '黑巧浓郁', bean: '曼特宁', full: '印尼·曼特宁 湿刨', sub: '黑巧 · 浓郁 · 低酸', element: '金' },
                9: { flavor: '复合层次', bean: '手冲拼配·花韵', full: '手冲拼配·花韵 中浅烘', sub: '复合层次 · 深邃 · 多变',
                    element: '火' },
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

            let currentFortuneBean = null;
            let userBirthSet = false;

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

            function getNearbyShopsByBean(targetBean) {
                if (!targetBean) return NEARBY_SHOPS.slice(0, 4);
                const keywords = targetBean.replace(/·/g, ' ').split(/\s+/);
                const mainKeyword = keywords[0] || targetBean;
                const scored = NEARBY_SHOPS.map(shop => {
                    let score = 0;
                    shop.beans.forEach(bean => {
                        if (bean.includes(mainKeyword) || mainKeyword.includes(bean)) score += 3;
                        if (bean.includes(targetBean.substring(0, 2)) || targetBean.includes(bean.substring(0,
                            2))) score += 1;
                    });
                    score += (shop.rating - 4) * 1.5;
                    return { ...shop, matchScore: score };
                });
                scored.sort((a, b) => b.matchScore - a.matchScore);
                return scored.slice(0, 5);
            }

            function renderNearbyShops(shops) {
                const container = document.getElementById('nearbyShopsContainer');
                if (!shops || shops.length === 0) {
                    container.innerHTML =
                        `<div class="nearby-empty">📍 附近暂未找到同款咖啡店<br><small>试试手动搜索</small></div>`;
                    return;
                }
                let html = '';
                shops.forEach(shop => {
                    const beanTags = shop.beans.map(b =>
                        `<span class="highlight-tag">🏷️ ${b}</span>`
                    ).join('');
                    html += `
                        <div class="nearby-shop-item">
                            <div class="shop-icon">${shop.icon}</div>
                            <div class="shop-info">
                                <div class="shop-name">${shop.name}</div>
                                <div class="shop-desc">⭐ ${shop.rating} · ${shop.comment}</div>
                                <div class="shop-tags">
                                    ${beanTags}
                                    ${shop.tags.map(t => `<span>${t}</span>`).join('')}
                                </div>
                            </div>
                            <div class="shop-distance">📍 ${shop.distance}</div>
                        </div>
                    `;
                });
                container.innerHTML = html;
            }

            // =========================================================
            // 六、数据存储（localStorage模拟）
            // =========================================================
            const DB = {
                getRecords() {
                    try { return JSON.parse(localStorage.getItem('coffee_records')) || []; } catch (e) { return []; }
                },
                setRecords(records) {
                    localStorage.setItem('coffee_records', JSON.stringify(records));
                },
                addRecord(record) {
                    const records = this.getRecords();
                    record.id = Date.now();
                    record.created_at = new Date().toISOString();
                    records.unshift(record);
                    this.setRecords(records);
                    return record;
                },
                getBirth() {
                    try { return JSON.parse(localStorage.getItem('coffee_birth')); } catch (e) { return null; }
                },
                setBirth(birth) {
                    localStorage.setItem('coffee_birth', JSON.stringify(birth));
                    userBirthSet = true;
                },
                getOnboardingDone() {
                    return localStorage.getItem('coffee_onboarding_done') === 'true';
                },
                setOnboardingDone() {
                    localStorage.setItem('coffee_onboarding_done', 'true');
                },
                getPushEnabled() {
                    return localStorage.getItem('coffee_push_enabled') === 'true';
                },
                setPushEnabled(val) {
                    localStorage.setItem('coffee_push_enabled', String(val));
                },
                getPushDismissed() {
                    return localStorage.getItem('coffee_push_dismissed') === 'true';
                },
                setPushDismissed() {
                    localStorage.setItem('coffee_push_dismissed', 'true');
                }
            };

            // =========================================================
            // 七、渲染函数
            // =========================================================
            function renderHome() {
                const records = DB.getRecords();
                const container = document.getElementById('homeContent');
                const birth = DB.getBirth();

                // 更新今日宜喝
                if (birth) {
                    const num = getLifeNumber(birth.year, birth.month, birth.day);
                    const elem = getElement(birth.year);
                    const data = LIFE_MAP[num] || LIFE_MAP[5];
                    document.getElementById('summaryBean').textContent = data.full || data.bean;
                    document.getElementById('summaryBadge').textContent = '✨ 查看解读';
                    document.getElementById('summaryBadge').className = 'match-badge';
                    currentFortuneBean = data.bean;
                } else {
                    document.getElementById('summaryBean').textContent = '设置生辰，获取专属指引';
                    document.getElementById('summaryBadge').textContent = '⚙️ 设置';
                    document.getElementById('summaryBadge').className = 'match-badge setup';
                }

                if (records.length === 0) {
                    container.innerHTML = `
                        <div class="empty-state">
                            <span class="icon">☕</span>
                            <div class="title">还没有记录</div>
                            <div class="sub">点击下方 ☕ 按钮，记录你的第一杯咖啡</div>
                            <button class="btn-primary" onclick="document.getElementById('fabButton').click()">开始记录</button>
                        </div>
                    `;
                    return;
                }

                let html = '';
                records.slice(0, 10).forEach(r => {
                    const bean = COFFEE_BEANS.find(b => b.name === r.beanName) || { desc: r.flavor || '' };
                    html += `
                        <div class="mock-card" onclick="showToast('📖 查看详情')">
                            <div class="avatar">${r.emoji || '☕'}</div>
                            <div class="info">
                                <div class="title">${r.beanName || '未命名咖啡'}</div>
                                <div class="desc">${r.flavor || bean.desc || ''}</div>
                                <div class="meta">${r.created_at ? new Date(r.created_at).toLocaleDateString() : ''}</div>
                            </div>
                            <span class="badge">${'⭐'.repeat(Math.min(5, Math.round((r.rating || 3))))}</span>
                        </div>
                    `;
                });
                container.innerHTML = html;
            }

            function renderPersonality() {
                const records = DB.getRecords();
                const container = document.getElementById('personalityContent');
                const count = records.length;

                if (count < 3) {
                    container.innerHTML = `
                        <div class="empty-state">
                            <span class="icon">🌱</span>
                            <div class="title">记录 ${count} / 3 杯</div>
                            <div class="sub">记录 3 杯即可解锁初始人格<br>每一杯咖啡都在塑造你的风味人格</div>
                            <div class="progress-track">
                                <div class="bar" style="width:${(count/3)*100}%;"></div>
                            </div>
                            <div style="margin-top:12px; font-size:13px; color:var(--text-secondary);">还需 ${3-count} 杯</div>
                        </div>
                    `;
                    return;
                }

                // 简单人格分析（基于记录）
                const vectors = records.map(r => r.vector || [5, 5, 5, 5, 5, 5, 5, 5]);
                const avg = [0, 0, 0, 0, 0, 0, 0, 0];
                vectors.forEach(v => { for (let i = 0; i < 8; i++) avg[i] += (v[i] || 5) / vectors.length; });
                const flavorNames = ['酸度', '甜感', '苦度', '醇厚度', '果香', '花香', '坚果', '发酵感'];
                const topIdx = avg.indexOf(Math.max(...avg));

                container.innerHTML = `
                    <div style="text-align:center; padding:20px 0 30px;">
                        <span style="font-size:72px; display:block; margin-bottom:12px;">${['🌸','🍊','🌰','☕','🍇','🍯','🍃','🍫'][topIdx] || '🌱'}</span>
                        <div style="font-size:20px; font-weight:600; color:var(--text-primary);">${['花香探索者','果酸猎人','醇厚鉴赏家','均衡大师','发酵冒险家','甜感收藏家','茶韵品鉴师','浓郁追求者'][topIdx] || '风味探险家'} · ${count}杯</div>
                        <div style="font-size:14px; color:var(--text-secondary); margin-top:4px;">偏好：${flavorNames[topIdx]} · ${records.length}杯记录</div>
                        <div style="width:80%; max-width:280px; height:6px; background:var(--bg-secondary); border-radius:6px; margin:16px auto 0; overflow:hidden;">
                            <div style="width:${Math.min(100, count/10*100)}%; height:100%; background:linear-gradient(90deg, var(--accent-1), var(--accent-2)); border-radius:6px;"></div>
                        </div>
                        <div style="margin-top:8px; font-size:13px; color:var(--text-secondary);">下一形态：${count>=10 ? '✨ 已解锁' : count>=5 ? '🌿 风味收藏家 (还需'+(10-count)+'杯)' : '🌱 继续探索 (还需'+(10-count)+'杯)'}</div>
                    </div>
                    <div style="background:var(--bg-card); border-radius:14px; padding:16px; border:1px solid var(--card-border); margin-bottom:12px;">
                        <div style="display:flex; justify-content:space-between; align-items:center;">
                            <span style="color:var(--text-secondary); font-size:14px;">已解锁标签</span>
                            <span style="color:var(--accent-1); font-size:13px;">+${Math.min(3, Math.floor(count/3))} 杯解锁新标签</span>
                        </div>
                        <div style="display:flex; flex-wrap:wrap; gap:8px; margin-top:12px;">
                            ${flavorNames.slice(0, Math.min(4, 2+Math.floor(count/5))).map((f,i) => `
                                <span style="background:var(--bg-secondary); padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid var(--accent-1); color:var(--text-primary);">${['🌸','🍊','🌰','☕'][i]||'🏷️'} ${f}</span>
                            `).join('')}
                            ${Array.from({length: Math.max(0, 4 - Math.min(4, 2+Math.floor(count/5)))}).map(() => `
                                <span style="background:var(--bg-secondary); padding:4px 14px; border-radius:20px; font-size:13px; border:1px solid var(--card-border); color:var(--text-secondary);">⬜ 待解锁</span>
                            `).join('')}
                        </div>
                    </div>
                    <div style="text-align:center; margin-top:8px;">
                        <button class="btn-primary" style="padding:10px 28px; font-size:14px; background:var(--bg-secondary); color:var(--text-secondary); border:1px solid var(--card-border);" onclick="showToast('📤 分享人格结果')">📤 分享人格</button>
                    </div>
                    <div class="bottom-hint">— 持续记录，解锁更多 —</div>
                `;
            }

            function renderSocial() {
                const records = DB.getRecords();
                const container = document.getElementById('socialContent');

                if (records.length < 3) {
                    container.innerHTML = `
                        <div class="empty-state">
                            <span class="icon">🌊</span>
                            <div class="title">记录更多咖啡，匹配灵魂咖友</div>
                            <div class="sub">至少需要 3 杯记录才能找到口味相近的人</div>
                            <div style="font-size:13px; color:var(--text-secondary);">当前 ${records.length} / 3 杯</div>
                            <div class="progress-track" style="width:60%; max-width:200px;">
                                <div class="bar" style="width:${(records.length/3)*100}%;"></div>
                            </div>
                        </div>
                    `;
                    return;
                }

                // 模拟匹配
                const matches = [
                    { name: '@咖啡探险家', avatar: '🌸', common: '花香 · 果酸', match: 92 },
                    { name: '@风味猎人', avatar: '🌿', common: '发酵感 · 醇厚', match: 87 },
                    { name: '@手冲星人', avatar: '🍃', common: '茶感 · 干净', match: 81 },
                ];

                container.innerHTML = `
                    <div style="background:var(--bg-card); border-radius:14px; padding:14px 16px; border:1px solid var(--card-border); margin-bottom:16px; display:flex; align-items:center; gap:12px;">
                        <span style="font-size:28px;">🧩</span>
                        <div>
                            <div style="font-size:14px; color:var(--text-primary); font-weight:500;">基于风味相似度匹配</div>
                            <div style="font-size:12px; color:var(--text-secondary);">找到 ${matches.length} 位口味相近的咖友</div>
                        </div>
                    </div>
                    ${matches.map(m => `
                        <div class="mock-card">
                            <div class="avatar">${m.avatar}</div>
                            <div class="info">
                                <div class="title">${m.name}</div>
                                <div class="desc">🏆 共同偏好：${m.common}</div>
                            </div>
                            <span style="background:rgba(200,154,120,0.12); padding:4px 12px; border-radius:20px; font-size:11px; color:var(--accent-1); border:1px solid var(--card-border-light); flex-shrink:0;">${m.match}% 匹配</span>
                        </div>
                    `).join('')}
                    <div style="text-align:center; padding:12px 0 4px;">
                        <span style="background:var(--bg-secondary); padding:10px 28px; border-radius:30px; color:var(--accent-1); font-size:14px; border:1px solid var(--card-border); display:inline-block;" onclick="showToast('📨 已发送邀请')">📨 发送「隔空共饮」邀请</span>
                    </div>
                    <div class="bottom-hint">— 基于 8 维风味向量匹配 —</div>
                `;
            }

            function renderReport() {
                const records = DB.getRecords();
                const container = document.getElementById('reportContent');

                const count = records.length;

                // 统计
                const origins = {};
                const flavors = {};
                records.forEach(r => {
                    const bean = COFFEE_BEANS.find(b => b.name === r.beanName);
                    if (bean) {
                        origins[bean.origin] = (origins[bean.origin] || 0) + 1;
                    }
                    if (r.flavor) {
                        r.flavor.split('·').forEach(f => {
                            const key = f.trim();
                            if (key) flavors[key] = (flavors[key] || 0) + 1;
                        });
                    }
                });
                const topOrigin = Object.entries(origins).sort((a, b) => b[1] - a[1])[0]?.[0] || '—';
                const topFlavors = Object.entries(flavors).sort((a, b) => b[1] - a[1]).slice(0, 3).map(f => f[0]).join(' · ') ||
                    '—';

                // 附近推荐（静态展示）
                const nearbyShops = NEARBY_SHOPS.slice(0, 2);

                container.innerHTML = `
                    <div style="display:grid; grid-template-columns:1fr 1fr 1fr; gap:10px; margin-bottom:16px;">
                        <div style="background:var(--bg-card); border-radius:14px; padding:16px 8px; text-align:center; border:1px solid var(--card-border);">
                            <div style="font-size:22px; font-weight:700; color:var(--text-primary);">${count}</div>
                            <div style="font-size:11px; color:var(--text-secondary); margin-top:4px;">总杯数</div>
                        </div>
                        <div style="background:var(--bg-card); border-radius:14px; padding:16px 8px; text-align:center; border:1px solid var(--card-border);">
                            <div style="font-size:22px; font-weight:700; color:var(--text-primary);">${Object.keys(origins).length || 0}</div>
                            <div style="font-size:11px; color:var(--text-secondary); margin-top:4px;">探索产地</div>
                        </div>
                        <div style="background:var(--bg-card); border-radius:14px; padding:16px 8px; text-align:center; border:1px solid var(--card-border);">
                            <div style="font-size:22px; font-weight:700; color:var(--text-primary);">${count > 0 ? (records.reduce((s,r) => s + (r.rating||3), 0) / count).toFixed(1) : '—'}</div>
                            <div style="font-size:11px; color:var(--text-secondary); margin-top:4px;">平均评分</div>
                        </div>
                    </div>
                    <div style="background:var(--bg-card); border-radius:14px; padding:16px 18px; border:1px solid var(--card-border); margin-bottom:12px;">
                        <div style="display:flex; justify-content:space-between; padding:8px 0; border-bottom:1px solid var(--card-border);">
                            <span style="color:var(--text-secondary); font-size:14px;">🌍 最爱产区</span>
                            <span style="color:var(--text-primary); font-size:14px; font-weight:500;">${topOrigin}</span>
                        </div>
                        <div style="display:flex; justify-content:space-between; padding:8px 0; border-bottom:1px solid var(--card-border);">
                            <span style="color:var(--text-secondary); font-size:14px;">🏷️ 风味标签</span>
                            <span style="color:var(--text-primary); font-size:14px; font-weight:500;">${topFlavors}</span>
                        </div>
                        <div style="display:flex; justify-content:space-between; padding:8px 0;">
                            <span style="color:var(--text-secondary); font-size:14px;">📈 本月趋势</span>
                            <span style="color:#4CAF50; font-size:14px; font-weight:500;">${count > 5 ? '↑ 偏好向浅烘偏移' : '📝 继续积累数据'}</span>
                        </div>
                    </div>
                    ${count > 0 ? `
                        <div style="display:flex; gap:10px; margin-bottom:12px;">
                            <button class="refresh-btn" style="flex:1; margin-top:0;" onclick="showToast('📊 导出JSON')">📥 导出数据</button>
                            <button class="refresh-btn" style="flex:1; margin-top:0;" onclick="showToast('📤 分享报告')">📤 分享报告</button>
                        </div>
                    ` : ''}
                    <div class="section-title">📍 附近 · 风味匹配</div>
                    ${nearbyShops.map(shop => `
                        <div class="shop-card">
                            <div class="thumb">${shop.icon}</div>
                            <div class="info">
                                <div class="name">${shop.name}</div>
                                <div class="address">📍 ${shop.address} · 步行 ${shop.distance}</div>
                                <div class="tags">
                                    <span class="highlight">🏷️ 推荐：${shop.beans[0]}</span>
                                    ${shop.tags.slice(0,2).map(t => `<span>${t}</span>`).join('')}
                                    <span>⭐ ${shop.rating}</span>
                                </div>
                                <div class="reason">💡 ${shop.comment}</div>
                            </div>
                            <div class="right">
                                <div class="score">${shop.rating} <small>/5</small></div>
                                <span class="match-pct">${['🔥 94%','✨ 87%','🌱 74%','🍃 68%','💎 91%','🌟 83%','☕ 79%'][shop.id % 7]}</span>
                            </div>
                        </div>
                    `).join('')}
                    <button class="refresh-btn" id="refreshBtn">🔄 刷新附近推荐</button>
                    <div style="text-align:center; color:var(--text-secondary); opacity:0.3; font-size:11px; padding:12px 0 4px;">— 数据基于大众点评 · 小红书 AI 汇总 —</div>
                    <div class="bottom-hint">— 数据每日更新 —</div>
                `;
            }

            function renderAll() {
                renderHome();
                renderPersonality();
                renderSocial();
                renderReport();
            }

            // =========================================================
            // 八、UI 交互控制
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
            const helpBtn = document.getElementById('helpBtn');

            function switchTab(tabName) {
                navItems.forEach(item => { item.classList.toggle('active', item.getAttribute('data-tab') === tabName); });
                Object.keys(panels).forEach(k => panels[k].classList.toggle('active', k === tabName));
                mainContent.scrollTop = 0;
                if (navigator.vibrate) navigator.vibrate(5);
                // 切换时重新渲染对应面板
                if (tabName === 'home') renderHome();
                if (tabName === 'personality') renderPersonality();
                if (tabName === 'social') renderSocial();
                if (tabName === 'report') renderReport();
            }

            navItems.forEach(item => {
                item.addEventListener('click', function() { const t = this.getAttribute('data-tab'); if (t) switchTab(
                        t); });
            });

            // ---- 今日宜喝点击 ----
            document.getElementById('fortuneSummary').addEventListener('click', function() {
                const birth = DB.getBirth();
                if (!birth) {
                    // 未设置生辰，弹出命理设置并聚焦
                    fortuneModal.classList.add('open');
                    // 不自动生成，让用户选择后点击
                } else {
                    fortuneModal.classList.add('open');
                    generateFortune();
                }
            });

            // ---- 命理弹窗 ----
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

                // 保存生辰
                DB.setBirth({ year: y, month: m, day: d });

                document.getElementById('fTag').textContent = '✦ ' + roast.word;
                document.getElementById('fMain').textContent = data.full || data.bean;
                document.getElementById('fSub').textContent = data.sub;
                document.getElementById('fLife').textContent = num + ' (' + (LIFE_NAMES[num - 1] || '') + ')';
                document.getElementById('fElement').textContent = elem + ' (' + (ELEM_NAMES[elem] || '') + ')';
                document.getElementById('fRoast').textContent = roast.roast;
                document.getElementById('fTemp').textContent = roast.temp;
                document.getElementById('fMindful').textContent = getMindfulness(num, elem);
                document.getElementById('fortuneResult').classList.add('visible');

                currentFortuneBean = data.bean;
                document.getElementById('summaryBean').textContent = data.full || data.bean;
                document.getElementById('summaryBadge').textContent = '✨ 查看解读';
                document.getElementById('summaryBadge').className = 'match-badge';

                // 附近推荐
                const nearbyShops = getNearbyShopsByBean(data.bean);
                renderNearbyShops(nearbyShops);

                if (navigator.vibrate) navigator.vibrate(10);
                renderAll();
            }

            document.getElementById('generateBtn').addEventListener('click', generateFortune);

            // 如果已有生辰，自动填充
            const savedBirth = DB.getBirth();
            if (savedBirth) {
                document.getElementById('birthYear').value = savedBirth.year;
                document.getElementById('birthMonth').value = savedBirth.month;
                document.getElementById('birthDay').value = savedBirth.day;
                // 自动生成一次
                setTimeout(generateFortune, 200);
            }

            closeFortune.addEventListener('click', function() { fortuneModal.classList.remove('open'); });
            fortuneModal.addEventListener('click', function(e) { if (e.target === this) this.classList.remove('open'); });

            // ---- 分享今日宜喝 ----
            document.getElementById('shareFortuneBtn').addEventListener('click', function() {
                const main = document.getElementById('fMain').textContent;
                const sub = document.getElementById('fSub').textContent;
                document.getElementById('shareTitle').textContent = main;
                document.getElementById('shareSub').textContent = sub;
                document.getElementById('shareModal').classList.add('open');
                if (navigator.vibrate) navigator.vibrate(10);
            });

            document.getElementById('closeShare').addEventListener('click', function() {
                document.getElementById('shareModal').classList.remove('open');
            });
            document.getElementById('shareModal').addEventListener('click', function(e) {
                if (e.target === this) this.classList.remove('open');
            });

            // ---- 记录浮层 ----
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

            helpBtn.addEventListener('click', function() {
                showToast('💡 单指滑动轮盘，选择你的风味偏好');
            });

            doneBtn.addEventListener('click', function() {
                if (this.classList.contains('success')) return;
                const flavor = findClosestFlavor(posX, posY);
                const vector = posToVector(posX, posY);
                const bean = COFFEE_BEANS.find(b => b.desc.includes(flavor.main.split('·')[0]) || b.desc.includes(
                flavor.main)) || COFFEE_BEANS[0];

                // 获取选中的标签
                const activeTag = document.querySelector('.tag-scroll .tag.active');
                const roastTag = activeTag ? activeTag.textContent : '中烘';

                if (navigator.vibrate) navigator.vibrate([15, 50, 15]);

                // 保存记录
                const record = {
                    beanName: bean ? bean.name : '未知豆种',
                    flavor: flavor.main + ' · ' + flavor.sub,
                    vector: vector,
                    rating: Math.round((vector[0] + vector[1] + (10 - vector[2]) + vector[3] + vector[4] + vector[5] +
                        vector[6] + vector[7]) / 8 / 2 * 10) / 10,
                    emoji: ['🌺', '🍊', '🌰', '☕', '🍇', '🍯', '🍃', '🍫'][Math.floor(Math.random() * 8)],
                    roast: roastTag,
                };
                DB.addRecord(record);

                this.textContent = '✅ 已记录！';
                this.className = 'btn btn-done success';
                showToast('☕ 记录成功！' + flavor.main);

                setTimeout(() => {
                    closeOverlay();
                    renderAll();
                    setTimeout(() => {
                        this.textContent = '☕ 完成记录';
                        this.className = 'btn btn-done';
                    }, 300);
                }, 700);
            });

            // 下滑关闭
            let startY2 = 0;
            overlay.addEventListener('touchstart', function(e) { startY2 = e.touches[0].clientY; }, { passive: true });
            overlay.addEventListener('touchmove', function(e) {
                if (e.touches[0].clientY - startY2 > 60 && overlay.classList.contains('open')) closeOverlay();
            }, { passive: true });

            document.addEventListener('keydown', function(e) {
                if (e.key === 'Escape') {
                    if (overlay.classList.contains('open')) closeOverlay();
                    if (fortuneModal.classList.contains('open')) fortuneModal.classList.remove('open');
                }
            });

            // ---- 快捷标签 ----
            document.querySelectorAll('.tag-scroll .tag').forEach(tag => {
                tag.addEventListener('click', function() {
                    document.querySelectorAll('.tag-scroll .tag').forEach(t => t.classList.remove('active'));
                    this.classList.add('active');
                    if (navigator.vibrate) navigator.vibrate(5);
                });
            });

            // ---- 附近刷新 ----
            document.addEventListener('click', function(e) {
                if (e.target.id === 'refreshBtn' || e.target.closest('#refreshBtn')) {
                    const reasons = [
                        '💡 大众点评：“花魁手冲花香炸裂，与你今日命理高度匹配。”',
                        '💡 小红书：“冰冲瑰夏柑橘蜂蜜味绝了。”',
                        '💡 用户口碑：“深烘曼特宁的奶油坚果香很正。”',
                        '💡 咖啡群推荐：“肯尼亚 AA 莓果调性突出。”',
                        '💡 最新评价：“埃塞豆子新鲜，草莓香气明显。”',
                    ];
                    const bubbles = document.querySelectorAll('.shop-card .reason');
                    bubbles.forEach((b, i) => { b.textContent = reasons[(i + Math.floor(Math.random() * 3)) %
                            reasons.length]; });
                    const pcts = document.querySelectorAll('.shop-card .match-pct');
                    const vals = ['🔥 96%', '✨ 89%', '🌱 74%'];
                    pcts.forEach((p, i) => { if (i < vals.length) p.textContent = vals[i]; });
                    if (navigator.vibrate) navigator.vibrate(8);
                    const btn = e.target.closest('#refreshBtn');
                    if (btn) {
                        const orig = btn.textContent;
                        btn.textContent = '✅ 已刷新';
                        setTimeout(() => { btn.textContent = orig; }, 1000);
                    }
                    showToast('🔄 已刷新附近推荐');
                }
            });

            // ---- 推送授权 ----
            const pushPrompt = document.getElementById('pushPrompt');
            if (!DB.getPushDismissed() && !DB.getPushEnabled()) {
                pushPrompt.classList.add('visible');
            }

            document.getElementById('enablePush').addEventListener('click', function() {
                if (navigator.vibrate) navigator.vibrate(10);
                // 模拟浏览器推送授权
                if ('Notification' in window && Notification.permission === 'default') {
                    Notification.requestPermission().then(perm => {
                        if (perm === 'granted') {
                            DB.setPushEnabled(true);
                            pushPrompt.classList.remove('visible');
                            showToast('🔔 已开启每日宜喝提醒');
                            // 模拟注册推送
                            console.log('📨 Push subscription registered');
                        } else {
                            showToast('❌ 需要授权才能推送通知');
                        }
                    });
                } else if ('Notification' in window && Notification.permission === 'granted') {
                    DB.setPushEnabled(true);
                    pushPrompt.classList.remove('visible');
                    showToast('🔔 已开启每日宜喝提醒');
                } else {
                    // 降级：直接标记为已开启（演示）
                    DB.setPushEnabled(true);
                    pushPrompt.classList.remove('visible');
                    showToast('🔔 已开启每日宜喝提醒（演示模式）');
                }
            });

            document.getElementById('dismissPush').addEventListener('click', function() {
                DB.setPushDismissed();
                pushPrompt.classList.remove('visible');
            });

            // ---- 反馈功能 ----
            document.querySelectorAll('.settings-item .right .arrow').forEach(el => {
                if (el.closest('.settings-item')?.querySelector('.label')?.textContent === '意见反馈') {
                    el.closest('.settings-item').addEventListener('click', function() {
                        document.getElementById('feedbackModal').classList.add('open');
                    });
                }
            });

            // 快捷反馈入口（在记录浮层帮助按钮旁，通过长按触发）
            // 也可以通过页面底部的设置区域，但我们先在导航上加一个隐藏入口
            // 在首页添加反馈入口
            const feedbackEntry = document.createElement('div');
            feedbackEntry.style.cssText =
                'text-align:center; padding:8px 0; color:var(--text-secondary); opacity:0.3; font-size:12px; cursor:pointer;';
            feedbackEntry.textContent = '💬 意见反馈';
            feedbackEntry.addEventListener('click', function() {
                document.getElementById('feedbackModal').classList.add('open');
            });
            // 添加到首页底部
            const homePanel = document.getElementById('panel-home');
            const hint = homePanel.querySelector('.bottom-hint');
            if (hint) {
                hint.parentNode.insertBefore(feedbackEntry, hint);
            }

            // ---- 反馈弹窗逻辑 ----
            const fbModal = document.getElementById('feedbackModal');
            const fbForm = document.getElementById('fbForm');
            const fbSuccess = document.getElementById('fbSuccess');
            const fbTypeBtns = document.querySelectorAll('#fbTypeGroup button');
            const fbContent = document.getElementById('fbContent');
            const fbCancel = document.getElementById('fbCancel');
            const fbSubmit = document.getElementById('fbSubmit');
            const fbDone = document.getElementById('fbDone');

            let selectedType = 'idea';

            fbTypeBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    fbTypeBtns.forEach(b => b.classList.remove('active'));
                    this.classList.add('active');
                    selectedType = this.dataset.type;
                });
            });
            // 默认选中第一个
            fbTypeBtns[0].classList.add('active');
            selectedType = fbTypeBtns[0].dataset.type;

            fbCancel.addEventListener('click', function() { fbModal.classList.remove('open'); });
            fbModal.addEventListener('click', function(e) { if (e.target === this) this.classList.remove('open'); });

            fbSubmit.addEventListener('click', function() {
                const content = fbContent.value.trim();
                if (content.length < 10) {
                    showToast('📝 请至少输入10个字');
                    return;
                }
                const typeMap = { bug: '🐛 Bug反馈', idea: '💡 功能建议', content: '📝 内容纠错', other: '❤️ 其他' };
                console.log('📨 反馈提交：', { type: typeMap[selectedType] || selectedType, content });
                if (navigator.vibrate) navigator.vibrate(10);
                fbForm.style.display = 'none';
                fbSuccess.style.display = 'block';
                showToast('🎉 感谢你的反馈！');
            });

            fbDone.addEventListener('click', function() {
                fbModal.classList.remove('open');
                setTimeout(() => {
                    fbForm.style.display = 'block';
                    fbSuccess.style.display = 'none';
                    fbContent.value = '';
                }, 300);
            });

            // ---- 新手引导 ----
            if (!DB.getOnboardingDone()) {
                const obOverlay = document.getElementById('onboardingOverlay');
                const obIcon = document.getElementById('obIcon');
                const obTitle = document.getElementById('obTitle');
                const obDesc = document.getElementById('obDesc');
                const obDots = document.getElementById('obDots');
                const obNext = document.getElementById('obNext');
                const obSkip = document.getElementById('obSkip');

                const steps = [{
                    icon: '☕',
                    title: '欢迎来到咖啡手记',
                    desc: '记录每一杯咖啡，发现你的风味人格'
                }, {
                    icon: '🎯',
                    title: '滑动轮盘 · 找到风味',
                    desc: '单指在轮盘上滑动，系统会自动识别你的风味偏好'
                }, {
                    icon: '🔮',
                    title: '每日宜喝 · 命理指引',
                    desc: '输入出生日期，获取每日专属咖啡推荐和正念指引'
                }, {
                    icon: '🌊',
                    title: '连接同好 · 分享快乐',
                    desc: '基于风味相似度，找到口味相近的咖啡爱好者'
                }, ];

                let stepIndex = 0;

                function updateOnboarding() {
                    const s = steps[stepIndex];
                    obIcon.textContent = s.icon;
                    obTitle.textContent = s.title;
                    obDesc.textContent = s.desc;

                    const dots = obDots.querySelectorAll('.dot');
                    dots.forEach((dot, i) => {
                        dot.classList.toggle('active', i === stepIndex);
                    });

                    if (stepIndex === steps.length - 1) {
                        obNext.textContent = '🎉 开始使用';
                    } else {
                        obNext.textContent = '下一步 →';
                    }
                }

                obOverlay.classList.add('open');

                obNext.addEventListener('click', function() {
                    if (stepIndex < steps.length - 1) {
                        stepIndex++;
                        updateOnboarding();
                        if (navigator.vibrate) navigator.vibrate(8);
                    } else {
                        DB.setOnboardingDone();
                        obOverlay.classList.remove('open');
                        showToast('🎉 开始你的咖啡之旅！');
                        if (navigator.vibrate) navigator.vibrate(15);
                    }
                });

                obSkip.addEventListener('click', function() {
                    DB.setOnboardingDone();
                    obOverlay.classList.remove('open');
                    showToast('⏭️ 已跳过引导');
                });

                updateOnboarding();
            }

            // =========================================================
            // 九、初始化
            // =========================================================
            const now = new Date();
            document.getElementById('todayDate').textContent = (now.getMonth() + 1) + '月' + now.getDate() + '日';
            document.getElementById('personalityDate').textContent = (now.getMonth() + 1) + '月' + now.getDate() + '日';
            document.getElementById('socialDate').textContent = (now.getMonth() + 1) + '月' + now.getDate() + '日';
            document.getElementById('reportDate').textContent = (now.getMonth() + 1) + '月趋势';

            renderAll();

            console.log('☕ 咖啡手记 v5.0 已启动！');
            console.log('📦 数据库：' + COFFEE_BEANS.length + ' 款豆种');
            console.log('📍 附近店铺：' + NEARBY_SHOPS.length + ' 家');
            console.log('📝 已记录：' + DB.getRecords().length + ' 杯');
            console.log('🔮 命理状态：' + (DB.getBirth() ? '已设置' : '未设置'));

            // ---- 模拟每日宜喝推送（演示） ----
            if (DB.getPushEnabled() && DB.getBirth()) {
                setTimeout(() => {
                    const birth = DB.getBirth();
                    const num = getLifeNumber(birth.year, birth.month, birth.day);
                    const data = LIFE_MAP[num] || LIFE_MAP[5];
                    if ('Notification' in window && Notification.permission === 'granted') {
                        new Notification('☕ 今日宜喝 · ' + data.bean, {
                            body: data.sub + ' 点击查看完整解读',
                            icon: 'data:image/svg+xml,%3Csvg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"%3E%3Ctext y=".9em" font-size="90"%3E☕%3C/text%3E%3C/svg%3E'
                        });
                    }
                    console.log('🔔 推送通知：今日宜喝 · ' + data.bean);
                }, 3000);
            }

        })();
    </script>

</body>
</html>
