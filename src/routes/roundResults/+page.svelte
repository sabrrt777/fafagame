<script>
  import { onMount } from 'svelte';
  
  let playerName = '';
  let playerImage = '';
  let playerScore = 0;
  let showConfetti = false;
  
  function backToRounds() {
    window.location.href = '/rounds';
  }
  
  onMount(() => {
    // استرجاع بيانات اللاعب من localStorage
    playerName = localStorage.getItem('playerName') || 'اللاعب';
    playerImage = localStorage.getItem('playerImage') || '/api/placeholder/120/120';
    
    // استرجاع النتيجة من localStorage أو تعيين قيمة افتراضية
    playerScore = localStorage.getItem('playerScore') || 50;
    
    // عرض تأثير الاحتفال عند فتح الصفحة
    setTimeout(() => {
      showConfetti = true;
    }, 300);
  });
</script>

<svelte:head>
  <title>نتائج اللاعب - مسابقة الأسئلة</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
</svelte:head>

<div class="container">
  <!-- Logo -->
  <div class="logo-container">
    <div class="logo">
      <img src="/api/placeholder/120/120" alt="شعار المسابقة" class="logo-image" />
    </div>
  </div>

  <!-- Results Card -->
  <div class="results-card {showConfetti ? 'animate-in' : ''}">
    <h1>نتائجك 🏆</h1>
    
    <p class="player-name">اسم اللاعب: <span>{playerName}</span></p>
    
    <img src={playerImage} alt="صورة اللاعب" class="avatar">
    
    <div class="score-container">
      <p class="score-label">النتيجة النهائية</p>
      <div class="score-value">{playerScore}</div>
      
      {#if playerScore >= 70}
        <p class="score-message">ممتاز! 🌟</p>
      {:else if playerScore >= 40}
        <p class="score-message">جيد جداً! 👍</p>
      {:else}
        <p class="score-message">استمر في المحاولة! 💪</p>
      {/if}
    </div>
    
    <button on:click={backToRounds} class="return-button">الرجوع إلى الجولات</button>
  </div>

  <div class="footer">
    <a href="https://x.com/1Fafaaa" target="_blank">fafa</a>
  </div>
  
  {#if showConfetti}
  <div class="confetti-container">
    {#each Array(50) as _, i}
      <div class="confetti" style="
        --fall-duration: {4 + Math.random() * 6}s;
        --fall-delay: {Math.random() * 3}s;
        left: {Math.random() * 100}%;
        background-color: {['#6a492f', '#ecdcb3', '#866448', '#a77b57', '#4CAF50'][Math.floor(Math.random() * 5)]};
      "></div>
    {/each}
  </div>
  {/if}
</div>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Tajawal', sans-serif;
  }

  :global(body) {
    background-color: #ecdcb3;
    color: #6a492f;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 20px;
    overflow-x: hidden;
  }

  .container {
    width: 100%;
    max-width: 800px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
    position: relative;
  }

  .logo-container {
    margin-bottom: 10px;
    text-align: center;
  }

  .logo {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    background-color: #6a492f;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ecdcb3;
    font-size: 1.8rem;
    font-weight: bold;
    box-shadow: 0 4px 15px rgba(106, 73, 47, 0.5);
    overflow: hidden;
  }
  
  .logo-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  h1 {
    font-size: 2.5rem;
    text-align: center;
    color: #6a492f;
    margin-bottom: 20px;
  }

  p {
    font-size: 1.3rem;
    text-align: center;
    margin-bottom: 15px;
    color: #6a492f;
  }

  .results-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 15px;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 15px;
    padding: 30px;
    width: 100%;
    box-shadow: 0 5px 15px rgba(106, 73, 47, 0.2);
    transform: translateY(20px);
    opacity: 0;
    transition: transform 0.5s ease, opacity 0.5s ease;
  }
  
  .animate-in {
    transform: translateY(0);
    opacity: 1;
  }

  .avatar {
    width: 120px;
    height: 120px;
    border-radius: 50%;
    border: 4px solid #6a492f;
    object-fit: cover;
    box-shadow: 0 4px 10px rgba(106, 73, 47, 0.3);
    margin: 15px 0;
  }

  .score-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: 20px 0;
  }

  .score-label {
    font-size: 1.5rem;
    font-weight: bold;
    margin-bottom: 10px;
  }

  .score-value {
    font-size: 2.8rem;
    font-weight: bold;
    color: #6a492f;
    background-color: rgba(255, 255, 255, 0.7);
    border-radius: 12px;
    padding: 15px 30px;
    box-shadow: 0 4px 8px rgba(106, 73, 47, 0.2);
    transition: transform 0.3s ease;
    animation: scoreAppear 1s forwards;
  }
  
  @keyframes scoreAppear {
    0% { transform: scale(0.5); opacity: 0; }
    70% { transform: scale(1.1); }
    100% { transform: scale(1); opacity: 1; }
  }
  
  .score-message {
    font-size: 1.4rem;
    font-weight: bold;
    margin-top: 15px;
    color: #6a492f;
    animation: fadeIn 1s 0.5s forwards;
    opacity: 0;
  }
  
  @keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
  }

  button {
    padding: 15px 30px;
    background-color: #6a492f;
    color: #ecdcb3;
    border: none;
    border-radius: 8px;
    font-size: 1.2rem;
    cursor: pointer;
    transition: transform 0.3s, background-color 0.3s;
    box-shadow: 0 4px 8px rgba(106, 73, 47, 0.3);
    margin-top: 20px;
    width: 80%;
    max-width: 300px;
  }

  button:hover {
    background-color: #866448;
    transform: translateY(-3px);
  }
  
  .return-button {
    animation: pulseButton 2s infinite;
  }
  
  @keyframes pulseButton {
    0% { transform: scale(1); }
    50% { transform: scale(1.05); }
    100% { transform: scale(1); }
  }

  .footer {
    margin-top: 30px;
    width: 100%;
    text-align: center;
    padding: 15px;
  }

  .footer a {
    color: #6a492f;
    text-decoration: none;
    font-weight: bold;
    transition: color 0.3s;
  }

  .footer a:hover {
    color: #866448;
  }
  
  /* تأثير الاحتفال */
  .confetti-container {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    z-index: 100;
  }
  
  .confetti {
    position: absolute;
    width: 12px;
    height: 12px;
    top: -20px;
    opacity: 0;
    animation: fall var(--fall-duration) var(--fall-delay) linear infinite;
  }
  
  @keyframes fall {
    0% {
      opacity: 1;
      top: -20px;
      transform: rotate(0deg) translateX(0);
    }
    50% {
      opacity: 1;
      transform: rotate(180deg) translateX(100px);
    }
    100% {
      opacity: 0;
      top: 100vh;
      transform: rotate(360deg) translateX(-100px);
    }
  }
  
  /* تحسينات للأجهزة المحمولة */
  @media (max-width: 600px) {
    .results-card {
      padding: 20px;
    }
    
    h1 {
      font-size: 2rem;
    }
    
    .score-value {
      font-size: 2.4rem;
      padding: 12px 24px;
    }
    
    .avatar {
      width: 100px;
      height: 100px;
    }
  }
</style>