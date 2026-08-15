<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>My Simple Calendar</title>
<style>
  body {
    font-family: Arial, sans-serif;
    background: #f5f5f5;
    margin: 0;
    padding: 20px;
  }

  h1 {
    text-align: center;
    color: #333;
  }

  .container {
    max-width: 1100px;
    margin: 0 auto;
  }

  .box {
    background: white;
    border: 1px solid #ccc;
    border-radius: 6px;
    padding: 15px;
    margin-bottom: 20px;
  }

  input, select, button {
    padding: 8px;
    margin: 5px 0;
    font-size: 14px;
  }

  button {
    background: #4CAF50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  button:hover {
    background: #45a049;
  }

  label {
    display: inline-block;
    width: 110px;
  }

  /* The calendar itself - one big row of 7 big day boxes */
  .calendar {
    display: flex;
    gap: 10px;
  }

  .day-column {
    flex: 1;
    background: white;
    border: 1px solid #ccc;
    border-radius: 6px;
    padding: 10px;
    min-height: 500px;
    display: flex;
    flex-direction: column;
  }

  .day-title {
    font-weight: bold;
    text-align: center;
    margin-bottom: 10px;
    background: #eee;
    padding: 8px;
    border-radius: 4px;
    font-size: 16px;
  }

  .event {
    background: #d9edf7;
    border-left: 4px solid #31708f;
    padding: 6px;
    margin-bottom: 6px;
    font-size: 13px;
    border-radius: 3px;
  }
</style>
</head>
<body>

<div class="container">
  <h1>My Simple Calendar</h1>

  <!-- Form to add an event -->
  <div class="box">
    <h2>Add an Event</h2>
    <div>
      <label>Day:</label>
      <select id="eventDay">
        <option>Mon</option>
        <option>Tue</option>
        <option>Wed</option>
        <option>Thu</option>
        <option>Fri</option>
        <option>Sat</option>
        <option>Sun</option>
      </select>
    </div>
    <div>
      <label>Event name:</label>
      <input type="text" id="eventName" placeholder="e.g. Team meeting">
    </div>
    <div>
      <label>Start hour:</label>
      <input type="number" id="eventStart" min="0" max="23" value="9">
      <label style="width:auto; margin-left:10px;">Duration (hrs):</label>
      <input type="number" id="eventDuration" min="1" max="8" value="1">
    </div>
    <button onclick="addEvent()">Add Event</button>
  </div>

  <!-- The calendar display: 7 big boxes, one per day -->
  <div class="calendar" id="calendar"></div>
</div>

<script>
  // Simple in-memory calendar.
  // Data is stored in a plain JavaScript object while the page is open.

  const days = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];

  // events object looks like: { Mon: [ {name, start, duration} ], Tue: [...] }
  let events = {};
  days.forEach(day => events[day] = []);

  function addEvent() {
    const day = document.getElementById("eventDay").value;
    const name = document.getElementById("eventName").value;
    const start = parseInt(document.getElementById("eventStart").value);
    const duration = parseInt(document.getElementById("eventDuration").value);

    if (!name) {
      alert("Please type an event name.");
      return;
    }

    events[day].push({ name: name, start: start, duration: duration });

    document.getElementById("eventName").value = "";
    renderCalendar();
  }

  function renderCalendar() {
    const calendarDiv = document.getElementById("calendar");
    calendarDiv.innerHTML = "";

    days.forEach(day => {
      let dayHtml = '<div class="day-column">';
      dayHtml += '<div class="day-title">' + day + '</div>';

      // sort events by start time so the day looks in order
      const dayEvents = events[day].slice().sort((a, b) => a.start - b.start);

      dayEvents.forEach(ev => {
        dayHtml += '<div class="event">' +
          ev.start + ':00 - ' + (ev.start + ev.duration) + ':00<br>' +
          ev.name +
          '</div>';
      });

      dayHtml += '</div>';
      calendarDiv.innerHTML += dayHtml;
    });
  }

  // draw the empty calendar when the page first loads
  renderCalendar();
</script>

</body>
</html>
