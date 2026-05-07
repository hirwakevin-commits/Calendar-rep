<template>
  <div class="timetable-app">
    <!-- Header -->
    <header class="app-header">
      <div class="header-content">
        <h1>Academic Planner</h1>
        <div class="current-info">
          <span class="status-badge">Active Semester</span>
        </div>
      </div>
      
      <!-- Professional Day Navigation -->
      <nav class="day-nav">
        <button
          v-for="(day, index) in days"
          :key="day"
          @click="selectedDay = day"
          :class="['day-link', { active: selectedDay === day }]"
          :style="{ '--i': index }"
        >
          <span class="day-abbr">{{ day.substring(0, 2) }}</span>
          <span class="day-full">{{ day }}</span>
          <div class="active-indicator"></div>
        </button>
      </nav>
    </header>

    <!-- Main Content Area -->
    <main class="schedule-container">
      <transition name="page-slide" mode="out-in">
        <div :key="selectedDay" class="day-view">
          <div class="day-title-row">
            <h2>{{ selectedDay }}'s Lectures</h2>
            <p v-if="getDaySchedule().length">{{ getDaySchedule().length }} sessions scheduled</p>
          </div>

          <div v-if="getDaySchedule().length === 0" class="empty-state">
            <div class="empty-icon">☕</div>
            <p>No classes scheduled for today. Enjoy your break!</p>
          </div>

          <div v-else class="course-grid">
            <div 
              v-for="(course, idx) in getDaySchedule()" 
              :key="idx" 
              class="course-card"
              :style="{ '--delay': idx * 0.1 + 's' }"
            >
              <div class="card-accent"></div>
              <div class="card-time">
                <span class="period">Period {{ course.period }}</span>
                <span class="clock">{{ course.time }}</span>
              </div>
              <div class="card-main">
                <h3 class="course-code">{{ course.subject }}</h3>
                <div class="card-footer">
                  <span class="room-tag" v-if="course.subject !== 'No Class'">
                    <i class="icon">📍</i> Main Campus
                  </span>
                  <span class="type-tag">{{ course.subject === 'VC Prac' ? 'Practical' : 'Lecture' }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedDay: 'Monday',
      days: ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday'],
      // Standardized time slots based on your original 1-2-3 structure
      times: {
        p1: '08:30 - 10:30',
        p2: '11:00 - 01:00',
        p3: '02:00 - 04:00',
        last: '04:15 - 05:15'
      },
      schedule: {
        Monday: [
          { period: '1', subject: 'WUX', time: '08:30 - 10:30' },
          { period: '2', subject: 'DJF', time: '11:00 - 01:00' },
          { period: '3', subject: 'MATH', time: '02:00 - 04:00' },
          { period: 'Last', subject: 'GD', time: '04:15 - 05:15' }
        ],
        Tuesday: [
          { period: '1', subject: 'DJF', time: '08:30 - 09:30' },
          { period: '1', subject: 'DJF', time: '09:30 - 10:30' },
          { period: '2', subject: 'CL', time: '11:00 - 12:00' },
          { period: '3', subject: 'VC Prac', time: '02:00 - 03:00' }
        ],
        Wednesday: [
          { period: '1', subject: 'VC', time: '08:30 - 09:30' },
          { period: '2', subject: 'DGV', time: '11:00 - 01:00' },
          { period: 'Last', subject: 'MATH', time: '04:15 - 05:15' }
        ],
        Thursday: [
          { period: '1', subject: 'WUX', time: '08:30 - 09:30' },
          { period: '2', subject: 'DJF', time: '11:00 - 01:00' },
          { period: '3', subject: 'PHY', time: '02:00 - 03:00' },
          { period: 'Last', subject: 'DJF', time: '04:15 - 05:15' }
        ],
        Friday: [
          { period: '1', subject: 'DGV', time: '08:30 - 09:30' },
          { period: '2', subject: 'FR', time: '11:00 - 01:00' }
        ]
      }
    };
  },
  methods: {
    getDaySchedule() {
      return this.schedule[this.selectedDay] || [];
    }
  }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;600;800&display=swap');

.timetable-app {
  --primary: #6366f1;
  --primary-light: #eef2ff;
  --text-dark: #0f172a;
  --text-gray: #64748b;
  --bg-app: #f8fafc;
  
  font-family: 'Plus Jakarta Sans', sans-serif;
  background-color: var(--bg-app);
  min-height: 100vh;
  color: var(--text-dark);
}

.app-header {
  background: white;
  padding: 2rem 2rem 0 2rem;
  border-bottom: 1px solid #e2e8f0;
  position: sticky;
  top: 0;
  z-index: 100;
}

.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: 1000px;
  margin: 0 auto 2rem;
}

h1 { font-size: 1.75rem; font-weight: 800; margin: 0; }

.status-badge {
  background: #f0fdf4;
  color: #166534;
  padding: 6px 12px;
  border-radius: 99px;
  font-size: 0.8rem;
  font-weight: 600;
}

/* Navigation - Now looks like actual tabs */
.day-nav {
  display: flex;
  gap: 2rem;
  max-width: 1000px;
  margin: 0 auto;
}

.day-link {
  background: none;
  border: none;
  padding: 1rem 0;
  cursor: pointer;
  color: var(--text-gray);
  font-weight: 600;
  position: relative;
  transition: color 0.3s ease;
  display: flex;
  gap: 8px;
}

.day-link:hover { color: var(--primary); }
.day-link.active { color: var(--primary); }

.day-link.active .active-indicator {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  height: 3px;
  background: var(--primary);
  border-radius: 3px 3px 0 0;
}

/* Grid and Cards */
.schedule-container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 3rem 2rem;
}

.day-title-row { margin-bottom: 2rem; }
.day-title-row h2 { font-size: 1.5rem; margin-bottom: 0.5rem; }
.day-title-row p { color: var(--text-gray); }

.course-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
}

.course-card {
  background: white;
  border-radius: 20px;
  padding: 1.5rem;
  display: flex;
  gap: 1.5rem;
  box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05);
  border: 1px solid #f1f5f9;
  position: relative;
  overflow: hidden;
  animation: cardIn 0.5s ease forwards;
  animation-delay: var(--delay);
  opacity: 0;
}

.card-accent {
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 5px;
  background: var(--primary);
}

.card-time {
  display: flex;
  flex-direction: column;
  min-width: 80px;
}

.period { font-size: 0.75rem; font-weight: 800; color: var(--primary); text-transform: uppercase; }
.clock { font-size: 0.85rem; color: var(--text-gray); margin-top: 4px; }

.course-code { font-size: 1.25rem; font-weight: 800; margin: 0 0 1rem 0; }

.card-footer {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}

.room-tag, .type-tag {
  font-size: 0.75rem;
  padding: 4px 8px;
  border-radius: 6px;
  background: #f1f5f9;
  color: var(--text-gray);
  font-weight: 600;
}

.type-tag { background: var(--primary-light); color: var(--primary); }

/* Transitions */
@keyframes cardIn {
  from { transform: translateY(20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

.page-slide-enter-active, .page-slide-leave-active {
  transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.page-slide-enter-from { opacity: 0; transform: translateX(30px); }
.page-slide-leave-to { opacity: 0; transform: translateX(-30px); }

.empty-state {
  text-align: center;
  padding: 5rem 0;
  color: var(--text-gray);
}

.empty-icon { font-size: 3rem; margin-bottom: 1rem; }

@media (max-width: 600px) {
  .day-full { display: none; }
  .day-nav { justify-content: space-between; gap: 0; }
  .course-card { flex-direction: column; gap: 10px; }
}
</style>