<script>
  import { onMount } from 'svelte';
  import anime from 'https://cdn.jsdelivr.net/npm/animejs/+esm';
  import { ShoppingBag } from 'lucide-svelte';

  let isOpen = false;
  let navbar;
  let logoElement;
  let navLinks;
  let cartButton;
  let menuToggle;

  function toggleMenu() {
    isOpen = !isOpen;
    
    if (isOpen) {
      // Animate menu opening
      anime({
        targets: navLinks,
        opacity: [0, 1],
        translateY: [-20, 0],
        duration: 300,
        easing: 'easeOutCubic'
      });
      
      anime({
        targets: navLinks.querySelectorAll('li'),
        opacity: [0, 1],
        translateY: [20, 0],
        scale: [0.8, 1],
        duration: 400,
        delay: anime.stagger(100, {start: 100}),
        easing: 'easeOutBack'
      });
    } else {
      // Animate menu closing
      anime({
        targets: navLinks.querySelectorAll('li'),
        opacity: [1, 0],
        translateY: [0, -10],
        scale: [1, 0.9],
        duration: 200,
        delay: anime.stagger(50),
        easing: 'easeInCubic'
      });
      
      anime({
        targets: navLinks,
        opacity: [1, 0],
        translateY: [0, -20],
        duration: 250,
        delay: 150,
        easing: 'easeInCubic'
      });
    }
  }

  onMount(() => {
    // Initial navbar animation
    anime({
      targets: navbar,
      opacity: [0, 1],
      translateY: [-50, 0],
      duration: 800,
      easing: 'easeOutExpo',
      delay: 500
    });

    // Logo animation
    anime({
      targets: logoElement.querySelectorAll('span'),
      opacity: [0, 1],
      scale: [0.5, 1],
      rotateY: [90, 0],
      duration: 600,
      delay: anime.stagger(100, {start: 700}),
      easing: 'easeOutBack'
    });

    // Nav links stagger animation
    anime({
      targets: navLinks.querySelectorAll('a'),
      opacity: [0, 1],
      translateY: [-20, 0],
      duration: 500,
      delay: anime.stagger(100, {start: 1000}),
      easing: 'easeOutCubic'
    });

    // Cart button animation
    anime({
      targets: cartButton,
      opacity: [0, 1],
      scale: [0.8, 1],
      rotateZ: [10, 0],
      duration: 600,
      delay: 1400,
      easing: 'easeOutBack'
    });

    // Continuous floating animation for logo
    anime({
      targets: logoElement,
      translateY: [-2, 2],
      duration: 3000,
      loop: true,
      direction: 'alternate',
      easing: 'easeInOutSine'
    });

    // Scroll-based navbar background animation
    let ticking = false;
    
    function updateNavbar() {
      const scrolled = window.scrollY > 50;
      
      if (scrolled) {
        navbar.classList.add('scrolled');
      } else {
        navbar.classList.remove('scrolled');
      }
      
      ticking = false;
    }
    
    function requestTick() {
      if (!ticking) {
        requestAnimationFrame(updateNavbar);
        ticking = true;
      }
    }
    
    window.addEventListener('scroll', requestTick);
    
    return () => {
      window.removeEventListener('scroll', requestTick);
    };
  });

  // Add hover animations
  function handleLinkHover(event) {
    anime({
      targets: event.target,
      scale: [1, 1.05],
      duration: 200,
      easing: 'easeOutCubic'
    });
  }

  function handleLinkLeave(event) {
    anime({
      targets: event.target,
      scale: [1.05, 1],
      duration: 200,
      easing: 'easeOutCubic'
    });
  }

  function handleCartHover() {
    anime({
      targets: cartButton,
      scale: [1, 1.1],
      rotateZ: [0, 5],
      duration: 300,
      easing: 'easeOutBack'
    });
  }

  function handleCartLeave() {
    anime({
      targets: cartButton,
      scale: [1.1, 1],
      rotateZ: [5, 0],
      duration: 300,
      easing: 'easeOutBack'
    });
  }
</script>

