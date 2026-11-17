<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Urban City Living - Responsive Design</title>
    <!-- jQuery & jQuery UI (Required for Drag-and-Drop Sorting) -->
    <script src="https://code.jquery.com/jquery-3.7.1.min.js"></script>
    <script src="https://code.jquery.com/ui/1.13.3/jquery-ui.min.js"></script>
    <style>
        /* Base Styles (Desktop and Larger Screens) */
        body {
            font-family: Constantia, Georgia, serif;
            max-width: 1024px;
            width: 100%;
            margin: 0 auto;
            padding: 20px;
            background-color: #f5f5f5;
            box-sizing: border-box;
            font-size: 16px;
            line-height: 1.6;
            color: #333333;
            transition: background 0.3s ease, color 0.3s ease;
        }
        /* Neon Glow Heading */
        h1 {
            text-align: center;
            font-size: 42pt;
            margin-bottom: 20px;
            color: #2c3e50;
            text-shadow:
                0 0 6px #E033FF,
                0 0 12px #E033FF,
                0 0 24px #E033FF;
        }
        h2 {
            color: #1a252f;
            margin-top: 30px;
        }
        /* Subtitle */
        .subtitle {
            text-align: center;
            font-size: 1.1rem;
            color: #555555;
            margin-bottom: 25px;
        }
        /* Story Image Styling */
        .story-image {
            display: block;
            width: 100%;
            max-height: 450px;
            object-fit: cover;
            border: 2px solid #E033FF;
            border-radius: 6px;
            margin: 25px auto;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
        }
        /* Interactive Controls Toolbar */
        .js-controls {
            margin: 25px 0;
            display: flex;
            justify-content: center;
            gap: 12px;
            flex-wrap: wrap;
        }
        .js-controls button {
            padding: 10px 18px;
            font-family: Constantia, Georgia, serif;
            font-size: 1rem;
            cursor: pointer;
            background-color: #2c3e50;
            color: #ffffff;
            border: 1px solid #E033FF;
            border-radius: 4px;
            transition: all 0.2s ease;
        }
        .js-controls button:hover {
            background-color: #E033FF;
            color: #000000;
            box-shadow: 0 0 10px #E033FF;
        }
        .content-section {
            border-left: 3px solid #E033FF;
            padding-left: 18px;
            margin-top: 30px;
            margin-bottom: 30px;
        }
        .drag-hint {
            font-style: italic;
            color: #666666;
            font-size: 0.95rem;
            margin-bottom: 15px;
        }
        /* Sortable Ranked Boxes Layout */
        #sortable-ranks {
            display: flex;
            flex-direction: column;
            gap: 18px;
            margin-top: 15px;
        }
        .rank-card {
            background-color: #ffffff;
            border: 1px solid #ddd;
            border-left: 5px solid #E033FF;
            border-radius: 6px;
            padding: 20px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            cursor: grab;
            transition: box-shadow 0.2s ease, border-color 0.2s ease;
            user-select: none;
        }
        .rank-card:hover {
            box-shadow: 0 4px 14px rgba(224, 51, 255, 0.25);
        }
        .rank-card:active {
            cursor: grabbing;
        }
        /* First item gets the Top #1 highlight styling automatically */
        #sortable-ranks .rank-card:first-child {
            background: linear-gradient(135deg, #ffffff 0px, #fdf8ff 100%);
            border-top: 2px solid #E033FF;
        }
        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        .rank-number {
            display: inline-block;
            background-color: #E033FF;
            color: #ffffff;
            font-weight: bold;
            padding: 4px 12px;
            border-radius: 4px;
            font-size: 0.9rem;
        }
        .drag-handle {
            color: #888888;
            font-size: 0.9rem;
            letter-spacing: 1px;
        }
        .rank-card h3 {
            margin-top: 0;
            color: #2c3e50;
        }
        /* Dashed placeholder outline while dragging */
        .drag-placeholder {
            border: 2px dashed #E033FF;
            background-color: rgba(224, 51, 255, 0.08);
            border-radius: 6px;
            height: 120px;
        }
        /* High Contrast Mode Overrides */
        body.high-contrast {
            background-color: #000000 !important;
            color: #ffffff !important;
        }
        body.high-contrast h1 {
            color: #ffffff !important;
        }
        body.high-contrast h2, body.high-contrast h3 {
            color: #ffff00 !important;
        }
        body.high-contrast .subtitle, body.high-contrast .drag-hint {
            color: #cccccc !important;
        }
        body.high-contrast button {
            background-color: #ffff00;
            color: #000000;
            border-color: #ffffff;
            font-weight: bold;
        }
        body.high-contrast .story-image {
            border-color: #ffff00;
        }
        body.high-contrast .rank-card {
            background: #1a1a1a !important;
            border-color: #444 !important;
            border-left-color: #ffff00 !important;
            color: #ffffff !important;
        }
        body.high-contrast .rank-number {
            background-color: #ffff00;
            color: #000000;
        }
        body.high-contrast .drag-placeholder {
            border-color: #ffff00;
            background-color: rgba(255, 255, 0, 0.1);
        }
    </style>
