<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>오늘의 노래는?</title>
    <style>
        body {
            font-family: 'Comic Sans MS', 'Arial', sans-serif;
            background-color: #D6EAF8;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            padding: 0;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 20px; 
            box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2); 
            text-align: center;
            max-width: 400px;
            width: 100%;
        }
        h1 {
            color: #000000; 
        }
        h3 {
            color: #434343; 
        }
        label {
            display: block;
            margin-top: 15px;
            font-weight: bold;
            color: #34495E;
        }
        select, button {
            margin-top: 10px;
            padding: 10px;
            width: 100%;
            border: 1px solid #AAB7B8;
            border-radius: 10px;
            box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
            font-size: 16px;
            outline: none;
        }
        select:hover, button:hover {
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }
        button {
            background-color: #85C1E9;
            color: white;
            font-size: 18px;
            cursor: pointer;
            font-weight: bold;
        }
        button:hover {
            background-color: #5DADE2; 
        }
        .result {
            margin-top: 20px;
            font-size: 16px;
            color: #212F3C;
        }
        .song-box {
            background: #E5E8E8; 
            padding: 20px;
            border-radius: 10px;
            text-align: center; 
            margin-top: 20px;
        }
        .song-box h1 {
            font-size: 25px;
            color: #34495E;
            margin: 0 0 10px 0;
        }
        .lyrics {
            color: #58a9df; 
            font-style: italic;
            font-size: 14px;
            margin: 10px 0;
        }
        .ment {
            background: #f9f9f9;
            color: #707070; 
            font-size: 14px;
            border-radius: 5px;
            margin-top: 10px;
            font-style: italic;
            line-height: 2; 
        }
        .player {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-top: 10px;
        }
        .progress-bar {
            position: relative;
            flex: 1;
            height: 5px;
            background: #D5DBDB;
            border-radius: 5px;
            margin: 0 10px;
            overflow: hidden;
        }
        .progress {
            position: absolute;
            height: 100%;
            width: 30%; 
            background: #85C1E9;
        }
        .buttons {
            display: flex;
            gap: 10px;
            justify-content: center;
            margin-top: 10px;
        }
        .buttons span {
            cursor: pointer;
            color: #5DADE2;
            font-weight: bold;
        }
        .buttons span:hover {
            color: #3498DB;
        }
        
    </style>
