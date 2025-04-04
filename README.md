<!DOCTYPE html>
<html lang="zh-HK">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>香港小食對對碰</title>
<style>

        body {

            font-family: 'Noto Sans HK', sans-serif;

            background-color: #fff4e6;

            margin: 0;

            padding: 20px;

            display: flex;

            justify-content: center;

            align-items: center;

            min-height: 100vh;

            background-image: url('https://i.imgur.com/JKjQb0a.png');

            background-size: cover;

            background-attachment: fixed;

        }

        .game-container {

            background-color: rgba(255, 255, 255, 0.9);

            border-radius: 15px;

            padding: 30px;

            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);

            text-align: center;

            max-width: 600px;

            width: 100%;

            border: 3px solid #e74c3c;

        }

        h1 {

            color: #e74c3c;

            margin-bottom: 20px;

            font-size: 2.5em;

            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);

            background-color: #ffd700;

            padding: 10px;

            border-radius: 10px;

            border: 2px dashed #e74c3c;

        }

        .controls {

            margin-bottom: 20px;

            display: flex;

            justify-content: space-between;

            align-items: center;

            flex-wrap: wrap;

            gap: 10px;

        }

        button {

            background-color: #e74c3c;

            color: white;

            border: none;

            padding: 10px 20px;

            border-radius: 50px;

            font-family: 'Noto Sans HK', sans-serif;

            font-size: 1em;

            cursor: pointer;

            transition: all 0.3s;

            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);

        }

        button:hover {

            background-color: #c0392b;

            transform: translateY(-2px);

        }

        .score {

            background-color: #3498db;

            padding: 10px 20px;

            border-radius: 50px;

            font-size: 1.2em;

            color: white;

        }

        .game-board {

            display: grid;

            grid-template-columns: repeat(4, 1fr);

            gap: 15px;

            margin: 0 auto;

            max-width: 500px;

        }

        .card {

            aspect-ratio: 1/1;

            background-color: #ffd700;

            border-radius: 15px;

            display: flex;

            justify-content: center;

            align-items: center;

            cursor: pointer;

            transition: all 0.3s;

            position: relative;

            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);

            font-size: 40px;

            color: #e74c3c;

        }

        .card::before {

            content: "?";

            position: absolute;

            font-size: 50px;

        }

        .card.flipped {

            background-color: white;

        }

        .card.flipped::before {

            content: none;

        }

        .card.matched {

            background-color: #2ecc71;

            cursor: default;

        }

        .card img {

            width: 80%;

            height: 80%;

            object-fit: contain;

            display: none;

        }

        .card.flipped img, .card.matched img {

            display: block;

        }

        .image-selector {

            margin-top: 30px;

            padding: 20px;

            background-color: #f0f9ff;

            border-radius: 15px;

        }

        .image-options {

            display: grid;

            grid-template-columns: repeat(4, 1fr);

            gap: 10px;

            margin: 20px 0;

        }

        .image-option {

            aspect-ratio: 1/1;

            background-color: white;

            border-radius: 10px;

            display: flex;

            justify-content: center;

            align-items: center;

            cursor: pointer;

            border: 3px solid transparent;

            transition: all 0.2s;

        }

        .image-option:hover {

            transform: scale(1.05);

        }

        .image-option.selected {

            border-color: #e74c3c;

        }

        .image-option img {

            width: 80%;

            height: 80%;

            object-fit: contain;

        }

        .uploader {

            margin-top: 30px;

            padding: 20px;

            background-color: #f0f9ff;

            border-radius: 15px;

        }

        #file-input {

            display: none;

        }

        .upload-preview {

            display: grid;

            grid-template-columns: repeat(4, 1fr);

            gap: 10px;

            margin: 20px 0;

        }

        .upload-preview-item {

            aspect-ratio: 1/1;

            background-color: white;

            border-radius: 10px;

            display: flex;

            justify-content: center;

            align-items: center;

            overflow: hidden;

            position: relative;

        }

        .upload-preview-item img {

            width: 100%;

            height: 100%;

            object-fit: cover;

        }

        .upload-preview-item .remove-btn {

            position: absolute;

            top: 5px;

            right: 5px;

            background-color: red;

            color: white;

            border: none;

            border-radius: 50%;

            width: 20px;

            height: 20px;

            font-size: 12px;

            cursor: pointer;

        }

        @media (max-width: 600px) {

            .game-board {

                grid-template-columns: repeat(2, 1fr);

            }

            .image-options {

                grid-template-columns: repeat(2, 1fr);

            }

            .upload-preview {

                grid-template-columns: repeat(2, 1fr);

            }

        }