<nav class="navbar" bind:this={navbar}>
  <div class="navbar-container">
    <!-- Logo -->
    <div class="logo" bind:this={logoElement}>
      <span class="logo-number">1000</span>
      <span class="logo-name">Styles</span>
      <div class="logo-shine"></div>
    </div>

    <!-- Menu Toggle Button -->
    <button 
      class="menu-toggle" 
      bind:this={menuToggle}
      on:click={toggleMenu} 
      aria-label="Toggle navigation"
      class:active={isOpen}
    >
      <span class="hamburger-line"></span>
      <span class="hamburger-line"></span>
      <span class="hamburger-line"></span>
      <div class="menu-bg"></div>
    </button>

    <!-- Navigation Links -->
    <ul class="nav-links" bind:this={navLinks} class:open={isOpen}>
      <li>
        <a 
          href="#home" 
          on:mouseenter={handleLinkHover} 
          on:mouseleave={handleLinkLeave}
        >
          <span>Home</span>
          <div class="link-underline"></div>
        </a>
      </li>
      <li>
        <a 
          href="#shop" 
          on:mouseenter={handleLinkHover} 
          on:mouseleave={handleLinkLeave}
        >
          <span>Shop</span>
          <div class="link-underline"></div>
        </a>
      </li>
      <li>
        <a 
          href="#design" 
          on:mouseenter={handleLinkHover} 
          on:mouseleave={handleLinkLeave}
        >
          <span>Design</span>
          <div class="link-underline"></div>
        </a>
      </li>
      <li>
        <a 
          href="#about" 
          on:mouseenter={handleLinkHover} 
          on:mouseleave={handleLinkLeave}
        >
          <span>About</span>
          <div class="link-underline"></div>
        </a>
      </li>
      <li>
        <a 
          href="#contact" 
          on:mouseenter={handleLinkHover} 
          on:mouseleave={handleLinkLeave}
        >
          <span>Contact</span>
          <div class="link-underline"></div>
        </a>
      </li>
      <li>
        <button 
          class="cart-btn" 
          bind:this={cartButton}
          on:mouseenter={handleCartHover}
          on:mouseleave={handleCartLeave}
        >
          <span class="cart-icon">
            <ShoppingBag size={20} strokeWidth={1.5} />
          </span>
          <span class="cart-text">Cart</span>
          <span class="cart-count">0</span>
          <div class="cart-glow"></div>
        </button>
      </li>
    </ul>
  </div>
  
  <!-- Navbar background blur effect -->
  <div class="navbar-bg"></div>
