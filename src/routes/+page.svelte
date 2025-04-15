<script>
  import { onMount } from 'svelte';
  
  let selectedAvatarIndex = -1;
  let customLogo = null;
  let playerName = '';
  let avatars;
  
  // مصفوفة تحتوي على روابط صور الأفاتار
  const avatarImages = [
    'https://pbs.twimg.com/media/GOrvnIRWwAAqPhI?format=jpg&name=large',
    'https://pbs.twimg.com/media/Gl6_Ng-XIAAV0Sg?format=jpg&name=medium',
    'https://pbs.twimg.com/media/Gl3whZIWYAAfFyR?format=jpg&name=medium',
    'https://pbs.twimg.com/media/GlY796gWgAEj9Yj?format=jpg&name=360x360',
    'https://pbs.twimg.com/media/GjqNRzebwAA73cv?format=jpg&name=360x360',
    'https://pbs.twimg.com/media/GjLkxMJWUAA9N6b?format=jpg&name=large'
  ];
  
  // افتراضي لوغو المسابقة
  const defaultLogo = 'https://cdn.discordapp.com/attachments/1331701798842404875/1360714343343194192/image.png';
  
  onMount(() => {
    avatars = document.querySelectorAll('.avatar');
    
    // تطبيق صور الأفاتار من المصفوفة
    avatars.forEach((avatar, index) => {
      if (index < avatarImages.length) {
        avatar.style.backgroundImage = `url(${avatarImages[index]})`;
        avatar.style.backgroundSize = 'cover';
        avatar.style.backgroundPosition = 'center';
        avatar.textContent = ''; // إزالة الرقم من داخل العنصر
      }
      
      // إضافة حدث النقر على كل صورة شخصية
      avatar.addEventListener('click', () => {
        // إزالة الفئة 'selected' من جميع الصور
        avatars.forEach(img => img.classList.remove('selected'));
        // إضافة الفئة 'selected' إلى الصورة المختارة
        avatar.classList.add('selected');
        // تحديث مؤشر الصورة المختارة
        selectedAvatarIndex = index;
      });
    });
  });
  
  function handleLogoUpload(event) {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = function(e) {
        const logo = document.querySelector('.logo');
        logo.style.backgroundImage = `url(${e.target.result})`;
        logo.style.backgroundSize = 'cover';
        logo.style.backgroundPosition = 'center';
        customLogo = e.target.result;
      };
      reader.readAsDataURL(file);
    }
  }
  
  function startCompetition() {
    // التحقق من إدخال الاسم واختيار صورة
    if (playerName.trim() === '') {
      showNotification('الرجاء إدخال اسمك!', 'error');
      return;
    }

    if (selectedAvatarIndex === -1) {
      showNotification('الرجاء اختيار صورة شخصية!', 'error');
      return;
    }

    // تخزين بيانات اللاعب في localStorage
    localStorage.setItem('playerName', playerName);
    localStorage.setItem('playerAvatarIndex', selectedAvatarIndex);
    localStorage.setItem('playerAvatarUrl', avatarImages[selectedAvatarIndex]);
    
    // تخزين الشعار المخصص إذا تم تحميله
    if (customLogo) {
      localStorage.setItem('customLogo', customLogo);
    } else {
      localStorage.setItem('customLogo', defaultLogo);
    }
    
    try {
      window.location.href = 'rounds';
    } catch (e) {
      showNotification('تم تسجيل اللاعب: ' + playerName, 'success');
      console.log('تم تسجيل الاسم ولكن لم يتم العثور على صفحة rounds');
    }
  }
  
  // إظهار الإشعارات
  function showNotification(message, type) {
    const notification = document.createElement('div');
    notification.className = `notification ${type}`;
    notification.textContent = message;
    document.body.appendChild(notification);
    
    // تطبيق أنيميشن الظهور
    setTimeout(() => {
      notification.classList.add('show');
    }, 10);
    
    // إزالة الإشعار بعد 3 ثواني
    setTimeout(() => {
      notification.classList.remove('show');
      setTimeout(() => {
        document.body.removeChild(notification);
      }, 300);
    }, 3000);
  }
</script>

<svelte:head>
  <title>تسجيل اللاعب - مسابقة الأسئلة المصورة</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</svelte:head>

