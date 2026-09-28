# Hafez-fal
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>فال حافظ</title>
  <style>
    body {
      font-family: Tahoma, sans-serif;
      background: #f5f1e8;
      margin: 0;
      padding: 30px 15px;
      text-align: center;
      color: #333;
    }

    .card {
      max-width: 700px;
      margin: auto;
      background: white;
      padding: 30px;
      border-radius: 18px;
      box-shadow: 0 5px 25px rgba(0,0,0,.08);
    }

    h1 {
      margin-top: 0;
    }

    #result {
      white-space: pre-wrap;
      line-height: 2.2;
      text-align: right;
      margin-top: 25px;
    }

    .number {
      font-size: 24px;
      font-weight: bold;
      margin-bottom: 20px;
    }

    .loading {
      opacity: .7;
    }
  </style>
</head>

<body>

<div class="card">
  <h1>🔮 فال حافظ</h1>
  <div id="result" class="loading">در حال دریافت فال...</div>
</div>

<script>
const SOURCE =
  "https://raw.githubusercontent.com/mahmoud-eskandari/HafezFaalDatabase/master/Faals.json";

async function getFaals() {
  const response = await fetch(SOURCE, {
    cache: "no-store"
  });

  if (!response.ok) {
    throw new Error("خطا در دریافت بانک فال");
  }

  const data = await response.json();

  if (!Array.isArray(data) || data.length !== 495) {
    throw new Error("بانک باید دقیقاً شامل 495 فال باشد");
  }

  return data.map((item, index) => ({
    id: `HAFEZ-${String(index + 1).padStart(3, "0")}`,
    number: index + 1,
    interpretation: item.interpretation.trim()
  }));
}

function getRequestedNumber() {
  const params = new URLSearchParams(window.location.search);
  const n = Number(params.get("number"));

  if (Number.isInteger(n) && n >= 1 && n <= 495) {
    return n;
  }

  return null;
}

async function showFaal() {
  const result = document.getElementById("result");

  try {
    const faals = await getFaals();

    const requestedNumber = getRequestedNumber();

    const faal = requestedNumber
      ? faals[requestedNumber - 1]
      : faals[Math.floor(Math.random() * faals.length)];

    result.className = "";

    result.innerHTML = `
      <div class="number">
        فال شماره ${faal.number}
      </div>

      <div>
        ✨ تعبیر فال
      </div>

      <div>
        ${faal.interpretation}
      </div>
    `;

    document.title = `فال حافظ ${faal.number}`;

  } catch (error) {
    result.className = "";
    result.textContent =
      "خطا در دریافت فال. لطفاً دوباره تلاش کنید.";
    console.error(error);
  }
}

showFaal();
</script>

</body>
</html>