</head>
<body>
    <!-- Neon Glow Heading -->
    <h1>Urban City Living</h1>
    <p class="subtitle">Exploring modern architectural aesthetics, cityscapes, and responsive design.</p>
    <!-- Interactive Controls Toolbar -->
    <div class="js-controls">
        <button id="contrast-btn" onclick="toggleContrast()">High Contrast</button>
        <button id="font-btn" onclick="increaseFontSize()">Increase Text Size</button>
        <button id="print-btn" onclick="printPage()">Print Page</button>
    </div>
    <div class="content-section">
        <h2>Modern Metropolitan Spaces</h2>
        <p>Use the interactive controls above to switch into high-contrast mode for better legibility, adjust the base typography sizing, or print this layout directly from your browser.</p>
    </div>
    <!-- Image Placed Underneath the Text -->
    <img src="IMG_3209.jpeg" alt="Moon and clouds" class="story-image">
    <!-- Interactive Draggable Ranked Boxes -->
    <div class="content-section">
        <h2>Top 3 Best Parts of Visiting Cities</h2>
        <p class="drag-hint">Click and drag any box up or down to change its ranking order:</p>
        <div id="sortable-ranks">
            <!-- Box 1 -->
            <div class="rank-card">
                <div class="card-header">
                    <span class="rank-number">Rank #1</span>
                    <span class="drag-handle">&#9776; Drag to Reorder</span>
                </div>
                <h3>Architectural Diversity & Historic Landmarks</h3>
                <p>Standing between centuries-old cathedrals and towering modern glass skyscrapers captures the true evolution of human culture and design.</p>
            </div>
            <!-- Box 2 -->
            <div class="rank-card">
                <div class="card-header">
                    <span class="rank-number">Rank #2</span>
                    <span class="drag-handle">&#9776; Drag to Reorder</span>
                </div>
                <h3>Local Food Culture & Street Markets</h3>
                <p>Tasting authentic regional dishes and exploring bustling local food districts gives you an immediate window into the community's heart and soul.</p>
            </div>
            <!-- Box 3 -->
            <div class="rank-card">
                <div class="card-header">
                    <span class="rank-number">Rank #3</span>
                    <span class="drag-handle">&#9776; Drag to Reorder</span>
                </div>
                <h3>Vibrant Street Life & Public Spaces</h3>
                <p>People-watching in lively plazas, parks, and busy pedestrian avenues reveals the pulse, energy, and daily rhythm of urban residents.</p>
            </div>
        </div>
    </div>
    <!-- JavaScript & jQuery Implementation -->
    <script>
        // 1. jQuery UI Sortable Drag-and-Drop Ranking
        $(document).ready(function() {
            $("#sortable-ranks").sortable({
                placeholder: "drag-placeholder",
                cursor: "grabbing",
                update: function(event, ui) {
                    // Automatically updates Rank #1, #2, #3 numbers after dropping
                    $("#sortable-ranks .rank-card").each(function(index) {
                        $(this).find(".rank-number").text("Rank #" + (index + 1));
                    });
                }
            });
        });
        // 2. High Contrast Mode Toggle
        function toggleContrast() {
            document.body.classList.toggle("high-contrast");
            const contrastBtn = document.getElementById("contrast-btn");
            if (document.body.classList.contains("high-contrast")) {
                contrastBtn.textContent = "Standard Contrast";
            } else {
                contrastBtn.textContent = "High Contrast";
            }
        }
        // 3. Font Size Increaser (Accessibility)
        let currentFontSize = 16;
        function increaseFontSize() {
            currentFontSize += 2;
            if (currentFontSize > 22) {
                currentFontSize = 16;
            }
            document.body.style.fontSize = currentFontSize + "px";
            const fontBtn = document.getElementById("font-btn");
            if (currentFontSize === 16) {
                fontBtn.textContent = "Increase Text Size";
            } else {
                fontBtn.textContent = "Text Size: " + currentFontSize + "px";
            }
        }
        // 4. Print Page Function
        function printPage() {
            window.print();
        }
    </script>

</body>
</html>
