<script>
  import { onMount } from 'svelte';
  
  // استرجاع بيانات اللاعب من localStorage
  let playerName = '';
  let playerImage = '';
  
  // إعداد الأسئلة - 15 سؤال
  const questions = [
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360714343343194192/image.png?ex=67ff6b3c&is=67fe19bc&hm=b0cb724ab5fa41b49c136ccc221ab7bc0ff644166db6526f8c276932690a5bc1&', correctAnswer: 'ماثيو' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360714215421251584/image.png?ex=67ff6b1d&is=67fe199d&hm=1431580b0fcffd9bb44373f8bd4252b8abd0edbbf5e727ecfe90e33fa1faae6b&', correctAnswer: 'عربيد' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360714151546322964/image.png?ex=67ff6b0e&is=67fe198e&hm=a7e327507eb6067b9fa8c863e647b71e019c200117e59057edaa4ced3ed45878&', correctAnswer: 'اذن قودمين' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360714125881380894/image.png?ex=67ff6b08&is=67fe1988&hm=3e876d6ddcd013569fe50b139d2226f41ae3ac3a4f2a2661774a1638be28fa57&', correctAnswer: 'موستاش' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360714101554413848/image.png?ex=67ff6b02&is=67fe1982&hm=a65e425a949d4cca8499b2fabcd8e93cf6a866de68f3e0653f028dc92f16df7a&', correctAnswer: 'علمدار' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360714071489646834/image.png?ex=67ff6afb&is=67fe197b&hm=2fbd29ddf8b2cba486214eb50c01d3f40a5f174b28c732fb5873b5583111fb03&', correctAnswer: 'ابو الهول' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360714047254691840/image.png?ex=67ff6af5&is=67fe1975&hm=7fd9746a7ee56b8404b201438ea603dd901bd41b51e2882e7ebb8dd27e30b6dc&', correctAnswer: 'موي حياك' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360714022177214675/image.png?ex=67ff6aef&is=67fe196f&hm=681ecc7fe7ba08ba26c1255b3c65f0113ed7f9fa813b3265e91468fafb326057&', correctAnswer: 'لبش' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360713989088084058/image.png?ex=67ff6ae7&is=67fe1967&hm=2ce0744c873cd03f3d30567ce4f997379c61b31ee0a028d6f8c3e03cfd290373&', correctAnswer: 'ابو سكره' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360713935216443482/image.png?ex=67ff6ada&is=67fe195a&hm=90186bdd099bac8677b4d344f35c2d9dd2ab2e0cbda1aaa6bd2c9010bd5a441e&', correctAnswer: 'فرعون' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360713905944658090/image.png?ex=67ff6ad4&is=67fe1954&hm=216bfdd5968dbb2644dc7f909d983afb9f14cc6be8f8614f97a63ae07403fa9d&', correctAnswer: 'زكريا سنارة' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360713883118993419/image.png?ex=67ff6ace&is=67fe194e&hm=30c4f57efad3e1546c328b9da9c1326049028eacad882d269760d65cb584644b&', correctAnswer: 'كبريت شاكا' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360713859677163700/image.png?ex=67ff6ac8&is=67fe1948&hm=3ecd5ae47b2a82b19c5e42de52823155d0d33118206640e4b3189933db863d6d&', correctAnswer: 'سعيدان قرقوش' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360713832636354620/image.png?ex=67ff6ac2&is=67fe1942&hm=7197f3e46f4647a6a07013f5203a41274dd7e30957269be1e3ec14ebd28abe3a&', correctAnswer: 'ذياب خلفان' },
    { image: 'https://cdn.discordapp.com/attachments/1331701798842404875/1360755468254969928/image.png?ex=67ff9189&is=67fe4009&hm=8cd86f61c9122d3d8a4ca6dbb0d1946f9f58157a27eb9ac6a24b2618fd4168c1&', correctAnswer: 'عنبر' }
  ];
  
  // تعريف المتغيرات
  let currentQuestionIndex = 0;
  let score = 0;
  let gameEnded = false;
  let userAnswer = '';
  let imagesLoaded = Array(questions.length).fill(false);
  let currentImageLoaded = false;
  let feedbackMessage = '';
  let feedbackType = '';
  let showFeedback = false;
  let timeoutId;
  
  // التحقق من الإجابة
  function checkAnswer() {
    const userInputAnswer = userAnswer.trim().toLowerCase();
    const correctAnswer = questions[currentQuestionIndex].correctAnswer.toLowerCase();
    
    // إظهار التغذية الراجعة
    clearTimeout(timeoutId);
    
    if (userInputAnswer === correctAnswer) {
      score += 10;
      feedbackMessage = 'إجابة صحيحة!';
      feedbackType = 'correct';
    } else {
      feedbackMessage = `إجابة خاطئة! الإجابة الصحيحة هي: ${questions[currentQuestionIndex].correctAnswer}`;
      feedbackType = 'incorrect';
    }
    
    showFeedback = true;
    
    // انتظر قليلاً قبل الانتقال للسؤال التالي
    timeoutId = setTimeout(() => {
      showFeedback = false;
      
      // الانتقال إلى السؤال التالي أو إنهاء اللعبة
      if (currentQuestionIndex < questions.length - 1) {
        currentQuestionIndex++;
        userAnswer = '';
        currentImageLoaded = imagesLoaded[currentQuestionIndex];
      } else {
        endGame();
      }
    }, 1500);
  }
  
  // إنهاء اللعبة
  function endGame() {
    gameEnded = true;
    localStorage.setItem('playerScore', score);
  }
  
  // التعامل مع ضغط مفتاح Enter
  function handleKeypress(event) {
    if (event.key === 'Enter' && !showFeedback) {
      checkAnswer();
    }
  }
  
  function goToResults() {
    window.location.href = '/roundResults';
  }
  
  function backToRounds() {
    window.location.href = '/rounds';
  }
  
  // معالجة حدث تحميل الصورة
  function handleImageLoad(index) {
    imagesLoaded[index] = true;
    if (index === currentQuestionIndex) {
      currentImageLoaded = true;
    }
  }
  
  onMount(() => {
    if (typeof window !== 'undefined') {
      playerName = localStorage.getItem('playerName') || 'اللاعب';
      playerImage = localStorage.getItem('playerImage') || 'https://cdn.discordapp.com/avatars/621268203057971200/e297911f1aa449e7eab4620727891694.webp?size=1024&format=webp';
      
      // تحميل مسبق للصور
      questions.forEach((question, index) => {
        const img = new Image();
        img.onload = () => handleImageLoad(index);
        img.src = question.image;
      });
    }
  });