</head>
<body>
    <div class="container">
        <h1>오늘의 루시노래 🎵</h1>
        <label for="weather">오늘의 날씨는?:</label>
        <select id="weather">
            <option value="">선택하세요</option>
            <option value="맑음">맑음</option>
            <option value="흐림">흐림</option>
            <option value="비">비</option>
            <option value="눈">눈</option>
        </select>

        <label for="mood">오늘의 기분은?:</label>
        <select id="mood">
            <option value="">선택하세요</option>
            <option value="행복">행복</option>
            <option value="우울">우울</option>
            <option value="피곤">피곤</option>
            <option value="불안">불안</option>
        </select>

        <button onclick="recommend()">추천 받기</button>

        <div class="result" id="result"></div>
    </div>

    <script>
        const data = {
            "맑음행복": { song: "21세기의 어떤 날", lyrics: "'오늘 지금 바로 여기 이 멋진 우주 한복판에서<br>너를 만나 정말 기뻤다.'", ment: "오늘은 정말 화사한 날씨네요!<br>당신의 행복이 이 하늘보다 밝게 빛나길 바라요."},
            "맑음우울": { song: "날아올라", lyrics: "'저 파란 구름 위에 그려가 늘 같은 곳에서<br>쉼 없이 너를 그려.'" , ment: "맑은 날의 푸른 하늘처럼,  당신의 마음에도<br>희망과 자유로움이 넘쳐흐르길 바랄게요."},
            "맑음피곤": { song: "조깅", lyrics: "'너는 빛보다 밝게 빛나 급하게 가지 마,<br>그렇게 머물러줘.'" , ment: "당신은 빛보다 더 밝게 빛나는 존재예요.<br>서둘지 말고 조금 쉬어가며<br>그 자리에서 머물러 보는 건 어떨까요?"},
            "맑음불안": { song: "Knowhow", lyrics: "'맘대로 생각하라 해, 너는 늘 아름다워.'" , ment: "불안한 마음을 떠나 자유롭게 마음을 따라가 봐요. 당신은 그 자체로 빛나고 아름다운 존재이니까요."},
            "흐림행복": { song: "mp3", lyrics: "'난 너의 세상을 함께 듣고 싶어<br>네 눈에 담은 걸 같이 보고 싶어.'" , ment: "흐린 날에도 순간순간 스치는 그 행복한 에너지가 마치 햇살처럼 세상을 비추고 있어요.<br>당신이 웃을 때 함께 웃을 수 있게 해줘서 고마워요."},
            "흐림우울": { song: "Opening", lyrics: "'때론 지쳐 주저앉겠지만 결코 포기하지 않아<br>봐 저 반짝이는 날들을.'" , ment: "어떤 순간에도 자신을 포기하지 마세요. <br>반짝이는 날들이 당신을 기다리고 있을 테니,<br>조금씩 앞으로 나아가 봐요."},
            "흐림피곤": { song: "아니근데 진짜", lyrics: "'새벽을 깨우는 환한 햇살 같아.'" , ment: "햇살 같은 당신을 감싸 안아줄게요.<br>지친 순간에도 미소를 잃지 말고,<br>앞으로 펼쳐질 더 밝고 희망찬 날들을 기대해 봐요."},
            "흐림불안": { song: "Ending", lyrics: "'우리 더 빛나자 이 순간이 영원할 것처럼.'" , ment: "흐린 날에도 함께 있는 우리만의 빛을 믿어봐요.<br>이 순간이 영원할 것처럼,<br>불안을 극복하며 더욱 빛나는 미래로 향해나아가요."},
            "비행복": { song: "히어로", lyrics: "'눈부셔 햇살을 닮아 환하게 웃어 주는 너.'" , ment: "당신의 햇살 같은 웃음 덕분에 비가 그치면<br>예쁜 무지개가 떠오를 거예요.<br>당신이 있는 곳은 언제나 희망과 아름다움으로<br>가득한 곳이니까요!"},
            "비우울": { song: "선잠", lyrics: "'But you don't have to be an adult<br>가끔은 철없게.'" , ment: "항상 마음이 어른일 필요는 없어요.<br>헤어나가기 어려운 순간이라면,<br>빗소리와 함께 잠깐의 휴식은 어때요?<br>힘들어도 괜찮아요."},
            "비피곤": { song: "내 쓸쓸함은 차갑지 않아요", lyrics: "'나를 쓱 지나가도 괜찮아요<br>나는 여기 있을게요.'" , ment: "비 오는 날 피곤한 당신에게 내 품을 빌려줄게요.<br>힘들땐 지나쳐도 괜찮아요.<br>나는 항상 여기 있으니까요."},
            "비불안": { song: "Sequel", lyrics: "'우리의 이야기 여기서 끝이라곤 생각하기 싫어.'" , ment: "비 오는 날에도 우리 함께 무지개를 찾아봐요.<br>함께 힘든 순간을 이겨내고,<br>행복한 순간을 만들어 나갈 우리만의 이야기를<br>계속 써내려 가요."},
            "눈행복": { song: "Flare", lyrics: "'수많은 우주가 택한 너야 그리고 난 널 담아둘게<br>너의 따뜻한 온기를 품고서 저 멀리서.'" , ment: "당신의 따듯한 온기로 겨울을 이겨내 봐요.<br>당신의 따듯한 행복은 추운겨울을 녹이고<br>따스한 봄을 기다리게해요."},
            "눈우울": { song: "난로", lyrics: "'눈꽃이 떨어지면 거리는 밝아지고<br>우리는 그 속에서 춤을 추게 될 거야.'" , ment: "밝은 거리 속 우리는 우울함을 뛰어넘고<br>희망을 찾게 될 거예요.<br>함께 춤을 추며 따뜻한 봄을 기대해 봐요"},
            "눈피곤": { song: "놀이", lyrics: "'다시 찾을게 우리가 떠나온 그날들을<br>넌 웃어줘 함께 놀던 그날처럼.'" , ment: "눈이 내리는 오늘처럼 당신의 웃음으로<br>아름답게 물들여줄게요.<br>피곤한 어깨를 내 품에 기대고,<br>함께 추억을 만들어가는<br>그 순간을 다시 기다릴게요."},
            "눈불안": { song: "이 밤을 잊지 말아요", lyrics: "'기억하고 싶지 않은 건 하지 말아요.<br>오늘의 기억들로 밤을 채워보아요.'", ment: "눈이 내리는 오늘,<br>불안한 마음을 한 장의 그림처럼 풀어내 보아요.<br>눈물을 녹여서 과거의 아픔을<br>세상에 흩뿌려 버리고,<br>오늘의 기억들로 달콤한 밤을 채워봐요."}
            };

        function recommend() {
            const weather = document.getElementById("weather").value;
            const mood = document.getElementById("mood").value;
            const resultDiv = document.getElementById("result");

            if (!weather || !mood) {
                resultDiv.innerHTML = "<p style='color: red;'>날씨와 기분을 모두 선택해주세요!</p>";
                return;
            }

            const key = weather + mood;
            const recommendation = data[key];

            if (recommendation) {
                resultDiv.innerHTML = `
                <blockquote class="ment">${recommendation.ment}</blockquote>
                    <div class="song-box">
                        <h1>${recommendation.song}</h1>
                        <blockquote class="lyrics"><h4>${recommendation.lyrics}</h4></blockquote>
                        <div class="player">
                            <span>1:23</span>
                            <div class="progress-bar">
                                <div class="progress"></div>
                            </div>
                            <span>4:00</span>
                        </div>
                        <div class="buttons">
                            <span>⇆</span>
                            <span>◁</span>
                            <span>❚❚</span>
                            <span>▷</span>
                            <span>↻</span>
                        </div>
                    </div>
                `;
            } else {
                resultDiv.innerHTML = "<p style='color: red;'>해당하는 추천 정보를 찾을 수 없습니다.</p>";
            }
        }
    </script>
</body>
</html>
