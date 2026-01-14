<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>EST Clock Widget</title>
<style>
  body {
    font-family: "Georgia", serif;
    background: transparent;
    color: #3E6985;
    margin: 0;
    padding: 0;
  }

  .widget {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    width: 400px;
    padding: 20px;
  }

  .time-block {
    text-align: left;
  }

  .time {
    font-size: 48px;
    font-weight: 500;
    letter-spacing: 1px;
  }

  .date {
    font-size: 16px;
    margin-top: 4px;
    letter-spacing: 1px;
    text-transform: uppercase;
  }

  .calendar {
    text-align: center;
    font-size: 12px;
  }

  .month {
    font-weight: bold;
    letter-spacing: 2px;
    margin-bottom: 6px;
  }

  .days, .dates {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 4px;
  }

  .days div {
    font-size: 10px;
    opacity: 0.8;
  }

  .dates div {
    padding: 4px;
    border-radius: 50%;
  }

  .today {
    background: #3E6985;
    color: white;
  }
</style>
</head>
<body>

<div class="widget">
  <div class="time-block">
    <div class="time" id="time">1:01</div>
    <div class="date" id="date">FRI, DEC.3</div>
  </div>

  <div class="calendar">
    <div class="month" id="month">DECEMBER</div>
    <div class="days">
      <div>S</div><div>M</div><div>T</div><div>W</div><div>T</div><div>F</div><div>S</div>
    </div>
    <div class="dates" id="dates"></div>
  </div>
</div>

<script>
function updateTime() {
  const now = new Date(
    new Date().toLocaleString("en-US", { timeZone: "America/New_York" })
  );

  let hours = now.getHours();
  let minutes = now.getMinutes();
  minutes = minutes < 10 ? "0" + minutes : minutes;

  const timeString = `${hours}:${minutes}`;
  document.getElementById("time").innerText = timeString;

  const options = { weekday: 'short', month: 'short', day: 'numeric' };
  const dateString = now.toLocaleDateString("en-US", options).toUpperCase();
  document.getElementById("date").innerText = dateString.replace(",", ".");

  generateCalendar(now);
}

function generateCalendar(now) {
  const year = now.getFullYear();
  const month = now.getMonth();
  const today = now.getDate();

  const firstDay = new Date(year, month, 1).getDay();
  const lastDate = new Date(year, month + 1, 0).getDate();

  const datesContainer = document.getElementById("dates");
  datesContainer.innerHTML = "";

  for (let i = 0; i < firstDay; i++) {
    datesContainer.appendChild(document.createElement("div"));
  }

  for (let i = 1; i <= lastDate; i++) {
    const div = document.createElement("div");
    div.innerText = i;
    if (i === today) {
      div.classList.add("today");
    }
    datesContainer.appendChild(div);
  }

  const monthName = now.toLocaleString("en-US", { month: "long" }).toUpperCase();
  document.getElementById("month").innerText = monthName;
}

setInterval(updateTime, 1000);
updateTime();
</script>

</body>
</html>

