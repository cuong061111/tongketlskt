<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Khảo Sát Sản Phẩm NIVEA</title>
  <style>
    body { font-family: Arial; padding: 20px; background: #f9f9f9; }
    h1 { color: #005aaa; }
    button {
      display: block; margin: 10px 0; padding: 10px;
      border: none; background-color: #007bff; color: white;
      cursor: pointer; border-radius: 8px; width: 100%;
    }
    button:hover { background-color: #0056b3; }
    .message { margin-top: 20px; font-weight: bold; color: green; }
  </style>
</head>
<body>
  <h1>Chọn sản phẩm NIVEA bạn yêu thích nhất</h1>
  <div id="product-list"></div>
  <div class="message" id="message"></div>

  <script>
    const products = [
      "Sữa Dưỡng Thể NIVEA Sáng Da Ban Đêm từ 8 Super Food",
      "Sữa Dưỡng Thể NIVEA Phục Hồi & Chống Nắng Ban Ngày từ 8 Super Foods",
      "Serum Dưỡng Thể NIVEA Sáng Da Ban Đêm từ 8 Super Foods",
      "Xịt ngăn mùi Pearl&beauty",
      "Lăn ngăn mùi sáng mịn",
      "Son dưỡng chuyên sâu",
      "Nước tẩy trang ngừa mụn",
      "Lăn ngăn mùi Pearl&beauty",
      "Lăn Ngăn Mùi NIVEA Serum Sáng Mịn Hương Hoa Hồng Hokkaido",
      "Kem Dưỡng Mềm Da NIVEA Soft Crème",
      "Kem Dưỡng Ẩm Da NIVEA Crème",
      "Xịt Ngăn Mùi NIVEA Men Silver",
      "Xịt Ngăn Mùi NIVEA MEN Black&White",
      "Xịt Ngăn Mùi NIVEA MEN Deep Than Đen Hương Gỗ Đen",
      "Sữa Rửa Mặt NIVEA MEN Deep Than Đen Hương Gỗ Đen",
      "Sữa Rửa Mặt NIVEA MEN Bùn Khoáng Kiểm Soát Nhờn"
    ];

    const listDiv = document.getElementById('product-list');
    const message = document.getElementById('message');

    products.forEach(product => {
      const btn = document.createElement('button');
      btn.innerText = product;
      btn.onclick = () => submitSurvey(product);
      listDiv.appendChild(btn);
    });

    function submitSurvey(product) {
      fetch("https://script.google.com/macros/s/AKfycbxI2GslUKaAbqkzh2sLVJND3cSziyzETM7AKVnAlpMS9HHXoZL3M93r0POTUSYiCqv2/exec", {
        method: "POST",
        body: JSON.stringify({ product })
      }).then(res => res.text())
        .then(response => {
          message.innerText = "Cảm ơn bạn đã tham gia khảo sát!";
        })
        .catch(error => {
          message.innerText = "Đã xảy ra lỗi, vui lòng thử lại.";
        });
    }
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Kết quả khảo sát NIVEA</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body { font-family: Arial; padding: 20px; background: #f0f0f0; }
    canvas { background: #fff; border-radius: 12px; padding: 20px; }
    h1 { color: #005aaa; }
  </style>
</head>
<body>
  <h1>Kết quả khảo sát sản phẩm NIVEA</h1>
  <canvas id="chart" width="600" height="400"></canvas>

  <script>
    const sheetURL = 'https://docs.google.com/spreadsheets/d/1u_IIVX85xrPKdkglqJ95aPPiUqqGmu8dTdO_elHvCjs/gviz/tq?tqx=out:json';

    fetch(sheetURL)
      .then(res => res.text())
      .then(text => {
        const json = JSON.parse(text.substr(47).slice(0, -2));
        const rows = json.table.rows;
        const counts = {};

        rows.forEach(row => {
          const product = row.c[1]?.v;
          if (product) counts[product] = (counts[product] || 0) + 1;
        });

        const labels = Object.keys(counts);
        const values = Object.values(counts);

        const total = values.reduce((a, b) => a + b, 0);
        const percentages = values.map(val => ((val / total) * 100).toFixed(1));

        new Chart(document.getElementById("chart"), {
          type: 'bar',
          data: {
            labels: labels,
            datasets: [{
              label: "% người chọn",
              data: percentages,
              backgroundColor: '#007bff'
            }]
          },
          options: {
            scales: {
              y: {
                beginAtZero: true,
                max: 100,
                title: {
                  display: true,
                  text: "Phần trăm (%)"
                }
              }
            }
          }
        });
      });
  </script>
</body>
</html>
