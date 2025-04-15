<script>
  import { onMount } from 'svelte';
  
  let playerName = '';
  let playerAvatarIndex = 0;
  let avatarImages = [
    'https://res.cloudinary.com/dbgx83xry/image/upload/v1744719872/414_l5uk1p.png', // استبدال روابط الصور بروابط الصور الفعلية أو استخدام صور placeholder
  ];
  
  function goToGame() {
    window.location.href = '/game';
  }
  
  function endGame() {
    // توليد نقاط عشوائية للاعب (للتوضيح فقط)
    const playerScore = Math.floor(Math.random() * 100);
    
    // تخزين النقاط في localStorage
    localStorage.setItem('playerScore', playerScore);
    
    // توجيه المستخدم إلى صفحة النتائج
    window.location.href = '';
  }
  
  onMount(() => {
    // استرجاع بيانات اللاعب من localStorage
    playerName = localStorage.getItem('playerName') || 'اللاعب';
    playerAvatarIndex = localStorage.getItem('playerAvatarIndex') || 0;
  });
</script>

<svelte:head>
  <title>اختيار الجولة - مسابقة الأسئلة</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
</svelte:head>

<div class="container">
  <!-- Logo -->
  <div class="logo-container">
    <div class="logo">
      <img src="https://res.cloudinary.com/dbgx83xry/image/upload/v1744719872/414_l5uk1p.png" alt="شعار المسابقة" class="logo-image" />
    </div>
  </div>

  <!-- Player Info and Rounds Selection -->
  <div class="player-info">
    <h1>أهلاً {playerName} 👋</h1>
    <p>جاهز تبدأ الجولة؟</p>
    <img src={avatarImages[playerAvatarIndex]} alt="صورة اللاعب" class="avatar">

    <div class="rounds-grid">
      <button disabled>
        <span class="emoji">🗺️</span>
        الجولة 1
        <small>(قريباً)</small>
      </button>
      <button disabled>
        <span class="emoji">🗺️</span>
        الجولة 2
        <small>(قريباً)</small>
      </button>
      <button disabled>
        <span class="emoji">🗺️</span>
        الجولة 3
        <small>(قريباً)</small>
      </button>
      <button on:click={goToGame} class="available">
        <span class="emoji">🎯</span>
        الجولة 4
        <small>(متاحة - نسخة ريسبكت)</small>
      </button>
    </div>

    <button on:click={endGame} class="end-game">قريبا - موقع تجريبي فقط</button>
  </div>

  <div class="footer">
    <a href="https://x.com/1Fafaaa" target="_blank">fafa</a>
  </div>
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
  }

  .container {
    width: 100%;
    max-width: 800px;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 30px;
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
    margin-bottom: 10px;
  }

  p {
    font-size: 1.3rem;
    text-align: center;
    margin-bottom: 20px;
    color: #6a492f;
  }

  .player-info {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 15px;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 15px;
    padding: 20px;
    width: 100%;
    box-shadow: 0 5px 15px rgba(106, 73, 47, 0.2);
  }

  .avatar {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    border: 4px solid #6a492f;
    object-fit: cover;
    box-shadow: 0 4px 10px rgba(106, 73, 47, 0.3);
  }

  .rounds-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 15px;
    width: 100%;
    margin: 20px 0;
  }

  button {
    padding: 15px 20px;
    background-color: #6a492f;
    color: #ecdcb3;
    border: none;
    border-radius: 8px;
    font-size: 1.2rem;
    cursor: pointer;
    transition: transform 0.3s, background-color 0.3s;
    box-shadow: 0 4px 8px rgba(106, 73, 47, 0.3);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 8px;
    text-align: center;
    width: 100%;
  }

  button:hover:not(:disabled) {
    background-color: #866448;
    transform: translateY(-3px);
  }

  button:disabled {
    background-color: #a9998a;
    cursor: not-allowed;
    opacity: 0.7;
  }

  .end-game {
    background-color: #865d3b;
    margin-top: 20px;
    padding: 12px 30px;
  }

  .end-game:hover {
    background-color: #a77b57;
  }

  .emoji {
    font-size: 1.5rem;
    margin-bottom: 5px;
  }

  .available {
    position: relative;
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0% { transform: scale(1); }
    50% { transform: scale(1.03); }
    100% { transform: scale(1); }
  }

  .available::after {
    content: '';
    position: absolute;
    top: 10px;
    right: 10px;
    width: 12px;
    height: 12px;
    background-color: #4CAF50;
    border-radius: 50%;
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
  
  /* تحسينات للأجهزة المحمولة */
  @media (max-width: 600px) {
    h1 {
      font-size: 2rem;
    }
    
    .player-info {
      padding: 15px;
    }
    
    .rounds-grid {
      grid-template-columns: 1fr;
    }
  }
</style>