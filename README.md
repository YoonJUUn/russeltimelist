<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>러셀 이동일지 작성</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
        }

        h1 {
            color: #336699;
        }

        form {
            display: inline-block;
            text-align: left;
            border: 1px solid #ccc;
            padding: 10px;
        }

        label {
            display: block;
            margin-bottom: 5px;
        }

        select {
            width: 100%;
            padding: 5px;
            margin-bottom: 10px;
        }

        button {
            padding: 8px 16px;
            background-color: #336699;
            color: #fff;
            border: none;
            cursor: pointer;
        }

        #resetButton {
            margin-bottom: 10px;
            float: left;
        }

        table {
            margin: 20px 0;
        }
    </style>
</head>
<body>
    <h1>러셀 이동일지 작성</h1>
    <button id="resetButton" onclick="resetTable()">테이블 초기화</button>
    <form id="moveForm">
        <label for="building">관:</label>
        <select id="building" required>
            <option value="">선택하세요</option>
            <option value="3-1관">3-1관</option>
            <option value="3-2관">3-2관</option>
            <option value="4-1관">4-1관</option>
            <option value="5-1관">5-1관</option>
            <option value="5-2관">5-2관</option>
            <option value="5-3관">5-3관</option>
            <option value="6-1관">6-1관</option>
            <option value="6-2관">6-2관</option>
            <option value="7-1관">7-1관</option>
        </select><br>
        
        <label for="seatNumber">좌석번호:</label>
        <select id="seatNumber" required>
            <option value="">선택하세요</option>
            <optgroup label="A~I">
                <option value="A">A</option>
                <option value="B">B</option>
                <option value="C">C</option>
                <option value="D">D</option>
                <option value="F">F</option>
                <option value="G">G</option>
                <option value="H">H</option>
                <option value="I">I</option>
                <option value="J">J</option>
                <option value="K">K</option>
                <option value="L">L</option>
                <option value="M">M</option>
            </optgroup>
        </select>

        <select id="seatNumber2" required>
            <option value="">선택하세요</option>
            <optgroup label="1~40">
                <option value="01">01</option>
                <option value="02">02</option>
                <option value="03">03</option>
                <option value="04">04</option>
                <option value="05">05</option>
                <option value="06">06</option>
                <option value="07">07</option>
                <option value="08">08</option>
                <option value="09">09</option>
                <option value="10">10</option>
                <option value="11">11</option>
                <option value="12">12</option>
                <option value="13">13</option>
                <option value="14">14</option>
                <option value="15">15</option>
                <option value="16">16</option>
                <option value="17">17</option>
                <option value="18">18</option>
                <option value="19">19</option>
                <option value="20">20</option>
                <option value="21">21</option>
                <option value="22">22</option>
                <option value="23">23</option>
                <option value="24">24</option>
                <option value="25">25</option>
                <option value="26">26</option>
                <option value="27">27</option>
                <option value="28">28</option>
                <option value="29">29</option>
                <option value="30">30</option>
                <option value="31">31</option>
                <option value="32">32</option>
                <option value="33">33</option>
                <option value="34">34</option>
                <option value="35">35</option>
                <option value="36">36</option>
                <option value="37">37</option>
                <option value="38">38</option>
            </optgroup>
        </select><br>

        <label for="returnTime">복귀교시:</label>
        <select id="returnTime" required>
            <option value="">선택하세요</option>
            <option value="1">1교시</option>
            <option value="2">2교시</option>
            <option value="3">3교시</option>
            <option value="4">4교시</option>
            <option value="5">5교시</option>
            <option value="6">6교시</option>
            <option value="7">7교시</option>
            <option value="8">8교시</option>
            <option value="복귀안함">복귀안함</option>
        </select><br>

        <label for="reason">이동사유:</label>
        <select id="reason" required>
            <option value="">선택하세요</option>
            <option value="화장실/정수기">화장실/정수기</option>
            <option value="튜터 질의응답">튜터 질의응답</option>
            <option value="학원 관련 외출">학원 관련 외출</option>
            <option value="개인(가족 행사)">개인(가족 행사)</option>
            <option value="병원">병원</option>
            <option value="식사">식사</option>
            <option value="조퇴(컨디션난조)">조퇴(컨디션난조)</option>
        </select><br>

        <button type="submit">제출</button>
    </form>

    <div id="result">
        <table id="moveList" border="1">
            <thead>
                <tr>
                    <th>관</th>
                    <th>좌석번호</th>
                    <th>복귀교시</th>
                    <th>이동사유</th>
                    <th>입력시간</th>
                </tr>
            </thead>
            <tbody>
            </tbody>
        </table>
    </div>
      <script>
   <script>
    let moveList = [];

    function refreshMoveList() {
        const tableBody = document.querySelector("#moveList tbody");
        tableBody.innerHTML = "";

        moveList.forEach((moveData) => {
            const row = document.createElement("tr");
            row.innerHTML = `
                <td>${moveData.building}</td>
                <td>${moveData.seatNumber}</td>
                <td>${moveData.returnTime}교시</td>
                <td>${moveData.reason}</td>
                <td>${moveData.inputDateTime}</td>
            `;
            tableBody.appendChild(row);
        });
    }

    document.getElementById("moveForm").addEventListener("submit", function (event) {
        event.preventDefault();

        const building = document.getElementById("building").value;
        const seatNumber = document.getElementById("seatNumber").value + document.getElementById("seatNumber2").value;
        const returnTime = document.getElementById("returnTime").value;
        const reason = document.getElementById("reason").value;

        const moveData = {
            building: building,
            seatNumber: seatNumber,
            returnTime: returnTime,
            reason: reason
        };

        const now = new Date();
        const year = now.getFullYear();
        const month = now.getMonth() + 1;
        const day = now.getDate();
        const hours = now.getHours();
        const minutes = now.getMinutes();

        const formattedDate = `${year}년 ${month}월 ${day}일`;
        const formattedTime = `${hours}시 ${minutes}분`;

        moveData.inputDateTime = `${formattedDate} ${formattedTime}`;

        moveList.push(moveData);

        refreshMoveList();

        document.getElementById("moveForm").reset();

        // POST request를 보냄
        fetch('https://script.google.com/macros/s/AKfycbwdADDtCDRLv4STiNDe-1ElbYmhRnPFbfmvx-4SqB4xEWuVHS3ySqfx_1T927-mc0MaPA/exec', {
            method: 'POST',
            body: JSON.stringify(moveData)
        });
    });

    function resetTable() {
        moveList = [];
        refreshMoveList();
    }
</script>
</body>
</html>