<div class="container">
  <!-- Logo -->
  <div class="logo-container">
    <div class="logo">
      <img src="https://res.cloudinary.com/dbgx83xry/image/upload/v1744719872/414_l5uk1p.png" alt="شعار المسابقة" class="logo-image">
    </div>
    <input type="file" id="logoUpload" class="logo-upload" accept="image/*" on:change={handleLogoUpload}>
  </div>

  <!-- Registration Form -->
  <div class="registration-form">
    <h1>أهلا بك في تحدي صور</h1>
    <input type="text" bind:value={playerName} placeholder="أدخل اسمك" required>
    <p>اختر صورة شخصية:</p>
    <div class="images" id="avatars">
      <div class="avatar"></div>
      <div class="avatar"></div>
      <div class="avatar"></div>
      <div class="avatar"></div>
      <div class="avatar"></div>
      <div class="avatar"></div>
    </div>
    <button on:click={startCompetition} class="start-btn">
      <span class="btn-text">ابدأ العب</span>
      <span class="btn-icon">🎮</span>
    </button>
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
    font-family: 'Tajawal', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
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
    gap: 20px;
  }

  .logo-container {
    margin-bottom: 20px;
    text-align: center;
    position: relative;
    width: 150px;
    height: 150px;
  }

  .logo {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    background-color: #6a492f;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #ecdcb3;
    font-size: 2rem;
    font-weight: bold;
    box-shadow: 0 4px 15px rgba(106, 73, 47, 0.5);
    overflow: hidden;
    transition: transform 0.3s ease;
  }
  
  .logo:hover {
    transform: scale(1.05);
  }

  .logo-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .logo-upload {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0;
    left: 0;
    opacity: 0;
    cursor: pointer;
  }

  h1 {
    margin-bottom: 30px;
    font-size: 2.5rem;
    text-align: center;
    color: #6a492f;
  }

  .registration-form {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 15px;
    padding: 30px;
    box-shadow: 0 5px 15px rgba(106, 73, 47, 0.2);
    animation: fadeIn 0.6s ease-out;
  }

  input[type="text"] {
    padding: 15px;
    width: 100%;
    max-width: 400px;
    border: 2px solid #6a492f;
    border-radius: 8px;
    font-size: 1.1rem;
    background-color: #f9f3e3;
    color: #6a492f;
    text-align: center;
    transition: all 0.3s ease;
  }
  
  input[type="text"]:focus {
    outline: none;
    box-shadow: 0 0 10px rgba(106, 73, 47, 0.4);
    border-color: #866448;
    transform: translateY(-2px);
  }

  .images {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 15px;
    margin: 20px 0;
  }

  .avatar {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    cursor: pointer;
    transition: all 0.3s ease;
    object-fit: cover;
    border: 3px solid transparent;
    background-color: #f0f0f0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    color: #6a492f;
  }

  .avatar:hover {
    transform: scale(1.1);
    box-shadow: 0 5px 15px rgba(106, 73, 47, 0.3);
  }

  .avatar.selected {
    border: 3px solid #6a492f;
    transform: scale(1.1);
    box-shadow: 0 5px 15px rgba(106, 73, 47, 0.4);
    animation: pulse 1.5s infinite;
  }

  button {
    padding: 15px 40px;
    background-color: #6a492f;
    color: #ecdcb3;
    border: none;
    border-radius: 8px;
    font-size: 1.3rem;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 4px 10px rgba(106, 73, 47, 0.3);
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    position: relative;
    overflow: hidden;
  }

  .start-btn::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
    transition: all 0.6s ease;
  }

  .start-btn:hover::before {
    left: 100%;
  }

  button:hover {
    background-color: #866448;
    transform: translateY(-3px);
    box-shadow: 0 6px 15px rgba(106, 73, 47, 0.4);
  }
  
  button:active {
    transform: translateY(0);
    box-shadow: 0 2px 5px rgba(106, 73, 47, 0.4);
  }

  .btn-icon {
    font-size: 1.5rem;
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
  
  /* إشعارات */
  :global(.notification) {
    position: fixed;
    top: 20px;
    left: 50%;
    transform: translateX(-50%) translateY(-100px);
    padding: 15px 25px;
    border-radius: 8px;
    color: white;
    font-weight: bold;
    box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    z-index: 1000;
    transition: transform 0.3s ease;
    text-align: center;
    max-width: 90%;
  }
  
  :global(.notification.show) {
    transform: translateX(-50%) translateY(0);
  }
  
  :global(.notification.success) {
    background-color: #4CAF50;
  }
  
  :global(.notification.error) {
    background-color: #f44336;
  }
  
  /* أنيميشن */
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(20px); }
    to { opacity: 1; transform: translateY(0); }
  }
  
  @keyframes pulse {
    0% { box-shadow: 0 0 0 0 rgba(106, 73, 47, 0.7); }
    70% { box-shadow: 0 0 0 10px rgba(106, 73, 47, 0); }
    100% { box-shadow: 0 0 0 0 rgba(106, 73, 47, 0); }
  }
  
  /* تحسينات للموبايل */
  @media (max-width: 480px) {
    h1 {
      font-size: 2rem;
    }
    
    .logo-container {
      width: 120px;
      height: 120px;
    }
    
    .logo {
      width: 120px;
      height: 120px;
    }
    
    .registration-form {
      padding: 20px;
    }
    
    .avatar {
      width: 65px;
      height: 65px;
    }
    
    button {
      padding: 12px 30px;
      font-size: 1.1rem;
    }
  }
</style>