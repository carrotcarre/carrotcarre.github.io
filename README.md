<!DOCTYPE html>
<html lang="nl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Meester Tom legt uit | Mindmap Maker 🚀✨</title>
    <link href="https://fonts.googleapis.com/css2?family=Bubblegum+Sans&family=Fredoka:wght@400;700&family=Patrick+Hand&family=Itim&family=Sniglet&family=Mali&family=Concert+One&family=Quicksand:wght@500&family=Baloo+2:wght@500&family=Boogaloo&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        :root {
            --bg-color: #f0f4f8;
            --center-color: #ffcc00;
            --branch-1: #e91e63;
            --branch-2: #03a9f4;
            --branch-3: #ff9800;
            --branch-4: #8bc34a;
            --glass-blue: rgba(3, 169, 244, 0.25);
            --glass-border: rgba(255, 255, 255, 0.4);
            --delete-color: #ff4444;
            --handle-color: #03a9f4;
            --menu-shadow: 0 12px 40px rgba(0, 40, 80, 0.15);
            --text-color: #333;
            --node-bg: #ffffff;
            --dark-menu-bg: rgba(0, 32, 63, 0.9);
            --glass-bright-blue: rgba(3, 169, 244, 0.7); 
            --ui-text: #01579b;
        }

        /* NACHTMODUS OVERRIDES */
        body.dark-mode {
            --bg-color: #1a1a2e;
            --text-color: #f0f4f8;
            --node-bg: #2d2d44;
            --glass-blue: rgba(0, 0, 0, 0.6);
            --glass-border: rgba(255, 255, 255, 0.15);
            --menu-shadow: 0 12px 40px rgba(0, 0, 0, 0.5);
            --ui-text: #81d4fa;
        }

        body {
            font-family: 'Fredoka', 'Comic Sans MS', cursive;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            display: flex;
            flex-direction: column;
            height: 100vh;
            overflow: hidden;
            touch-action: none;
            -webkit-user-select: none;
            user-select: none;
            transition: background-color 0.5s ease, color 0.5s ease;
        }

        /* OPENER STYLING */
        #opener {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100vh;
            background: #3DBFFF; display: flex; align-items: center;
            justify-content: center; z-index: 20000; 
            transition: clip-path 1.2s cubic-bezier(0.65, 0, 0.35, 1), opacity 0.8s ease;
            clip-path: circle(150% at 50% 50%); /* Start volledig zichtbaar */
        }
        .circle-pulse {
            width: 50px; height: 50px; background: #FFCC00;
            border-radius: 50%; animation: pulse 2s infinite;
        }
        #opener img { 
            position: absolute; 
            max-width: 300px; 
            height: auto;
            z-index: 20001;
        }
        @keyframes pulse {
            0% { transform: scale(1); opacity: 1; }
            100% { transform: scale(50); opacity: 0; }
        }
        .fade-out { 
            clip-path: circle(0% at 50% 50%) !important; /* Iris reveal reveal */
            opacity: 0; 
            pointer-events: none; 
        }

        /* THEMA KEUZE MODAL */
        #startup-modal, #info-modal, #help-modal {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.7); backdrop-filter: blur(10px);
            z-index: 9999; display: flex; justify-content: center; align-items: center;
        }
        
        #info-modal, #help-modal { z-index: 10001; display: none; }

        .startup-content, .info-content, .help-content {
            background: var(--node-bg); color: var(--text-color); padding: 30px; border-radius: 40px; text-align: center;
            max-width: 650px; width: 90%; box-shadow: 0 20px 60px rgba(0,0,0,0.5);
            max-height: 85vh; overflow-y: auto; transition: background 0.3s;
        }
        
        .info-content, .help-content { border: 4px solid var(--branch-2); }
        .help-content { text-align: left; max-width: 800px; }

        .help-section { margin-bottom: 25px; background: rgba(0,0,0,0.05); padding: 15px; border-radius: 20px; border-left: 5px solid var(--branch-2); }
        .help-section h3 { margin-top: 0; color: var(--branch-2); display: flex; align-items: center; gap: 10px; }
        .help-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; }
        .help-card { background: var(--node-bg); padding: 12px; border-radius: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.05); font-size: 14px; }

        .theme-grid {
            display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 25px 0;
        }
        .theme-card {
            padding: 15px; border: 3px solid #eee; border-radius: 20px; cursor: pointer;
            transition: all 0.3s; font-weight: bold; background: var(--node-bg);
        }
        .theme-card:hover { transform: scale(1.05); border-color: var(--branch-2); }
        .theme-card.active { border-color: var(--branch-2); background: rgba(3, 169, 244, 0.1); }
        
        .template-choice {
            margin-top: 20px; padding-top: 20px; border-top: 2px solid #eee;
            display: flex; flex-wrap: wrap; justify-content: center; gap: 15px;
        }

        @keyframes node-pop {
            0% { transform: scale(0.92) translateY(10px); opacity: 0; filter: blur(4px); }
            100% { transform: scale(1) translateY(0); opacity: 1; filter: blur(0); }
        }
        @keyframes branch-grow {
            from { opacity: 0; stroke-dashoffset: 100; }
            to { opacity: 1; stroke-dashoffset: 0; }
        }
        .animate-pop { animation: node-pop 0.5s cubic-bezier(0.2, 0.8, 0.2, 1) forwards; }

        .brand-container {
            position: absolute;
            right: 20px;
            bottom: 5px;
            display: flex;
            align-items: center;
            gap: 10px;
            z-index: 1100;
            pointer-events: auto;
            transform: scale(0.85);
            transform-origin: bottom right;
        }

        .logo-circle {
            width: 85px; height: 85px; 
            background: transparent; 
            border: none; 
            display: flex; align-items: center; justify-content: center;
            cursor: pointer; transition: transform 0.3s;
            box-shadow: none; 
        }
        .logo-circle:hover { transform: scale(1.1); }
        .logo-circle img { width: 140%; height: auto; filter: drop-shadow(0 2px 10px rgba(0,0,0,0.15)); }

        .btn-help-trigger {
            width: 50px; height: 50px; background: var(--center-color); border: 3px solid white; border-radius: 50%;
            display: flex; align-items: center; justify-content: center; font-size: 28px; color: white; cursor: pointer;
            box-shadow: var(--menu-shadow); transition: all 0.3s; font-weight: bold; text-shadow: 0 2px 4px rgba(0,0,0,0.2);
        }
        .btn-help-trigger:hover { transform: scale(1.1) rotate(10deg); background: #ff9800; }

        .toolbar {
            position: fixed; top: 10px; left: 50%; 
            transform: translateX(-50%) scale(0.8);
            transform-origin: top center;
            background: var(--glass-blue); 
            backdrop-filter: blur(15px) saturate(180%); -webkit-backdrop-filter: blur(15px) saturate(180%);
            padding: 10px 24px; display: flex; flex-direction: row; flex-wrap: nowrap; justify-content: center; align-items: center;
            gap: 12px; box-shadow: var(--menu-shadow); z-index: 1000; border-radius: 100px; 
            border: 1px solid var(--glass-border); width: auto; max-width: 120vw;
        }

        .branch-tools-wrapper {
            background: rgba(255, 255, 255, 0.2);
            padding: 6px 14px;
            border-radius: 50px;
            display: flex;
            align-items: center;
            gap: 8px;
            box-shadow: inset 0 2px 6px rgba(0,0,0,0.05);
            border: 1px solid rgba(255,255,255,0.1);
        }

        .title-container {
            display: flex; align-items: center; padding-right: 12px; border-right: 2px solid rgba(255,255,255,0.3); margin-right: 5px; gap: 8px;
            position: relative;
        }
        #map-title-input {
            background: rgba(255,255,255,0.8); border: 2px solid var(--branch-2); border-radius: 15px;
            padding: 6px 12px; font-family: 'Fredoka', sans-serif; font-weight: bold; color: #333;
            outline: none; width: 140px; transition: all 0.3s; font-size: 14px;
        }
        #map-title-input:focus { width: 200px; background: white; box-shadow: 0 0 10px rgba(3, 169, 244, 0.2); }

        .btn-quick-save { background-color: var(--branch-2); color: white; padding: 6px 12px; border-radius: 15px; font-size: 13px; }

        .save-warning-bubble {
            position: absolute; bottom: -55px; right: 0; background: #ffcc00; color: #333; padding: 8px 15px;
            border-radius: 15px; font-size: 12px; font-weight: bold; white-space: nowrap; box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            display: none; z-index: 2000; animation: node-pop 0.3s ease-out;
        }
        .save-warning-bubble::after {
            content: ''; position: absolute; top: -10px; right: 25px; border-left: 10px solid transparent; border-right: 10px solid transparent; border-bottom: 10px solid #ffcc00;
        }

        .button-group { display: flex; gap: 6px; align-items: center; }

        .formatting-group { 
            position: fixed; right: 15px; top: 50%; transform: translateY(-50%) scale(0.8); transform-origin: right center;
            display: none; flex-direction: column; gap: 12px; padding: 20px 15px; background: var(--glass-bright-blue); 
            backdrop-filter: blur(20px) saturate(180%); -webkit-backdrop-filter: blur(20px) saturate(180%);
            border: 1px solid rgba(255,255,255,0.4); box-shadow: 0 12px 40px rgba(0, 40, 80, 0.2); border-radius: 40px; 
            align-items: center; z-index: 1000; width: auto;
        }

        .formatting-group select { 
            width: 120px; font-size: 13px; padding: 8px 10px; border-radius: 15px; border: none; 
            background: rgba(255,255,255,0.25); color: white; outline: none; font-weight: bold;
        }
        .formatting-group select option { background: #03a9f4; color: white; }
        
        .font-size-wrapper { display: flex; flex-direction: column; align-items: center; gap: 6px; background: rgba(255,255,255,0.15); padding: 10px 8px; border-radius: 20px; }
        #font-size-picker { width: 45px; text-align: center; border: none; border-radius: 10px; background: white; font-weight: bold; font-size: 14px; padding: 4px; }
        
        .picker-container { display: flex; flex-direction: column; align-items: center; gap: 2px; width: 100%; }
        .picker-label { font-size: 9px; font-weight: bold; text-transform: uppercase; color: #ffffff; pointer-events: none; text-align: center; text-shadow: 0 1px 2px rgba(0,0,0,0.2); }

        .layer-buttons { display: flex; gap: 5px; width: 100%; justify-content: center; }
        .btn-layer { background: rgba(255,255,255,0.3); color: white; font-size: 10px; padding: 6px; border-radius: 10px; flex: 1; text-align: center; }

        .sticker-controls { display: none; flex-direction: column; gap: 8px; align-items: center; border-top: 1px solid rgba(255,255,255,0.3); padding-top: 10px; margin-top: 5px; }

        .settings-dropdown { position: relative; display: inline-block; }
        .btn-settings { background: #607d8b; color: white; width: 38px; height: 38px; border-radius: 50% !important; justify-content: center; padding: 0 !important; }
        
        .dropdown-content { 
            display: none; position: absolute; top: 48px; right: 0; background: var(--node-bg); 
            backdrop-filter: blur(25px); width: max-content; min-width: 200px; box-shadow: 0 15px 45px rgba(0, 30, 60, 0.3); 
            border-radius: 20px; overflow: hidden; flex-direction: column; z-index: 1001; border: 1px solid var(--glass-border);
        }
        .dropdown-content.active { display: flex; }
        .dropdown-content button { 
            border-radius: 0; width: 100%; background: transparent; color: var(--ui-text); padding: 14px 22px; 
            border-bottom: 1px solid rgba(255,255,255,0.1); text-align: left; font-size: 14px; white-space: nowrap; 
        }
        .dropdown-content button:hover { background: rgba(255, 255, 255, 0.1); transform: none; }

        .emoji-bar {
            position: fixed; left: 15px; top: 50%; transform: translateY(-50%) scale(0.8); transform-origin: left center;
            display: flex; flex-direction: column; gap: 10px; padding: 20px 10px; background: var(--glass-blue); 
            backdrop-filter: blur(20px) saturate(180%); border-radius: 40px; border: 1px solid var(--glass-border); 
            box-shadow: var(--menu-shadow); z-index: 99; align-items: center; max-height: 85vh; overflow-y: auto; scrollbar-width: none;
        }

        button, select, input[type="range"], input[type="number"] {
            padding: 8px 14px; border: none; border-radius: 22px; cursor: pointer; font-weight: bold; transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            font-size: 14px; display: flex; align-items: center; gap: 5px; touch-action: manipulation;
        }
        button:hover { transform: scale(1.15) rotate(-3deg); filter: brightness(1.1); }
        button:nth-child(even):hover { transform: scale(1.15) rotate(3deg); }
        button:active { transform: scale(0.9); }

        input[type="color"] { -webkit-appearance: none; appearance: none; width: 34px; height: 34px; background-color: transparent; border: none; cursor: pointer; padding: 0; border-radius: 50%; }
        input[type="color"]::-webkit-color-swatch-wrapper { padding: 0; }
        input[type="color"]::-webkit-color-swatch { border: 3px solid white; border-radius: 50%; box-shadow: 0 2px 8px rgba(0,0,0,0.2); }

        .btn-pink { background-color: var(--branch-1); color: white; }
        .btn-blue { background-color: var(--branch-2); color: white; }
        .btn-orange { background-color: var(--branch-3); color: white; }
        .btn-green { background-color: var(--branch-4); color: white; }
        .btn-undo { background-color: #f44336; color: white; }
        .btn-redo { background-color: #4caf50; color: white; }
        .btn-add-branch { background-color: rgba(255,255,255,0.4); border: 2px dashed #03a9f4 !important; color: #03a9f4; width: 38px; height: 38px; border-radius: 50% !important; justify-content: center; }
        .btn-emoji { background: rgba(255,255,255,0.2); border: 1px solid rgba(255,255,255,0.2); padding: 5px; font-size: 22px; border-radius: 50%; width: 44px; height: 44px; justify-content: center; flex-shrink: 0; color: var(--text-color); }
        .btn-img { background: #9c27b0; color: white; }
        .btn-library { background: #ffeb3b; color: #333; border: 2px solid #fbc02d; }
        .btn-paste { background: #4caf50; color: white; }
        .btn-step { background: rgba(255,255,255,0.3); color: white; width: 28px; height: 28px; justify-content: center; padding: 0; border-radius: 50%; border: 1px solid rgba(255,255,255,0.4); font-size: 14px; }

        #map-area { flex-grow: 1; position: relative; overflow: hidden; background: transparent; cursor: grab; touch-action: none; }
        #zoom-layer { position: absolute; top: 0; left: 0; width: 100%; height: 100%; transform-origin: 0 0; will-change: transform; }
        #grid-overlay { position: absolute; top: -50000px; left: -50000px; width: 100000px; height: 100000px; background: radial-gradient(circle, #d1d9e6 1px, transparent 1px); background-size: 40px 40px; pointer-events: none; z-index: 0; opacity: 0.3; }
        #svg-canvas { position: absolute; top: -50000px; left: -50000px; width: 100000px; height: 100000px; pointer-events: none; z-index: 1; }
        
        .branch-path { stroke: none; opacity: 0.8; transition: fill 0.3s ease; animation: branch-grow 0.6s ease-out; }

        .node {
            position: absolute; padding: 10px 22px; border-radius: 25px; background: var(--node-bg); color: var(--text-color); box-shadow: 3px 3px 15px rgba(0,0,0,0.1);
            min-width: 80px; width: auto; max-width: 450px; text-align: center; cursor: move; border: 3px solid transparent; 
            font-size: 16px; z-index: 10; outline: none; word-wrap: break-word; display: flex; align-items: center; justify-content: center; 
            touch-action: none; transition: background-color 0.3s, border-color 0.3s, transform 0.1s, clip-path 0.3s, border-radius 0.3s, color 0.3s;
            overflow: visible; -webkit-user-drag: none; user-drag: none;
        }

        .shape-round { border-radius: 25px; }
        .shape-rect { border-radius: 4px; }
        .shape-cloud { border-radius: 50% 50% 50% 50% / 60% 60% 40% 40%; }
        .shape-wavy { clip-path: polygon(100% 0%, 95% 50%, 100% 100%, 0% 100%, 5% 50%, 0% 0%); border-radius: 0; }
        .shape-angular { clip-path: polygon(20% 0%, 80% 0%, 100% 20%, 100% 80%, 80% 100%, 20% 100%, 0% 80%, 0% 20%); border-radius: 0; }

        .sticker { background: transparent !important; box-shadow: none !important; border: 3px solid transparent !important; font-size: 45px; z-index: 11; user-select: none; width: auto !important; min-width: auto !important; overflow: visible !important; }
        
        .image-node { background: transparent !important; padding: 0 !important; border: 3px solid transparent; user-select: none; width: 200px; overflow: visible !important; }
        .image-node img { width: 100%; height: 100%; pointer-events: none; display: block; object-fit: cover; border-radius: inherit; }
        
        .node-handle { position: absolute; width: 24px; height: 24px; background: var(--handle-color); border: 2px solid white; border-radius: 50%; display: none; z-index: 100; cursor: nwse-resize; }
        .handle-rotate { top: -45px; left: 50%; transform: translateX(-50%); background: #ff9800; cursor: pointer; }
        .handle-resizer { bottom: -12px; right: -12px; }
        .selected .node-handle { display: block; }
        .selected { border-style: dashed !important; border-color: var(--handle-color) !important; box-shadow: 0 0 20px rgba(3, 169, 244, 0.3) !important; }

        .node-main { background-color: var(--center-color); font-size: 24px; font-weight: bold; border: 4px solid #fbc02d; left: 0px; top: 0px; width: auto; z-index: 5; color: #333 !important; }

        #floating-delete-btn {
            position: absolute; width: 44px; height: 44px; background: var(--delete-color); color: white; border-radius: 50%; display: none; 
            justify-content: center; align-items: center; cursor: pointer; z-index: 2000; font-size: 24px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3); border: 2px solid white; transition: transform 0.2s;
        }

        #library-modal { 
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; 
            background: rgba(0, 20, 40, 0.4); backdrop-filter: blur(8px); 
            z-index: 5000; display: none; justify-content: center; align-items: center; 
        }
        .library-content { 
            background: rgba(3, 169, 244, 0.45); 
            backdrop-filter: blur(25px) saturate(180%); 
            -webkit-backdrop-filter: blur(25px) saturate(180%);
            width: 90%; max-width: 600px; max-height: 85vh; border-radius: 40px; 
            padding: 30px; box-shadow: 0 30px 90px rgba(0, 40, 80, 0.4); 
            display: flex; flex-direction: column; gap: 15px; 
            border: 1px solid rgba(255, 255, 255, 0.4);
            color: #ffffff;
        }
        .library-header { 
            display: flex; justify-content: space-between; align-items: center; 
            border-bottom: 2px solid rgba(255, 255, 255, 0.3); padding-bottom: 15px; 
        }
        .library-header h2 { text-shadow: 0 2px 4px rgba(0,0,0,0.2); }
        
        #sticker-search-container { margin: 5px 0; position: relative; }
        #sticker-search-input {
            width: 100%; padding: 12px 20px; border-radius: 20px; border: none;
            background: rgba(255, 255, 255, 0.2); color: white; font-family: 'Fredoka', sans-serif;
            font-size: 16px; outline: none; box-sizing: border-box;
            border: 1px solid rgba(255,255,255,0.3); transition: all 0.3s;
        }
        #sticker-search-input::placeholder { color: rgba(255,255,255,0.7); }
        #sticker-search-input:focus { background: rgba(255, 255, 255, 0.3); box-shadow: 0 0 15px rgba(255,255,255,0.2); }

        .library-grid { 
            display: grid; grid-template-columns: repeat(auto-fill, minmax(65px, 1fr)); 
            gap: 12px; overflow-y: auto; padding: 10px; 
            scrollbar-width: thin; scrollbar-color: rgba(255,255,255,0.3) transparent;
        }
        .library-item { 
            font-size: 35px; background: rgba(255, 255, 255, 0.25); 
            border: 1px solid rgba(255, 255, 255, 0.2); border-radius: 20px; 
            cursor: pointer; transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275); 
            display: flex; justify-content: center; align-items: center; height: 75px; 
        }
        .library-item:hover { 
            background: rgba(255, 255, 255, 0.5); 
            transform: scale(1.15) rotate(5deg); 
            box-shadow: 0 8px 20px rgba(0,0,0,0.1);
        }

        @media print {
            @page { size: A4 landscape; margin: 0; }
            html, body { width: 100% !important; height: 100% !important; overflow: hidden !important; background: white !important; -webkit-print-color-adjust: exact; print-color-adjust: exact; }
            .toolbar, .emoji-bar, .brand-container, footer, #floating-delete-btn, .node-handle, #library-modal, .formatting-group, #startup-modal, #info-modal, #help-modal, #grid-overlay, #opener { display: none !important; }
            #map-area { position: fixed !important; top: 0 !important; left: 0 !important; width: 100vw !important; height: 100vh !important; margin: 0 !important; padding: 0 !important; overflow: hidden !important; background: transparent !important; }
            #zoom-layer { position: absolute !important; transform-origin: 0 0 !important; }
            .node { box-shadow: none !important; border-style: solid !important; }
            .selected { border-color: transparent !important; }
            svg { filter: none !important; }
        }

        footer { 
            position: relative; 
            background: var(--glass-blue); 
            backdrop-filter: blur(10px); 
            text-align: center; 
            padding: 12px 5px; 
            font-size: 11px; 
            color: var(--ui-text); 
            font-weight: bold; 
            border-top: 1px solid var(--glass-border); 
            z-index: 1000; 
            min-height: 20px;
        }

        @media (max-width: 850px) {
            .toolbar { transform: translateX(-50%) scale(0.7); top: 5px; width: 98vw; }
            .formatting-group { right: 5px; transform: translateY(-50%) scale(0.7); }
            .title-container { display: none; } 
            .brand-container { transform: scale(0.7); right: 5px; }
        }
    </style>
</head>
<body oncontextmenu="return false;"> 

<div id="opener">
  <div class="circle-pulse"></div>
  <img src="https://static.wixstatic.com/media/72aa9d_a2a22aa211fb4be0a9439f057a5f0c99~mv2.png/v1/fill/w_160,h_108,al_c,q_85,usm_0.66_1.00_0.01,enc_avif,quality_auto/Meester%20Tom%20legt%20uit%207.png" alt="Meester Tom legt uit">
</div>

<div id="startup-modal">
    <div class="startup-content">
        <h2 style="font-family: 'Fredoka', sans-serif; color: #03a9f4;">🚀 Start je Mindmap!</h2>
        <p>Kies een thema om te beginnen:</p>
        <div class="theme-grid">
            <div class="theme-card active" onclick="setTheme('default', this)">🌟 Standaard<br><small>Vrolijk & Helder</small></div>
            <div class="theme-card" onclick="setTheme('nature', this)">🌿 Natuur<br><small>Groen & Rustig</small></div>
            <div class="theme-card" onclick="setTheme('space', this)">🌌 Ruimte<br><small>Donker & Stoer</small></div>
            <div class="theme-card" onclick="setTheme('ocean', this)">🌊 Oceaan<br><small>Blauw & Fris</small></div>
        </div>
        <div class="template-choice">
            <button class="btn-blue" onclick="startApp(false)">✨ Leeg Blad</button>
            <button class="btn-orange" onclick="startApp(true)">📋 Met Sjabloon</button>
            <button class="btn-green" onclick="openFile()">📂 Open Mindmap</button>
        </div>
    </div>
</div>

<div id="info-modal">
    <div class="info-content">
        <h2 style="font-family: 'Fredoka', sans-serif; color: #03a9f4;">ℹ️ Over MindMaker</h2>
        <p style="font-weight: bold; margin-bottom: 5px;">Mindmapmaker van Meester Tom legt uit.</p>
        <p style="font-size: 14px; color: #555;">Versie januari 2026 – Alle rechten voorbehouden | Ontwikkelaar: Tom Vanden Berghen.</p>
        <p style="font-size: 14px; background: rgba(3, 169, 244, 0.1); padding: 10px; border-radius: 15px;">Betaversie enkel te gebruiken in <br> het Onze-Lieve-Vrouwinstituut (lagere school) in Sint-Genesius-Rode.</br></p>
        <div style="margin: 20px 0; display: flex; flex-direction: column; gap: 10px; align-items: center;">
            <a href="mailto:meestertomlegtuit@gmail.com" class="btn-orange" style="text-decoration: none; width: 100%; justify-content: center; box-sizing: border-box;">📧 Klik hier om feedback te geven</a>
        </div>
        <button class="btn-blue" onclick="closeInfoModal()" style="margin: 0 auto;">Sluiten</button>
    </div>
</div>

<div id="help-modal">
    <div class="help-content">
        <img src="https://static.wixstatic.com/media/72aa9d_a2a22aa211fb4be0a9439f057a5f0c99~mv2.png/v1/fill/w_160,h_108,al_c,q_85,usm_0.66_1.00_0.01,enc_avif,quality_auto/Meester%20Tom%20legt%20uit%207.png" alt="Meester Tom" style="display: block; margin: 0 auto 10px auto; width: 120px;">
        
        <h2 style="font-family: 'Fredoka', sans-serif; color: #03a9f4; text-align: center; margin-bottom: 20px;">📖 MindMaker Helpcentrum</h2>
        
        <div class="help-section">
            <h3>🛠 De Basis: Je Mindmap Starten</h3>
            <div class="help-grid">
                <div class="help-card"><strong>Thema's:</strong> Kies uit Standaard, Natuur, Ruimte of Oceaan om de sfeer en tak-kleuren te bepalen.</div>
                <div class="help-card"><strong>Sjabloon:</strong> Gebruik 'Met Sjabloon' voor hulpvragen als Wie?, Wat?, Waar? en Waarom?.</div>
            </div>
        </div>

        <div class="help-section">
            <h3>🖋 Tekst en Takken</h3>
            <div class="help-grid">
                <div class="help-card"><strong>Hoofdonderwerp:</strong> Klik in het midden om je onderwerp te typen.</div>
                <div class="help-card"><strong>Takken+:</strong> Selecteer een vakje en klik op 🌸, 💎, 🎃 of 🍀 om een tak toe te voegen.</div>
                <div class="help-card"><strong>Verplaatsen:</strong> Sleep vakjes met je muis of vinger naar de gewenste plek.</div>
            </div>
        </div>

        <div class="help-section">
            <h3>✨ Opmaak & Stickers</h3>
            <div class="help-grid">
                <div class="help-card"><strong>Rechtermenu:</strong> Verander vorm, lettertype (Snoep, Hand, Retro), tekstgrootte en kleuren.</div>
                <div class="help-card"><strong>Stickers (📚):</strong> Kies emoji's uit de bibliotheek aan de linkerkant.</div>
                <div class="help-card"><strong>Eigen Foto's (🖼️):</strong> Upload foto's of gebruik de 'Plakken' (📋) knop voor klembord-afbeeldingen.</div>
            </div>
        </div>

        <div class="help-section">
            <h3>💾 Bestanden op Chromebook</h3>
            <div class="help-grid">
                <div class="help-card"><strong>Opslaan:</strong> Klik op 💾 in de bovenbalk. Kies 'Google Drive' en geef je bestand een duiderlijke naam (.json).</div>
                <div class="help-card"><strong>Verderwerken:</strong> Gebruik 'Open Mindmap' (📂) om je .json bestand weer in te laden.</div>
                <div class="help-card"><strong>Inleveren:</strong> Kies 'Opslaan als foto' onder het tandwiel (⚙️) voor een .png bestand.</div>
            </div>
        </div>

        <div style="text-align: center; margin-top: 20px;">
            <p style="font-size: 13px; color: #666;">💡 <em>Tip van Meester Tom: Sla je werk elke 10 minuten op! </em></p>
            <button class="btn-blue" onclick="closeHelp()" style="margin: 0 auto; width: 150px; justify-content: center;">Begrepen!</button>
        </div>
    </div>
</div>

<div class="toolbar">
    <div class="title-container">
        <input type="text" id="map-title-input" placeholder="Mindmap Naam..." value="Mijn Mindmap">
        <button class="btn-quick-save" onclick="saveMapData(false)">💾 Opslaan</button>
        <div id="save-reminder-bubble" class="save-warning-bubble">Vergeet niet op te slaan!</div>
    </div>

    <div class="branch-tools-wrapper">
        <div class="button-group" id="branch-buttons">
            <button class="btn-pink" onclick="addBranch(varColor('--branch-1'))">🌸 +</button>
            <button class="btn-blue" onclick="addBranch(varColor('--branch-2'))">💎 +</button>
            <button class="btn-orange" onclick="addBranch(varColor('--branch-3'))">🎃 +</button>
            <button class="btn-green" onclick="addBranch(varColor('--branch-4'))">🍀 +</button>
        </div>
        <button class="btn-add-branch" onclick="document.getElementById('new-branch-picker').click()">➕</button>
    </div>
    
    <input type="color" id="new-branch-picker" style="display:none" onchange="addNewBranchButton(this.value)">

    <div class="button-group">
        <div class="settings-dropdown">
            <button class="btn-settings" onclick="toggleSettingsMenu(event)">⚙️</button>
            <div class="dropdown-content" id="settings-menu">
                <button onclick="toggleNightMode()">🌙 Nachtmodus aan/uit</button>
                <hr style="width: 100%; border: 0; border-top: 1px solid rgba(255,255,255,0.1);">
                <button onclick="saveMapData(false)">💾 Mindmap opslaan</button>
                <button onclick="saveMapData(true)">📂 Mindmap opslaan als...</button>
                <button onclick="openFile()">📂 Mindmap openen</button>
                <button onclick="shareMap()">📤 Mindmap delen...</button>
                <button onclick="printMap()">🖨️ Mindmap printen</button>
                <button onclick="downloadMap()">🖼️ Mindmap opslaan als foto</button>
                <button onclick="centerView()">🎯 Centreer de mindmap</button>
                <button onclick="resetMap()" style="color: var(--delete-color); font-weight: bold;">🔥 Alles wissen</button>
            </div>
        </div>
    </div>

    <div class="button-group">
        <button class="btn-paste" onclick="pasteFromClipboard()">📋 Plakken</button>
    </div>

    <div class="button-group">
        <button class="btn-undo" onclick="undo()" style="flex-direction: column; padding: 4px 10px; min-height: 48px; gap: 2px;">
            <span>↩️</span>
            <span style="font-size: 9px; font-weight: normal; text-transform: lowercase;">terug</span>
        </button>
        <button class="btn-redo" onclick="redo()" style="flex-direction: column; padding: 4px 10px; min-height: 48px; gap: 2px;">
            <span>↪️</span>
            <span style="font-size: 9px; font-weight: normal; text-transform: lowercase;">opnieuw</span>
        </button>
    </div>
    <input type="file" id="file-input" style="display:none" accept=".json" onchange="loadMapData(event)">
</div>

<div class="formatting-group" id="text-formatting-menu">
    <div class="picker-container" id="shape-picker-container">
        <select id="shape-select" onchange="changeNodeShape(this.value)">
            <option value="shape-round">⭕ Rond</option>
            <option value="shape-rect">🟦 Blok</option>
            <option value="shape-cloud">☁️ Wolk</option>
            <option value="shape-wavy">🌊 Golf</option>
            <option value="shape-angular">💎 Diamant</option>
        </select>
        <span class="picker-label">Vorm</span>
    </div>

    <div class="picker-container" id="layer-picker-container">
        <div class="layer-buttons">
            <button class="btn-layer" onclick="changeLayer('front')">⬆️ Voor</button>
            <button class="btn-layer" onclick="changeLayer('back')">⬇️ Achter</button>
        </div>
        <span class="picker-label">Laag</span>
    </div>

    <div class="picker-container" id="font-picker-container">
        <select id="font-select" onchange="changeFont(this.value)">
            <option value="'Fredoka', sans-serif" style="font-family: 'Fredoka', sans-serif;">Speels</option>
            <option value="'Bubblegum Sans', cursive" style="font-family: 'Bubblegum Sans', cursive;">Snoep</option>
            <option value="'Comic Sans MS', cursive" style="font-family: 'Comic Sans MS', cursive;">Comic</option>
            <option value="'Patrick Hand', cursive" style="font-family: 'Patrick Hand', cursive;">Hand</option>
            <option value="'Sniglet', cursive" style="font-family: 'Sniglet', cursive;">Zacht</option>
            <option value="'Itim', cursive" style="font-family: 'Itim', cursive;">Schets</option>
            <option value="'Mali', cursive" style="font-family: 'Mali', cursive;">Cute</option>
            <option value="'Concert One', cursive" style="font-family: 'Concert One', cursive;">Stoer</option>
            <option value="'Baloo 2', cursive" style="font-family: 'Baloo 2', cursive;">Bubbel</option>
            <option value="'Boogaloo', cursive" style="font-family: 'Boogaloo', cursive;">Retro</option>
        </select>
        <span class="picker-label">Lettertype</span>
    </div>
    
    <div class="font-size-wrapper" id="font-size-wrapper">
        <button class="btn-step" onclick="stepFontSize(2)">▲</button>
        <input type="number" id="font-size-picker" value="16" min="8" max="150" oninput="changeFontSize(this.value)">
        <button class="btn-step" onclick="stepFontSize(-2)">▼</button>
        <span class="picker-label">Grootte</span>
    </div>
    
    <div class="picker-container" id="text-color-container">
        <input type="color" id="text-color-picker" oninput="changeTextColor(this.value)">
        <span class="picker-label">Tekst</span>
    </div>

    <div class="picker-container" id="bg-color-container">
        <input type="color" id="bg-color-picker" oninput="changeNodeColors(this.value)">
        <span class="picker-label">Vulling</span>
    </div>
    
    <div id="sticker-tools" class="sticker-controls">
        <input type="range" id="size-slider" min="20" max="600" value="50" oninput="changeStickerSize(this.value)" style="width: 80px;">
        <span class="picker-label">Grootte</span>
    </div>
</div>

<div class="emoji-bar" id="emoji-source-bar">
    <button class="btn-emoji btn-library" onclick="toggleLibrary()">📚</button>
    <button class="btn-emoji btn-img" onclick="document.getElementById('image-upload').click()">🖼️</button>
    <input type="file" id="image-upload" style="display:none" accept="image/*" onchange="handleImageUpload(event)">
    <hr style="width: 80%; border: 0; border-top: 2px solid rgba(255,255,255,0.4); margin: 5px 0;">
    <button class="btn-emoji" draggable="true" ondragstart="drag(event)" onclick="spawnSticker('💡')">💡</button>
    <button class="btn-emoji" draggable="true" ondragstart="drag(event)" onclick="spawnSticker('⭐')">⭐</button>
    <button class="btn-emoji" draggable="true" ondragstart="drag(event)" onclick="spawnSticker('🎯')">🎯</button>
    <button class="btn-emoji" draggable="true" ondragstart="drag(event)" onclick="spawnSticker('🔥')">🔥</button>
    <button class="btn-emoji" draggable="true" ondragstart="drag(event)" onclick="spawnSticker('✅')">✅</button>
    <button class="btn-emoji" draggable="true" ondragstart="drag(event)" onclick="spawnSticker('🚀')">🚀</button>
</div>

<div id="library-modal">
    <div class="library-content">
        <div class="library-header">
            <h2 style="margin:0; font-size: 24px;">📚 Stickerbibliotheek</h2>
            <button onclick="toggleLibrary()" style="background:rgba(255,255,255,0.3); border-radius:50%; width:35px; height:35px; justify-content:center; color:white; font-size:20px;">✕</button>
        </div>
        
        <div id="sticker-search-container">
            <input type="text" id="sticker-search-input" placeholder="Zoek een sticker (bijv. hond, zon, eten)..." oninput="filterLibrary(this.value)">
        </div>

        <div class="library-grid" id="library-grid-container"></div>
    </div>
</div>

<div id="map-area" ondrop="drop(event)" ondragover="allowDrop(event)">
    <div id="zoom-layer">
        <div id="grid-overlay"></div>
        <svg id="svg-canvas"></svg>
        <div id="center-node" class="node node-main shape-round" contenteditable="true" oninput="handleNodeInput(this)">
            Mijn Onderwerp
        </div>
    </div>
    <div id="floating-delete-btn">🗑️</div>
</div>

<footer>
    Meester Tom legt uit © 2026 (alle rechten voorbehouden) Betaversie
    <div class="brand-container">
        <div class="logo-circle" onclick="showAppInfo()">
            <img src="https://static.wixstatic.com/media/72aa9d_a2a22aa211fb4be0a9439f057a5f0c99~mv2.png/v1/fill/w_160,h_108,al_c,q_85,usm_0.66_1.00_0.01,enc_avif,quality_auto/Meester%20Tom%20legt%20uit%207.png" alt="Meester Tom Logo">
        </div>
        <div class="btn-help-trigger" onclick="showHelp()" title="Hulp nodig?">?</div>
    </div>
</footer>

<script>
    const mapArea = document.getElementById('map-area');
    const zoomLayer = document.getElementById('zoom-layer');
    const svgCanvas = document.getElementById('svg-canvas');
    const centerNode = document.getElementById('center-node');
    const floatingDelete = document.getElementById('floating-delete-btn');
    const stickerTools = document.getElementById('sticker-tools');
    const textFormattingMenu = document.getElementById('text-formatting-menu');
    const settingsMenu = document.getElementById('settings-menu');
    const fontSizePicker = document.getElementById('font-size-picker');
    const libraryModal = document.getElementById('library-modal');
    const shapeSelect = document.getElementById('shape-select');
    const mapTitleInput = document.getElementById('map-title-input'); 
    const startupModal = document.getElementById('startup-modal');
    const infoModal = document.getElementById('info-modal');
    const helpModal = document.getElementById('help-modal');

    // LOGICA VOOR OPENER
    window.addEventListener('load', () => {
        setTimeout(() => {
            document.getElementById('opener').classList.add('fade-out');
        }, 1200); 
    });
    
    let selectedNode = centerNode;
    let scale = 1;
    let translateX = window.innerWidth / 2;
    let translateY = window.innerHeight / 2;
    let isPanning = false; let isAppInitialized = false;
    let startX, startY;
    let initialPinchDist = null;
    const CANVAS_OFFSET = 50000;
    
    let currentFileHandle = null;
    let undoStack = [];
    let redoStack = [];

    const stickerData = [
        {char: '🐶', tags: 'hond dier woef pup'}, {char: '🐱', tags: 'kat dier poes miauw'}, {char: '🐭', tags: 'muis dier piep'}, {char: '🐹', tags: 'hamster dier'}, {char: '🐰', tags: 'konijn dier paas'}, {char: '🦊', tags: 'vos dier'}, {char: '🐻', tags: 'beer dier'}, {char: '🐼', tags: 'panda dier'}, {char: '🦁', tags: 'leeuw dier koning'}, {char: '🐯', tags: 'tijger dier'}, {char: '🐮', tags: 'koe dier boerderij'}, {char: '🐷', tags: 'varken dier knor'}, {char: '🐸', tags: 'kikker dier kwaak'}, {char: '🐵', tags: 'aap dier banaan'}, {char: '🦄', tags: 'eenhoorn fantasy paard'}, {char: '🐉', tags: 'draak fantasy vuur'}, {char: '🦖', tags: 'dino rex dier'}, {char: '🐢', tags: 'schildpad dier langzaam'}, {char: '🦒', tags: 'giraf dier lang'}, {char: '🐘', tags: 'olifant dier slurf'}, {char: '🐙', tags: 'inktvis zee water'}, {char: '🐝', tags: 'bij insect honing'}, {char: '🦋', tags: 'vlinder insect kleur'}, {char: '🍎', tags: 'appel fruit eten gezond'}, {char: '🍓', tags: 'aardbei fruit eten'}, {char: '🍌', tags: 'banaan fruit eten'}, {char: '🍉', tags: 'meloen fruit eten'}, {char: '🍇', tags: 'druif fruit eten'}, {char: '🍕', tags: 'pizza eten warm'}, {char: '🍔', tags: 'hamburger eten snack'}, {char: '🍟', tags: 'friet patat eten snack'}, {char: '🍦', tags: 'ijs koud eten toetje'}, {char: '🍭', tags: 'snoep lolly suiker'}, {char: '🍪', tags: 'koekje eten'}, {char: '⚽', tags: 'voetbal sport spel'}, {char: '🏀', tags: 'basketbal sport'}, {char: '🎸', tags: 'gitaar muziek instrument'}, {char: '🎹', tags: 'piano muziek instrument'}, {char: '🎮', tags: 'game controller spelen'}, {char: '🎨', tags: 'verf kunst tekenen'}, {char: '📚', tags: 'boeken lezen school'}, {char: '📖', tags: 'boek lezen'}, {char: '🖍️', tags: 'krijtje tekenen school'}, {char: '📝', tags: 'schrijven pen school'}, {char: '✏️', tags: 'potlood schrijven school'}, {char: '🏫', tags: 'school gebouw leren'}, {char: '🎒', tags: 'rugzak tas school'}, {char: '🧪', tags: 'proefje science lab'}, {char: '🔭', tags: 'verrekijker sterren science'}, {char: '🌍', tags: 'aarde wereld globe'}, {char: '🌈', tags: 'regenboog weer kleur'}, {char: '☀️', tags: 'zon weer warm'}, {char: '⭐', tags: 'ster geel blink'}, {char: '🌙', tags: 'maan nacht slapen'}, {char: '☁️', tags: 'wolk weer lucht'}, {char: '🌧️', tags: 'regen weer nat'}, {char: '❄️', tags: 'sneeuw koud weer'}, {char: '🔥', tags: 'vuur warm heet'}, {char: '💧', tags: 'water druppel nat'}, {char: '🚀', tags: 'raket ruimte vliegen'}, {char: '🛸', tags: 'ufo ruimte alien'}, {char: '🚁', tags: 'helikopter vliegen'}, {char: '🚲', tags: 'fiets rijden'}, {char: '🛹', tags: 'skateboard sport'}, {char: '❤️', tags: 'hart liefde rood'}, {char: '🥳', tags: 'feest blij jarig'}, {char: '😎', tags: 'cool bril zon'}, {char: '🤔', tags: 'denken vraag'}, {char: '🤯', tags: 'wauw ontploffing'}, {char: '🤖', tags: 'robot computer'}, {char: '👽', tags: 'alien ruimte'}, {char: '👑', tags: 'kroon koning goud'}, {char: '💡', tags: 'lamp idee licht'}, {char: '🎯', tags: 'doel pijl raken'}, {char: '✅', tags: 'goed klaar vinkje'}, {char: '❌', tags: 'fout kruisje stop'}, {char: '🏆', tags: 'beker winnaar goud'}, {char: '🎁', tags: 'cadeau feest jarig'}, {char: '🎈', tags: 'ballon feest'}, {char: '🏝️', tags: 'eiland vakantie zee'}, {char: '🌋', tags: 'vulkaan vuur berg'}, {char: '🍄', tags: 'paddenstoel natuur bos'}, {char: '🌸', tags: 'bloem lente'}, {char: '👟', tags: 'schoen lopen sport'}, {char: '📐', tags: 'liniaal school meten'}, {char: '🔋', tags: 'batterij stroom'}, {char: '💻', tags: 'laptop computer'}, {char: '📽️', tags: 'film bioscoop'}, {char: '📢', tags: 'roeper nieuws'}, {char: '💤', tags: 'slapen moe'}, {char: '🏰', tags: 'kasteel ridder'}, {char: '🧙', tags: 'tovenaar fantasy'}, {char: '🧜', tags: 'zeemeermin fantasy'}, {char: '🧛', tags: 'vampier griezel'}, {char: '👻', tags: 'spook griezel'}, {char: '🎃', tags: 'pompoen halloween'}, {char: '🍩', tags: 'donut eten zoet'}, {char: '🥨', tags: 'pretzel eten'}, {char: '🥛', tags: 'melk drinken'}, {char: '🌽', tags: 'mais groente eten'}, {char: '🥑', tags: 'avocado eten'}, {char: '🛹', tags: 'skateboard rollen'}, {char: '🛼', tags: 'rolschaats sport'}, {char: '🧶', tags: 'wol breien'}, {char: '🧿', tags: 'oog blauw'}, {char: '💎', tags: 'diamant edelsteen'}, {char: '🧪', tags: 'wetenschap proefje'}, {char: '🧬', tags: 'dna science'}
    ];

    const themes = {
        default: { bg: '#f0f4f8', center: '#ffcc00', b1: '#e91e63', b2: '#03a9f4', b3: '#ff9800', b4: '#8bc34a', text: '#333' },
        nature: { bg: '#e8f5e9', center: '#4caf50', b1: '#2e7d32', b2: '#8bc34a', b3: '#795548', b4: '#ffeb3b', text: '#1b5e20' },
        space: { bg: '#0b0e14', center: '#673ab7', b1: '#e91e63', b2: '#00bcd4', b3: '#ffeb3b', b4: '#9c27b0', text: '#ffffff' },
        ocean: { bg: '#e0f7fa', center: '#0288d1', b1: '#00acc1', b2: '#26c6da', b3: '#ff7043', b4: '#4db6ac', text: '#01579b' }
    };

    function setTheme(themeName, element) {
        if (element) {
            document.querySelectorAll('.theme-card').forEach(c => c.classList.remove('active'));
            element.classList.add('active');
        }
        const t = themes[themeName];
        const root = document.documentElement;
        if (!document.body.classList.contains('dark-mode') || themeName === 'space') {
            root.style.setProperty('--bg-color', t.bg);
        }
        root.style.setProperty('--center-color', t.center);
        root.style.setProperty('--branch-1', t.b1);
        root.style.setProperty('--branch-2', t.b2);
        root.style.setProperty('--branch-3', t.b3);
        root.style.setProperty('--branch-4', t.b4);
        centerNode.style.backgroundColor = t.center;
    }

    function toggleNightMode() {
        document.body.classList.toggle('dark-mode');
        const isDark = document.body.classList.contains('dark-mode');
        localStorage.setItem('nightMode', isDark);
        const activeTheme = document.querySelector('.theme-card.active');
        const themeName = activeTheme ? activeTheme.innerText.split('\n')[0].toLowerCase().trim() : 'default';
        const nameMap = {'standaard': 'default', 'natuur': 'nature', 'ruimte': 'space', 'oceaan': 'ocean'};
        setTheme(nameMap[themeName] || 'default', activeTheme);
    }

    if (localStorage.getItem('nightMode') === 'true') {
        document.body.classList.add('dark-mode');
    }

    function startApp(withTemplate) { startupModal.style.display = 'none'; init(); if (withTemplate) applyTemplate(); }
    function showAppInfo() { infoModal.style.display = 'flex'; }
    function closeInfoModal() { infoModal.style.display = 'none'; }
    function showHelp() { helpModal.style.display = 'flex'; }
    function closeHelp() { helpModal.style.display = 'none'; }

    function applyTemplate() {
        const items = [
            { text: 'Wie?', color: varColor('--branch-1'), x: -200, y: -100 },
            { text: 'Wat?', color: varColor('--branch-2'), x: 200, y: -100 },
            { text: 'Waar?', color: varColor('--branch-3'), x: -200, y: 150 },
            { text: 'Waarom?', color: varColor('--branch-4'), x: 200, y: 150 }
        ];
        items.forEach(item => {
            const id = 'node-' + Math.random().toString(36).substr(2, 9);
            const node = createNodeElement({
                id: id, text: item.text, color: item.color, parentId: centerNode.id,
                level: 1, left: centerNode.offsetLeft + item.x, top: centerNode.offsetTop + item.y,
                shape: 'shape-round'
            }, true);
            createLine(centerNode, node);
        });
        updateLines();
        saveState();
    }

    function init() { if (isAppInitialized) return; isAppInitialized = true;
        centerNode.dataset.level = 0;
        centerNode.dataset.rotation = 0;
        centerNode.dataset.shape = 'shape-round';
        centerNode.style.zIndex = 5;
        addHandles(centerNode);
        selectNode(centerNode);
        makeDraggable(centerNode);
        setTimeout(() => { centerView(); }, 100);
        updateLines();
        saveState(); 
        renderStickers(stickerData);
        floatingDelete.onclick = (e) => { e.stopPropagation(); deleteSelectedNode(); };
        window.addEventListener('paste', (e) => {
            const items = (e.clipboardData || e.originalEvent.clipboardData).items;
            for (let item of items) {
                if (item.kind === 'file' && item.type.includes('image')) {
                    const reader = new FileReader();
                    reader.onload = (event) => { spawnImage(event.target.result); saveState(); };
                    reader.readAsDataURL(item.getAsFile());
                }
            }
        });
    }

    function renderStickers(list) {
        const grid = document.getElementById('library-grid-container');
        grid.innerHTML = '';
        list.forEach(item => {
            const div = document.createElement('div');
            div.className = 'library-item';
            div.innerText = item.char;
            div.title = item.tags;
            div.onclick = () => { spawnSticker(item.char); toggleLibrary(); };
            grid.appendChild(div);
        });
    }

    function filterLibrary(term) {
        const filtered = stickerData.filter(item => 
            item.tags.toLowerCase().includes(term.toLowerCase()) || 
            item.char.includes(term)
        );
        renderStickers(filtered);
    }

    function toggleLibrary() { 
        libraryModal.style.display = (libraryModal.style.display === 'flex') ? 'none' : 'flex'; 
        if(libraryModal.style.display === 'flex') {
            document.getElementById('sticker-search-input').value = '';
            renderStickers(stickerData);
            document.getElementById('sticker-search-input').focus();
        }
    }

    function deselectAll() {
        document.querySelectorAll('.node').forEach(n => n.classList.remove('selected'));
        selectedNode = null;
        floatingDelete.style.display = 'none';
        stickerTools.style.display = 'none';
        textFormattingMenu.style.display = 'none'; 
        if (window.getSelection) { window.getSelection().removeAllRanges(); }
        if (document.activeElement && document.activeElement.blur) { document.activeElement.blur(); }
    }

    mapArea.addEventListener('mousedown', (e) => {
        if (e.target === mapArea || e.target.id === 'grid-overlay' || e.target.id === 'zoom-layer') {
            deselectAll(); isPanning = true; startX = e.clientX - translateX; startY = e.clientY - translateY;
        }
    });

    mapArea.addEventListener('touchstart', (e) => {
        if (e.touches.length === 1 && (e.target === mapArea || e.target.id === 'grid-overlay' || e.target.id === 'zoom-layer')) {
            deselectAll(); isPanning = true; startX = e.touches[0].clientX - translateX; startY = e.touches[0].clientY - translateY;
        } else if (e.touches.length === 2) {
            isPanning = false; initialPinchDist = Math.hypot(e.touches[0].clientX - e.touches[1].clientX, e.touches[0].clientY - e.touches[1].clientY);
        }
    }, { passive: false });

    window.addEventListener('mousemove', (e) => { if (isPanning) { translateX = e.clientX - startX; translateY = e.clientY - startY; applyTransform(); } });
    window.addEventListener('touchmove', (e) => {
        if (isPanning && e.touches.length === 1) {
            translateX = e.touches[0].clientX - startX; translateY = e.touches[0].clientY - startY; applyTransform();
        } else if (e.touches.length === 2 && initialPinchDist) {
            e.preventDefault();
            const dist = Math.hypot(e.touches[0].clientX - e.touches[1].clientX, e.touches[0].clientY - e.touches[1].clientY);
            const zoomFactor = dist / initialPinchDist;
            const oldScale = scale; scale = Math.min(Math.max(0.1, scale * zoomFactor), 4);
            const midX = (e.touches[0].clientX + e.touches[1].clientX) / 2;
            const midY = (e.touches[0].clientY + e.touches[1].clientY) / 2;
            translateX -= (midX - translateX) * (scale / oldScale - 1); translateY -= (midY - translateY) * (scale / oldScale - 1);
            initialPinchDist = dist; applyTransform();
        }
    }, { passive: false });

    window.addEventListener('mouseup', () => isPanning = false);
    window.addEventListener('touchend', () => { isPanning = false; initialPinchDist = null; });
    mapArea.addEventListener('wheel', (e) => {
        e.preventDefault();
        const delta = e.deltaY > 0 ? -0.1 : 0.1;
        const oldScale = scale; scale = Math.min(Math.max(0.1, scale + delta), 4);
        const rect = mapArea.getBoundingClientRect();
        const mouseX = e.clientX - rect.left; const mouseY = e.clientY - rect.top;
        translateX -= (mouseX - translateX) * (scale / oldScale - 1); translateY -= (mouseY - translateY) * (scale / oldScale - 1);
        applyTransform();
    }, { passive: false });

    function applyTransform() { zoomLayer.style.transform = `translate(${translateX}px, ${translateY}px) scale(${scale})`; updateFloatingDeletePos(); }
    function centerView() { scale = 1; translateX = (window.innerWidth / 2) - (centerNode.offsetWidth / 2); translateY = (window.innerHeight / 2) - (centerNode.offsetHeight / 2); applyTransform(); }

    function makeDraggable(elm) {
        let p1 = 0, p2 = 0, p3 = 0, p4 = 0;
        const startDragging = (e) => {
            if (isTransforming) return; selectNode(elm); e.stopPropagation();
            const cx = e.type.includes('touch') ? e.touches[0].clientX : e.clientX;
            const cy = e.type.includes('touch') ? e.touches[0].clientY : e.clientY;
            p3 = cx; p4 = cy;
            let moved = false;
            const dragging = (me) => {
                const mcx = me.type.includes('touch') ? me.touches[0].clientX : me.clientX;
                const mcy = me.type.includes('touch') ? me.touches[0].clientY : me.clientY;
                p1 = (p3 - mcx) / scale; p2 = (p4 - mcy) / scale; p3 = mcx; p4 = mcy;
                elm.style.top = (elm.offsetTop - p2) + "px"; elm.style.left = (elm.offsetLeft - p1) + "px";
                updateLines(); updateFloatingDeletePos(); moved = true;
            };
            const stopDragging = () => {
                if (moved) saveState();
                window.removeEventListener('mousemove', dragging); window.removeEventListener('mouseup', stopDragging);
                window.removeEventListener('touchmove', dragging); window.removeEventListener('touchend', stopDragging);
            };
        window.addEventListener('mousemove', dragging); window.addEventListener('mouseup', stopDragging);
        window.addEventListener('touchmove', dragging, { passive: false }); window.addEventListener('touchend', stopDragging);
    };
        elm.addEventListener('mousedown', startDragging); elm.addEventListener('touchstart', startDragging, { passive: false });
    }

    let isTransforming = false;
    function addHandles(node) {
        if (node.querySelector('.node-handle')) return;
        const rotate = document.createElement('div'); rotate.className = 'node-handle handle-rotate';
        const resize = document.createElement('div'); resize.className = 'node-handle handle-resizer';
        node.appendChild(rotate); node.appendChild(resize);
        const startTransform = (e) => {
            e.stopPropagation(); e.preventDefault(); isTransforming = true;
            const handle = e.target; const isRotate = handle.classList.contains('handle-rotate');
            const rect = node.getBoundingClientRect();
            const centerX = rect.left + rect.width / 2;
            const centerY = rect.top + rect.height / 2;
            const onMove = (mv) => {
                const mx = mv.type.includes('touch') ? mv.touches[0].clientX : mv.clientX;
                const mcy = mv.type.includes('touch') ? mv.touches[0].clientY : mv.clientY;
                if (isRotate) {
                    const angle = Math.atan2(mcy - centerY, mx - centerX) * 180 / Math.PI + 90;
                    node.dataset.rotation = angle; node.style.transform = `rotate(${angle}deg)`;
                } else {
                    const dist = Math.hypot(mx - centerX, mcy - centerY) * 2 / scale;
                    if (node.classList.contains('sticker')) node.style.fontSize = Math.max(20, dist / 1.2) + 'px';
                    else if (node.classList.contains('image-node')) { node.style.width = Math.max(50, dist) + 'px'; node.style.height = 'auto'; }
                    else node.style.width = Math.max(50, dist) + 'px';
                }
                updateLines(); updateFloatingDeletePos();
            };
            const onEnd = () => { isTransforming = false; saveState(); window.removeEventListener('mousemove', onMove); window.removeEventListener('mouseup', onEnd); window.removeEventListener('touchmove', onMove); window.removeEventListener('touchend', onEnd); };
            window.addEventListener('mousemove', onMove); window.addEventListener('mouseup', onEnd);
            window.addEventListener('touchmove', onMove, { passive: false }); window.addEventListener('touchend', onEnd);
        };
        [rotate, resize].forEach(h => { h.addEventListener('mousedown', startTransform); h.addEventListener('touchstart', startTransform); });
    }

    function selectNode(elm) {
        if (!elm) return;
        document.querySelectorAll('.node').forEach(n => n.classList.remove('selected'));
        selectedNode = elm; selectedNode.classList.add('selected');
        const isSticker = elm.classList.contains('sticker');
        const isImage = elm.classList.contains('image-node');
        const hasTextContent = elm.innerText.trim().length > 0 && !isImage;
        textFormattingMenu.style.display = 'flex';
        document.getElementById('layer-picker-container').style.display = 'flex';
        fontSizePicker.value = parseInt(window.getComputedStyle(elm).fontSize);
        stickerTools.style.display = (isSticker || isImage) ? 'flex' : 'none';
        floatingDelete.style.display = (elm === centerNode) ? 'none' : 'flex';
        if (!isSticker) { document.getElementById('shape-picker-container').style.display = 'flex'; shapeSelect.value = elm.dataset.shape || 'shape-round'; }
        else document.getElementById('shape-picker-container').style.display = 'none';
        if (hasTextContent && !isSticker) {
            document.getElementById('font-picker-container').style.display = 'flex';
            document.getElementById('font-size-wrapper').style.display = 'flex';
            document.getElementById('text-color-container').style.display = 'flex';
            document.getElementById('bg-color-container').style.display = 'flex';
        } else {
            document.getElementById('font-picker-container').style.display = 'none';
            document.getElementById('font-size-wrapper').style.display = 'none';
            document.getElementById('text-color-container').style.display = 'none';
            document.getElementById('bg-color-container').style.display = isImage ? 'none' : 'flex';
        }
        updateFloatingDeletePos(); 
        if(!isSticker && !isImage) {
            elm.focus();
            setTimeout(() => {
                const range = document.createRange(); range.selectNodeContents(elm);
                const selection = window.getSelection(); selection.removeAllRanges(); selection.addRange(range);
            }, 1);
        }
    }

    function changeLayer(dir) {
        if (!selectedNode) return;
        let currentZ = parseInt(selectedNode.style.zIndex) || 10;
        if (dir === 'front') selectedNode.style.zIndex = currentZ + 1;
        else selectedNode.style.zIndex = Math.max(1, currentZ - 1);
        saveState();
    }

    function handleNodeInput(elm) { updateLines(); saveState(); if(elm === selectedNode) textFormattingMenu.style.display = 'flex'; }

    function createNodeElement(d, isNew = false) {
        const n = document.createElement('div'); n.id = d.id;
        n.addEventListener('dragstart', (e) => e.preventDefault());
        if (isNew) { n.classList.add('animate-pop'); setTimeout(() => n.classList.remove('animate-pop'), 500); }
        if (d.isImage) {
            n.className = 'node image-node';
            const img = document.createElement('img'); img.src = d.imageSrc; n.appendChild(img);
            const shapeClass = d.shape || 'shape-round'; n.classList.add(shapeClass); n.dataset.shape = shapeClass;
        } else {
            n.className = d.isSticker ? 'node sticker' : 'node';
            if (!d.isSticker) { n.contentEditable = true; const shapeClass = d.shape || 'shape-round'; n.classList.add(shapeClass); n.dataset.shape = shapeClass; }
            n.innerText = d.text || '';
        }
        n.style.left = d.left + 'px'; n.style.top = d.top + 'px';
        n.style.borderColor = d.color || 'transparent';
        n.style.zIndex = d.zIndex || (d.isMain ? 5 : (d.isSticker ? 11 : 10));
        if(d.bgColor) n.style.backgroundColor = d.bgColor;
        if(d.fontFamily) n.style.fontFamily = d.fontFamily;
        if(d.textColor) n.style.color = d.textColor;
        n.dataset.level = d.level || 0; n.dataset.parentId = d.parentId || '';
        n.dataset.rotation = d.rotation || 0; n.style.transform = `rotate(${d.rotation || 0}deg)`;
        if(d.width) n.style.width = d.width + 'px';
        if(d.fontSize) n.style.fontSize = d.fontSize;
        n.oninput = () => handleNodeInput(n);
        zoomLayer.appendChild(n); addHandles(n); makeDraggable(n);
        return n;
    }

    function changeNodeShape(shapeClass) {
        if (!selectedNode || selectedNode.classList.contains('sticker')) return;
        ['shape-round', 'shape-rect', 'shape-cloud', 'shape-wavy', 'shape-angular'].forEach(s => selectedNode.classList.remove(s));
        selectedNode.classList.add(shapeClass); selectedNode.dataset.shape = shapeClass; saveState();
    }

    function addBranch(color) {
        if(!selectedNode) selectNode(centerNode);
        const id = 'node-' + Math.random().toString(36).substr(2, 9);
        const node = createNodeElement({
            id: id, text: 'Nieuw...', color: color, parentId: selectedNode.id,
            level: parseInt(selectedNode.dataset.level || 0) + 1,
            left: selectedNode.offsetLeft + (Math.random()*100-50),
            top: selectedNode.offsetTop + 120, shape: 'shape-round'
        }, true);
        createLine(selectedNode, node); selectNode(node); saveState();
    }

    function createLine(start, end) {
        const path = document.createElementNS("http://www.w3.org/2000/svg", "path");
        const lineId = 'line-' + Math.random().toString(36).substr(2, 9);
        path.id = lineId; path.setAttribute('class', 'branch-path');
        end.dataset.lineId = lineId; svgCanvas.appendChild(path); updateLines();
    }

    function updateLines() {
        document.querySelectorAll('.node').forEach(node => {
            const parentId = node.dataset.parentId; if (!parentId) return;
            const parent = document.getElementById(parentId);
            const path = document.getElementById(node.dataset.lineId);
            if (path && parent) {
                const x1 = parent.offsetLeft + parent.offsetWidth / 2 + CANVAS_OFFSET;
                const y1 = parent.offsetTop + parent.offsetHeight / 2 + CANVAS_OFFSET;
                const x2 = node.offsetLeft + node.offsetWidth / 2 + CANVAS_OFFSET;
                const y2 = node.offsetTop + node.offsetHeight / 2 + CANVAS_OFFSET;
                const dx = x2 - x1; const dy = y2 - y1; const level = parseInt(node.dataset.level || 1);
                const startThickness = Math.max(4, 24 / level); const endThickness = Math.max(2, 24 / (level + 1));
                const d = `M ${x1} ${y1-startThickness/2} C ${x1+dx*0.5} ${y1-startThickness/2}, ${x2-dx*0.5} ${y2-endThickness/2}, ${x2} ${y2-endThickness/2} L ${x2} ${y2+endThickness/2} C ${x2-dx*0.5} ${y2+endThickness/2}, ${x1+dx*0.5} ${y1+startThickness/2}, ${x1} ${y1+startThickness/2} Z`;
                path.setAttribute('d', d); path.setAttribute('fill', node.style.borderColor || '#ccc');
            }
        });
    }

    function deleteSelectedNode() {
        if (!selectedNode || selectedNode === centerNode) return;
        const removeRecursive = (id) => {
            document.querySelectorAll(`[data-parent-id="${id}"]`).forEach(child => {
                removeRecursive(child.id); const l = document.getElementById(child.dataset.lineId); if (l) l.remove(); child.remove();
            });
        };
        const line = document.getElementById(selectedNode.dataset.lineId); if (line) line.remove();
        removeRecursive(selectedNode.id); selectedNode.remove(); selectNode(centerNode); updateLines(); saveState();
    }

    function spawnSticker(emoji, x, y) {
        const n = createNodeElement({
            id: 'stk-' + Math.random().toString(36).substr(2, 9), text: emoji, isSticker: true,
            left: x || (window.innerWidth/2 - translateX)/scale, top: y || (window.innerHeight/2 - translateY)/scale
        }, true);
        selectNode(n); saveState();
    }

    function spawnImage(src) {
        const n = createNodeElement({
            id: 'img-' + Math.random().toString(36).substr(2, 9), isImage: true, imageSrc: src,
            left: (window.innerWidth/2 - translateX)/scale, top: (window.innerHeight/2 - translateY)/scale, width: 200,
            shape: 'shape-round'
        }, true);
        selectNode(n);
    }

    function handleImageUpload(e) {
        const file = e.target.files[0]; if (!file) return;
        const r = new FileReader(); r.onload = (ev) => { spawnImage(ev.target.result); saveState(); }; r.readAsDataURL(file);
    }

    function toggleSettingsMenu(e) { e.stopPropagation(); settingsMenu.classList.toggle('active'); }
    window.addEventListener('click', () => settingsMenu.classList.remove('active'));

    function updateFloatingDeletePos() {
        if (!selectedNode || selectedNode === centerNode) { floatingDelete.style.display = 'none'; return; }
        const r = selectedNode.getBoundingClientRect(); const a = mapArea.getBoundingClientRect();
        floatingDelete.style.left = (r.right - a.left - 5) + 'px'; floatingDelete.style.top = (r.top - a.top - 20) + 'px'; floatingDelete.style.display = 'flex';
    }

    function changeFont(f) { if(selectedNode) { selectedNode.style.fontFamily = f; saveState(); } }
    function changeFontSize(s) { if(selectedNode) { selectedNode.style.fontSize = s + 'px'; updateLines(); saveState(); } }
    function stepFontSize(delta) { let val = Math.max(8, Math.min(150, parseInt(fontSizePicker.value) + delta)); fontSizePicker.value = val; changeFontSize(val); }
    function changeTextColor(c) { if(selectedNode) { selectedNode.style.color = c; saveState(); } }
    function changeNodeColors(c) {
        if(!selectedNode || selectedNode.classList.contains('sticker') || selectedNode.classList.contains('image-node')) return;
        selectedNode.style.backgroundColor = c; if(selectedNode !== centerNode) { selectedNode.style.borderColor = c; updateLines(); } saveState();
    }
    function changeStickerSize(s) { if(!selectedNode) return; if(selectedNode.classList.contains('sticker')) selectedNode.style.fontSize = s + 'px'; else selectedNode.style.width = s + 'px'; saveState(); }

    function getCurrentState() {
        const nodes = Array.from(document.querySelectorAll('.node')).map(n => ({
            id: n.id, text: n.innerText, left: parseInt(n.style.left), top: parseInt(n.style.top),
            color: n.style.borderColor, bgColor: n.style.backgroundColor, textColor: n.style.color,
            fontFamily: n.style.fontFamily, level: n.dataset.level, parentId: n.dataset.parentId,
            isSticker: n.classList.contains('sticker'), isImage: n.classList.contains('image-node'),
            imageSrc: n.classList.contains('image-node') ? n.querySelector('img').src : null,
            isMain: n.id === 'center-node', rotation: n.dataset.rotation, 
            width: n.style.width ? parseInt(n.style.width) : null, fontSize: n.style.fontSize, 
            lineId: n.dataset.lineId, shape: n.dataset.shape, zIndex: n.style.zIndex
        }));
        return { mapName: mapTitleInput.value, nodes: nodes };
    }

    function saveState() {
        const state = JSON.stringify(getCurrentState());
        if (undoStack.length > 0 && undoStack[undoStack.length - 1] === state) return;
        undoStack.push(state); redoStack = []; if (undoStack.length > 50) undoStack.shift();
    }

    function undo() { if (undoStack.length <= 1) return; redoStack.push(undoStack.pop()); applyState(JSON.parse(undoStack[undoStack.length - 1])); }
    function redo() { if (redoStack.length === 0) return; const s = redoStack.pop(); undoStack.push(s); applyState(JSON.parse(s)); }

    function applyState(data) {
        const nodesData = Array.isArray(data) ? data : data.nodes;
        mapTitleInput.value = data.mapName || "Mijn Mindmap";
        zoomLayer.querySelectorAll('.node').forEach(el => el.id !== 'center-node' && el.remove());
        svgCanvas.innerHTML = '';
        const main = nodesData.find(d => d.isMain);
        if(main) {
            centerNode.innerText = main.text; centerNode.style.left = main.left + 'px'; centerNode.style.top = main.top + 'px'; 
            centerNode.style.backgroundColor = main.bgColor; centerNode.style.color = main.textColor; 
            centerNode.style.fontFamily = main.fontFamily; centerNode.style.fontSize = main.fontSize; 
            centerNode.dataset.shape = main.shape || 'shape-round'; centerNode.className = 'node node-main ' + (main.shape || 'shape-round');
            centerNode.style.width = 'auto'; centerNode.style.zIndex = main.zIndex || 5;
        }
        nodesData.filter(d => !d.isMain).sort((a,b) => a.level - b.level).forEach(d => {
            const n = createNodeElement(d, false); if(d.parentId) { const p = document.getElementById(d.parentId); if (p) createLine(p, n); }
        });
        updateLines();
    }

    async function saveMapData(isSaveAs = false) {
        const content = JSON.stringify(getCurrentState());
        const fileName = (mapTitleInput.value.trim() || "mindmap") + ".json";
        let saveConfirmed = false;
        if ('showSaveFilePicker' in window) {
            try {
                if (!isSaveAs && currentFileHandle) {
                    const writable = await currentFileHandle.createWritable(); await writable.write(content); await writable.close(); saveConfirmed = true;
                } else {
                    const handle = await window.showSaveFilePicker({ suggestedName: fileName, types: [{ description: 'JSON Bestanden', accept: { 'application/json': ['.json'] } }] });
                    currentFileHandle = handle; const writable = await handle.createWritable(); await writable.write(content); await writable.close(); saveConfirmed = true;
                }
            } catch (err) {}
        } else {
            const blob = new Blob([content], {type: "application/json"}); const a = document.createElement('a');
            a.href = URL.createObjectURL(blob); a.download = fileName; a.click(); saveConfirmed = true;
        }
        if (saveConfirmed) {
            document.querySelectorAll('button[onclick="saveMapData(false)"]').forEach(btn => {
                const originalContent = btn.innerHTML; btn.innerHTML = btn.classList.contains('btn-quick-save') ? "✅ Opgeslagen" : "✅ Opgeslagen!";
                btn.style.color = "#4caf50"; setTimeout(() => { btn.innerHTML = originalContent; btn.style.color = ""; }, 2000);
            });
        }
    }

    async function openFile() {
        if ('showOpenFilePicker' in window) {
            try {
                const [handle] = await window.showOpenFilePicker({ types: [{ description: 'JSON Bestanden', accept: { 'application/json': ['.json'] } }], multiple: false });
                currentFileHandle = handle; const file = await handle.getFile(); const content = await file.text();
                applyState(JSON.parse(content)); centerView(); saveState(); startupModal.style.display = 'none'; init();
            } catch (err) {}
        } else document.getElementById('file-input').click();
    }

    function loadMapData(event) {
        const f = event.target.files[0]; if(!f) return;
        const r = new FileReader(); r.onload = (ev) => { try { applyState(JSON.parse(ev.target.result)); centerView(); saveState(); startupModal.style.display = 'none'; init(); } catch(e) { alert("Bestand kon niet worden geladen."); } }; r.readAsText(f);
    }

    async function downloadMap() {
        deselectAll(); 
        const nodes = document.querySelectorAll('.node');
        if (nodes.length === 0) return;

        const oldTransform = zoomLayer.style.transform;
        zoomLayer.style.transform = 'none';

        let minX = Infinity, minY = Infinity, maxX = -Infinity, maxY = -Infinity;
        nodes.forEach(n => {
            const x = parseFloat(n.style.left);
            const y = parseFloat(n.style.top);
            const w = n.offsetWidth;
            const h = n.offsetHeight;
            if (x < minX) minX = x;
            if (y < minY) minY = y;
            if (x + w > maxX) maxX = x + w;
            if (y + h > maxY) maxY = y + h;
        });

        const padding = 80;
        const width = (maxX - minX) + (padding * 2);
        const height = (maxY - minY) + (padding * 2);

        const fileName = (mapTitleInput.value.trim() || "mindmap") + ".png";
        
        try {
            const canvas = await html2canvas(zoomLayer, {
                backgroundColor: getComputedStyle(document.body).backgroundColor,
                scale: 2, 
                useCORS: true, 
                allowTaint: true,
                x: minX + CANVAS_OFFSET - padding,
                y: minY + CANVAS_OFFSET - padding,
                width: width,
                height: height,
                logging: false
            });
            const link = document.createElement('a'); 
            link.download = fileName; 
            link.href = canvas.toDataURL("image/png"); 
            link.click();
        } catch (err) { 
            console.error(err);
            alert("Oeps! Er ging iets mis bij het maken van de foto."); 
        } finally { 
            zoomLayer.style.transform = oldTransform; 
        }
    }

    async function shareMap() {
        deselectAll();
        const nodes = document.querySelectorAll('.node');
        if (nodes.length === 0) return;

        const oldTransform = zoomLayer.style.transform;
        zoomLayer.style.transform = 'none';

        let minX = Infinity, minY = Infinity, maxX = -Infinity, maxY = -Infinity;
        nodes.forEach(n => {
            const x = parseFloat(n.style.left), y = parseFloat(n.style.top);
            const w = n.offsetWidth, h = n.offsetHeight;
            if (x < minX) minX = x; if (y < minY) minY = y;
            if (x + w > maxX) maxX = x + w; if (y + h > maxY) maxY = y + h;
        });

        const padding = 60;
        const width = (maxX - minX) + (padding * 2);
        const height = (maxY - minY) + (padding * 2);

        try {
            const canvas = await html2canvas(zoomLayer, {
                backgroundColor: getComputedStyle(document.body).backgroundColor,
                scale: 2, 
                useCORS: true,
                x: minX + CANVAS_OFFSET - padding,
                y: minY + CANVAS_OFFSET - padding,
                width: width,
                height: height
            });
            const blob = await new Promise(resolve => canvas.toBlob(resolve, 'image/png'));
            const fileName = (mapTitleInput.value.trim() || "mindmap") + ".png";
            const file = new File([blob], fileName, { type: 'image/png' });
            if (navigator.share && navigator.canShare({ files: [file] })) {
                await navigator.share({ files: [file], title: mapTitleInput.value, text: 'Kijk naar mijn mindmap!' });
            } else { downloadMap(); }
        } catch (err) { alert("Er ging iets mis."); } finally { zoomLayer.style.transform = oldTransform; }
    }

    function printMap() {
        deselectAll(); const nodes = document.querySelectorAll('.node'); if (nodes.length === 0) return;
        const oldX = translateX, oldY = translateY, oldS = scale;
        let minX = Infinity, minY = Infinity, maxX = -Infinity, maxY = -Infinity;
        nodes.forEach(n => {
            const x = parseFloat(n.style.left), y = parseFloat(n.style.top); const w = n.offsetWidth, h = n.offsetHeight;
            if (x < minX) minX = x; if (y < minY) minY = y; if (x + w > maxX) maxX = x + w; if (y + h > maxY) maxY = y + h;
        });
        const padding = 60; const contentW = (maxX - minX) + (padding * 2); const contentH = (maxY - minY) + (padding * 2);
        const targetW = window.innerWidth; const targetH = window.innerHeight;
        const printScale = Math.min(targetW / contentW, targetH / contentH);
        translateX = (-minX * printScale) + (targetW - (maxX - minX) * printScale) / 2;
        translateY = (-minY * printScale) + (targetH - (maxY - minY) * printScale) / 2;
        scale = printScale; applyTransform(); updateLines();
        setTimeout(() => { window.print(); translateX = oldX; translateY = oldY; scale = oldS; applyTransform(); updateLines(); }, 600);
    }

    function addNewBranchButton(c) { const b = document.createElement('button'); b.style.backgroundColor = c; b.style.color = "white"; b.innerHTML = "+"; b.onclick = () => addBranch(c); document.getElementById('branch-buttons').appendChild(b); }
    function resetMap() { if(confirm("Alles wissen en opnieuw beginnen?")) location.reload(); }
    function varColor(n) { return getComputedStyle(document.documentElement).getPropertyValue(n.replace('var(','').replace(')','')).trim(); }
    function allowDrop(ev) { ev.preventDefault(); }
    function drag(ev) { ev.dataTransfer.setData("text", ev.target.innerText); }
    function drop(ev) { ev.preventDefault(); const emoji = ev.dataTransfer.getData("text"); const rect = zoomLayer.getBoundingClientRect(); spawnSticker(emoji, (ev.clientX - rect.left) / scale - 25, (ev.clientY - rect.top) / scale - 25); }

    async function pasteFromClipboard() {
        try {
            const items = await navigator.clipboard.read();
            for (const item of items) {
                for (const type of item.types) {
                    if (type.startsWith('image/')) {
                        const blob = await item.getType(type); const r = new FileReader();
                        r.onload = (e) => { spawnImage(e.target.result); saveState(); }; r.readAsDataURL(blob); return;
                    }
                }
            }
        } catch (err) { alert("Gebruik Ctrl+V of geef toestemming voor het klembord."); }
    }
    
    setInterval(async () => { if (currentFileHandle) await saveMapData(false); }, 60000);
    function showSaveReminder() { const bubble = document.getElementById('save-reminder-bubble'); if (bubble) { bubble.style.display = 'block'; setTimeout(() => { bubble.style.display = 'none'; }, 5000); } }
    setInterval(showSaveReminder, 300000);
    window.onresize = () => { applyTransform(); updateLines(); };
</script>
</body>
</html>
