<template>
  <div class="timetable-container">
    <header class="header">
      <h1>University Schedule</h1>
      <p class="subtitle">Select a day to view your sessions</p>
    </header>
    
    <!-- Professional Day Selector -->
    <nav class="day-selector">
      <button
        v-for="day in days"
        :key="day"
        @click="selectedDay = day"
        :class="['day-btn', { active: selectedDay === day }]"
      >
        <span class="day-name">{{ day.substring(0, 3) }}</span>
        <span class="day-full">{{ day }}</span>
      </button>
    </nav>

    <!-- Animated Schedule Section -->
    <div class="schedule-wrapper">
      <transition name="slide-fade" mode="out-in">
        <div :key="selectedDay" class="daily-schedule">
          <div class="schedule-header">
            <h2>{{ selectedDay }}</h2>
            <span class="count">{{ getDaySchedule().length }} Classes</span>
          </div>

          <div class="course-list">
            <div 
              v-for="(lesson, index) in getDaySchedule()" 
              :key="index" 
              class="course-card"
              :style="{ '--delay': index * 0.1 + 's' }"
            >
              <div class="time-slot">
                <span class="time">{{ lesson.time }}</span>
              </div>
              <div class="course-info">
                <h3 class="subject">{{ lesson.subject }}</h3>
                <div class="meta">
                  <span class="room">
                    <i class="icon">📍</i> {{ lesson.room }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </transition>
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
          { time: '09:00 AM', subject: 'UI/UX Design', room: 'Room 101' },
          { time: '10:00 AM', subject: 'JavaScript', room: 'Lab A' },
          { time: '11:20 AM', subject: 'JavaScript', room: 'Lab A' },
          { time: '01:00 PM', subject: 'Mathematics', room: 'Room 102' },
          { time: '02:20 PM', subject: 'Graphic Design', room: 'Room 103' },
        ],
        Tuesday: [
          { time: '09:00 AM', subject: 'JavaScript', room: 'Lab A' },
          { time: '10:00 AM', subject: 'English', room: 'Room 201' },
          { time: '11:20 AM', subject: 'Chemistry Lab', room: 'Lab B' },
          { time: '01:00 PM', subject: 'Video Creation', room: 'Room 104' },
          { time: '02:20 PM', subject: 'Physics Practical', room: 'Lab C' },
        ],
        // ... (Keep your other days data here)
        Sunday: [
          { time: 'All Day', subject: 'Rest Day', room: 'Home' },
        ]
      },
    };
  },
  methods: {
    getDaySchedule() {
      return this.schedule[this.selectedDay] || [];
    },
  },
};
</script>

<style scoped>
:root {
  --primary: #4f46e5;
  --bg: #f8fafc;
  --text-main: #1e293b;
  --text-muted: #64748b;
}

.timetable-container {
  padding: 40px 20px;
  max-width: 800px;
  margin: 0 auto;
  font-family: 'Inter', system-ui, sans-serif;
  color: #1e293b;
}

.header {
  text-align: center;
  margin-bottom: 40px;
}

h1 {
  font-size: 2.5rem;
  font-weight: 800;
  letter-spacing: -0.025em;
  margin-bottom: 8px;
}

.subtitle {
  color: #64748b;
  font-size: 1.1rem;
}

/* Professional Day Selector */
.day-selector {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 8px;
  background: #f1f5f9;
  padding: 8px;
  border-radius: 16px;
  margin-bottom: 32px;
}

.day-btn {
  border: none;
  background: transparent;
  padding: 12px 8px;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  display: flex;
  flex-direction: column;
  align-items: center;
}

.day-name { font-weight: 700; font-size: 0.9rem; }
.day-full { font-size: 0.7rem; opacity: 0.7; display: none; }

.day-btn.active {
  background: white;
  color: #4f46e5;
  box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
}

/* Course Cards */
.course-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.course-card {
  display: flex;
  align-items: center;
  background: white;
  padding: 20px;
  border-radius: 16px;
  border: 1px solid #e2e8f0;
  transition: transform 0.2s ease;
  animation: slideUp 0.4s ease forwards;
  animation-delay: var(--delay);
  opacity: 0;
}

.course-card:hover {
  transform: translateX(8px);
  border-color: #4f46e5;
}

.time-slot {
  min-width: 100px;
  font-weight: 600;
  color: #4f46e5;
  font-size: 0.9rem;
}

.subject {
  font-size: 1.1rem;
  font-weight: 700;
  margin: 0 0 4px 0;
}

.room {
  font-size: 0.85rem;
  color: #64748b;
  display: flex;
  align-items: center;
  gap: 4px;
}

/* Animations */
@keyframes slideUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.2s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from {
  transform: translateX(20px);
  opacity: 0;
}

.slide-fade-leave-to {
  transform: translateX(-20px);
  opacity: 0;
}

/* Responsive */
@media (max-width: 640px) {
  .day-selector { grid-template-columns: repeat(4, 1fr); }
  .course-card { flex-direction: column; align-items: flex-start; gap: 10px; }
  .time-slot { min-width: auto; }
}
</style>