<script>
/**
 * @component navbar
 * @description Main navigation bar component with responsive slide drawer menu
 * Integrates PrimeVue components and i18n translations
 */
import LanguageSwitcher from "./language-switcher.component.vue";

export default {
  name: "navbar",
  components: {
    LanguageSwitcher
  },
  data() {
    return {
      menu: [
        { label: 'option.home', to: '/home' },
        { label: 'option.myMachines', to: '/equipment' },
        { label: 'option.myServiceRequests', to: '/service-requests' },
        { label: 'option.rent', to: '/rental' },
        { label: 'option.plans', to: '/plans' },
        { label: 'option.contact', to: '/contact' },
      ],
      mobileMenuOpen: false
    }
  },
  methods: {
    toggleMobileMenu() {
      this.mobileMenuOpen = !this.mobileMenuOpen;
    },
    closeMobileMenu() {
      this.mobileMenuOpen = false;
    },
    goNotifications(){
      this.$router.push({ name: 'notifications' });
      this.closeMobileMenu();
    }
  }
}
</script>

<template>
  <header class="navbar-container">
    <div class="navbar">
      <!-- Logo section -->
      <div class="navbar-brand">
        <router-link to="/home" class="logo-link">
          <div class="logo">
            <!-- FrostLink Logo -->
            <img src="/src/public/images/FrostLinkLogo.jpeg" alt="FrostLink Logo" class="logo-image" />
            <div class="logo-text">
              <span class="company-name">FrostLink</span>
              <span class="company-slogan">Smart Cold Chain Management</span>
            </div>
          </div>
        </router-link>
      </div>

      <!-- Hamburger menu button - Only visible on mobile -->
      <pv-button
          icon="pi pi-bars"
          class="p-button-text hamburger-button"
          @click="toggleMobileMenu"
          :aria-label="$t('mobile.menu')"
      />

      <!-- Desktop menu and actions -->
      <div class="desktop-menu-container">
        <!-- Menu items -->
        <nav class="navbar-menu">
          <pv-button v-for="item in menu"
                     :key="item.label"
                     class="p-button-text menu-button"
                     as-child
                     v-slot="slotProps">
            <router-link :to="item.to" :class="slotProps['class']" class="menu-item">
              {{ $t(item.label) }}
            </router-link>
          </pv-button>
        </nav>

        <!-- User section -->
        <div class="navbar-actions">
          <!-- Language Switcher -->
          <div class="language-switcher">
            <LanguageSwitcher />
          </div>

          <!-- Notification Bell -->
          <pv-button icon="pi pi-bell"
                     class="p-button-text p-button-rounded notification-button"
                     @click="goNotifications"/>

          <!-- User Profile -->
          <pv-button icon="pi pi-user" class="p-button-text p-button-rounded user-button" />
        </div>
      </div>
    </div>

    <!-- Mobile menu drawer -->
    <transition name="slide">
      <div v-if="mobileMenuOpen" class="mobile-menu-backdrop" @click="closeMobileMenu">
        <div class="mobile-menu-drawer" @click.stop>
          <!-- Close button -->
          <div class="mobile-menu-header">
            <h3>{{ $t('mobile.menu') }}</h3>
            <pv-button icon="pi pi-times"
                       class="p-button-text p-button-rounded close-button"
                       @click="closeMobileMenu"
                       :aria-label="$t('mobile.close') || 'Close'" />
          </div>

          <!-- Menu items -->
          <nav class="mobile-nav">
            <router-link v-for="item in menu"
                         :key="item.label"
                         :to="item.to"
                         class="mobile-menu-item"
                         @click="closeMobileMenu">
              <i class="pi pi-angle-right menu-icon"></i>
              {{ $t(item.label) }}
            </router-link>
          </nav>

          <!-- Divider -->
          <div class="menu-divider"></div>

          <!-- Language switcher -->
          <div class="mobile-section">
            <h4>{{ $t('option.language') }}</h4>
            <LanguageSwitcher />
          </div>

          <!-- User actions -->
          <div class="mobile-section">
            <pv-button
                icon="pi pi-bell"
                :label="$t('mobile.notifications')"
                class="p-button-text p-button-plain mobile-action-button"
                @click="goNotifications" />
            <pv-button
                icon="pi pi-user"
                :label="$t('mobile.myAccount')"
                class="p-button-text p-button-plain mobile-action-button" />
          </div>
        </div>
      </div>
    </transition>
  </header>