</script>

<svelte:head>
  <title>الأسئلة - مسابقة الأسئلة</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
</svelte:head>

<div class="container">
  <div class="logo-container">
    <div class="logo">
      <img src="https://res.cloudinary.com/dbgx83xry/image/upload/v1744719872/414_l5uk1p.png" alt="شعار المسابقة" class="logo-image" />
    </div>
  </div>

  <div class="question-card">
    <h1>الأسئلة</h1>
    <div class="score-indicator">النقاط: <span>{score}</span></div>
    <div class="question-counter">السؤال <span>{currentQuestionIndex + 1}</span> من <span>{questions.length}</span></div>
    
    {#if !gameEnded}
      <div class="image-wrapper">
        {#if !currentImageLoaded}
          <div class="loading-spinner">
            <div class="spinner"></div>
            <div>جاري تحميل الصورة...</div>
          </div>
        {/if}
        <img 
          src={questions[currentQuestionIndex].image} 
          alt="صورة السؤال" 
          on:load={() => handleImageLoad(currentQuestionIndex)}
          class={currentImageLoaded ? 'loaded' : 'loading'}
        />
      </div>

      <div class="input-section">
        <input 
          type="text" 
          id="userAnswer"
          bind:value={userAnswer} 
          placeholder="اكتب إجابتك هنا"
          on:keypress={handleKeypress}
          disabled={showFeedback}
        >
        
        <button on:click={checkAnswer} disabled={showFeedback}>تحقق من الإجابة</button>
        
        {#if showFeedback}
          <div class="feedback {feedbackType}">
            {feedbackMessage}
          </div>
        {/if}
      </div>
    {:else}
      <div id="gameEndSection">
        <h2>تم إنهاء جميع الأسئلة!</h2>
        <div class="score-display">النتيجة: <span>{score}</span></div>
        <div class="result-actions">
          <button on:click={goToResults}>انتقل إلى النتائج</button>
          <button on:click={backToRounds}>الرجوع إلى الجولات</button>
        </div>
      </div>
    {/if}
  </div>

  <div class="footer">
    <a href="https://x.com/1Fafaaa" target="_blank">fafa</a>
  </div>
</div>

<style>
  /* تعيين خصائص عامة */
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Tajawal', sans-serif;
  }

  /* إصلاح مشكلة المسافات البيضاء */
  :global(html), :global(body) {
    margin: 0;
    padding: 0;
    height: 100%;
    width: 100%;
    overflow-x: hidden;
    background-color: #ecdcb3;
    color: #6a492f;
  }

  :global(body) {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .container {
    width: 100%;
    max-width: 600px;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 0.5rem;
    gap: 1rem;
    height: 100vh; /* استخدم ارتفاع الشاشة كامل */
    justify-content: space-between; /* توزيع العناصر بالتساوي */
    margin: 0 auto;
  }

  .logo-container {
    padding-top: 0.5rem;
  }

  .logo {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    background-color: #6a492f;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ecdcb3;
    overflow: hidden;
    box-shadow: 0 4px 10px rgba(106, 73, 47, 0.5);
  }

  .logo-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .score-indicator {
    font-size: 1.1rem;
    font-weight: bold;
    color: #6a492f;
    background-color: rgba(255, 255, 255, 0.7);
    padding: 0.3rem 0.8rem;
    border-radius: 20px;
    margin-bottom: 0.5rem;
  }

  .question-card {
    width: 100%;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 15px;
    padding: 1rem;
    box-shadow: 0 5px 15px rgba(106, 73, 47, 0.2);
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.8rem;
    flex: 1; /* يأخذ المساحة المتوفرة */
    max-height: calc(100vh - 150px); /* تحديد الحد الأقصى للارتفاع */
    overflow-y: auto; /* إضافة تمرير عند الحاجة */
  }

  h1 {
    font-size: 1.7rem;
    text-align: center;
    color: #6a492f;
    margin-bottom: 0.3rem;
  }

  .question-counter {
    font-size: 1rem;
    font-weight: bold;
    color: #6a492f;
    padding: 0.3rem 0.8rem;
    background-color: rgba(255, 255, 255, 0.7);
    border-radius: 20px;
  }

  .image-wrapper {
    position: relative;
    width: 100%;
    max-height: 240px; /* تم تقليل الارتفاع */
    border-radius: 10px;
    border: 2px solid #6a492f;
    overflow: hidden;
    background-color: #fff;
    box-shadow: 0 4px 8px rgba(106, 73, 47, 0.3);
    display: flex;
    align-items: center;
    justify-content: center;
    flex: 0 0 auto; /* منع التمدد */
  }

  .loading-spinner {
    position: absolute;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 100%;
    background-color: rgba(255, 255, 255, 0.8);
    color: #6a492f;
    font-weight: bold;
    z-index: 1;
    gap: 10px;
  }

  .spinner {
    width: 40px;
    height: 40px;
    border: 4px solid rgba(106, 73, 47, 0.3);
    border-radius: 50%;
    border-top-color: #6a492f;
    animation: spin 1s linear infinite;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  .image-wrapper img {
    width: 100%;
    height: auto;
    max-height: 240px;
    object-fit: contain;
    display: block;
    transition: opacity 0.3s;
  }

  .image-wrapper img.loading {
    opacity: 0.3;
  }

  .image-wrapper img.loaded {
    opacity: 1;
  }

  .input-section {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.8rem;
  }

  input[type="text"] {
    width: 100%;
    padding: 0.8rem;
    border: 2px solid #6a492f;
    border-radius: 8px;
    font-size: 1rem;
    background-color: rgba(255, 255, 255, 0.8);
    text-align: center;
    direction: rtl;
    transition: border-color 0.3s, box-shadow 0.3s;
  }

  input[type="text"]:focus {
    outline: none;
    border-color: #866448;
    box-shadow: 0 0 8px rgba(134, 100, 72, 0.5);
  }

  input[type="text"]:disabled {
    background-color: rgba(236, 220, 179, 0.5);
    cursor: not-allowed;
  }

  button {
    padding: 0.8rem 1.5rem;
    background-color: #6a492f;
    color: #ecdcb3;
    border: none;
    border-radius: 8px;
    font-size: 1rem;
    cursor: pointer;
    transition: transform 0.3s, background-color 0.3s;
    box-shadow: 0 4px 8px rgba(106, 73, 47, 0.3);
    width: 100%;
    max-width: 200px;
  }

  button:hover:not(:disabled) {
    background-color: #866448;
    transform: translateY(-2px);
  }

  button:disabled {
    opacity: 0.7;
    cursor: not-allowed;
  }

  .feedback {
    width: 100%;
    padding: 0.8rem;
    border-radius: 8px;
    text-align: center;
    font-weight: bold;
    animation: fadeIn 0.3s ease-in-out;
  }

  .feedback.correct {
    background-color: rgba(76, 175, 80, 0.2);
    color: #2e7d32;
    border: 1px solid #2e7d32;
  }

  .feedback.incorrect {
    background-color: rgba(244, 67, 54, 0.2);
    color: #c62828;
    border: 1px solid #c62828;
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(-10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .result-actions {
    display: flex;
    flex-direction: column;
    gap: 0.8rem;
    width: 100%;
    align-items: center;
  }

  .score-display {
    font-size: 1.3rem;
    font-weight: bold;
    margin: 0.5rem 0;
    padding: 0.8rem 1.2rem;
    background-color: rgba(255, 255, 255, 0.7);
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(106, 73, 47, 0.2);
  }

  #gameEndSection {
    width: 100%;
    text-align: center;
  }

  .footer {
    margin-top: 0.5rem;
    width: 100%;
    text-align: center;
    padding: 0.5rem;
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
  @media (max-width: 480px) {
    .container {
      padding: 0.3rem;
      height: 100vh;
    }
    
    .question-card {
      padding: 0.8rem;
      max-height: calc(100vh - 120px);
    }
    
    .image-wrapper {
      max-height: 220px;
    }
    
    .image-wrapper img {
      max-height: 220px;
    }
    
    h1 {
      font-size: 1.5rem;
      margin-bottom: 0.2rem;
    }
    
    .question-counter {
      font-size: 0.9rem; 
    }
    
    button {
      padding: 0.7rem 1.2rem;
    }
    
    .feedback {
      font-size: 0.9rem;
      padding: 0.6rem;
    }
  }
  
  /* تحريك واهتزاز لحظي للشاشة عند الإجابة الخاطئة */
  @keyframes shake {
    0%, 100% { transform: translateX(0); }
    10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
    20%, 40%, 60%, 80% { transform: translateX(5px); }
  }
  
  .shake {
    animation: shake 0.5s cubic-bezier(.36,.07,.19,.97) both;
  }
</style>