</nav>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
  }

  .navbar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1000;
    padding: 1.5rem 0;
    transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  }

  .navbar-bg {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(10, 10, 10, 0.1);
    backdrop-filter: blur(0px);
    border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    transition: all 0.4s ease;
  }

  .navbar.scrolled .navbar-bg {
    background: rgba(10, 10, 10, 0.95);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
  }

  .navbar.scrolled {
    padding: 1rem 0;
  }

  .navbar-container {
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 2rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    position: relative;
    z-index: 2;
  }

  .logo {
    position: relative;
    display: flex;
    align-items: baseline;
    gap: 0.5rem;
    cursor: pointer;
    overflow: hidden;
  }

  .logo-number {
    font-size: 2rem;
    font-weight: 900;
    background: linear-gradient(135deg, #8b5cf6 0%, #d946ef 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    letter-spacing: -1px;
  }

  .logo-name {
    font-size: 1.6rem;
    font-weight: 300;
    color: #ffffff;
    font-style: italic;
    letter-spacing: 1px;
  }

  .logo-shine {
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
    animation: logoShine 3s infinite;
  }

  @keyframes logoShine {
    0% { left: -100%; }
    50% { left: 100%; }
    100% { left: 100%; }
  }

  .nav-links {
    display: flex;
    list-style: none;
    gap: 3rem;
    align-items: center;
    margin: 0;
    padding: 0;
  }

  .nav-links li {
    position: relative;
  }

  .nav-links a {
    position: relative;
    color: rgba(255, 255, 255, 0.9);
    text-decoration: none;
    font-weight: 500;
    font-size: 1rem;
    transition: all 0.3s ease;
    padding: 0.5rem 0;
    display: block;
    overflow: hidden;
  }

  .nav-links a:hover {
    color: #ffffff;
  }

  .link-underline {
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0;
    height: 2px;
    background: linear-gradient(135deg, #8b5cf6 0%, #d946ef 100%);
    transition: width 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
  }

  .nav-links a:hover .link-underline {
    width: 100%;
  }

  .cart-btn {
    position: relative;
    display: flex;
    align-items: center;
    gap: 0.5rem;
    background: rgba(139, 92, 246, 0.15);
    color: white;
    border: 2px solid rgba(139, 92, 246, 0.3);
    padding: 0.8rem 1.5rem;
    border-radius: 50px;
    cursor: pointer;
    font-weight: 600;
    font-size: 0.95rem;
    transition: all 0.3s ease;
    backdrop-filter: blur(10px);
    overflow: hidden;
  }

  .cart-icon {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .cart-count {
    background: linear-gradient(135deg, #8b5cf6 0%, #d946ef 100%);
    color: white;
    border-radius: 50%;
    width: 20px;
    height: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.8rem;
    font-weight: 700;
  }

  .cart-glow {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(135deg, rgba(139, 92, 246, 0.4), rgba(217, 70, 239, 0.4));
    opacity: 0;
    transition: opacity 0.3s ease;
    border-radius: 50px;
  }

  .cart-btn:hover {
    background: rgba(139, 92, 246, 0.25);
    border-color: rgba(139, 92, 246, 0.5);
    transform: translateY(-2px);
    box-shadow: 0 10px 25px rgba(139, 92, 246, 0.3);
  }

  .cart-btn:hover .cart-glow {
    opacity: 1;
  }

  .menu-toggle {
    display: none;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 255, 255, 0.1);
    border-radius: 12px;
    backdrop-filter: blur(10px);
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
  }

  .menu-toggle:hover {
    background: rgba(255, 255, 255, 0.1);
    border-color: rgba(139, 92, 246, 0.3);
  }

  .hamburger-line {
    width: 24px;
    height: 2px;
    background: white;
    border-radius: 2px;
    margin: 2px 0;
    transition: all 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    transform-origin: center;
  }

  .menu-toggle.active .hamburger-line:nth-child(1) {
    transform: rotate(45deg) translate(5px, 5px);
  }

  .menu-toggle.active .hamburger-line:nth-child(2) {
    opacity: 0;
    transform: scale(0);
  }

  .menu-toggle.active .hamburger-line:nth-child(3) {
    transform: rotate(-45deg) translate(5px, -5px);
  }

  .menu-bg {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(135deg, rgba(139, 92, 246, 0.2), rgba(217, 70, 239, 0.2));
    opacity: 0;
    transition: opacity 0.3s ease;
    border-radius: 12px;
  }

  .menu-toggle.active .menu-bg {
    opacity: 1;
  }

  /* Responsive Design */
  @media (max-width: 768px) {
    .navbar-container {
      padding: 0 1rem;
    }

    .logo-number {
      font-size: 1.6rem;
    }

    .logo-name {
      font-size: 1.3rem;
    }

    .menu-toggle {
      display: flex;
    }

    .nav-links {
      position: absolute;
      top: calc(100% + 1rem);
      left: 1rem;
      right: 1rem;
      background: rgba(10, 10, 10, 0.95);
      backdrop-filter: blur(20px);
      flex-direction: column;
      gap: 0;
      padding: 2rem;
      border-radius: 20px;
      border: 1px solid rgba(255, 255, 255, 0.1);
      box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
      opacity: 0;
      pointer-events: none;
      transform: translateY(-20px);
      transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    }

    .nav-links.open {
      opacity: 1;
      pointer-events: all;
      transform: translateY(0);
    }

    .nav-links li {
      width: 100%;
      text-align: center;
      margin-bottom: 1.5rem;
    }

    .nav-links li:last-child {
      margin-bottom: 0;
    }

    .nav-links a {
      padding: 1rem;
      font-size: 1.1rem;
      border-radius: 12px;
      transition: all 0.3s ease;
    }

    .nav-links a:hover {
      background: rgba(139, 92, 246, 0.1);
    }

    .cart-btn {
      width: 100%;
      justify-content: center;
      padding: 1rem 2rem;
      font-size: 1rem;
    }
  }

  @media (max-width: 480px) {
    .logo-number {
      font-size: 1.4rem;
    }

    .logo-name {
      font-size: 1.1rem;
    }

    .nav-links {
      padding: 1.5rem;
    }

    .nav-links a {
      font-size: 1rem;
    }
  }
</style>