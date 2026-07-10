<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Letters of the Heart</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    
    body {
      min-height: 100vh;
      background: linear-gradient(160deg, #1a0a1a, #2d1b2e, #1a1025);
      font-family: 'Georgia', 'Times New Roman', serif;
      position: relative;
      overflow: hidden;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    /* Ambient background layers */
    .ambient {
      position: absolute;
      inset: 0;
      background: 
        radial-gradient(circle at 20% 80%, rgba(180,100,120,0.08) 0%, transparent 50%),
        radial-gradient(circle at 80% 20%, rgba(120,80,160,0.08) 0%, transparent 50%);
      pointer-events: none;
      z-index: 0;
    }

    /* Stars */
    .star {
      position: absolute;
      border-radius: 50%;
      background: rgba(255,255,255,0.2);
      pointer-events: none;
      z-index: 0;
    }

    @keyframes twinkle {
      0%, 100% { opacity: 0.2; transform: scale(1); }
      50% { opacity: 0.8; transform: scale(1.5); }
    }

    /* Floating hearts */
    @keyframes floatUp {
      0% { transform: translateY(0) translateX(0) rotate(0deg); opacity: 0; }
      10% { opacity: 1; }
      90% { opacity: 0.6; }
      100% { transform: translateY(-110vh) translateX(20px) rotate(360deg); opacity: 0; }
    }

    .heart-particle {
      position: absolute;
      color: rgba(201,160,176,0.15);
      pointer-events: none;
      z-index: 1;
    }

    /* Landing Page */
    #landing {
      position: relative;
      z-index: 2;
      text-align: center;
      padding: 40px;
      max-width: 600px;
      transition: opacity 0.8s ease, transform 0.8s ease;
    }

    .landing-subtitle {
      font-size: 13px;
      letter-spacing: 6px;
      text-transform: uppercase;
      color: #c9a0b0;
      margin-bottom: 24px;
      opacity: 0.8;
    }

    .landing-title {
      font-size: clamp(2.5rem, 6vw, 4.5rem);
      color: #f5e6e0;
      margin: 0 0 16px 0;
      font-weight: 400;
      line-height: 1.1;
      font-style: italic;
      letter-spacing: 1px;
    }

    .landing-line {
      width: 80px;
      height: 1px;
      background: linear-gradient(90deg, transparent, #c9a0b0, transparent);
      margin: 24px auto;
    }

    .landing-desc {
      color: rgba(245,230,224,0.6);
      font-size: 1.05rem;
      line-height: 1.8;
      max-width: 400px;
      margin: 0 auto 40px auto;
      font-style: italic;
    }

    .enter-btn {
      background: transparent;
      border: 1px solid rgba(201,160,176,0.5);
      color: #f5e6e0;
      padding: 14px 48px;
      border-radius: 50px;
      cursor: pointer;
      font-family: inherit;
      font-size: 15px;
      letter-spacing: 3px;
      text-transform: uppercase;
      transition: all 0.4s ease;
      position: relative;
      overflow: hidden;
    }

    .enter-btn span {
      position: relative;
      z-index: 1;
    }

    .enter-btn::before {
      content: '';
      position: absolute;
      inset: 0;
      background: linear-gradient(135deg, rgba(201,160,176,0.2), rgba(180,100,120,0.2));
      transform: scaleX(0);
      transform-origin: left;
      transition: transform 0.4s ease;
      z-index: 0;
    }

    .enter-btn:hover::before {
      transform: scaleX(1);
    }

    .enter-btn:hover {
      transform: translateY(-2px);
      border-color: rgba(201,160,176,0.8);
    }

    .landing-footer {
      margin-top: 48px;
      font-size: 12px;
      color: rgba(245,230,224,0.3);
      letter-spacing: 2px;
    }

    /* Reader */
    #reader {
      position: relative;
      z-index: 2;
      display: none;
      width: 100%;
      max-width: 900px;
      padding: 24px;
      opacity: 0;
      transform: translateY(20px);
      transition: opacity 0.8s ease, transform 0.8s ease;
    }

    .top-bar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      padding: 0 8px;
    }

    .close-btn {
      background: transparent;
      border: none;
      color: rgba(245,230,224,0.5);
      cursor: pointer;
      font-family: inherit;
      font-size: 13px;
      letter-spacing: 2px;
      transition: color 0.3s;
    }

    .close-btn:hover {
      color: rgba(245,230,224,0.9);
    }

    .progress-track {
      flex: 1;
      max-width: 200px;
      height: 2px;
      background: rgba(255,255,255,0.08);
      border-radius: 2px;
      margin: 0 20px;
      position: relative;
    }

    .progress-fill {
      position: absolute;
      left: 0;
      top: 0;
      height: 100%;
      width: 20%;
      background: linear-gradient(90deg, #c9a0b0, #b46478);
      border-radius: 2px;
      transition: width 0.5s ease;
    }

    .page-counter {
      font-size: 12px;
      color: rgba(245,230,224,0.4);
      letter-spacing: 2px;
    }

    /* Letter Card */
    .letter-card {
      background: linear-gradient(145deg, rgba(255,255,255,0.04), rgba(255,255,255,0.01));
      backdrop-filter: blur(30px);
      border: 1px solid rgba(201,160,176,0.12);
      border-radius: 20px;
      padding: clamp(32px, 5vw, 56px);
      box-shadow: 0 30px 100px rgba(0,0,0,0.5), inset 0 1px 0 rgba(255,255,255,0.05);
      position: relative;
      overflow: hidden;
      min-height: 500px;
      display: flex;
      flex-direction: column;
    }

    .corner {
      position: absolute;
      width: 60px;
      height: 60px;
      pointer-events: none;
    }

    .corner-tl {
      top: 0;
      left: 0;
      border-top: 1px solid rgba(201,160,176,0.3);
      border-left: 1px solid rgba(201,160,176,0.3);
      border-top-left-radius: 20px;
    }

    .corner-tr {
      top: 0;
      right: 0;
      border-top: 1px solid rgba(201,160,176,0.3);
      border-right: 1px solid rgba(201,160,176,0.3);
      border-top-right-radius: 20px;
    }

    .corner-bl {
      bottom: 0;
      left: 0;
      border-bottom: 1px solid rgba(201,160,176,0.3);
      border-left: 1px solid rgba(201,160,176,0.3);
      border-bottom-left-radius: 20px;
    }

    .corner-br {
      bottom: 0;
      right: 0;
      border-bottom: 1px solid rgba(201,160,176,0.3);
      border-right: 1px solid rgba(201,160,176,0.3);
      border-bottom-right-radius: 20px;
    }

    .letter-header {
      text-align: center;
      margin-bottom: 32px;
    }

    .letter-subtitle {
      font-size: 11px;
      letter-spacing: 4px;
      text-transform: uppercase;
      color: #c9a0b0;
      margin-bottom: 12px;
      opacity: 0.8;
    }

    .letter-title {
      font-size: clamp(1.6rem, 4vw, 2.4rem);
      color: #f5e6e0;
      margin: 0;
      font-weight: 400;
      font-style: italic;
      line-height: 1.3;
    }

    .letter-title-line {
      width: 40px;
      height: 1px;
      background: linear-gradient(90deg, transparent, #c9a0b0, transparent);
      margin: 20px auto 0;
    }

    .letter-body {
      flex: 1;
      color: rgba(245,230,224,0.82);
      font-size: clamp(0.95rem, 2vw, 1.1rem);
      line-height: 2.2;
      text-align: center;
      overflow-y: auto;
      max-height: 50vh;
      padding: 0 8px;
    }

    .letter-body::-webkit-scrollbar {
      width: 4px;
    }

    .letter-body::-webkit-scrollbar-track {
      background: transparent;
    }

    .letter-body::-webkit-scrollbar-thumb {
      background: rgba(201,160,176,0.2);
      border-radius: 4px;
    }

    #typewriter-text {
      margin: 0;
      white-space: pre-line;
    }

    .letter-nav {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 20px;
      margin-top: 36px;
      padding-top: 24px;
      border-top: 1px solid rgba(255,255,255,0.05);
    }

    .nav-btn {
      background: transparent;
      border: 1px solid rgba(201,160,176,0.2);
      color: rgba(245,230,224,0.6);
      padding: 10px 28px;
      border-radius: 50px;
      cursor: pointer;
      font-family: inherit;
      font-size: 13px;
      letter-spacing: 1px;
      transition: all 0.3s ease;
    }

    .nav-btn:hover {
      border-color: rgba(201,160,176,0.4);
      color: rgba(245,230,224,0.9);
      transform: translateY(-1px);
    }

    .nav-btn.next {
      background: linear-gradient(135deg, #8b5a6b, #6b4c6e);
      border: none;
      color: #f5e6e0;
      box-shadow: 0 4px 20px rgba(139,90,107,0.2);
    }

    .nav-btn.next:hover {
      box-shadow: 0 6px 30px rgba(139,90,107,0.35);
    }

    .nav-btn.disabled {
      opacity: 0.3;
      pointer-events: none;
    }

    .nav-dots {
      display: flex;
      gap: 10px;
    }

    .nav-dot {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: rgba(255,255,255,0.15);
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .nav-dot.active {
      background: #c9a0b0;
      transform: scale(1.4);
    }

    .nav-dot:hover {
      background: rgba(201,160,176,0.5);
    }
  </style>
</head>
<body>

  <div class="ambient"></div>
  <div id="stars"></div>
  <div id="hearts" style="position:absolute;inset:0;pointer-events:none;z-index:1;overflow:hidden;"></div>

  <!-- Landing Page -->
  <div id="landing">
    <div class="landing-subtitle">A Collection of Devotion</div>
    <h1 class="landing-title">Letters of<br>the Heart</h1>
    <div class="landing-line"></div>
    <p class="landing-desc">Five letters written in the quiet hours,<br>where love speaks louder than words.</p>
    <button class="enter-btn" onclick="enterBook()">
      <span>Open the Letters</span>
    </button>
    <div class="landing-footer">WRITTEN WITH LOVE • FOR ETERNITY</div>
  </div>

  <!-- Reader -->
  <div id="reader">
    <div class="top-bar">
      <button class="close-btn" onclick="backToLanding()">← CLOSE</button>
      <div class="progress-track">
        <div class="progress-fill" id="progress-fill"></div>
      </div>
      <div class="page-counter"><span id="current-num">01</span> / 05</div>
    </div>

    <div class="letter-card">
      <div class="corner corner-tl"></div>
      <div class="corner corner-tr"></div>
      <div class="corner corner-bl"></div>
      <div class="corner corner-br"></div>

      <div class="letter-header">
        <div class="letter-subtitle" id="letter-subtitle">Letter One</div>
        <h2 class="letter-title" id="letter-title">A Letter to Hollyness</h2>
        <div class="letter-title-line"></div>
      </div>

      <div class="letter-body">
        <p id="typewriter-text"></p>
      </div>

      <div class="letter-nav">
        <button class="nav-btn" id="prev-btn" onclick="prevLetter()">← Previous</button>
        <div class="nav-dots" id="nav-dots">
          <span class="nav-dot active" onclick="goToLetter(0)"></span>
          <span class="nav-dot" onclick="goToLetter(1)"></span>
          <span class="nav-dot" onclick="goToLetter(2)"></span>
          <span class="nav-dot" onclick="goToLetter(3)"></span>
          <span class="nav-dot" onclick="goToLetter(4)"></span>
        </div>
        <button class="nav-btn next" id="next-btn" onclick="nextLetter()">Next →</button>
      </div>
    </div>
  </div>

  <script>
    const letters = [
      {
        subtitle: "Letter One",
        title: "A Letter to Hollyness",
        text: `I wish I could tell the world about her
I wish I could write poems and recite them to her
Just in hopes of seeing her smile.
Her beauty is something poets could write about.
She is masterpiece of God's creation
So let me paint a picture about her
In hope of keeping her in the pages of my heart forever oh how I wish I could marry her
So, I tell God about her telling him of how I don't deserve the perfect creation she is.
Well, her love language it's Jesus that how I knew she is dime
Her purity reminds me of Mary the mother Jesus. Hollyness that's who she is.
Her honor, kindness and loyalty reminds me of Ruth
Oh God, I just want her to be bone of my bone flesh my flesh
How can not fall for her in hopes becoming one!!! One flesh with her
When she speaks her courage reminds me of Esther a prayer filled woman who just wants to be close to God
She is a mother WoW!! How she raises her child I wish writers could write books on that.
Definitely how she raises her child is something to behold
Her heart makes me want to protect it but I am afraid if I do it by myself I will do it the wrong way so I will ask her Father and my Father to help me.
She is the reason a man would want to leave his mother and father house to be joined to her.
Oh, how I care about her
So, I will no longer care for her with my own power I will ask God first how I should do it
I will no longer love with my strength and heart I will ask God first of how I should love her
I will look to Jesus for that
This is the care, admiration and love I have for Hollyness
So, I will continue writing poems about her in hope of keeping her in my heart
While I wait for God's Perfect Will for her and I.`
      },
      {
        subtitle: "Letter Two",
        title: "How Did You Make Her",
        text: `God how did You make her
That when she speaks, I just sit still and I listen to her with my heart
How did You make her
That when I look at her I find myself lost in the thought of her beauty
How did You make her
That when I gaze upon her
I wish to stop time and just be with her in those moments forever
How did You make her
When she is meters from me, I feel like we are worlds apart
When I look at her I pass the beauty of her face and her brown eyes and see her heart
How did You make her
That I look at her I see how she loves You and how You love her
How did You make her
That when I see her my heart skips, dances and beats loudly all out of joy.
How did You make Her
That when I see her I ask myself and You how I should love her and care for her`
      },
      {
        subtitle: "Letter Three",
        title: "My Intentions",
        text: `my motive is to let you taste the intensity of random affection
i shower you with the unpredictable words that feed your inner child
nothing compares to you
my intentions are pure i need nothing from you
there's no manipulation or hidden agendas in my love i get everything i need from giving you care that is foreign to you
I love from a foreign land unknown by others i claw and scratch to find the soul beneath the flesh
i can finish your sentences before you even say them I know your thoughts before you say them
I seek to cleanse your tainted thoughts
i long to be the bartender that quenches your thirst
i'm the instrument of karma to rectify all the love you wasted all the faith that you gave away the slithering partners who robbed your precious heart
i'm here to settle the emotional score to sow self-worth back into a broken rib
my queen i brought my dry eraser board and one by one i shall wipe away their corrupted fingerprints from your reflection
i shall find the you that you hid away i will love the you that you threw away every day i will vindicate your lungs for choosing to seek air i will be there under your skin scratching the emotions that itch i shall show you my love until you believe in love again until you trust again until all your insecurities melt into the horizon of yesterday your wounded wings i will nurture with my eyes your distorted image i will caress with my truth i will repaint your canvas until love has meaning again
you can run from my love and care but I will shackle you to my passion till you slowly begin to surrender
our relationship makes you extremely uncomfortable because you gave up on the dream of finding love long ago
I am here to correct that if God wills it`
      },
      {
        subtitle: "Letter Four",
        title: "Deeper",
        text: `I try to capture you but words don't do you justice
my language is surface but my love is deeper
I have no GPS I'm trying to get my love lost in You
Why do you reject what you want
this type of Love Changes Everything it touches
since you were a child, you have fantasized about this type of connection unselfish, pure and unconditional love with true intent
that would excite you to wake up in the morning
with the kind of passion that makes you scared to close your eyes out of the fear of missing something
Your heart protection and safety are your priority
I still feel the fingerprints of tainted lovers who could not love you right
I see the locked doors and walls on your heart. Well, I am here to re-define those walls
I want to be the wall of love that surrounds your heart
I want to tear down the traumas they gave you by the warm embrace of my love
I will always run to you. I never want to leave you I will always be here when you need me especially on the days you don't need me
Hey I almost died yesterday
I noticed I have no time to waste
So I will love you deeper whether you want me or not I will shower you with the warm rain of my love
From this point forth I will love you louder
Deeper and like a fool I am crazy about you
I will love you in accordance to the will of My God
I hope He guides me through
I guess I just wanted you to know I love you`
      },
      {
        subtitle: "Letter Five",
        title: "The Missing Rib",
        text: `I don't really know how to begin because every time I think about you, my thoughts become too many for words.
There is so much I feel, yet somehow it all seems impossible to put into a simple letter.
But if there is one thing I can say, it is this: I believe you are the missing rib I have been searching for.
When I met you, something changed.
There was a sense of peace, familiarity, and joy that I cannot fully describe.
When I am with you, time feels different.
Sometimes I wish I could pray like Joshua and ask God to stop the sun from setting, just so I could spend a little more time with you.
Every conversation feels too short.
The sound of your laughter echoes in my mind when the day is quiet.
When I am with you those little moments that may seem ordinary to everyone else somehow become unforgettable memories to me.
And honestly, I never get tired of your drama.
It is one of the things I secretly look forward to.
Your personality, your energy, the way you express yourself those things make you uniquely you.
Your love language with God makes me smile even on days when I don't feel like smiling.
Sometimes I catch myself getting lost in your eyes and forgetting what I was about to say.
It feels as though there are stories hidden there in your eyes
Dreams.....hopes....and a beautiful soul that I want to know more and more each day.
There are moments when all I want to do is hold your hand and never let go
Because being close to you feels like being home.
It feels natural.
It feels right.
I pray often that God teaches me how to love the right way....love..patient, kind, honest and know how to hold your heart right.
The kind of love that reflects His heart.
The kind of love that protects, supports, and never gives up.
I may not always have the perfect words and perhaps this letter still does not fully capture what is in my heart. But I hope you know that you have become very special to me.
And maybe one day, if it is God's will, the prayer I have whispered so many times will become reality.
Maybe one day I will be able to look at you and say that the missing rib is now flesh of my flesh bone of my bone and call you wife because I wanna spend forever with you.
Until then, I will continue to pray, to hope and to thank God for every moment I get to spend with you.`
      },
      {
        subtitle: "I am always here ",
        title: "I Heart never wants to leave you",
        text: `Hey sometime i dont know how to show you 
Sometimes words are never enough 
I wish i cold hold and remind you that in my arms all is real 
When all does not make sense i wish God could show how to be there for 
Now i can only Hope that my words will reach all the parts of your heart 
And let you know that in everything you have your Father and the man who will always stand by your side 
If my word can not reach i hope my arms can but i know should not hold or when i should hold or even if should 
Cause in my actions i might be doing some thing wrong 
I hope my heart will know how to hold 
Coz in all things i just wanna say the Word of God to you
Coz i have that will hold you and be there for you 
I want you to know i am always here for you 
Even in the times i dont how to be` 
      },
      {
        subtitle: "I am sorry",
        title: "I am sorry",
        text: `Word we say carry weight, i have said words that are wrong
I know time i am the cause of sorry I dont know how act right 
I wanna be your peace 
I know sometimes i dont have the right words to say or how to act right and calm you heart
I wanna protect you with words 
I wanna shower you with the word of God
I wanna cover you with the warm embrace of my heart
Let my actions speak calm your soul and not cause wars 
I want my actions to be your home
I hope become a better man for you
I  am sorry`
      },
      {
        subtitle: "Hollyness and Ciana",
        title: "Two Threads, One Tapestry",
        text: `Two people but one person, Two Threads, One Tapestry
I hear it when you speak at time i hear the voice of you daughter even when i know its you 
When she pray before she sleeps i hear her mother speaking 
You are woven into her so is her woven in to you.
Two Threads, One Tapestry
Your language is the same all because 
    ];

    let currentIdx = 0;
    let typewriterInterval = null;
    let isTyping = false;

    // Create floating stars
    const starsContainer = document.getElementById('stars');
    for (let i = 0; i < 50; i++) {
      const star = document.createElement('div');
      const size = Math.random() * 2 + 1;
      star.className = 'star';
      star.style.width = size + 'px';
      star.style.height = size + 'px';
      star.style.background = `rgba(255,255,255,${Math.random() * 0.4 + 0.1})`;
      star.style.left = Math.random() * 100 + '%';
      star.style.top = Math.random() * 100 + '%';
      star.style.animation = `twinkle ${Math.random() * 4 + 3}s infinite ease-in-out`;
      star.style.animationDelay = Math.random() * 5 + 's';
      starsContainer.appendChild(star);
    }

    // Create floating hearts
    const heartsContainer = document.getElementById('hearts');
    function spawnHeart() {
      const heart = document.createElement('div');
      heart.innerHTML = '♥';
      heart.className = 'heart-particle';
      heart.style.fontSize = (Math.random() * 20 + 10) + 'px';
      heart.style.left = Math.random() * 100 + '%';
      heart.style.bottom = '-30px';
      heart.style.animation = `floatUp ${Math.random() * 8 + 10}s linear forwards`;
      heartsContainer.appendChild(heart);
      setTimeout(() => heart.remove(), 18000);
    }
    setInterval(spawnHeart, 2000);
    for (let i = 0; i < 5; i++) setTimeout(spawnHeart, i * 400);

    function enterBook() {
      const landing = document.getElementById('landing');
      const reader = document.getElementById('reader');
      landing.style.opacity = '0';
      landing.style.transform = 'translateY(-20px)';
      setTimeout(() => {
        landing.style.display = 'none';
        reader.style.display = 'block';
        setTimeout(() => {
          reader.style.opacity = '1';
          reader.style.transform = 'translateY(0)';
          typewriteLetter(0);
        }, 50);
      }, 800);
    }

    function backToLanding() {
      const landing = document.getElementById('landing');
      const reader = document.getElementById('reader');
      reader.style.opacity = '0';
      reader.style.transform = 'translateY(20px)';
      setTimeout(() => {
        reader.style.display = 'none';
        landing.style.display = 'block';
        setTimeout(() => {
          landing.style.opacity = '1';
          landing.style.transform = 'translateY(0)';
        }, 50);
      }, 800);
    }

    function typewriteLetter(idx) {
      if (isTyping) {
        clearInterval(typewriterInterval);
      }
      isTyping = true;
      currentIdx = idx;
      const textEl = document.getElementById('typewriter-text');
      const subtitleEl = document.getElementById('letter-subtitle');
      const titleEl = document.getElementById('letter-title');
      const numEl = document.getElementById('current-num');
      const progressFill = document.getElementById('progress-fill');
      const dots = document.querySelectorAll('.nav-dot');
      const prevBtn = document.getElementById('prev-btn');
      const nextBtn = document.getElementById('next-btn');

      const letter = letters[idx];
      subtitleEl.textContent = letter.subtitle;
      titleEl.textContent = letter.title;
      numEl.textContent = String(idx + 1).padStart(2, '0');
      progressFill.style.width = ((idx + 1) / letters.length * 100) + '%';

      dots.forEach((d, i) => {
        d.classList.toggle('active', i === idx);
      });

      prevBtn.classList.toggle('disabled', idx === 0);
      nextBtn.classList.toggle('disabled', idx === letters.length - 1);

      textEl.textContent = '';
      const words = letter.text.split(' ');
      let wordIndex = 0;

      typewriterInterval = setInterval(() => {
        if (wordIndex >= words.length) {
          clearInterval(typewriterInterval);
          isTyping = false;
          return;
        }
        textEl.textContent += (wordIndex > 0 ? ' ' : '') + words[wordIndex];
        wordIndex++;
        const body = document.querySelector('.letter-body');
        body.scrollTop = body.scrollHeight;
      }, 35);
    }

    function nextLetter() {
      if (currentIdx < letters.length - 1) {
        typewriteLetter(currentIdx + 1);
      }
    }

    function prevLetter() {
      if (currentIdx > 0) {
        typewriteLetter(currentIdx - 1);
      }
    }

    function goToLetter(idx) {
      typewriteLetter(idx);
    }

    // Keyboard navigation
    document.addEventListener('keydown', (e) => {
      if (document.getElementById('reader').style.display === 'none') return;
      if (e.key === 'ArrowRight') nextLetter();
      if (e.key === 'ArrowLeft') prevLetter();
    });
  </script>
</body>
</html>
