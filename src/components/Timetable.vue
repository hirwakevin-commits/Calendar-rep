<template>
  <div class="timetable-container">
    <h1>My Timetable</h1>
    
    <!-- Day Selector -->
    <div class="day-selector">
      <button
        v-for="day in days"
        :key="day"
        @click="selectedDay = day"
        :class="['day-btn', { active: selectedDay === day }]"
      >
        {{ day }}
      </button>
    </div>

    <!-- Daily Schedule -->
    <div class="daily-schedule">
      <h2>{{ selectedDay }} Schedule</h2>
      <table class="schedule-table">
        <thead>
          <tr>
            <th>Time</th>
            <th>Subject</th>
            <th>Room</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(lesson, index) in getDaySchedule()" :key="index">
            <td>{{ lesson.time }}</td>
            <td class="subject">{{ lesson.subject }}</td>
            <td>{{ lesson.room }}</td>
          </tr>
        </tbody>
      </table>
    </div>

    <!-- Overview Table -->
    <div class="overview">
      <h3>Weekly Overview</h3>
      <table class="overview-table">
        <tr>
          <th>Day</th>
          <th colspan="2">Period 1</th>
          <th rowspan="6" class="vertical">BREAK</th>
          <th colspan="2">Period 2</th>
          <th rowspan="6" class="vertical">LUNCH</th>
          <th colspan="2">Period 3</th>
          <th rowspan="6" class="vertical">BREAK</th>
          <th>Last</th>
        </tr>

        <tr v-for="day in days" :key="day">
          <td class="day-cell">{{ day }}</td>
          <td v-for="(lesson, idx) in getWeeklyLessons(day)" :key="idx" :colspan="lesson.colspan || 1">
            {{ lesson.subject }}
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Timetable',
  data() {
    return {
      selectedDay: 'Monday',
      days: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'],
      schedule: {
        Monday: [
          { time: '9:00 - 10:00', subject: 'UI/UX Design', room: 'Room 101' },
          { time: '10:00 - 11:00', subject: 'JavaScript', room: 'Lab A' },
          { time: '11:00 - 11:20', subject: 'BREAK', room: '-' },
          { time: '11:20 - 12:20', subject: 'JavaScript', room: 'Lab A' },
          { time: '12:20 - 1:00', subject: 'LUNCH', room: 'Cafeteria' },
          { time: '1:00 - 2:00', subject: 'Mathematics', room: 'Room 102' },
          { time: '2:00 - 2:20', subject: 'BREAK', room: '-' },
          { time: '2:20 - 3:20', subject: 'Graphic Design', room: 'Room 103' },
        ],
        Tuesday: [
          { time: '9:00 - 10:00', subject: 'JavaScript', room: 'Lab A' },
          { time: '10:00 - 11:00', subject: 'English', room: 'Room 201' },
          { time: '11:00 - 11:20', subject: 'BREAK', room: '-' },
          { time: '11:20 - 12:20', subject: 'Chemistry Lab', room: 'Lab B' },
          { time: '12:20 - 1:00', subject: 'LUNCH', room: 'Cafeteria' },
          { time: '1:00 - 2:00', subject: 'Video Creation', room: 'Room 104' },
          { time: '2:00 - 2:20', subject: 'BREAK', room: '-' },
          { time: '2:20 - 3:20', subject: 'Physics Practical', room: 'Lab C' },
        ],
        Wednesday: [
          { time: '9:00 - 10:00', subject: 'Video Editing', room: 'Room 104' },
          { time: '10:00 - 11:00', subject: 'Physics Theory', room: 'Room 205' },
          { time: '11:00 - 11:20', subject: 'BREAK', room: '-' },
          { time: '11:20 - 12:20', subject: 'Digital Graphics', room: 'Lab D' },
          { time: '12:20 - 1:00', subject: 'LUNCH', room: 'Cafeteria' },
          { time: '1:00 - 2:00', subject: 'Web Development', room: 'Lab A' },
          { time: '2:00 - 2:20', subject: 'BREAK', room: '-' },
          { time: '2:20 - 3:20', subject: 'Mathematics', room: 'Room 102' },
        ],
        Thursday: [
          { time: '9:00 - 10:00', subject: 'UI/UX Design', room: 'Room 101' },
          { time: '10:00 - 11:00', subject: 'JavaScript Advanced', room: 'Lab A' },
          { time: '11:00 - 11:20', subject: 'BREAK', room: '-' },
          { time: '11:20 - 12:20', subject: 'JavaScript Advanced', room: 'Lab A' },
          { time: '12:20 - 1:00', subject: 'LUNCH', room: 'Cafeteria' },
          { time: '1:00 - 2:00', subject: 'Physics Lab', room: 'Lab C' },
          { time: '2:00 - 2:20', subject: 'BREAK', room: '-' },
          { time: '2:20 - 3:20', subject: 'English Literature', room: 'Room 201' },
        ],
        Friday: [
          { time: '9:00 - 10:00', subject: 'Digital Graphics', room: 'Lab D' },
          { time: '10:00 - 11:00', subject: 'French Language', room: 'Room 301' },
          { time: '11:00 - 11:20', subject: 'BREAK', room: '-' },
          { time: '11:20 - 12:20', subject: 'Web Design Project', room: 'Lab A' },
          { time: '12:20 - 1:00', subject: 'LUNCH', room: 'Cafeteria' },
          { time: '1:00 - 2:00', subject: 'Physics', room: 'Room 205' },
          { time: '2:00 - 2:20', subject: 'BREAK', room: '-' },
          { time: '2:20 - 3:20', subject: 'Free Study', room: 'Library' },
        ],
        Saturday: [
          { time: '9:00 - 10:00', subject: 'English Writing', room: 'Room 201' },
          { time: '10:00 - 11:00', subject: 'Physics Revision', room: 'Room 205' },
          { time: '11:00 - 11:30', subject: 'BREAK', room: '-' },
          { time: '11:30 - 12:30', subject: 'UI/UX Workshop', room: 'Room 101' },
        ],
        Sunday: [
          { time: 'No Classes', subject: 'Rest Day', room: '-' },
        ],
      },
      weeklyLessons: {
        Monday: [
          { subject: 'UI/UX', colspan: 2 },
          { subject: 'JavaScript', colspan: 2 },
          { subject: 'Math', colspan: 2 },
          { subject: 'GD' },
        ],
        Tuesday: [
          { subject: 'JavaScript' },
          { subject: 'English' },
          { subject: 'Chemistry' },
          { subject: 'VC' },
          { subject: 'Physics' },
        ],
        Wednesday: [
          { subject: 'Video Edit' },
          { subject: 'Physics', colspan: 2 },
          { subject: 'DGV', colspan: 2 },
          { subject: 'Math' },
        ],
        Thursday: [
          { subject: 'UI/UX' },
          { subject: 'JavaScript', colspan: 2 },
          { subject: 'Physics' },
          { subject: 'English' },
        ],
        Friday: [
          { subject: 'DGV' },
          { subject: 'French', colspan: 2 },
          { subject: 'Physics' },
          { subject: 'Study' },
        ],
        Saturday: [
          { subject: 'English' },
          { subject: 'Physics' },
          { subject: 'UI/UX' },
        ],
        Sunday: [
          { subject: 'Rest' },
        ],
      },
    };
  },
  methods: {
    getDaySchedule() {
      return this.schedule[this.selectedDay] || [];
    },
    getWeeklyLessons(day) {
      return this.weeklyLessons[day] || [];
    },
  },
};
</script>

