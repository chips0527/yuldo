<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>기숙사 배정</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 20px;
    }
    label {
      display: block;
      margin: 10px 0 5px;
    }
    select, input[type="date"], input[type="text"] {
      padding: 5px;
      width: 200px;
    }
    button {
      margin-top: 20px;
      padding: 10px 20px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #45a049;
    }
    .result {
      margin-top: 20px;
      font-size: 18px;
      padding: 20px;
      text-align: center;
    }
    .brown { background-color: brown; color: white; }
    .blue { background-color: blue; color: white; }
    .orange { background-color: orange; color: white; }
    .green { background-color: green; color: white; }
    .gray { background-color: gray; color: white; }
  </style>
</head>
<body>
  <h1>일간 오행에 따른 기숙사 배정</h1>
  
  <form id="saju-form">
    <label for="name">이름:</label>
    <input type="text" id="name" required>

    <label for="city">출신 도시:</label>
    <input type="text" id="city" required>
    
    <label for="birthdate">생년월일:</label>
    <input type="date" id="birthdate" required>
    
    <button type="button" onclick="assignDorm()">배정하기</button>
  </form>
  
  <div class="result" id="result"></div>

  <script>
    function assignDorm() {
      const name = document.getElementById('name').value;
      const city = document.getElementById('city').value;
      const birthdate = document.getElementById('birthdate').value;
      const resultElement = document.getElementById('result');
      
      if (!name || !city || !birthdate) {
        resultElement.textContent = '모든 정보를 입력해주세요.';
        return;
      }

      const year = new Date(birthdate).getFullYear();
      const dayGanJi = calculateDayGanJi(birthdate);  // 날짜를 기준으로 일간 추출

      // 일간에 따른 오행 계산
      const ohaeng = calculateOhaeng(dayGanJi);

      // 기숙사 배정
      let dormAssignment = '';
      let dormClass = '';
      switch(ohaeng) {
        case 'wood':
          dormAssignment = '청양관';
          dormClass = 'green';  // 초록색
          break;
        case 'fire':
          dormAssignment = '적초관';
          dormClass = 'orange';  // 주황색
          break;
        case 'earth':
          dormAssignment = '황접관';
          dormClass = 'brown';  // 갈색
          break;
        case 'metal':
          dormAssignment = '백작관';
          dormClass = 'gray';  // 회색
          break;
        case 'water':
          dormAssignment = '흑상관';
          dormClass = 'blue';  // 파란색
          break;
        default:
          dormAssignment = '기숙사 배정 불가';
          dormClass = ''; // 기본값
          break;
      }

      resultElement.className = dormClass;
      resultElement.textContent = `축하합니다. ${name}님의 기숙사는 ${dormAssignment}입니다. 반장들의 안내에 따라 질서정연하게 이동해주시기 바랍니다.`;
    }

    // 생일을 기반으로 일간 계산 (이 부분에서 문제 해결)
    function calculateDayGanJi(birthdate) {
      const ganji = ['갑', '을', '병', '정', '무', '기', '경', '신', '임', '계'];
      const date = new Date(birthdate);
      const dayOfYear = date.getDate() + (date.getMonth() * 30);  // 대략적인 월별 일수를 더함
      const dayGanJi = ganji[dayOfYear % 10];  // 일간은 날짜를 기준으로 계산
      return dayGanJi;
    }

    // 천간에 따른 오행 계산
    function calculateOhaeng(dayGanJi) {
      switch(dayGanJi) {
        case '갑':
        case '을':
          return 'wood';  // 목
        case '병':
        case '정':
          return 'fire';  // 화
        case '무':
        case '기':
          return 'earth'; // 토
        case '경':
        case '신':
          return 'metal'; // 금
        case '임':
        case '계':
          return 'water'; // 수
        default:
          return 'unknown';
      }
    }
  </script>
</body>
</html>