</template>

<style scoped>
.navbar-container {
  background: linear-gradient(135deg, #ffffff 0%, #f8fafc 100%);
  box-shadow: 0 4px 20px rgba(0, 123, 255, 0.1);
  position: relative;
  z-index: 100;
  border-bottom: 1px solid rgba(0, 123, 255, 0.1);
}

.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 2rem;
  max-width: 1400px;
  margin: 0 auto;
  position: relative;
}

.navbar-brand {
  display: flex;
  align-items: center;
}

.logo-link {
  text-decoration: none;
  transition: transform 0.2s ease;
}

.logo-link:hover {
  transform: translateY(-1px);
  text-decoration: none;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.logo-image {
  width: 40px;
  height: 40px;
  border-radius: 8px;
  object-fit: contain;
  transition: transform 0.2s ease;
}

.logo:hover .logo-image {
  transform: scale(1.05);
}

.logo-text {
  display: flex;
  flex-direction: column;
}

.company-name {
  font-size: 1.6rem;
  font-weight: 700;
  background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  letter-spacing: -0.5px;
}

.company-slogan {
  font-size: 0.75rem;
  color: #6c757d;
  font-weight: 500;
  margin-top: -2px;
}

.desktop-menu-container {
  display: flex;
  align-items: center;
  gap: 2rem;
}

.navbar-menu {
  display: flex;
  gap: 0.25rem;
  background: rgba(248, 250, 252, 0.8);
  padding: 0.5rem;
  border-radius: 12px;
  backdrop-filter: blur(10px);
}

.menu-button {
  margin: 0;
}

.menu-item {
  color: #495057;
  text-decoration: none;
  font-weight: 600;
  position: relative;
  padding: 0.75rem 1.25rem;
  border-radius: 8px;
  transition: all 0.3s ease;
  font-size: 0.9rem;
}

.menu-item:hover {
  color: #007bff;
  background: rgba(0, 123, 255, 0.1);
  text-decoration: none;
  transform: translateY(-1px);
}

.menu-item:hover::after {
  content: '';
  position: absolute;
  bottom: 0.25rem;
  left: 50%;
  transform: translateX(-50%);
  width: 60%;
  height: 2px;
  background: linear-gradient(90deg, #007bff 0%, #0056b3 100%);
  border-radius: 1px;
}

.navbar-actions {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.language-switcher {
  margin-right: 0.5rem;
}

/* Customize PrimeVue button styles */
:deep(.notification-button), :deep(.user-button) {
  padding: 0.75rem;
  color: #6c757d;
  border-radius: 10px;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
}

:deep(.notification-button:hover), :deep(.user-button:hover) {
  color: #007bff;
  background: rgba(0, 123, 255, 0.1);
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 123, 255, 0.2);
}

:deep(.notification-button:hover::before), :deep(.user-button:hover::before) {
  content: '';
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
  transition: left 0.5s;
}

:deep(.notification-button:hover::before), :deep(.user-button:hover::before) {
  left: 100%;
}

/* Hamburger button - Hidden by default */
.hamburger-button {
  display: none;
  color: #007bff;
  padding: 0.75rem;
  border-radius: 10px;
  transition: all 0.3s ease;
}

.hamburger-button:hover {
  background: rgba(0, 123, 255, 0.1);
  transform: scale(1.05);
}

/* Mobile menu drawer styles */
.mobile-menu-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.6);
  z-index: 999;
  backdrop-filter: blur(8px);
}