<style scoped>
.timetable-container {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  text-align: center;
  color: #333;
  margin-bottom: 30px;
}

h2, h3 {
  color: #555;
  margin-top: 20px;
}

/* Day Selector */
.day-selector {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 30px;
  justify-content: center;
}

.day-btn {
  padding: 10px 20px;
  border: 2px solid #007bff;
  background-color: white;
  color: #007bff;
  border-radius: 5px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.3s ease;
}

.day-btn:hover {
  background-color: #e7f3ff;
  transform: translateY(-2px);
}

.day-btn.active {
  background-color: #007bff;
  color: white;
  box-shadow: 0 4px 8px rgba(0, 123, 255, 0.3);
}

/* Daily Schedule */
.daily-schedule {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  margin-bottom: 30px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.schedule-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 15px;
}

.schedule-table thead {
  background-color: #007bff;
  color: white;
}

.schedule-table th,
.schedule-table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

.schedule-table tbody tr:hover {
  background-color: #f0f0f0;
}

.subject {
  font-weight: 600;
  color: #007bff;
}

/* Weekly Overview */
.overview {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  overflow-x: auto;
}

.overview-table {
  border-collapse: collapse;
  width: 100%;
  min-width: 900px;
}

.overview-table td,
.overview-table th {
  border: 2px solid #333;
  padding: 10px;
  text-align: center;
  font-size: 13px;
}

.overview-table th {
  background-color: #007bff;
  color: white;
  font-weight: bold;
}

.overview-table .day-cell {
  font-weight: bold;
  background-color: #e7f3ff;
  width: 50px;
}

.overview-table tbody tr:hover {
  background-color: #f0f0f0;
}

.vertical {
  writing-mode: vertical-rl;
  font-weight: bold;
  background-color: #ffc107;
}

@media (max-width: 768px) {
  .day-selector {
    flex-direction: column;
  }

  .day-btn {
    width: 100%;
  }

  .schedule-table {
    font-size: 13px;
  }

  .schedule-table th,
  .schedule-table td {
    padding: 8px;
  }
}
</style>