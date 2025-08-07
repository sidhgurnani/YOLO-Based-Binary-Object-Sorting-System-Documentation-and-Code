# Project Progress

<style>
.progress-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  border-left: 4px solid #4A90E2;
  padding-left: 20px;
  margin-top: 2rem;
  font-size: 1.1rem;
}

.progress-step {
  position: relative;
  margin-bottom: 30px;
}

.progress-step::before {
  content: '●';
  position: absolute;
  left: -1.6em;
  top: 0;
  font-size: 1.2em;
  color: #4A90E2;
}

.current-step::before {
  content: '➤';
  color: #e91e63;
  font-size: 1.2em;
}
</style>

<div class="progress-container">

  <div class="progress-step">Refresh Programming Knowledge</div>

  <div class="progress-step">Understand Basics of Machine Learning</div>

  <div class="progress-step">Develop Basic Software</div>

  <div class="progress-step current-step">3D Model and Design Physical Product</div>

  <div class="progress-step">Integrate Software and Hardware</div>

  <div class="progress-step">Final Testing</div>

  <div class="progress-step">Reflection and Next Steps</div>

</div>