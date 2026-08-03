<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Кибер-Пантера — большие кнопки в магазине</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            -webkit-touch-callout: none;
            -webkit-tap-highlight-color: transparent;
        }
        html, body {
            background: #0a0a0f;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', Arial, sans-serif;
            touch-action: none;
            overflow: hidden;
            width: 100%;
            height: 100%;
            position: fixed;
            top: 0;
            left: 0;
        }
        body {
            padding: 6px;
        }
        .game-wrapper {
            background: linear-gradient(145deg, #0a0a1a, #050510);
            padding: 8px 8px 12px 8px;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.9), inset 0 0 0 1px #2a2a5a;
            border: 1px solid #2a2a5a;
            max-width: 100vw;
            max-height: 100vh;
            transition: all 0.3s ease;
            position: relative;
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        .game-wrapper.fullscreen {
            border-radius: 0;
            padding: 0;
            border: none;
            box-shadow: none;
            max-width: 100vw;
            max-height: 100vh;
            width: 100vw;
            height: 100vh;
            background: #050510;
        }
        .game-wrapper.fullscreen canvas {
            border-radius: 0;
            width: 100% !important;
            height: auto !important;
            max-height: 100vh;
            object-fit: contain;
        }
        .game-wrapper.fullscreen .ui-panel {
            border-radius: 0;
            margin-top: 0;
            padding: 6px 10px;
            border-left: none;
            border-right: none;
        }
        .game-wrapper.fullscreen .touch-controls {
            padding: 0 6px 8px 6px;
        }
        canvas {
            display: block;
            width: 100%;
            height: auto;
            aspect-ratio: 900/450;
            border-radius: 12px;
            background: #050510;
            box-shadow: inset 0 0 0 2px #1a1a3a;
            image-rendering: auto;
            touch-action: none;
            max-height: calc(100vh - 180px);
            object-fit: contain;
        }
        .ui-panel {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 8px;
            color: #00ffcc;
            font-weight: 700;
            font-size: 0.85rem;
            background: rgba(0,20,30,0.8);
            padding: 5px 12px;
            border-radius: 24px;
            border: 1px solid #00ffcc;
            box-shadow: 0 0 15px rgba(0,255,204,0.1);
            flex-wrap: wrap;
            gap: 4px;
            transition: all 0.3s ease;
            flex-shrink: 0;
        }
        .score-box {
            background: rgba(0,10,20,0.8);
            padding: 2px 12px;
            border-radius: 20px;
            border: 1px solid #00ffcc;
            text-shadow: 0 0 10px rgba(0,255,204,0.3);
            font-size: 0.8rem;
            display: flex;
            align-items: center;
            gap: 6px;
        }
        .score-box .coins {
            color: #ffcc00;
            font-size: 0.9rem;
        }
        .controls {
            display: flex;
            gap: 4px;
            font-size: 0.5rem;
            font-weight: 500;
            flex-wrap: wrap;
        }
        .controls span {
            background: rgba(0,10,20,0.8);
            padding: 1px 6px;
            border-radius: 12px;
            border: 1px solid #00ffcc;
            white-space: nowrap;
            color: #00ffcc;
        }
        .controls .key {
            color: #ff66ff;
            font-weight: 700;
        }
        .buttons {
            display: flex;
            gap: 4px;
            flex-wrap: wrap;
        }
        button {
            background: linear-gradient(135deg, #00ffcc, #00ccff);
            border: none;
            color: #0a0a1a;
            font-weight: 700;
            font-size: 0.65rem;
            padding: 3px 10px;
            border-radius: 24px;
            cursor: pointer;
            box-shadow: 0 0 10px rgba(0,255,204,0.3);
            transition: 0.07s linear;
            letter-spacing: 0.3px;
            touch-action: manipulation;
            white-space: nowrap;
        }
        button:active {
            transform: scale(0.92);
        }
        #pauseBtn {
            background: linear-gradient(135deg, #ff66ff, #cc44cc);
            box-shadow: 0 0 10px rgba(255,0,255,0.3);
            min-width: 44px;
            font-size: 0.6rem;
        }
        #pauseBtn.paused {
            background: linear-gradient(135deg, #66ffcc, #44ccaa);
            box-shadow: 0 0 10px rgba(0,255,204,0.3);
        }
        #fullscreenBtn {
            background: linear-gradient(135deg, #ffcc00, #ff8800);
            box-shadow: 0 0 10px rgba(255,200,0,0.3);
            min-width: 28px;
            font-size: 0.8rem;
            padding: 2px 6px;
        }
        #shopBtn {
            background: linear-gradient(135deg, #ff8800, #ff4400);
            box-shadow: 0 0 10px rgba(255,100,0,0.3);
            font-size: 0.65rem;
        }
        #musicBtn {
            background: linear-gradient(135deg, #ff44aa, #ff0088);
            box-shadow: 0 0 10px rgba(255,0,150,0.3);
            font-size: 0.65rem;
            min-width: 44px;
        }
        #musicBtn.muted {
            background: linear-gradient(135deg, #444455, #333344);
            box-shadow: 0 0 10px rgba(255,255,255,0.05);
            color: #888;
        }

        /* МОБИЛЬНЫЕ КНОПКИ */
        .touch-controls {
            display: none;
            justify-content: space-between;
            align-items: flex-end;
            margin-top: 8px;
            gap: 10px;
            touch-action: none;
            transition: all 0.3s ease;
            flex-shrink: 0;
            padding: 0 4px;
        }
        .touch-btn {
            background: rgba(0,255,204,0.12);
            border: 2px solid rgba(0,255,204,0.35);
            border-radius: 18px;
            color: #00ffcc;
            font-size: 1.8rem;
            font-weight: 700;
            padding: 12px 16px;
            min-width: 72px;
            min-height: 72px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            touch-action: none;
            cursor: pointer;
            user-select: none;
            -webkit-touch-callout: none;
            -webkit-tap-highlight-color: transparent;
            text-shadow: 0 0 20px rgba(0,255,204,0.3);
            transition: 0.05s linear;
            box-shadow: 0 0 15px rgba(0,255,204,0.05);
        }
        .touch-btn:active {
            transform: scale(0.92);
            background: rgba(0,255,204,0.3);
            border-color: #00ffcc;
            box-shadow: 0 0 35px rgba(0,255,204,0.2);
        }
        .touch-btn.purple {
            border-color: rgba(255,0,255,0.35);
            background: rgba(255,0,255,0.08);
            color: #ff66ff;
            box-shadow: 0 0 15px rgba(255,0,255,0.05);
        }
        .touch-btn.purple:active {
            background: rgba(255,0,255,0.2);
            border-color: #ff66ff;
            box-shadow: 0 0 35px rgba(255,0,255,0.15);
        }
        .touch-btn.down {
            border-color: rgba(0,255,200,0.25);
            background: rgba(0,255,200,0.06);
        }
        .touch-btn.down:active {
            background: rgba(0,255,200,0.2);
            border-color: #00ffcc;
        }
        .touch-left {
            display: flex;
            gap: 10px;
        }
        .touch-right {
            display: flex;
            gap: 10px;
        }
        .touch-btn .label {
            font-size: 0.55rem;
            text-align: center;
            opacity: 0.6;
            margin-top: 4px;
        }
        .touch-btn .icon {
            font-size: 2rem;
            line-height: 1;
        }

        @media (pointer: coarse) {
            .touch-controls {
                display: flex !important;
            }
            .controls {
                display: none !important;
            }
        }

        /* ===== ПОЛНОЭКРАННЫЙ МАГАЗИН С БОЛЬШИМИ КНОПКАМИ ===== */
        .shop-overlay {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.92);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            animation: shopFadeIn 0.3s ease;
        }
        .shop-overlay.active {
            display: flex;
        }
        @keyframes shopFadeIn {
            from { opacity: 0; transform: scale(0.95); }
            to { opacity: 1; transform: scale(1); }
        }
        .shop-container {
            background: linear-gradient(160deg, #0a0a2a, #050510);
            border: 2px solid #00ffcc;
            border-radius: 28px;
            padding: 24px 28px 28px 28px;
            max-width: 520px;
            width: 92%;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 0 80px rgba(0,255,204,0.12);
            position: relative;
        }
        .shop-container::-webkit-scrollbar {
            width: 4px;
        }
        .shop-container::-webkit-scrollbar-track {
            background: rgba(255,255,255,0.05);
            border-radius: 10px;
        }
        .shop-container::-webkit-scrollbar-thumb {
            background: #00ffcc;
            border-radius: 10px;
        }
        .shop-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 16px;
            padding-bottom: 12px;
            border-bottom: 1px solid rgba(0,255,204,0.15);
        }
        .shop-title {
            color: #00ffcc;
            font-size: 1.6rem;
            font-weight: 700;
            text-shadow: 0 0 30px rgba(0,255,204,0.2);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .shop-title span {
            font-size: 1.8rem;
        }
        .shop-close {
            background: rgba(255,50,50,0.15);
            border: 1px solid rgba(255,50,50,0.3);
            color: #ff5555;
            font-size: 1.8rem;
            padding: 0 14px;
            border-radius: 50%;
            cursor: pointer;
            transition: 0.2s;
            line-height: 1.4;
            min-width: 50px;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .shop-close:hover {
            background: rgba(255,50,50,0.3);
            transform: rotate(90deg);
        }
        .shop-close:active {
            transform: scale(0.9) rotate(90deg);
        }
        .shop-balance {
            color: #aaccee;
            font-size: 1.1rem;
            text-align: center;
            margin-bottom: 18px;
            padding: 10px 20px;
            background: rgba(0,255,204,0.05);
            border-radius: 16px;
            border: 1px solid rgba(0,255,204,0.08);
        }
        .shop-balance .coins-count {
            color: #ffcc00;
            font-weight: 700;
            font-size: 1.4rem;
        }
        .shop-items {
            display: flex;
            flex-direction: column;
            gap: 14px;
        }
        .shop-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 14px 18px;
            border-radius: 18px;
            background: rgba(255,255,255,0.03);
            border: 1px solid rgba(255,255,255,0.06);
            transition: 0.2s;
        }
        .shop-item:hover {
            background: rgba(255,255,255,0.06);
        }
        .shop-item.unlocked {
            border-color: rgba(0,255,204,0.2);
            background: rgba(0,255,204,0.05);
        }
        .shop-item.locked {
            border-color: rgba(255,50,50,0.15);
            background: rgba(255,50,50,0.03);
        }
        .shop-item-info {
            display: flex;
            align-items: center;
            gap: 14px;
        }
        .shop-item-icon {
            font-size: 2.4rem;
            width: 54px;
            text-align: center;
        }
        .shop-item-name {
            color: #ffffff;
            font-weight: 600;
            font-size: 1.1rem;
        }
        .shop-item-status {
            font-size: 0.85rem;
            margin-top: 2px;
        }
        .shop-item-status.unlocked-text {
            color: #00ffcc;
        }
        .shop-item-status.locked-text {
            color: #ff6644;
        }
        .shop-item-status.selected-text {
            color: #ffcc00;
        }

        /* ===== БОЛЬШИЕ КНОПКИ В МАГАЗИНЕ ===== */
        .shop-item-btn {
            padding: 10px 24px;
            border-radius: 34px;
            font-size: 1rem;
            font-weight: 700;
            border: none;
            cursor: pointer;
            transition: 0.15s;
            white-space: nowrap;
            min-width: 110px;
            min-height: 48px;
            touch-action: manipulation;
        }
        .shop-item-btn:active {
            transform: scale(0.92);
        }
        .shop-item-btn.select {
            background: linear-gradient(135deg, #00ffcc, #00ccff);
            color: #0a0a1a;
            box-shadow: 0 0 25px rgba(0,255,204,0.25);
            font-size: 1rem;
        }
        .shop-item-btn.select:hover {
            box-shadow: 0 0 45px rgba(0,255,204,0.4);
        }
        .shop-item-btn.selected {
            background: rgba(0,255,204,0.15);
            color: #00ffcc;
            border: 2px solid #00ffcc;
            cursor: default;
            font-size: 1rem;
        }
        .shop-item-btn.buy {
            background: linear-gradient(135deg, #ff8800, #ff4400);
            color: #ffffff;
            box-shadow: 0 0 25px rgba(255,100,0,0.25);
            font-size: 1rem;
        }
        .shop-item-btn.buy:hover {
            box-shadow: 0 0 45px rgba(255,100,0,0.4);
        }
        .shop-item-btn.buy-disabled {
            background: rgba(255,255,255,0.05);
            color: #666;
            border: 1px solid rgba(255,255,255,0.08);
            cursor: default;
            font-size: 1rem;
        }
        .shop-footer {
            margin-top: 18px;
            text-align: center;
            font-size: 0.75rem;
            color: rgba(255,255,255,0.25);
            border-top: 1px solid rgba(255,255,255,0.05);
            padding-top: 14px;
        }

        /* АДАПТАЦИЯ МАГАЗИНА ПОД ОРИЕНТАЦИЮ */
        @media (orientation: portrait) {
            .shop-container {
                padding: 16px 18px 20px 18px;
                max-width: 95%;
                border-radius: 20px;
            }
            .shop-title {
                font-size: 1.3rem;
            }
            .shop-item {
                padding: 12px 14px;
            }
            .shop-item-icon {
                font-size: 2rem;
                width: 46px;
            }
            .shop-item-name {
                font-size: 0.95rem;
            }
            .shop-item-btn {
                padding: 8px 18px;
                font-size: 0.85rem;
                min-width: 90px;
                min-height: 42px;
            }
            .shop-close {
                min-width: 42px;
                min-height: 42px;
                font-size: 1.5rem;
            }
        }
        @media (orientation: portrait) and (max-width: 400px) {
            .shop-container {
                padding: 12px 12px 16px 12px;
                border-radius: 16px;
                max-width: 98%;
            }
            .shop-title {
                font-size: 1.1rem;
            }
            .shop-title span {
                font-size: 1.3rem;
            }
            .shop-close {
                font-size: 1.4rem;
                padding: 0 10px;
                min-width: 36px;
                min-height: 36px;
            }
            .shop-item {
                padding: 10px 12px;
            }
            .shop-item-icon {
                font-size: 1.6rem;
                width: 38px;
            }
            .shop-item-name {
                font-size: 0.8rem;
            }
            .shop-item-status {
                font-size: 0.7rem;
            }
            .shop-item-btn {
                padding: 6px 14px;
                font-size: 0.75rem;
                min-width: 76px;
                min-height: 36px;
                border-radius: 28px;
            }
            .shop-balance {
                font-size: 0.85rem;
                padding: 6px 12px;
            }
            .shop-balance .coins-count {
                font-size: 1.1rem;
            }
            .shop-footer {
                font-size: 0.65rem;
            }
        }
        @media (orientation: landscape) {
            .shop-container {
                max-width: 600px;
                max-height: 85vh;
                padding: 24px 32px 28px 32px;
            }
            .shop-item-btn {
                padding: 10px 26px;
                font-size: 1.05rem;
                min-width: 120px;
                min-height: 50px;
            }
        }
        @media (orientation: landscape) and (max-width: 700px) {
            .shop-container {
                padding: 16px 20px 20px 20px;
                max-width: 90%;
            }
            .shop-title {
                font-size: 1.3rem;
            }
            .shop-item {
                padding: 10px 14px;
            }
            .shop-item-btn {
                padding: 8px 18px;
                font-size: 0.85rem;
                min-width: 90px;
                min-height: 40px;
            }
            .shop-close {
                min-width: 40px;
                min-height: 40px;
                font-size: 1.5rem;
            }
        }

        /* ОСТАЛЬНЫЕ СТИЛИ ОРИЕНТАЦИИ */
        @media (orientation: portrait) {
            .game-wrapper { padding: 4px 4px 8px 4px; border-radius: 14px; }
            canvas { aspect-ratio: auto; width: 100%; height: auto; max-height: 45vh; }
            .ui-panel { font-size: 0.6rem; padding: 3px 8px; margin-top: 4px; border-radius: 16px; }
            .score-box { font-size: 0.6rem; padding: 1px 8px; }
            .buttons button { font-size: 0.5rem; padding: 2px 6px; }
            #fullscreenBtn { font-size: 0.6rem; padding: 1px 4px; min-width: 20px; }
            #shopBtn, #musicBtn { font-size: 0.5rem; padding: 2px 6px; }
            #musicBtn { min-width: 34px; }
            #pauseBtn { min-width: 34px; font-size: 0.5rem; }
            .touch-btn { min-width: 60px; min-height: 60px; padding: 8px 12px; font-size: 1.4rem; border-radius: 14px; }
            .touch-btn .icon { font-size: 1.6rem; }
            .touch-btn .label { font-size: 0.45rem; }
            .touch-controls { gap: 6px; margin-top: 4px; }
            .touch-left { gap: 6px; }
            .touch-right { gap: 6px; }
            .game-wrapper.fullscreen canvas { max-height: 55vh; }
        }
        @media (orientation: portrait) and (max-width: 400px) {
            .touch-btn { min-width: 50px; min-height: 50px; padding: 6px 8px; font-size: 1.2rem; border-radius: 12px; }
            .touch-btn .icon { font-size: 1.3rem; }
            .touch-btn .label { font-size: 0.4rem; }
            .touch-controls { gap: 4px; }
            .touch-left { gap: 4px; }
            .touch-right { gap: 4px; }
            .ui-panel { font-size: 0.5rem; padding: 2px 6px; }
            .score-box { font-size: 0.5rem; padding: 1px 6px; }
            .buttons button { font-size: 0.45rem; padding: 1px 5px; }
            #pauseBtn { min-width: 28px; font-size: 0.45rem; }
            #fullscreenBtn { font-size: 0.5rem; padding: 1px 3px; min-width: 16px; }
            #shopBtn, #musicBtn { font-size: 0.45rem; padding: 1px 5px; }
            #musicBtn { min-width: 28px; }
        }
        @media (orientation: landscape) {
            canvas { max-height: calc(100vh - 140px); }
            .game-wrapper.fullscreen canvas { max-height: calc(100vh - 110px); }
            .touch-btn { min-width: 80px; min-height: 80px; padding: 14px 20px; font-size: 2rem; border-radius: 20px; }
            .touch-btn .icon { font-size: 2.2rem; }
            .touch-btn .label { font-size: 0.6rem; }
        }
        @media (orientation: landscape) and (max-width: 700px) {
            .touch-btn { min-width: 64px; min-height: 64px; padding: 10px 14px; font-size: 1.6rem; }
            .touch-btn .icon { font-size: 1.8rem; }
            .ui-panel { font-size: 0.7rem; padding: 3px 10px; }
            .score-box { font-size: 0.7rem; padding: 1px 10px; }
            .buttons button { font-size: 0.55rem; padding: 2px 8px; }
            #shopBtn, #musicBtn { font-size: 0.55rem; padding: 2px 8px; }
            #musicBtn { min-width: 38px; }
            #pauseBtn { min-width: 38px; font-size: 0.55rem; }
        }
        @media (orientation: landscape) and (max-width: 500px) {
            .touch-btn { min-width: 52px; min-height: 52px; padding: 6px 10px; font-size: 1.2rem; border-radius: 12px; }
            .touch-btn .icon { font-size: 1.4rem; }
            .touch-btn .label { font-size: 0.4rem; }
            .touch-controls { gap: 4px; }
            .touch-left { gap: 4px; }
            .touch-right { gap: 4px; }
            .ui-panel { font-size: 0.5rem; padding: 2px 6px; }
            .score-box { font-size: 0.5rem; padding: 1px 6px; }
            .buttons button { font-size: 0.45rem; padding: 1px 5px; }
            #pauseBtn { min-width: 28px; font-size: 0.45rem; }
            #fullscreenBtn { font-size: 0.5rem; padding: 1px 3px; min-width: 16px; }
            #shopBtn, #musicBtn { font-size: 0.45rem; padding: 1px 5px; }
            #musicBtn { min-width: 28px; }
        }

        .game-wrapper.fullscreen.portrait canvas { max-height: 60vh; }
        .game-wrapper.fullscreen.portrait .touch-btn { min-width: 70px; min-height: 70px; font-size: 1.6rem; }
        .game-wrapper.fullscreen.portrait .touch-btn .icon { font-size: 1.8rem; }
        .game-wrapper.fullscreen.landscape canvas { max-height: 82vh; }
        .game-wrapper.fullscreen.landscape .touch-btn { min-width: 90px; min-height: 90px; font-size: 2.2rem; }
        .game-wrapper.fullscreen.landscape .touch-btn .icon { font-size: 2.4rem; }
    </style>
</head>
<body>

<div class="game-wrapper" id="gameWrapper">
    <canvas id="gameCanvas" width="900" height="450"></canvas>
    
    <!-- МОБИЛЬНЫЕ КНОПКИ -->
    <div class="touch-controls" id="touchControls">
        <div class="touch-left">
            <div class="touch-btn" id="btnUp">
                <span class="icon">⬆</span>
                <span class="label">прыжок</span>
            </div>
            <div class="touch-btn down" id="btnDown">
                <span class="icon">⬇</span>
                <span class="label">присесть</span>
            </div>
        </div>
        <div class="touch-right">
            <div class="touch-btn purple" id="btnDash">
                <span class="icon">⚡</span>
                <span class="label">рывок</span>
            </div>
            <div class="touch-btn purple" id="btnRoll">
                <span class="icon">🌀</span>
                <span class="label">кувырок</span>
            </div>
        </div>
    </div>

    <div class="ui-panel">
        <div class="score-box">
            🏆 <span id="scoreDisplay">0</span>
            <span class="coins">🪙 <span id="coinDisplay">0</span></span>
        </div>
        <div class="controls">
            <span><span class="key">␣/▲</span> прыжок</span>
            <span><span class="key">▼</span> присесть</span>
            <span><span class="key">→</span> рывок</span>
            <span><span class="key">Z</span> кувырок</span>
        </div>
        <div class="buttons">
            <button id="musicBtn">🎵 Вкл</button>
            <button id="shopBtn">🛒 Магазин</button>
            <button id="fullscreenBtn" title="Полноэкранный режим">⛶</button>
            <button id="pauseBtn">⏸ Пауза</button>
            <button id="restartBtn">⟳ Новая</button>
        </div>
    </div>
</div>

<!-- ===== ПОЛНОЭКРАННЫЙ МАГАЗИН С БОЛЬШИМИ КНОПКАМИ ===== -->
<div class="shop-overlay" id="shopOverlay">
    <div class="shop-container" id="shopContainer">
        <div class="shop-header">
            <div class="shop-title">
                <span>🛒</span> МАГАЗИН
            </div>
            <button class="shop-close" id="closeShop">✕</button>
        </div>
        <div class="shop-balance">
            🪙 Монет: <span class="coins-count" id="shopCoins">0</span>
        </div>
        <div class="shop-items" id="shopItems">
            <!-- Товары будут добавлены через JS -->
        </div>
        <div class="shop-footer">
            ⭐ За прохождение препятствий даются монеты!<br>
            🎯 Чем выше комбо, тем больше монет!
        </div>
    </div>
</div>

<script>
    (function(){
        // ===================== МУЗЫКА =====================
        class MusicPlayer {
            constructor() {
                this.ctx = null;
                this.isPlaying = false;
                this.isMuted = false;
                this.audioNodes = [];
                this.scheduledTimeouts = [];
                this.bpm = 130;
                this.volume = 0.15;
                this.beatCounter = 0;
                this.initialized = false;
                this.musicBtn = document.getElementById('musicBtn');
                
                this.musicBtn.addEventListener('click', () => this.toggle());
                document.addEventListener('click', () => this.init(), { once: true });
                document.addEventListener('touchstart', () => this.init(), { once: true });
            }
            
            init() {
                if (this.initialized) return;
                try {
                    this.ctx = new (window.AudioContext || window.webkitAudioContext)();
                    this.initialized = true;
                } catch(e) {}
            }
            
            toggle() {
                if (!this.initialized) { this.init(); if (!this.initialized) return; }
                if (this.isPlaying) { this.stop(); } else { this.start(); }
            }
            
            start() {
                if (this.isPlaying) return;
                if (!this.initialized) return;
                if (this.ctx.state === 'suspended') this.ctx.resume();
                this.isPlaying = true;
                this.beatCounter = 0;
                this.musicBtn.textContent = '🎵 Выкл';
                this.musicBtn.className = '';
                this.scheduleBeats();
            }
            
            stop() {
                this.isPlaying = false;
                this.musicBtn.textContent = '🎵 Вкл';
                this.musicBtn.className = 'muted';
                for (let t of this.scheduledTimeouts) clearTimeout(t);
                this.scheduledTimeouts = [];
                for (let node of this.audioNodes) { try { node.disconnect(); } catch(e) {} }
                this.audioNodes = [];
            }
            
            scheduleBeats() {
                if (!this.isPlaying) return;
                const interval = 60 / this.bpm;
                const now = this.ctx.currentTime;
                
                this.playKick(now);
                if (this.beatCounter % 2 === 1) this.playSnare(now + interval * 0.5);
                this.playHiHat(now + interval * 0.25);
                this.playHiHat(now + interval * 0.75);
                if (this.beatCounter % 4 === 0) this.playKick(now + interval * 0.5);
                if (this.beatCounter % 4 === 2) this.playKick(now + interval * 0.75);
                this.playBass(now, interval);
                
                this.beatCounter++;
                const timeoutId = setTimeout(() => { this.scheduleBeats(); }, interval * 1000 * 0.95);
                this.scheduledTimeouts.push(timeoutId);
            }
            
            playKick(time) {
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(150, time);
                osc.frequency.exponentialRampToValueAtTime(40, time + 0.08);
                gain.gain.setValueAtTime(this.volume * 1.2, time);
                gain.gain.exponentialRampToValueAtTime(0.001, time + 0.1);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start(time);
                osc.stop(time + 0.1);
                this.audioNodes.push(osc, gain);
                
                const bufferSize = this.ctx.sampleRate * 0.02;
                const buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate);
                const data = buffer.getChannelData(0);
                for (let i = 0; i < bufferSize; i++) data[i] = (Math.random() * 2 - 1) * Math.exp(-i / bufferSize * 5);
                const noise = this.ctx.createBufferSource();
                const noiseGain = this.ctx.createGain();
                noise.buffer = buffer;
                noiseGain.gain.setValueAtTime(this.volume * 0.3, time);
                noiseGain.gain.exponentialRampToValueAtTime(0.001, time + 0.04);
                noise.connect(noiseGain);
                noiseGain.connect(this.ctx.destination);
                noise.start(time);
                noise.stop(time + 0.04);
                this.audioNodes.push(noise, noiseGain);
            }
            
            playSnare(time) {
                const bufferSize = this.ctx.sampleRate * 0.08;
                const buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate);
                const data = buffer.getChannelData(0);
                for (let i = 0; i < bufferSize; i++) data[i] = (Math.random() * 2 - 1) * Math.exp(-i / bufferSize * 3);
                const noise = this.ctx.createBufferSource();
                const gain = this.ctx.createGain();
                noise.buffer = buffer;
                gain.gain.setValueAtTime(this.volume * 0.6, time);
                gain.gain.exponentialRampToValueAtTime(0.001, time + 0.08);
                noise.connect(gain);
                gain.connect(this.ctx.destination);
                noise.start(time);
                noise.stop(time + 0.08);
                this.audioNodes.push(noise, gain);
                
                const osc = this.ctx.createOscillator();
                const oscGain = this.ctx.createGain();
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(180, time);
                osc.frequency.exponentialRampToValueAtTime(80, time + 0.06);
                oscGain.gain.setValueAtTime(this.volume * 0.3, time);
                oscGain.gain.exponentialRampToValueAtTime(0.001, time + 0.06);
                osc.connect(oscGain);
                oscGain.connect(this.ctx.destination);
                osc.start(time);
                osc.stop(time + 0.06);
                this.audioNodes.push(osc, oscGain);
            }
            
            playHiHat(time) {
                const bufferSize = this.ctx.sampleRate * 0.015;
                const buffer = this.ctx.createBuffer(1, bufferSize, this.ctx.sampleRate);
                const data = buffer.getChannelData(0);
                for (let i = 0; i < bufferSize; i++) data[i] = (Math.random() * 2 - 1) * Math.exp(-i / bufferSize * 8);
                const noise = this.ctx.createBufferSource();
                const gain = this.ctx.createGain();
                noise.buffer = buffer;
                gain.gain.setValueAtTime(this.volume * 0.2, time);
                gain.gain.exponentialRampToValueAtTime(0.001, time + 0.015);
                noise.connect(gain);
                gain.connect(this.ctx.destination);
                noise.start(time);
                noise.stop(time + 0.015);
                this.audioNodes.push(noise, gain);
            }
            
            playBass(time, interval) {
                const freq = 50 + Math.sin(this.beatCounter * 0.5) * 5;
                const osc = this.ctx.createOscillator();
                const gain = this.ctx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.value = freq;
                gain.gain.setValueAtTime(this.volume * 0.15, time);
                gain.gain.exponentialRampToValueAtTime(0.001, time + interval * 0.7);
                osc.connect(gain);
                gain.connect(this.ctx.destination);
                osc.start(time);
                osc.stop(time + interval * 0.7);
                this.audioNodes.push(osc, gain);
            }
        }

        // ===================== ОСНОВНАЯ ИГРА =====================
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const scoreSpan = document.getElementById('scoreDisplay');
        const coinSpan = document.getElementById('coinDisplay');
        const shopCoinSpan = document.getElementById('shopCoins');
        const pauseBtn = document.getElementById('pauseBtn');
        const fullscreenBtn = document.getElementById('fullscreenBtn');
        const gameWrapper = document.getElementById('gameWrapper');
        const shopBtn = document.getElementById('shopBtn');
        const shopOverlay = document.getElementById('shopOverlay');
        const closeShopBtn = document.getElementById('closeShop');
        const shopItemsDiv = document.getElementById('shopItems');

        const music = new MusicPlayer();

        const W = 900, H = 450;
        const GROUND_Y = 370;
        const PLAYER_X = 120;

        // ----- ПЕРСОНАЖИ -----
        const CHARACTERS = [
            { id: 'panther', name: 'Пантера', price: 0, unlocked: true, color: '#ff00ff', icon: '🐾' },
            { id: 'sonic', name: 'Соник', price: 50, unlocked: false, color: '#00aaff', icon: '🦔' },
            { id: 'shadow', name: 'Шэдоу', price: 100, unlocked: false, color: '#ff2244', icon: '🌑' },
            { id: 'cyber', name: 'Кибер-пантера', price: 200, unlocked: false, color: '#ff6600', icon: '🐱' },
            { id: 'dragon', name: 'Дракон', price: 350, unlocked: false, color: '#ff4444', icon: '🐉' },
            { id: 'phoenix', name: 'Феникс', price: 500, unlocked: false, color: '#ff8800', icon: '🔥' },
        ];

        let currentCharacter = 'panther';
        let coins = 0;

        function loadSave() {
            try {
                const saved = localStorage.getItem('cyberPantherSave');
                if (saved) {
                    const data = JSON.parse(saved);
                    coins = data.coins || 0;
                    currentCharacter = data.currentCharacter || 'panther';
                    for (let char of CHARACTERS) {
                        if (data.unlocked && data.unlocked.includes(char.id)) {
                            char.unlocked = true;
                        }
                    }
                }
            } catch(e) {}
        }

        function saveGame() {
            try {
                const data = {
                    coins: coins,
                    currentCharacter: currentCharacter,
                    unlocked: CHARACTERS.filter(c => c.unlocked).map(c => c.id)
                };
                localStorage.setItem('cyberPantherSave', JSON.stringify(data));
            } catch(e) {}
        }

        loadSave();
        updateCoinDisplay();

        function updateCoinDisplay() {
            coinSpan.textContent = Math.floor(coins);
            if (shopCoinSpan) shopCoinSpan.textContent = Math.floor(coins);
        }

        // ----- ПОЛНОЭКРАННЫЙ МАГАЗИН С БОЛЬШИМИ КНОПКАМИ -----
        function openShop() {
            shopOverlay.classList.add('active');
            renderShop();
            document.body.style.overflow = 'hidden';
        }

        function closeShop() {
            shopOverlay.classList.remove('active');
            document.body.style.overflow = '';
        }

        function renderShop() {
            if (!shopItemsDiv) return;
            shopItemsDiv.innerHTML = '';
            for (let char of CHARACTERS) {
                const item = document.createElement('div');
                item.className = `shop-item ${char.unlocked ? 'unlocked' : 'locked'}`;
                
                const info = document.createElement('div');
                info.className = 'shop-item-info';
                info.innerHTML = `
                    <div class="shop-item-icon">${char.icon}</div>
                    <div>
                        <div class="shop-item-name">${char.name}</div>
                        <div class="shop-item-status ${char.unlocked ? (char.id === currentCharacter ? 'selected-text' : 'unlocked-text') : 'locked-text'}">
                            ${char.unlocked ? (char.id === currentCharacter ? '⭐ Выбран' : '✅ Разблокирован') : `🪙 ${char.price} монет`}
                        </div>
                    </div>
                `;
                item.appendChild(info);

                const btn = document.createElement('button');
                if (char.unlocked) {
                    if (char.id === currentCharacter) {
                        btn.textContent = '✓ Выбран';
                        btn.className = 'shop-item-btn selected';
                    } else {
                        btn.textContent = 'Выбрать';
                        btn.className = 'shop-item-btn select';
                        btn.onclick = function(e) {
                            e.stopPropagation();
                            currentCharacter = char.id;
                            saveGame();
                            renderShop();
                            updateCoinDisplay();
                        };
                    }
                } else {
                    if (coins >= char.price) {
                        btn.textContent = `Купить 🪙${char.price}`;
                        btn.className = 'shop-item-btn buy';
                        btn.onclick = function(e) {
                            e.stopPropagation();
                            if (coins >= char.price) {
                                coins -= char.price;
                                char.unlocked = true;
                                currentCharacter = char.id;
                                saveGame();
                                renderShop();
                                updateCoinDisplay();
                            }
                        };
                    } else {
                        btn.textContent = `🪙 ${char.price}`;
                        btn.className = 'shop-item-btn buy-disabled';
                    }
                }
                item.appendChild(btn);
                shopItemsDiv.appendChild(item);
            }
            if (shopCoinSpan) shopCoinSpan.textContent = Math.floor(coins);
        }

        shopBtn.onclick = openShop;
        closeShopBtn.onclick = closeShop;
        shopOverlay.onclick = function(e) {
            if (e.target === shopOverlay) closeShop();
        };

        document.addEventListener('keydown', function(e) {
            if (e.key === 'Escape' && shopOverlay.classList.contains('active')) {
                closeShop();
            }
        });

        // ----- ОРИЕНТАЦИЯ -----
        function updateOrientation() {
            const isPortrait = window.innerHeight > window.innerWidth;
            gameWrapper.classList.remove('portrait', 'landscape');
            gameWrapper.classList.add(isPortrait ? 'portrait' : 'landscape');
        }
        window.addEventListener('resize', updateOrientation);
        window.addEventListener('orientationchange', function() { setTimeout(updateOrientation, 300); });
        updateOrientation();

        // ----- ПОЛНОЭКРАННЫЙ РЕЖИМ -----
        function toggleFullscreen() {
            if (!document.fullscreenElement && !document.webkitFullscreenElement) {
                const elem = gameWrapper;
                if (elem.requestFullscreen) elem.requestFullscreen();
                else if (elem.webkitRequestFullscreen) elem.webkitRequestFullscreen();
                else if (elem.msRequestFullscreen) elem.msRequestFullscreen();
                gameWrapper.classList.add('fullscreen');
                fullscreenBtn.textContent = '⛶';
            } else {
                if (document.exitFullscreen) document.exitFullscreen();
                else if (document.webkitExitFullscreen) document.webkitExitFullscreen();
                else if (document.msExitFullscreen) document.msExitFullscreen();
                gameWrapper.classList.remove('fullscreen');
                fullscreenBtn.textContent = '⛶';
            }
            setTimeout(updateOrientation, 200);
        }
        document.addEventListener('fullscreenchange', function() {
            if (!document.fullscreenElement && !document.webkitFullscreenElement) {
                gameWrapper.classList.remove('fullscreen');
                fullscreenBtn.textContent = '⛶';
            }
            setTimeout(updateOrientation, 200);
        });

        // ----- СОСТОЯНИЕ -----
        let gameOver = false;
        let paused = false;
        let score = 0;
        let frame = 0;
        let speed = 5.5;
        let combo = 0;
        let difficulty = 1;

        // ----- ИГРОК -----
        const player = {
            x: PLAYER_X,
            y: GROUND_Y - 44,
            width: 38,
            height: 46,
            vy: 0,
            gravity: 0.58,
            jumpPower: -13.5,
            isGrounded: true,
            isDucking: false,
            normalHeight: 46,
            duckHeight: 28,

            doubleJump: false,
            canDoubleJump: true,
            isDashing: false,
            dashTimer: 0,
            dashCooldown: 0,
            isRolling: false,
            rollTimer: 0,

            runCycle: 0,
            eyeBlink: 0,
            trail: [],
            
            jumpBuffer: false,
            jumpBufferTimer: 0,
            coyoteTime: 0,
            
            animation: 'idle',
            animFrame: 0,
            animTimer: 0,
            
            tailWave: 0,
            glowIntensity: 0,
        };

        // ----- ОБЪЕКТЫ -----
        let obstacles = [];
        let platforms = [];
        let coinsList = [];
        let springs = [];
        let spawnCounter = 30;
        let particles = [];
        let buildings = [];

        function generateBuildings() {
            buildings = [];
            for (let i = 0; i < 15; i++) {
                buildings.push({
                    x: i * 65 + Math.random() * 20,
                    width: 30 + Math.random() * 40,
                    height: 80 + Math.random() * 150,
                    color: `hsl(${200 + Math.random() * 40}, 80%, ${10 + Math.random() * 15}%)`,
                    neonColor: `hsl(${280 + Math.random() * 60}, 100%, 60%)`,
                    windows: Math.floor(3 + Math.random() * 5),
                });
            }
        }
        generateBuildings();

        // ----- ВСПОМОГАТЕЛЬНЫЕ -----
        function resetGame() {
            gameOver = false;
            paused = false;
            pauseBtn.textContent = '⏸ Пауза';
            pauseBtn.className = '';
            score = 0;
            frame = 0;
            speed = 5.5;
            combo = 0;
            difficulty = 1;
            obstacles = [];
            platforms = [];
            coinsList = [];
            springs = [];
            particles = [];
            player.y = GROUND_Y - player.normalHeight;
            player.height = player.normalHeight;
            player.vy = 0;
            player.isGrounded = true;
            player.isDucking = false;
            player.doubleJump = false;
            player.canDoubleJump = true;
            player.isDashing = false;
            player.dashTimer = 0;
            player.dashCooldown = 0;
            player.isRolling = false;
            player.rollTimer = 0;
            player.trail = [];
            player.runCycle = 0;
            player.jumpBuffer = false;
            player.jumpBufferTimer = 0;
            player.coyoteTime = 0;
            player.animation = 'idle';
            player.animFrame = 0;
            player.glowIntensity = 0;
            player.tailWave = 0;
            spawnCounter = 30;
            updateScore();
            generateBuildings();
            saveGame();
        }

        function togglePause() {
            if (gameOver) return;
            paused = !paused;
            pauseBtn.textContent = paused ? '▶ Продолжить' : '⏸ Пауза';
            pauseBtn.className = paused ? 'paused' : '';
            if (paused && music.isPlaying) music.stop();
        }

        function updateScore() {
            scoreSpan.textContent = Math.floor(score);
            updateCoinDisplay();
        }

        function rectCollide(r1, r2) {
            return !(r2.x > r1.x + r1.width ||
                r2.x + r2.width < r1.x ||
                r2.y > r1.y + r1.height ||
                r2.y + r2.height < r1.y);
        }

        // ----- СПАВН -----
        function spawnObstacle() {
            const r = Math.random();
            let type, height, width, neonColor;
            const neonColors = ['#ff00ff', '#00ffff', '#ff6600', '#ff0066', '#66ff00', '#ffcc00'];
            neonColor = neonColors[Math.floor(Math.random() * neonColors.length)];

            if (r < 0.11) {
                type = 'low'; height = 18; width = 30;
            } else if (r < 0.22) {
                type = 'high'; height = 38; width = 22;
            } else if (r < 0.33) {
                type = 'wide'; height = 28; width = 44;
            } else if (r < 0.44) {
                type = 'spike'; height = 24; width = 18;
            } else if (r < 0.55) {
                type = 'moving'; height = 30; width = 26;
            } else if (r < 0.66) {
                type = 'crate'; height = 34; width = 28;
            } else if (r < 0.77) {
                type = 'barrel'; height = 32; width = 30;
            } else if (r < 0.88) {
                type = 'saw'; height = 20; width = 20;
            } else {
                type = 'wall'; height = 45; width = 18;
            }

            obstacles.push({
                x: W + 40,
                y: GROUND_Y - height,
                width: width,
                height: height,
                type: type,
                scored: false,
                phase: Math.random() * Math.PI * 2,
                startY: GROUND_Y - height,
                rotation: 0,
                neonColor: neonColor,
                glowIntensity: 0.5 + Math.random() * 0.5,
                isStandable: true,
                isHazard: (type === 'spike' || type === 'saw'),
            });
        }

        function spawnPlatform() {
            if (platforms.length > 6) return;
            const height = 14;
            const width = 45 + Math.random() * 55;
            const y = GROUND_Y - 40 - Math.random() * 110;
            const type = Math.random() < 0.35 ? 'moving_platform' : 'platform';
            const neonColor = ['#00ffff', '#ff00ff', '#ff6600', '#00ff66'][Math.floor(Math.random() * 4)];
            
            platforms.push({
                x: W + 20 + Math.random() * 100,
                y: y,
                width: width,
                height: height,
                type: type,
                scored: false,
                phase: Math.random() * Math.PI * 2,
                startY: y,
                hasCoin: Math.random() < 0.3,
                coinCollected: false,
                coinX: 0,
                coinY: 0,
                color: `hsl(${Math.random() * 60 + 200}, 80%, 30%)`,
                neonColor: neonColor,
            });
            
            const plat = platforms[platforms.length - 1];
            if (plat.hasCoin) {
                plat.coinX = plat.x + plat.width/2;
                plat.coinY = plat.y - 20;
            }
        }

        function spawnCoin() {
            if (coinsList.length > 8) return;
            const y = GROUND_Y - 30 - Math.random() * 100;
            const colors = ['#ff00ff', '#00ffff', '#ffcc00', '#ff6600'];
            coinsList.push({
                x: W + 20 + Math.random() * 150,
                y: y,
                width: 16,
                height: 16,
                collected: false,
                phase: Math.random() * Math.PI * 2,
                color: colors[Math.floor(Math.random() * colors.length)],
            });
        }

        function spawnSpring() {
            if (springs.length > 3) return;
            const height = 20;
            springs.push({
                x: W + 30 + Math.random() * 80,
                y: GROUND_Y - height,
                width: 18,
                height: height,
                activated: false,
                cooldown: 0,
                neonColor: '#00ffcc',
            });
        }

        function spawnParticles(x, y, color, count) {
            for (let i = 0; i < count; i++) {
                const angle = Math.random() * Math.PI * 2;
                const speed = 1 + Math.random() * 5;
                particles.push({
                    x: x,
                    y: y,
                    vx: Math.cos(angle) * speed,
                    vy: Math.sin(angle) * speed - 1,
                    life: 15 + Math.random() * 25,
                    maxLife: 40,
                    size: 1 + Math.random() * 4,
                    color: color,
                    gravity: 0.1,
                    glow: true,
                });
            }
        }

        // ----- ОБНОВЛЕНИЕ -----
        function update() {
            if (gameOver || paused) return;
            frame++;

            for (let b of buildings) {
                b.x -= speed * 0.3;
                if (b.x + b.width < -50) {
                    b.x = W + 50 + Math.random() * 100;
                    b.width = 30 + Math.random() * 40;
                    b.height = 80 + Math.random() * 150;
                    b.color = `hsl(${200 + Math.random() * 40}, 80%, ${10 + Math.random() * 15}%)`;
                    b.neonColor = `hsl(${280 + Math.random() * 60}, 100%, 60%)`;
                }
            }

            for (let i = particles.length - 1; i >= 0; i--) {
                const p = particles[i];
                p.x += p.vx;
                p.y += p.vy;
                p.vy += p.gravity;
                p.life--;
                if (p.life <= 0) particles.splice(i, 1);
            }

            player.tailWave = Math.sin(frame * 0.08) * 3;
            
            if (player.isGrounded && !player.isDucking && !player.isRolling) {
                if (player.isDashing) {
                    player.animation = 'dash';
                    player.glowIntensity = Math.min(player.glowIntensity + 0.05, 1);
                } else {
                    player.animation = 'run';
                    player.glowIntensity = Math.max(player.glowIntensity - 0.02, 0);
                }
                player.runCycle = (player.runCycle + speed * 0.5) % 20;
            } else if (player.isDucking && player.isGrounded) {
                player.animation = 'duck';
                player.glowIntensity = Math.max(player.glowIntensity - 0.02, 0);
            } else if (player.isRolling) {
                player.animation = 'roll';
                player.runCycle = (player.runCycle + speed * 0.8) % 15;
                player.glowIntensity = Math.min(player.glowIntensity + 0.03, 1);
            } else if (!player.isGrounded) {
                player.animation = 'jump';
                player.glowIntensity = Math.max(player.glowIntensity - 0.02, 0);
            }
            
            player.animTimer++;
            if (player.animTimer > 8) {
                player.animTimer = 0;
                player.animFrame = (player.animFrame + 1) % 4;
            }

            if (player.isDashing) {
                if (frame % 2 === 0) {
                    player.trail.push({
                        x: player.x + player.width/2,
                        y: player.y + player.height/2,
                        life: 15,
                        maxLife: 15,
                    });
                    spawnParticles(player.x + player.width/2, player.y + player.height/2, '#ff00ff', 3);
                }
            }
            player.trail = player.trail.filter(t => { t.life--; return t.life > 0; });

            // УПРАВЛЕНИЕ
            if (player.jumpBuffer) {
                player.jumpBufferTimer++;
                if (player.jumpBufferTimer > 8) player.jumpBuffer = false;
            }

            if (player.isGrounded) {
                player.coyoteTime = 6;
            } else {
                player.coyoteTime--;
                if (player.coyoteTime < 0) player.coyoteTime = 0;
            }

            if (player.jumpBuffer && (player.isGrounded || player.coyoteTime > 0) && !player.isDucking && !player.isRolling) {
                player.vy = player.jumpPower;
                player.isGrounded = false;
                player.coyoteTime = 0;
                player.canDoubleJump = true;
                player.jumpBuffer = false;
                player.jumpBufferTimer = 0;
                spawnParticles(player.x + player.width/2, player.y + player.height, '#00ffff', 8);
            }
            else if (player.jumpBuffer && !player.isGrounded && player.canDoubleJump && !player.isRolling) {
                player.vy = player.jumpPower * 0.85;
                player.canDoubleJump = false;
                player.doubleJump = true;
                combo = Math.min(combo + 2, 10);
                player.jumpBuffer = false;
                player.jumpBufferTimer = 0;
                spawnParticles(player.x + player.width/2, player.y + player.height/2, '#ff00ff', 12);
            }

            if (player.dashTimer > 0) {
                player.dashTimer--;
                if (player.dashTimer === 0) player.isDashing = false;
            }
            if (player.dashCooldown > 0) player.dashCooldown--;

            if (player.isRolling) {
                player.rollTimer--;
                if (player.rollTimer === 0) {
                    player.isRolling = false;
                    if (player.isGrounded) {
                        player.height = player.normalHeight;
                        player.y = GROUND_Y - player.normalHeight;
                    }
                }
            }

            let gravityMul = player.isRolling ? 0.3 : 1.0;
            player.vy += player.gravity * gravityMul;
            player.y += player.vy;

            const playerRect = {
                x: player.x + 3,
                y: player.y + 2,
                width: player.width - 6,
                height: player.height - 4,
            };

            let landedOnObject = false;
            
            // Платформы
            for (let plat of platforms) {
                if (player.vy > 0) {
                    const platRect = {
                        x: plat.x + 5,
                        y: plat.y,
                        width: plat.width - 10,
                        height: plat.height,
                    };
                    const playerBottom = {
                        x: player.x + 4,
                        y: player.y + player.height - 2,
                        width: player.width - 8,
                        height: 4,
                    };
                    if (rectCollide(playerBottom, platRect)) {
                        player.y = plat.y - player.height;
                        player.vy = 0;
                        landedOnObject = true;
                        combo = Math.min(combo + 1, 10);
                        spawnParticles(player.x + player.width/2, player.y + player.height, plat.neonColor, 4);
                        break;
                    }
                }
            }

            // Препятствия
            if (!landedOnObject) {
                for (let obs of obstacles) {
                    if (!obs.isStandable || obs.isHazard) continue;
                    if (player.vy > 0) {
                        const obsRect = {
                            x: obs.x + 2,
                            y: obs.y,
                            width: obs.width - 4,
                            height: obs.height,
                        };
                        const playerBottom = {
                            x: player.x + 4,
                            y: player.y + player.height - 2,
                            width: player.width - 8,
                            height: 4,
                        };
                        if (rectCollide(playerBottom, obsRect)) {
                            player.y = obs.y - player.height;
                            player.vy = 0;
                            landedOnObject = true;
                            combo = Math.min(combo + 2, 10);
                            spawnParticles(player.x + player.width/2, player.y + player.height, obs.neonColor, 6);
                            break;
                        }
                    }
                }
            }

            // Земля
            if (!landedOnObject) {
                const floorY = GROUND_Y - player.height;
                if (player.y >= floorY) {
                    player.y = floorY;
                    player.vy = 0;
                    if (!player.isGrounded) {
                        player.isGrounded = true;
                        player.canDoubleJump = true;
                        player.doubleJump = false;
                        combo = Math.min(combo + 1, 10);
                        spawnParticles(player.x + player.width/2, player.y + player.height, '#00ffcc', 4);
                    }
                } else {
                    player.isGrounded = false;
                }
            } else {
                player.isGrounded = true;
                player.canDoubleJump = true;
                player.doubleJump = false;
            }

            // Приседание
            if (player.isDucking && player.isGrounded && !player.isRolling) {
                player.height = player.duckHeight;
                player.y = player.y + (player.normalHeight - player.duckHeight);
            } else if (!player.isDucking && !player.isRolling) {
                player.height = player.normalHeight;
                if (player.isGrounded) {
                    player.y = player.y - (player.normalHeight - player.duckHeight);
                }
            }

            if (player.isGrounded && !player.isDucking && !player.isRolling) {
                player.runCycle = (player.runCycle + speed * 0.5) % 20;
            } else if (player.isRolling) {
                player.runCycle = (player.runCycle + speed * 0.8) % 15;
            }
            player.eyeBlink = (player.eyeBlink + 1) % 200;

            if (spawnCounter <= 0) {
                spawnObstacle();
                if (Math.random() < 0.45) {
                    spawnPlatform();
                    if (Math.random() < 0.3) spawnPlatform();
                }
                if (Math.random() < 0.2) spawnCoin();
                if (Math.random() < 0.08) spawnSpring();
                spawnCounter = Math.floor(Math.random() * 20 + 18) / difficulty;
                if (speed < 9.0) speed += 0.02;
                difficulty = 1 + Math.floor(score / 200) * 0.2;
            } else {
                spawnCounter--;
            }

            // Опасные препятствия
            for (let obs of obstacles) {
                if (!obs.isHazard) continue;
                const obsRect = {
                    x: obs.x + 2,
                    y: obs.y + 2,
                    width: obs.width - 4,
                    height: obs.height - 4,
                };
                if (rectCollide(playerRect, obsRect)) {
                    if (player.isRolling && obs.type !== 'spike') continue;
                    if (player.isDashing) continue;
                    gameOver = true;
                    spawnParticles(player.x + player.width/2, player.y + player.height/2, '#ff00ff', 30);
                    if (music.isPlaying) music.stop();
                }
            }

            // Движение препятствий
            for (let i = obstacles.length - 1; i >= 0; i--) {
                const obs = obstacles[i];
                let moveSpeed = speed;

                if (obs.type === 'moving') {
                    obs.phase += 0.04;
                    obs.y = obs.startY + Math.sin(obs.phase) * 15;
                }
                if (obs.type === 'saw') {
                    obs.rotation += 0.1;
                }

                obs.x -= moveSpeed;

                if (!obs.scored && obs.x + obs.width < player.x) {
                    obs.scored = true;
                    let bonus = 10 + Math.floor(combo * 0.5);
                    score += bonus;
                    coins += 1 + Math.floor(combo / 3);
                    updateScore();
                    spawnParticles(obs.x + obs.width/2, obs.y + obs.height/2, obs.neonColor, 8);
                }

                if (obs.x + obs.width < -30) {
                    obstacles.splice(i, 1);
                    continue;
                }
            }

            // Платформы
            for (let i = platforms.length - 1; i >= 0; i--) {
                const plat = platforms[i];
                
                if (plat.type === 'moving_platform') {
                    plat.phase += 0.025;
                    plat.y = plat.startY + Math.sin(plat.phase) * 35;
                }
                
                plat.x -= speed;

                if (plat.hasCoin && !plat.coinCollected) {
                    plat.coinX = plat.x + plat.width/2;
                    plat.coinY = plat.y - 20;
                    const coinRect = {
                        x: plat.coinX - 8,
                        y: plat.coinY - 8,
                        width: 16,
                        height: 16,
                    };
                    if (rectCollide(playerRect, coinRect)) {
                        plat.coinCollected = true;
                        score += 25;
                        coins += 3;
                        updateScore();
                        combo = Math.min(combo + 2, 10);
                        spawnParticles(plat.coinX, plat.coinY, '#ffcc00', 15);
                    }
                }

                if (plat.x + plat.width < -30) {
                    platforms.splice(i, 1);
                    continue;
                }
            }

            // Монетки
            for (let i = coinsList.length - 1; i >= 0; i--) {
                const coin = coinsList[i];
                coin.x -= speed;
                coin.phase += 0.05;

                if (!coin.collected) {
                    const coinRect = {
                        x: coin.x - 6,
                        y: coin.y - 6 + Math.sin(coin.phase) * 3,
                        width: 12,
                        height: 12,
                    };
                    if (rectCollide(playerRect, coinRect)) {
                        coin.collected = true;
                        score += 15;
                        coins += 2;
                        updateScore();
                        combo = Math.min(combo + 1, 10);
                        spawnParticles(coin.x, coin.y, coin.color, 12);
                    }
                }

                if (coin.x < -30) coinsList.splice(i, 1);
            }

            // Пружины
            for (let i = springs.length - 1; i >= 0; i--) {
                const spring = springs[i];
                spring.x -= speed;

                if (spring.cooldown > 0) spring.cooldown--;

                if (!spring.activated && spring.cooldown === 0) {
                    const springRect = {
                        x: spring.x,
                        y: spring.y,
                        width: spring.width,
                        height: spring.height,
                    };
                    if (rectCollide(playerRect, springRect) && player.vy > 0) {
                        spring.activated = true;
                        spring.cooldown = 30;
                        player.vy = -14;
                        player.isGrounded = false;
                        combo = Math.min(combo + 3, 10);
                        spawnParticles(spring.x + spring.width/2, spring.y, '#00ffcc', 20);
                        setTimeout(() => { spring.activated = false; }, 300);
                    }
                }

                if (spring.x < -30) springs.splice(i, 1);
            }

            if (player.y > H + 60) gameOver = true;
            if (combo > 0 && player.isGrounded) {
                combo = Math.max(combo - 0.03, 0);
            }
            saveGame();
        }

        // ----- ОТРИСОВКА (сокращённая) -----
        function draw() {
            ctx.clearRect(0, 0, W, H);

            // Небо
            const skyGrad = ctx.createLinearGradient(0, 0, 0, H);
            skyGrad.addColorStop(0, '#050510');
            skyGrad.addColorStop(0.3, '#0a0515');
            skyGrad.addColorStop(0.6, '#150520');
            skyGrad.addColorStop(0.8, '#0a0510');
            skyGrad.addColorStop(1, '#050508');
            ctx.fillStyle = skyGrad;
            ctx.fillRect(0, 0, W, H);

            // Звёзды
            for (let i = 0; i < 80; i++) {
                let sx = (i * 137 + i * i * 13) % W;
                let sy = (i * 97 + i * i * 7) % (GROUND_Y - 50);
                let size = 0.5 + (i % 3) * 0.5;
                let alpha = 0.3 + Math.sin(frame * 0.02 + i * 1.7) * 0.2;
                ctx.globalAlpha = alpha;
                ctx.fillStyle = i % 3 === 0 ? '#ff66ff' : '#66ffff';
                ctx.shadowColor = i % 3 === 0 ? '#ff66ff' : '#66ffff';
                ctx.shadowBlur = 5;
                ctx.beginPath();
                ctx.arc(sx, sy, size, 0, Math.PI * 2);
                ctx.fill();
            }
            ctx.globalAlpha = 1;
            ctx.shadowBlur = 0;

            // Здания
            for (let b of buildings) {
                ctx.fillStyle = b.color;
                ctx.shadowColor = b.neonColor;
                ctx.shadowBlur = 5;
                ctx.fillRect(b.x, GROUND_Y - b.height, b.width, b.height);
                const winRows = Math.floor(b.height / 20);
                const winCols = Math.floor(b.width / 15);
                for (let r = 0; r < winRows; r++) {
                    for (let c = 0; c < winCols; c++) {
                        if (Math.random() > 0.3 || frame % 120 < 5) {
                            const bright = 0.3 + Math.sin(frame * 0.02 + r + c) * 0.2;
                            ctx.fillStyle = `rgba(255, 255, 200, ${bright * 0.6})`;
                            ctx.shadowBlur = 0;
                            ctx.fillRect(b.x + 4 + c * 15, GROUND_Y - b.height + 8 + r * 20, 6, 8);
                            if (r % 2 === 0 && c % 2 === 0) {
                                ctx.fillStyle = `rgba(0, 255, 200, ${bright * 0.3})`;
                                ctx.shadowColor = '#00ffcc';
                                ctx.shadowBlur = 8;
                                ctx.fillRect(b.x + 4 + c * 15, GROUND_Y - b.height + 8 + r * 20, 6, 8);
                            }
                        }
                    }
                }
                ctx.shadowBlur = 0;
            }

            // Неоновые линии
            for (let i = 0; i < 30; i++) {
                let lx = (i * 35 + frame * 2) % (W + 50) - 25;
                let alpha = 0.1 + Math.sin(frame * 0.05 + i) * 0.05;
                ctx.fillStyle = `rgba(0, 255, 200, ${alpha})`;
                ctx.fillRect(lx, GROUND_Y - 2, 2, 4);
                if (i % 3 === 0) {
                    ctx.fillStyle = `rgba(255, 0, 255, ${alpha * 0.5})`;
                    ctx.fillRect(lx + 10, GROUND_Y - 2, 2, 4);
                }
            }

            // Земля
            const groundGrad = ctx.createLinearGradient(0, GROUND_Y, 0, H);
            groundGrad.addColorStop(0, '#0a0a2a');
            groundGrad.addColorStop(0.1, '#0a051a');
            groundGrad.addColorStop(0.5, '#080510');
            groundGrad.addColorStop(1, '#040208');
            ctx.fillStyle = groundGrad;
            ctx.fillRect(0, GROUND_Y, W, H - GROUND_Y + 6);

            ctx.shadowColor = '#00ffcc';
            ctx.shadowBlur = 15;
            ctx.strokeStyle = '#00ffcc';
            ctx.lineWidth = 2;
            ctx.beginPath();
            ctx.moveTo(0, GROUND_Y);
            ctx.lineTo(W, GROUND_Y);
            ctx.stroke();
            ctx.shadowBlur = 0;

            // Частицы
            for (let p of particles) {
                const alpha = p.life / p.maxLife;
                ctx.globalAlpha = alpha;
                ctx.fillStyle = p.color;
                ctx.shadowColor = p.color;
                ctx.shadowBlur = p.glow ? 15 : 0;
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.size * alpha, 0, Math.PI * 2);
                ctx.fill();
            }
            ctx.globalAlpha = 1;
            ctx.shadowBlur = 0;

            // Препятствия
            for (let obs of obstacles) {
                ctx.shadowColor = obs.neonColor;
                ctx.shadowBlur = 20;
                ctx.shadowOffsetY = 4;

                if (obs.isHazard) {
                    ctx.shadowColor = '#ff0000';
                    ctx.shadowBlur = 30;
                }

                if (obs.type === 'high' || obs.type === 'wide') {
                    const grad = ctx.createLinearGradient(obs.x, obs.y, obs.x + obs.width, obs.y + obs.height);
                    grad.addColorStop(0, obs.isHazard ? '#ff2222' : '#1a0a2a');
                    grad.addColorStop(0.5, obs.isHazard ? '#cc1111' : '#0a051a');
                    grad.addColorStop(1, obs.isHazard ? '#880000' : '#05020a');
                    ctx.fillStyle = grad;
                    ctx.fillRect(obs.x, obs.y, obs.width, obs.height);
                    ctx.fillStyle = obs.isHazard ? '#ff4444' : obs.neonColor;
                    ctx.shadowBlur = obs.isHazard ? 30 : 15;
                    ctx.fillRect(obs.x+2, obs.y-1, obs.width-4, 3);
                    ctx.fillRect(obs.x+2, obs.y+obs.height-3, obs.width-4, 3);
                    ctx.shadowBlur = 0;
                } else if (obs.type === 'low') {
                    ctx.fillStyle = obs.isHazard ? '#ff2222' : '#0a0a2a';
                    ctx.beginPath();
                    ctx.ellipse(obs.x+obs.width/2, obs.y+obs.height/2, obs.width/2, obs.height/2, 0, 0, Math.PI*2);
                    ctx.fill();
                    ctx.fillStyle = obs.isHazard ? '#ff4444' : obs.neonColor;
                    ctx.shadowBlur = obs.isHazard ? 30 : 15;
                    ctx.beginPath();
                    ctx.ellipse(obs.x+obs.width/2, obs.y+obs.height/2, obs.width/2-2, obs.height/2-2, 0, 0, Math.PI*2);
                    ctx.stroke();
                    ctx.shadowBlur = 0;
                } else if (obs.type === 'spike') {
                    ctx.fillStyle = '#ff4444';
                    ctx.shadowColor = '#ff0000';
                    ctx.shadowBlur = 30;
                    for (let s = 0; s < 3; s++) {
                        ctx.beginPath();
                        ctx.moveTo(obs.x + s * 7, obs.y + obs.height);
                        ctx.lineTo(obs.x + s * 7 + 6, obs.y);
                        ctx.lineTo(obs.x + s * 7 + 12, obs.y + obs.height);
                        ctx.fill();
                    }
                    ctx.shadowBlur = 0;
                } else if (obs.type === 'saw') {
                    ctx.shadowColor = '#ff0000';
                    ctx.shadowBlur = 30;
                    ctx.save();
                    ctx.translate(obs.x + obs.width/2, obs.y + obs.height/2);
                    ctx.rotate(obs.rotation);
                    ctx.fillStyle = '#440000';
                    ctx.beginPath();
                    ctx.arc(0, 0, obs.width/2, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.strokeStyle = '#ff4444';
                    ctx.lineWidth = 2;
                    ctx.beginPath();
                    ctx.arc(0, 0, obs.width/2, 0, Math.PI * 2);
                    ctx.stroke();
                    for (let s = 0; s < 8; s++) {
                        let angle = s * Math.PI/4;
                        ctx.fillStyle = '#ff4444';
                        ctx.beginPath();
                        ctx.moveTo(Math.cos(angle) * 4, Math.sin(angle) * 4);
                        ctx.lineTo(Math.cos(angle) * obs.width/2, Math.sin(angle) * obs.width/2);
                        ctx.lineTo(Math.cos(angle + 0.2) * obs.width/2, Math.sin(angle + 0.2) * obs.width/2);
                        ctx.fill();
                    }
                    ctx.restore();
                } else {
                    ctx.fillStyle = '#0a0a1a';
                    ctx.fillRect(obs.x, obs.y, obs.width, obs.height);
                    ctx.fillStyle = obs.neonColor;
                    ctx.shadowBlur = 15;
                    ctx.fillRect(obs.x+2, obs.y-1, obs.width-4, 2);
                    ctx.shadowBlur = 0;
                }
                ctx.shadowBlur = 0;
                ctx.shadowOffsetY = 0;
            }

            // Платформы
            for (let plat of platforms) {
                ctx.shadowColor = plat.neonColor;
                ctx.shadowBlur = 20;
                ctx.shadowOffsetY = 4;

                const grad = ctx.createLinearGradient(plat.x, plat.y, plat.x, plat.y + plat.height);
                grad.addColorStop(0, plat.color || '#1a1a3a');
                grad.addColorStop(0.4, '#0a0a2a');
                grad.addColorStop(1, '#05051a');
                ctx.fillStyle = grad;
                ctx.fillRect(plat.x, plat.y, plat.width, plat.height);

                ctx.fillStyle = plat.neonColor;
                ctx.shadowBlur = 15;
                ctx.fillRect(plat.x + 2, plat.y - 1, plat.width - 4, 2);
                ctx.shadowBlur = 0;

                if (plat.hasCoin && !plat.coinCollected) {
                    ctx.shadowColor = '#ffcc00';
                    ctx.shadowBlur = 20;
                    ctx.fillStyle = '#ffcc00';
                    ctx.beginPath();
                    ctx.arc(plat.coinX, plat.coinY + Math.sin(frame * 0.05) * 3, 10, 0, Math.PI * 2);
                    ctx.fill();
                    ctx.shadowBlur = 0;
                }
                ctx.shadowBlur = 0;
            }

            // Монетки
            for (let coin of coinsList) {
                if (coin.collected) continue;
                const cy = coin.y + Math.sin(coin.phase) * 3;
                const scale = 1 + Math.sin(coin.phase) * 0.1;
                ctx.shadowColor = coin.color;
                ctx.shadowBlur = 25;
                ctx.fillStyle = coin.color;
                ctx.beginPath();
                ctx.arc(coin.x, cy, 8 * scale, 0, Math.PI * 2);
                ctx.fill();
                ctx.shadowBlur = 10;
                ctx.fillStyle = '#ffffff';
                ctx.beginPath();
                ctx.arc(coin.x, cy, 4 * scale, 0, Math.PI * 2);
                ctx.fill();
                ctx.shadowBlur = 0;
            }

            // Пружины
            for (let spring of springs) {
                ctx.shadowColor = spring.neonColor;
                ctx.shadowBlur = 15;
                ctx.shadowOffsetY = 3;
                ctx.fillStyle = '#1a1a3a';
                ctx.fillRect(spring.x, spring.y + spring.height - 4, spring.width, 4);
                ctx.strokeStyle = spring.neonColor;
                ctx.lineWidth = 2;
                const compress = spring.activated ? 0.6 : 1;
                for (let s = 0; s < 5; s++) {
                    let sy = spring.y + s * (spring.height / 5) * compress;
                    ctx.beginPath();
                    ctx.moveTo(spring.x + 2, sy);
                    ctx.lineTo(spring.x + spring.width - 2, sy + 4);
                    ctx.stroke();
                }
                ctx.fillStyle = spring.activated ? '#ff00ff' : '#00ffcc';
                ctx.fillRect(spring.x, spring.y + spring.height * compress - 3, spring.width, 4);
                ctx.shadowBlur = 0;
            }

            // Персонаж
            drawCharacter();

            // Комбо
            if (combo > 2) {
                ctx.font = 'bold 24px "Segoe UI", Arial';
                ctx.fillStyle = '#ff00ff';
                ctx.textAlign = 'left';
                ctx.shadowColor = 'rgba(255,0,255,0.5)';
                ctx.shadowBlur = 30;
                ctx.fillText(`⚡ x${Math.floor(combo)}`, 20, 45);
                ctx.shadowBlur = 0;
            }

            ctx.font = '13px "Segoe UI", Arial';
            ctx.fillStyle = 'rgba(0,255,200,0.4)';
            ctx.textAlign = 'right';
            ctx.fillText(`платформ: ${platforms.length}`, W - 20, 28);

            // Пауза
            if (paused && !gameOver) {
                ctx.fillStyle = 'rgba(0,0,0,0.6)';
                ctx.fillRect(0, 0, W, H);
                ctx.font = 'bold 60px "Segoe UI", Arial';
                ctx.textAlign = 'center';
                ctx.shadowColor = '#ff00ff';
                ctx.shadowBlur = 50;
                ctx.fillStyle = '#ff00ff';
                ctx.fillText('⏸ ПАУЗА', W/2, H/2 - 10);
                ctx.shadowBlur = 0;
                ctx.font = '20px "Segoe UI", Arial';
                ctx.fillStyle = '#00ffcc';
                ctx.fillText('нажмите кнопку "Продолжить"', W/2, H/2 + 50);
            }

            // Game Over
            if (gameOver) {
                ctx.fillStyle = 'rgba(0,0,0,0.8)';
                ctx.fillRect(0, 0, W, H);
                ctx.font = 'bold 56px "Segoe UI", Arial';
                ctx.textAlign = 'center';
                ctx.shadowColor = '#ff00ff';
                ctx.shadowBlur = 50;
                ctx.fillStyle = '#ff00ff';
                ctx.fillText('💀 GAME OVER', W/2, H/2 - 10);
                ctx.shadowBlur = 0;
                ctx.font = '22px "Segoe UI", Arial';
                ctx.fillStyle = '#00ffcc';
                ctx.shadowColor = '#00ffcc';
                ctx.shadowBlur = 20;
                ctx.fillText('нажми "Новая игра"', W/2, H/2 + 60);
                ctx.shadowBlur = 0;
                ctx.fillStyle = '#66aacc';
                ctx.font = '16px "Segoe UI", Arial';
                ctx.fillText(`очков: ${Math.floor(score)}`, W/2, H/2 + 100);
            }
        }

        // ----- ОТРИСОВКА ПЕРСОНАЖА -----
        function drawCharacter() {
            const char = CHARACTERS.find(c => c.id === currentCharacter);
            if (!char) return;
            
            const colors = {
                'panther': { body: '#3a2a4a', glow: '#ff00ff', trail: '#ff00ff' },
                'sonic': { body: '#1f7fc9', glow: '#00aaff', trail: '#00aaff' },
                'shadow': { body: '#440011', glow: '#ff2244', trail: '#ff2244' },
                'cyber': { body: '#1a0a2a', glow: '#ff6600', trail: '#ff6600' },
                'dragon': { body: '#2a0a0a', glow: '#ff4444', trail: '#ff4444' },
                'phoenix': { body: '#2a1a0a', glow: '#ff8800', trail: '#ff8800' },
            };
            
            const col = colors[char.id] || colors['panther'];
            
            const px = player.x, py = player.y;
            const pw = player.width, ph = player.height;
            const duck = player.isDucking && player.isGrounded;
            const roll = player.isRolling;
            const dash = player.isDashing;

            // След
            for (let t of player.trail) {
                const alpha = t.life / t.maxLife;
                ctx.globalAlpha = alpha * 0.3;
                const grad = ctx.createRadialGradient(t.x, t.y, 0, t.x, t.y, 20 * (1 - alpha * 0.5));
                grad.addColorStop(0, col.trail);
                grad.addColorStop(0.5, col.trail + '88');
                grad.addColorStop(1, 'rgba(100,0,200,0)');
                ctx.fillStyle = grad;
                ctx.beginPath();
                ctx.arc(t.x, t.y, 20 * (1 - alpha * 0.3), 0, Math.PI * 2);
                ctx.fill();
            }
            ctx.globalAlpha = 1;

            // Тень
            ctx.fillStyle = 'rgba(100,50,200,0.2)';
            ctx.shadowColor = col.glow;
            ctx.shadowBlur = 10;
            ctx.beginPath();
            ctx.ellipse(px + pw/2, GROUND_Y + 3, pw/2 + 12, 6 + Math.sin(frame * 0.05) * 1, 0, 0, Math.PI*2);
            ctx.fill();
            ctx.shadowBlur = 0;

            // Свечение
            if (dash || roll) {
                const glow = dash ? 50 : 30;
                const grad = ctx.createRadialGradient(px + pw/2, py + ph/2, 0, px + pw/2, py + ph/2, glow);
                grad.addColorStop(0, col.glow + '33');
                grad.addColorStop(0.5, col.glow + '11');
                grad.addColorStop(1, col.glow + '00');
                ctx.fillStyle = grad;
                ctx.beginPath();
                ctx.arc(px + pw/2, py + ph/2, glow, 0, Math.PI * 2);
                ctx.fill();
            }

            ctx.shadowColor = 'rgba(0,0,0,0.5)';
            ctx.shadowBlur = 20;
            ctx.shadowOffsetY = 4;

            if (roll) {
                const grad = ctx.createRadialGradient(px+8, py+6, 4, px+14, py+14, 22);
                grad.addColorStop(0, col.glow);
                grad.addColorStop(0.3, col.glow + 'aa');
                grad.addColorStop(0.6, col.body);
                grad.addColorStop(0.8, col.body + '88');
                grad.addColorStop(1, col.body + '44');
                ctx.fillStyle = grad;
                ctx.shadowColor = col.glow + '66';
                ctx.shadowBlur = 40;
                ctx.beginPath();
                ctx.arc(px + pw/2, py + ph/2, 20, 0, Math.PI*2);
                ctx.fill();
                ctx.shadowBlur = 20;
                ctx.fillStyle = col.glow;
                for (let i = 0; i < 6; i++) {
                    let angle = i * Math.PI/3 + frame * 0.08;
                    let dist = 18 + Math.sin(frame * 0.1 + i) * 2;
                    ctx.beginPath();
                    ctx.arc(px + pw/2 + Math.cos(angle) * dist, py + ph/2 + Math.sin(angle) * dist, 4, 0, Math.PI*2);
                    ctx.fill();
                }
                ctx.shadowBlur = 0;
                ctx.fillStyle = 'rgba(255,255,255,0.2)';
                ctx.beginPath();
                ctx.arc(px + pw/2 - 5, py + ph/2 - 6, 5, 0, Math.PI*2);
                ctx.fill();
            } else {
                // Тело
                const bodyGrad = ctx.createRadialGradient(px+10, py+8, 4, px+16, py+16, 26);
                bodyGrad.addColorStop(0, col.glow);
                bodyGrad.addColorStop(0.2, col.body);
                bodyGrad.addColorStop(0.5, col.body);
                bodyGrad.addColorStop(0.8, col.body + '88');
                bodyGrad.addColorStop(1, col.body + '44');
                ctx.fillStyle = bodyGrad;
                ctx.shadowColor = 'rgba(0,0,0,0.5)';
                ctx.shadowBlur = 18;
                ctx.beginPath();
                ctx.ellipse(px + pw/2, py + ph/2 - (duck? 6 : 0), pw/2-2, ph/2-4, 0, 0, Math.PI*2);
                ctx.fill();

                // Линии
                ctx.shadowBlur = 15;
                ctx.shadowColor = col.glow;
                ctx.strokeStyle = col.glow + '44';
                ctx.lineWidth = 1;
                ctx.beginPath();
                ctx.moveTo(px + 8, py + 12);
                ctx.quadraticCurveTo(px + pw/2, py + 6, px + pw - 8, py + 12);
                ctx.stroke();
                ctx.beginPath();
                ctx.moveTo(px + 10, py + 18);
                ctx.quadraticCurveTo(px + pw/2, py + 12, px + pw - 10, py + 18);
                ctx.stroke();

                // Брюшко
                ctx.shadowBlur = 8;
                ctx.shadowColor = 'rgba(0,0,0,0.3)';
                const bellyGrad = ctx.createRadialGradient(px + pw/2 - 4, py + ph/2 + 2, 2, px + pw/2 - 4, py + ph/2 + 2, 14);
                bellyGrad.addColorStop(0, col.body + '88');
                bellyGrad.addColorStop(0.5, col.body + '44');
                bellyGrad.addColorStop(1, col.body);
                ctx.fillStyle = bellyGrad;
                ctx.beginPath();
                ctx.ellipse(px + pw/2 - 4, py + ph/2 + 2, 14, 14, 0, 0, Math.PI*2);
                ctx.fill();

                // Хвост
                ctx.shadowBlur = 15;
                ctx.shadowColor = col.glow;
                ctx.fillStyle = col.body;
                const tailLen = duck ? 20 : 40;
                const tailWave = player.tailWave;
                ctx.beginPath();
                ctx.moveTo(px + pw - 6, py + ph/2 - 4);
                for (let t = 0; t <= tailLen; t += 4) {
                    let tx = px + pw - 6 + t * 0.8 + Math.sin(frame * 0.06 + t * 0.15) * 4;
                    let ty = py + ph/2 - 4 + t * 0.3 + Math.sin(frame * 0.08 + t * 0.12) * 3 + tailWave * 0.3;
                    ctx.lineTo(tx, ty);
                }
                ctx.strokeStyle = col.body;
                ctx.lineWidth = 5;
                ctx.stroke();
                ctx.shadowBlur = 20;
                ctx.fillStyle = col.glow;
                let endX = px + pw - 6 + tailLen * 0.8 + Math.sin(frame * 0.06 + tailLen * 0.15) * 4;
                let endY = py + ph/2 - 4 + tailLen * 0.3 + Math.sin(frame * 0.08 + tailLen * 0.12) * 3 + tailWave * 0.3;
                ctx.beginPath();
                ctx.arc(endX, endY, 5, 0, Math.PI * 2);
                ctx.fill();

                // Уши
                ctx.shadowBlur = 15;
                ctx.shadowColor = col.glow;
                ctx.fillStyle = col.body;
                ctx.beginPath();
                ctx.moveTo(px + pw - 16, py + 6);
                ctx.lineTo(px + pw - 22, py - 4);
                ctx.lineTo(px + pw - 10, py + 4);
                ctx.fill();
                ctx.beginPath();
                ctx.moveTo(px + pw - 8, py + 4);
                ctx.lineTo(px + pw - 4, py - 6);
                ctx.lineTo(px + pw, py + 4);
                ctx.fill();
                ctx.fillStyle = col.glow;
                ctx.shadowBlur = 20;
                ctx.beginPath();
                ctx.moveTo(px + pw - 16, py + 5);
                ctx.lineTo(px + pw - 20, py - 2);
                ctx.lineTo(px + pw - 12, py + 3);
                ctx.fill();
                ctx.beginPath();
                ctx.moveTo(px + pw - 8, py + 3);
                ctx.lineTo(px + pw - 5, py - 4);
                ctx.lineTo(px + pw - 2, py + 3);
                ctx.fill();

                // Голова
                ctx.shadowBlur = 20;
                ctx.shadowColor = col.glow + '44';
                ctx.fillStyle = col.body;
                ctx.beginPath();
                ctx.ellipse(px + pw-10, py + 12, 15, 14, 0, 0, Math.PI*2);
                ctx.fill();

                const faceGrad = ctx.createRadialGradient(px + pw-6, py + 14, 2, px + pw-6, py + 14, 12);
                faceGrad.addColorStop(0, col.body + 'cc');
                faceGrad.addColorStop(0.5, col.body + '88');
                faceGrad.addColorStop(1, col.body);
                ctx.fillStyle = faceGrad;
                ctx.shadowBlur = 5;
                ctx.beginPath();
                ctx.ellipse(px + pw-6, py + 14, 11, 10, 0, 0, Math.PI*2);
                ctx.fill();

                // Глаза
                const blink = (player.eyeBlink > 185 && player.eyeBlink < 195);
                ctx.shadowBlur = 0;

                ctx.shadowColor = col.glow;
                ctx.shadowBlur = 30;
                ctx.fillStyle = col.glow;
                ctx.beginPath();
                ctx.ellipse(px + pw-4, py + 9, 7, blink ? 2 : 8, 0, 0, Math.PI*2);
                ctx.fill();
                ctx.shadowBlur = 15;
                ctx.fillStyle = col.glow + 'cc';
                ctx.beginPath();
                ctx.ellipse(px + pw-2, py + 9, 4, blink ? 1 : 5, 0, 0, Math.PI*2);
                ctx.fill();
                ctx.shadowBlur = 0;
                ctx.fillStyle = col.body;
                ctx.beginPath();
                ctx.arc(px + pw-1, py + 9, 2.5, 0, Math.PI*2);
                ctx.fill();
                ctx.fillStyle = col.body + '88';
                ctx.fillRect(px + pw-2, py + 7, 2, 4);
                ctx.fillStyle = 'rgba(255,255,255,0.6)';
                ctx.beginPath();
                ctx.arc(px + pw-5, py + 6, 2, 0, Math.PI*2);
                ctx.fill();

                ctx.shadowColor = col.glow;
                ctx.shadowBlur = 25;
                ctx.fillStyle = col.glow;
                ctx.beginPath();
                ctx.ellipse(px + pw-14, py + 9, 5, blink ? 1.5 : 6, 0, 0, Math.PI*2);
                ctx.fill();
                ctx.shadowBlur = 10;
                ctx.fillStyle = col.glow + 'cc';
                ctx.beginPath();
                ctx.ellipse(px + pw-13, py + 9, 3, blink ? 1 : 3.5, 0, 0, Math.PI*2);
                ctx.fill();
                ctx.shadowBlur = 0;
                ctx.fillStyle = col.body;
                ctx.beginPath();
                ctx.arc(px + pw-12, py + 9, 2, 0, Math.PI*2);
                ctx.fill();
                ctx.fillStyle = col.body + '88';
                ctx.fillRect(px + pw-13, py + 7.5, 1.5, 3);
                ctx.fillStyle = 'rgba(255,255,255,0.6)';
                ctx.beginPath();
                ctx.arc(px + pw-15, py + 7, 1.5, 0, Math.PI*2);
                ctx.fill();

                // Усы
                ctx.shadowBlur = 10;
                ctx.shadowColor = col.glow;
                ctx.strokeStyle = col.glow + '44';
                ctx.lineWidth = 1;
                for (let side = -1; side <= 1; side += 2) {
                    for (let w = 0; w < 3; w++) {
                        ctx.beginPath();
                        ctx.moveTo(px + pw-6 + side * 4, py + 16 + w * 3);
                        ctx.lineTo(px + pw-6 + side * 25, py + 12 + w * 6 + Math.sin(frame * 0.03 + w) * 2);
                        ctx.stroke();
                    }
                }

                // Пасть
                ctx.shadowBlur = 0;
                ctx.strokeStyle = col.glow;
                ctx.lineWidth = 1.5;
                ctx.beginPath();
                ctx.arc(px + pw-8, py + 20, 5, 0.1, Math.PI - 0.1);
                ctx.stroke();
                ctx.fillStyle = col.glow + '88';
                ctx.fillRect(px + pw-12, py + 19, 2, 4);
                ctx.fillRect(px + pw-5, py + 19, 2, 4);
                ctx.fillStyle = col.body + '88';
                ctx.beginPath();
                ctx.arc(px + pw-8, py + 22, 3, 0, Math.PI);
                ctx.fill();

                // Нос
                ctx.fillStyle = col.glow;
                ctx.shadowColor = col.glow;
                ctx.shadowBlur = 15;
                ctx.beginPath();
                ctx.ellipse(px + pw-3, py + 14, 3, 2.5, 0, 0, Math.PI*2);
                ctx.fill();

                // Лапы
                ctx.shadowBlur = 15;
                ctx.shadowColor = col.glow;
                const pawGrad = ctx.createRadialGradient(px+4, py+16, 2, px+4, py+16, 10);
                pawGrad.addColorStop(0, col.body + 'cc');
                pawGrad.addColorStop(0.5, col.body + '88');
                pawGrad.addColorStop(1, col.body);
                ctx.fillStyle = pawGrad;
                const handWave = Math.sin(frame * 0.1) * 3;
                ctx.beginPath();
                ctx.ellipse(px + 2 + handWave, py + 16 + (duck? -6:0), 9, 10, -0.2, 0, Math.PI*2);
                ctx.fill();
                ctx.beginPath();
                ctx.ellipse(px + pw-16 - handWave, py + 20 + (duck? -8:0), 9, 10, 0.2, 0, Math.PI*2);
                ctx.fill();
                ctx.fillStyle = col.glow;
                ctx.shadowColor = col.glow;
                ctx.shadowBlur = 15;
                ctx.fillRect(px + 5 + handWave, py + 14 + (duck? -6:0), 1.5, 4);
                ctx.fillRect(px + 8 + handWave, py + 14 + (duck? -6:0), 1.5, 4);
                ctx.fillRect(px + 11 + handWave, py + 14 + (duck? -6:0), 1.5, 4);
                ctx.fillRect(px + pw-14 - handWave, py + 18 + (duck? -8:0), 1.5, 4);
                ctx.fillRect(px + pw-11 - handWave, py + 18 + (duck? -8:0), 1.5, 4);
                ctx.fillRect(px + pw-8 - handWave, py + 18 + (duck? -8:0), 1.5, 4);

                // Задние лапы
                ctx.shadowBlur = 15;
                ctx.shadowColor = col.glow;
                ctx.fillStyle = col.body;
                const legOffset = Math.sin(frame * 0.15) * 3;
                if (!duck) {
                    ctx.fillRect(px-2 + legOffset, py+ph-8, 14, 10);
                    ctx.fillRect(px + pw-12 - legOffset, py+ph-8, 14, 10);
                    ctx.fillStyle = col.body + '88';
                    ctx.fillRect(px + legOffset, py+ph-2, 10, 3);
                    ctx.fillRect(px + pw-10 - legOffset, py+ph-2, 10, 3);
                    ctx.fillStyle = col.glow;
                    ctx.shadowColor = col.glow;
                    ctx.shadowBlur = 15;
                    ctx.fillRect(px+2 + legOffset, py+ph-7, 1.5, 3);
                    ctx.fillRect(px+5 + legOffset, py+ph-7, 1.5, 3);
                    ctx.fillRect(px+8 + legOffset, py+ph-7, 1.5, 3);
                    ctx.fillRect(px + pw-9 - legOffset, py+ph-7, 1.5, 3);
                    ctx.fillRect(px + pw-6 - legOffset, py+ph-7, 1.5, 3);
                    ctx.fillRect(px + pw-3 - legOffset, py+ph-7, 1.5, 3);
                } else {
                    ctx.fillRect(px-2, py+ph-8, 14, 10);
                    ctx.fillRect(px + pw-12, py+ph-8, 14, 10);
                    ctx.fillStyle = col.body + '88';
                    ctx.fillRect(px, py+ph-2, 10, 3);
                    ctx.fillRect(px + pw-10, py+ph-2, 10, 3);
                }
            }

            ctx.shadowBlur = 0;
            ctx.shadowOffsetY = 0;
        }

        // ----- ИГРОВОЙ ЦИКЛ -----
        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        // ----- МОБИЛЬНЫЕ КНОПКИ -----
        function setupTouchButton(elementId, action) {
            const el = document.getElementById(elementId);
            if (!el) return;
            
            el.addEventListener('touchstart', function(e) {
                e.preventDefault();
                if (gameOver || paused) return;
                action(true);
            }, { passive: false });
            
            el.addEventListener('touchend', function(e) {
                e.preventDefault();
                if (elementId === 'btnDown') {
                    action(false);
                }
            }, { passive: false });
            
            el.addEventListener('touchcancel', function(e) {
                if (elementId === 'btnDown') {
                    action(false);
                }
            }, { passive: false });
            
            el.addEventListener('mousedown', function(e) {
                e.preventDefault();
                if (gameOver || paused) return;
                action(true);
            });
            
            el.addEventListener('mouseup', function(e) {
                e.preventDefault();
                if (elementId === 'btnDown') {
                    action(false);
                }
            });
            
            el.addEventListener('mouseleave', function(e) {
                if (elementId === 'btnDown') {
                    action(false);
                }
            });
        }

        function doJump(pressed) {
            if (pressed) {
                player.jumpBuffer = true;
                player.jumpBufferTimer = 0;
            }
        }

        function doDown(pressed) {
            if (pressed) {
                if (player.isGrounded && !player.isRolling) {
                    player.isDucking = true;
                    player.height = player.duckHeight;
                    player.y = player.y + (player.normalHeight - player.duckHeight);
                }
            } else {
                player.isDucking = false;
                if (player.isGrounded && !player.isRolling) {
                    player.height = player.normalHeight;
                    player.y = player.y - (player.normalHeight - player.duckHeight);
                }
            }
        }

        function doDash(pressed) {
            if (pressed) {
                if (!player.isDashing && player.dashCooldown === 0 && !player.isRolling) {
                    player.isDashing = true;
                    player.dashTimer = 12;
                    player.dashCooldown = 30;
                    speed += 1.2;
                    setTimeout(() => { speed -= 1.2; }, 200);
                    combo = Math.min(combo + 3, 10);
                    spawnParticles(player.x + player.width/2, player.y + player.height/2, '#ff00ff', 20);
                }
            }
        }

        function doRoll(pressed) {
            if (pressed) {
                if (player.isGrounded && !player.isRolling && !player.isDucking) {
                    player.isRolling = true;
                    player.rollTimer = 20;
                    player.height = player.duckHeight;
                    player.y = player.y + (player.normalHeight - player.duckHeight);
                    speed += 0.8;
                    setTimeout(() => { speed -= 0.8; }, 150);
                    spawnParticles(player.x + player.width/2, player.y + player.height, '#ff00ff', 15);
                }
            }
        }

        // ----- КЛАВИАТУРА -----
        function handleKeyDown(e) {
            const key = e.key;

            if (key === 'p' || key === 'P' || key === 'Escape') {
                e.preventDefault();
                togglePause();
                return;
            }

            if (key === 'f' || key === 'F') {
                e.preventDefault();
                toggleFullscreen();
                return;
            }

            if (key === 'm' || key === 'M') {
                e.preventDefault();
                music.toggle();
                return;
            }

            if (gameOver || paused) return;

            if (key === ' ' || key === 'Space' || key === 'ArrowUp') {
                e.preventDefault();
                player.jumpBuffer = true;
                player.jumpBufferTimer = 0;
            }

            if (key === 'ArrowDown' || key === 'Down') {
                e.preventDefault();
                if (player.isGrounded && !player.isRolling) {
                    player.isDucking = true;
                    player.height = player.duckHeight;
                    player.y = player.y + (player.normalHeight - player.duckHeight);
                }
            }

            if (key === 'ArrowRight' || key === 'Right') {
                e.preventDefault();
                if (!player.isDashing && player.dashCooldown === 0 && !player.isRolling) {
                    player.isDashing = true;
                    player.dashTimer = 12;
                    player.dashCooldown = 30;
                    speed += 1.2;
                    setTimeout(() => { speed -= 1.2; }, 200);
                    combo = Math.min(combo + 3, 10);
                    spawnParticles(player.x + player.width/2, player.y + player.height/2, '#ff00ff', 20);
                }
            }

            if (key === 'z' || key === 'Z') {
                e.preventDefault();
                if (player.isGrounded && !player.isRolling && !player.isDucking) {
                    player.isRolling = true;
                    player.rollTimer = 20;
                    player.height = player.duckHeight;
                    player.y = player.y + (player.normalHeight - player.duckHeight);
                    speed += 0.8;
                    setTimeout(() => { speed -= 0.8; }, 150);
                    spawnParticles(player.x + player.width/2, player.y + player.height, '#ff00ff', 15);
                }
            }
        }

        function handleKeyUp(e) {
            const key = e.key;

            if (key === 'p' || key === 'P' || key === 'Escape' || key === 'f' || key === 'F' || key === 'm' || key === 'M') return;

            if (key === 'ArrowDown' || key === 'Down') {
                e.preventDefault();
                player.isDucking = false;
                if (player.isGrounded && !player.isRolling) {
                    player.height = player.normalHeight;
                    player.y = player.y - (player.normalHeight - player.duckHeight);
                }
            }
            
            if ([' ', 'Space', 'ArrowUp', 'ArrowRight', 'Right', 'z', 'Z'].includes(key)) {
                e.preventDefault();
            }
        }

        // ----- ИНИЦИАЛИЗАЦИЯ -----
        window.addEventListener('keydown', function(e) {
            if (['Space', 'ArrowUp', 'ArrowDown', 'ArrowRight', 'Right', 'Down', 'z', 'Z', ' ', 'p', 'P', 'Escape', 'f', 'F', 'm', 'M'].includes(e.key)) {
                e.preventDefault();
            }
        }, false);

        window.addEventListener('keydown', handleKeyDown);
        window.addEventListener('keyup', handleKeyUp);

        document.getElementById('restartBtn').addEventListener('click', resetGame);
        document.getElementById('pauseBtn').addEventListener('click', togglePause);
        document.getElementById('fullscreenBtn').addEventListener('click', toggleFullscreen);

        setupTouchButton('btnUp', doJump);
        setupTouchButton('btnDown', doDown);
        setupTouchButton('btnDash', doDash);
        setupTouchButton('btnRoll', doRoll);

        resetGame();
        gameLoop();

        window.addEventListener('blur', function() {
            player.isDucking = false;
            if (player.isGrounded) {
                player.height = player.normalHeight;
                player.y = player.y - (player.normalHeight - player.duckHeight);
            }
            player.jumpBuffer = false;
        });

        console.log('🐾 Кибер-пантера — БОЛЬШИЕ КНОПКИ В МАГАЗИНЕ!');
        console.log('🛒 Кнопки "Купить" и "Выбрать" стали крупнее и удобнее');
        console.log('📱 Теперь легко покупать персонажей даже на маленьком экране');
        console.log('🎮 Управление: ␣/▲ прыжок, ▼ присесть, → рывок, Z кувырок');
        console.log('🎵 Музыка: кнопка 🎵 или клавиша M');
    })();
</script>
</body>
</html>