.mobile-menu-drawer {
  position: fixed;
  top: 0;
  right: 0;
  width: 320px;
  max-width: 85vw;
  height: 100vh;
  background: linear-gradient(180deg, #ffffff 0%, #f8fafc 100%);
  box-shadow: -8px 0 32px rgba(0, 123, 255, 0.15);
  overflow-y: auto;
  z-index: 1000;
  border-left: 1px solid rgba(0, 123, 255, 0.1);
}

.mobile-menu-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2rem 1.5rem;
  border-bottom: 1px solid rgba(0, 123, 255, 0.1);
  background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
  color: white;
}

.mobile-menu-header h3 {
  margin: 0;
  font-size: 1.4rem;
  font-weight: 700;
  letter-spacing: -0.5px;
}

.close-button {
  padding: 0.75rem;
  color: white;
  border-radius: 10px;
  transition: all 0.3s ease;
}

.close-button:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}

.mobile-nav {
  padding: 1.5rem 0;
}

.mobile-menu-item {
  display: flex;
  align-items: center;
  padding: 1rem 1.5rem;
  color: #495057;
  text-decoration: none;
  font-size: 1rem;
  font-weight: 600;
  transition: all 0.3s ease;
  margin: 0 1rem;
  border-radius: 10px;
}

.mobile-menu-item:hover {
  background: rgba(0, 123, 255, 0.1);
  color: #007bff;
  transform: translateX(8px);
}

.menu-icon {
  margin-right: 1rem;
  font-size: 0.9rem;
  color: #007bff;
  transition: all 0.3s ease;
}

.mobile-menu-item:hover .menu-icon {
  transform: scale(1.2);
  color: #0056b3;
}

.menu-divider {
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(0, 123, 255, 0.2), transparent);
  margin: 2rem 1.5rem;
}

.mobile-section {
  padding: 1.5rem;
}

.mobile-section h4 {
  margin: 0 0 1.5rem 0;
  color: #495057;
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: 700;
}

.mobile-action-button {
  width: 100%;
  justify-content: flex-start;
  padding: 1rem 1.25rem;
  margin: 0.5rem 0;
  border-radius: 12px;
  transition: all 0.3s ease;
  font-weight: 600;
  color: #495057;
}

.mobile-action-button:hover {
  background: rgba(0, 123, 255, 0.1);
  color: #007bff;
  transform: translateX(8px);
}

/* Slide transition */
.slide-enter-active .mobile-menu-drawer,
.slide-leave-active .mobile-menu-drawer {
  transition: transform 0.3s ease;
}

.slide-enter-from .mobile-menu-drawer,
.slide-leave-to .mobile-menu-drawer {
  transform: translateX(100%);
}

.slide-enter-active .mobile-menu-backdrop,
.slide-leave-active .mobile-menu-backdrop {
  transition: opacity 0.3s ease;
}

.slide-enter-from .mobile-menu-backdrop,
.slide-leave-to .mobile-menu-backdrop {
  opacity: 0;
}

/* Responsive styles */
@media (max-width: 768px) {
  .desktop-menu-container {
    display: none;
  }

  .hamburger-button {
    display: inline-flex;
    color: #007bff;
  }

  .navbar {
    padding: 1rem;
  }

  .company-slogan {
    display: none;
  }

  .logo {
    gap: 0.5rem;
  }

  .logo-image {
    width: 36px;
    height: 36px;
  }

  .company-name {
    font-size: 1.4rem;
  }
}

@media (max-width: 480px) {
  .navbar {
    padding: 0.75rem;
  }

  .logo-image {
    width: 32px;
    height: 32px;
  }

  .company-name {
    font-size: 1.2rem;
  }
}

/* Prevent body scroll when menu is open */
body.mobile-menu-open {
  overflow: hidden;
}
</style>