</style>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+HK:wght@700&display=swap" rel="stylesheet">
</head>
<body>
<div class="game-container">
<h1>🍡 香港小食對對碰 🥟</h1>
<div class="controls">
<button id="reset-btn">重新開始</button>
<button id="change-images-btn">更換小食</button>
<button id="upload-images-btn">上傳圖片</button>
<div class="score">配對成功: <span id="matches">0</span>/4</div>
</div>
<div class="game-board" id="game-board"></div>
<div class="image-selector" id="image-selector" style="display:none;">
<h3>選擇小食圖片</h3>
<div class="image-options"></div>
<button id="confirm-images-btn">確認選擇</button>
</div>
<div class="uploader" id="uploader" style="display:none;">
<h3>上傳自訂圖片</h3>
<input type="file" id="file-input" accept="image/*" multiple>
<div class="upload-preview"></div>
<button id="confirm-upload-btn">使用這些圖片</button>
</div>
</div>
<script>

        document.addEventListener('DOMContentLoaded', () => {

            // 香港小食圖片URL

            const defaultFoodImages = [

                'https://i.imgur.com/5XJgZ9U.jpg', // 雞蛋仔

                'https://i.imgur.com/8Kj3L7f.jpg', // 格仔餅

                'https://i.imgur.com/3QjvR9T.jpg', // 魚蛋

                'https://i.imgur.com/9ZLXp1Y.jpg', // 燒賣

                'https://i.imgur.com/2Wmz5qN.jpg', // 蛋撻

                'https://i.imgur.com/7Hk9JjP.jpg', // 腸粉

                'https://i.imgur.com/1MvLQ3Z.jpg', // 碗仔翅

                'https://i.imgur.com/4DkL9yG.jpg'  // 煎釀三寶

            ];

            let foodImages = [...defaultFoodImages];

            let selectedImages = [...defaultFoodImages.slice(0, 4), ...defaultFoodImages.slice(0, 4)];

            let flippedCards = [];

            let matchedPairs = 0;

            let lockBoard = false;

            let uploadedImages = [];

            const gameBoard = document.getElementById('game-board');

            const matchesDisplay = document.getElementById('matches');

            const resetBtn = document.getElementById('reset-btn');

            const changeImagesBtn = document.getElementById('change-images-btn');

            const uploadImagesBtn = document.getElementById('upload-images-btn');

            const imageSelector = document.getElementById('image-selector');

            const imageOptionsContainer = document.querySelector('.image-options');

            const confirmImagesBtn = document.getElementById('confirm-images-btn');

            const uploader = document.getElementById('uploader');

            const fileInput = document.getElementById('file-input');

            const uploadPreview = document.querySelector('.upload-preview');

            const confirmUploadBtn = document.getElementById('confirm-upload-btn');

            // 初始化遊戲

            function initGame() {

                gameBoard.innerHTML = '';

                flippedCards = [];

                matchedPairs = 0;

                matchesDisplay.textContent = '0';

                lockBoard = false;

                // 洗牌

                const shuffledImages = shuffleArray([...selectedImages]);

                // 創建卡片

                shuffledImages.forEach((image, index) => {

                    const card = document.createElement('div');

                    card.classList.add('card');

                    card.dataset.index = index;

                    card.dataset.image = image;

                    const img = document.createElement('img');

                    img.src = image;

                    img.alt = "香港小食圖片";

                    card.appendChild(img);

                    card.addEventListener('click', flipCard);

                    gameBoard.appendChild(card);

                });

            }

            // 翻牌功能

            function flipCard() {

                if (lockBoard) return;

                if (this === flippedCards[0]) return;

                if (this.classList.contains('matched')) return;

                this.classList.add('flipped');

                flippedCards.push(this);

                if (flippedCards.length === 2) {

                    checkForMatch();

                }

            } 
// 檢查是否匹配

            function checkForMatch() {

                const card1 = flippedCards[0];

                const card2 = flippedCards[1];

                const isMatch = card1.dataset.image === card2.dataset.image;

                lockBoard = true;

                if (isMatch) {

                    card1.classList.add('matched');

                    card2.classList.add('matched');

                    matchedPairs++;

                    matchesDisplay.textContent = matchedPairs;

                    flippedCards = [];

                    lockBoard = false;

                    // 檢查遊戲是否結束

                    if (matchedPairs === 4) {

                        setTimeout(() => {

                            alert('恭喜你贏了！所有小食都配對成功！');

                        }, 500);

                    }

                } else {

                    setTimeout(() => {

                        card1.classList.remove('flipped');

                        card2.classList.remove('flipped');

                        flippedCards = [];

                        lockBoard = false;

                    }, 1000);

                }

            }

            // 洗牌函數

            function shuffleArray(array) {

                for (let i = array.length - 1; i > 0; i--) {

                    const j = Math.floor(Math.random() * (i + 1));

                    [array[i], array[j]] = [array[j], array[i]];

                }

                return array;

            }

            // 重置遊戲

            resetBtn.addEventListener('click', initGame);

            // 更換圖片功能

            changeImagesBtn.addEventListener('click', () => {

                imageSelector.style.display = 'block';

                uploader.style.display = 'none';

                populateImageOptions();

            });

            // 上傳圖片功能

            uploadImagesBtn.addEventListener('click', () => {

                uploader.style.display = 'block';

                imageSelector.style.display = 'none';

                fileInput.click();

            });

            // 處理文件上傳

            fileInput.addEventListener('change', handleFileSelect);

            function handleFileSelect(event) {

                const files = event.target.files;

                uploadPreview.innerHTML = '';

                uploadedImages = [];

                if (files.length < 4) {

                    alert('請至少選擇4張圖片！');

                    return;

                }

                // 只取前8張圖片

                const filesToProcess = Array.from(files).slice(0, 8);

                filesToProcess.forEach((file, index) => {

                    const reader = new FileReader();

                    reader.onload = function(e) {

                        const img = document.createElement('img');

                        img.src = e.target.result;

                        // 創建預覽項

                        const previewItem = document.createElement('div');

                        previewItem.classList.add('upload-preview-item');

                        // 添加刪除按鈕

                        const removeBtn = document.createElement('button');

                        removeBtn.classList.add('remove-btn');

                        removeBtn.innerHTML = '×';

                        removeBtn.addEventListener('click', () => {

                            previewItem.remove();

                            uploadedImages = uploadedImages.filter(img => img !== e.target.result);

                        });

                        previewItem.appendChild(img);

                        previewItem.appendChild(removeBtn);

                        uploadPreview.appendChild(previewItem);

                        uploadedImages.push(e.target.result);

                        // 更新食物圖片列表

                        if (index < 8) {

                            foodImages[index] = e.target.result;

                        }

                    };

                    reader.readAsDataURL(file);

                });

            }

            // 確認使用上傳的圖片

            confirmUploadBtn.addEventListener('click', () => {

                if (uploadedImages.length < 4) {

                    alert('請至少上傳4張圖片！');

                    return;

                }

                // 更新選中的圖片

                const newSelected = [];

                uploadedImages.forEach((image, index) => {

                    if (index < 4) {

                        newSelected.push(image);

                    }

                });

                selectedImages = [...newSelected, ...newSelected];

                uploader.style.display = 'none';

                initGame();

            });

            // 填充圖片選項

            function populateImageOptions() {

                imageOptionsContainer.innerHTML = '';

                foodImages.forEach((image, index) => {

                    const option = document.createElement('div');

                    option.classList.add('image-option');

                    if (selectedImages.includes(image)) {

                        option.classList.add('selected');

                    }

                    const img = document.createElement('img');

                    img.src = image;

                    img.alt = "小食選項";

                    option.appendChild(img);

                    option.addEventListener('click', () => {

                        option.classList.toggle('selected');

                    });

                    imageOptionsContainer.appendChild(option);

                });

            }

            // 確認圖片選擇

            confirmImagesBtn.addEventListener('click', () => {

                const selectedOptions = document.querySelectorAll('.image-option.selected');

                if (selectedOptions.length < 4) {

                    alert('請至少選擇4種不同的小食！');

                    return;

                }

                // 更新選中的圖片

                const newSelected = [];

                selectedOptions.forEach((option, index) => {

                    if (index < 4) {

                        const imgSrc = option.querySelector('img').src;

                        newSelected.push(imgSrc);

                    }

                });

                selectedImages = [...newSelected, ...newSelected];

                imageSelector.style.display = 'none';

                initGame();

            });

            // 初始化遊戲

            initGame();

        });
</script>
</body>
</html> 
