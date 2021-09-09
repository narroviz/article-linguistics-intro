<script>
  let isMenuVisible = false
  let isFixed = true
</script>

<header class:is-fixed="{isFixed}">
  <a href='https://narro.design' class='logo'>
    <img src="/assets/images/narro-official-inverse.png" alt="" class="logo">
  </a>
  <div class='header__menu tk-national' class:is-visible="{isMenuVisible}" >
      <p class='menu__tagline'><a href='https://narro.design'>Narro</a> is a creative visualization space built to explore unique datasets, topics, questions and ideas.</p>
      <ul class='menu__links'>
          <li><a href='https://narro.design/#new' target='_blank'>What’s New</a></li>
          <li><a href='https://narro.design/#about' target='_blank'>About Us</a>
      </ul>
      <img src="/assets/images/narro-official-inverse.png" alt="" class="bottom-logo">
  </div>
  <button class='header__toggle' class:is-visible="{isMenuVisible}" on:click="{() => isMenuVisible = !isMenuVisible}">
      <span><svg width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-menu" viewBox="0 0 24 24"><line x1="3" y1="12" x2="21" y2="12"></line><line x1="3" y1="6" x2="21" y2="6"></line><line x1="3" y1="18" x2="21" y2="18"></line></svg></span>
      <span><svg width="24" height="24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-x" viewBox="0 0 24 24"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg></span>
  </button> 

</header>


<style>
  header {
    z-index: 1000;
    width: 100%;
    background-color: white;
    height: 4rem;
    box-shadow: 0 0 7px 1px #c9c9c9;
    
    -webkit-transform: translate3d(0, 0, 0);
            transform: translate3d(0, 0, 0);
    -webkit-box-shadow: 0 0 7px 1px #c9c9c9;
            box-shadow: 0 0 7px 1px #c9c9c9;
    height: 4rem;
    border: none;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
        -ms-flex-align: center;
            align-items: center;
  }

  .is-fixed {
    position: fixed;
  }

  .logo {
    max-width: 50px;
    max-height: 50px;
    display: block;
    position: relative;
    color: inherit;
    font-weight: 700px;
    margin: 0 auto;
  }

  .bottom-logo {
    max-width: min(100px, 40%);
    display: block;
    margin: auto;
    margin-bottom: 4.5rem;
  }

  .header__menu {
    position: fixed;
    top: 0;
    left: 0;
    -webkit-transform: translateX(-100%);
        -ms-transform: translateX(-100%);
            transform: translateX(-100%);
    height: 100vh;
    width: 100%;
    max-width: min(18rem, 70vw);
    background-color: #282828;
    color: #f4f4f4;
    z-index: 0;
    border: 2px solid #282828;
    -webkit-transition: -webkit-transform 200ms ease-in-out;
    transition: -webkit-transform 200ms ease-in-out;
    -o-transition: transform 200ms ease-in-out;
    transition: transform 200ms ease-in-out;
    transition: transform 200ms ease-in-out, -webkit-transform 200ms ease-in-out;
  }
  .menu__tagline {
    padding-top: 4rem;
    padding-right: 1rem;
    padding-left: 0.75rem;
    font-weight: 200;
  }
  @media only screen and (min-width: 30em) {
    .header__menu {
      border-right: 1px solid #5b5b5b;
    }
  }
  .header__menu.is-visible {
    -webkit-transform: translateX(0);
        -ms-transform: translateX(0);
            transform: translateX(0);
  }
  .header__menu a {
    color: #f4f4f4;
    border-bottom: 1px solid #f4f4f4;
  }
  .header__menu img {
    color: #f4f4f4;
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    width: 100px;
    margin-bottom: 4.5rem;
  }
  .header__menu a:hover {
    text-decoration: none;
    color: #11ded9;
    border-color: #11ded9;
  }
  .header__menu p {
    font-family: 'Raleway';
    color: #f4f4f4;
    margin-top: 0;
    margin-left: 1.5rem;
    margin-right: .5rem;
  }
  .header__menu .menu__links {
    text-decoration: none;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
        flex-wrap: wrap;
    -webkit-box-orient: horizontal;
    -webkit-box-direction: normal;
        -ms-flex-direction: row;
            flex-direction: row;
  }
  @media only screen and (min-width: 40em) {
    .header__menu .menu__links {
      -webkit-box-orient: vertical;
      -webkit-box-direction: normal;
          -ms-flex-direction: column;
              flex-direction: column;
    }
  }
  .header__menu .menu__links li {
    font-family: 'Avenir';
    font-weight: 700;
    text-transform: uppercase;
    text-decoration: none;
    margin-bottom: 0.75em;
    padding-right: 4rem;
    font-size: 18px;
  }
  @media only screen and (min-width: 30em) {
    .header__menu .menu__links li {
      font-size: 24px;
    }
  }
  .header__toggle {
  position: fixed;
  -webkit-transform: translate3d(0, 0, 0);
          transform: translate3d(0, 0, 0);
  top: 0;
  left: 0;
  padding: 0.75rem;
  padding-top: 1.25rem;
  cursor: pointer;
  line-height: 1;
  z-index: 1000;
  border: none;
  background-color: transparent;
}


.header__toggle svg {
  stroke: #282828;
}
.header__toggle svg:hover {
  stroke: #11ded9;
}
.header__toggle span:first-of-type {
  display: block;
}
.header__toggle span:last-of-type {
  display: none;
}
.header__toggle.is-visible span:first-of-type {
  display: none;
}
.header__toggle.is-visible span:last-of-type {
  display: block;
}
.header__toggle.is-visible span svg {
  stroke: #f4f4f4;
}
</style>