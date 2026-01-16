<template>
  <div class="microempresa-register">
    <!-- Marcador de pasos -->
    <div class="steps-indicator">
      <div class="step" :class="{ 'active': step === 1 }">Paso 1: Cuenta</div>
      <div class="step" :class="{ 'active': step === 2 }">Paso 2: Plan</div>
      <div class="step" :class="{ 'active': step === 3 }">Paso 3: Empresa</div>
    </div>

    <div class="form-container">
      <!-- Paso 1: Registro de cuenta -->
      <div v-if="step === 1" class="form-step" :class="{ 'slide-in': step === 1 }">
        <h2>Crear Cuenta</h2>

        <!-- Usuario -->
        <div class="input-group">
          <input v-model="username" type="text" placeholder="Carnet Identidad" required />
        </div>

        <!-- Correo electrónico -->
        <div class="input-group">
          <input v-model="email" type="email" placeholder="Correo Electrónico" required />
        </div>

        <!-- Nombre -->
        <div class="input-group">
          <input v-model="firstName" type="text" placeholder="Nombre" required />
        </div>

        <!-- Apellido -->
        <div class="input-group">
          <input v-model="lastName" type="text" placeholder="Apellido" required />
        </div>

        <!-- Contraseña -->
        <div class="input-group password-field">
          <input v-model="password" :type="passwordVisible ? 'text' : 'password'" placeholder="Contraseña" required />
          <span @click="togglePassword" class="eye-icon">
            <i :class="passwordVisible ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
          </span>
        </div>

        <!-- Confirmar Contraseña -->
        <div class="input-group password-field">
          <input v-model="confirmPassword" :type="confirmPasswordVisible ? 'text' : 'password'"
            placeholder="Confirmar Contraseña" required />
          <span @click="toggleConfirmPassword" class="eye-icon">
            <i :class="confirmPasswordVisible ? 'fas fa-eye-slash' : 'fas fa-eye'"></i>
          </span>
        </div>

        <button @click="submitForm">Siguiente</button>
      </div>

      <!-- Paso 2: Selección de plan -->
      <div v-if="step === 2" class="form-step" :class="{ 'slide-in': step === 2 }">
        <h2>Selecciona tu Plan</h2>
        <div class="plans">
          <div v-for="plan in plans" :key="plan.id" class="plan-card"
            :class="{ 'plan-selected': selectedPlan && selectedPlan.id === plan.id }" @click="selectPlan(plan)">
            <h4>{{ plan.name }}</h4>
            <p>{{ plan.description }}</p>
            <p>Bs {{ plan.price }} / mes</p>
          </div>
        </div>
        <div class="button-container">
          <button @click="previousStep">Atrás</button>
          <button @click="nextStep" :disabled="!selectedPlan">Siguiente</button>
        </div>
      </div>

      <!-- Pantalla de confirmación -->
      <div v-if="step === 3" class="confirmation">
        <h3>¡Registro Completado!</h3>
        <p>Tu cuenta ha sido registrada. Ahora, inicia sesión para crear la empresa correspondiente y realizar el pago.
        </p>
        <button @click="backToLogin">Volver al login</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      step: 1,
      username: '',
      email: '',
      firstName: '',
      lastName: '',
      password: '',
      confirmPassword: '',
      formData: {
        companyName: '',
        address: '',
        phone: '',
        plan: ''
      },
      passwordVisible: false,
      confirmPasswordVisible: false,
      plans: [],
      selectedPlan: null
    };
  },
  mounted() {
    this.fetchPlans();
  },
  methods: {
    nextStep() {
      if (this.step < 4) {
        this.step++;
      }
    },
    previousStep() {
      if (this.step > 1) {
        this.step--;
      }
    },
    async fetchPlans() {
      try {
        const response = await axios.get('https://hs1sbz-ip-190-181-17-18.tunnelmole.net/api/auth/getAllPlans');
        this.plans = response.data;
      } catch (error) {
        console.error('Error al obtener los planes:', error);
      }
    },
    selectPlan(plan) {
      this.selectedPlan = plan;
    },
    async submitForm() {
      if (this.password !== this.confirmPassword) {
        alert('Las contraseñas no coinciden');
        return;
      }

      // Enviar los datos del formulario a la API para registrar al usuario
      try {
        const response = await axios.post('https://hs1sbz-ip-190-181-17-18.tunnelmole.net/api/auth/registerUsers', {
          carnet_identidad: this.username, // Suponemos que el carnet de identidad es el nombre de usuario
          nombre_completo: `${this.firstName} ${this.lastName}`,
          email: this.email,
          password: this.password,
          role_id: 2, // Asignamos el rol de "microempresa" como ejemplo
        });

        if (response.status === 201) {
          console.log('Usuario registrado:', response.data);
          this.step = 3; // Avanzar a la pantalla de confirmación
        }
      } catch (error) {
        console.error('Error al registrar el usuario:', error);
        alert('Hubo un problema al registrar el usuario. Por favor, inténtalo de nuevo.');
      }
    },
    backToLogin() {
      this.$router.push('/login');
    },
    togglePassword() {
      this.passwordVisible = !this.passwordVisible;
    },
    toggleConfirmPassword() {
      this.confirmPasswordVisible = !this.confirmPasswordVisible;
    }
  }
};
</script>

<style scoped lang="scss">
.microempresa-register {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-color: #f9f9f9;

  .steps-indicator {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-bottom: 20px;

    .step {
      padding: 10px 20px;
      border-radius: 25px;
      background-color: #d0d0d0;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    .step.active {
      background-color: #2980b9;
      color: white;
    }
  }

  .form-container {
    width: 100%;
    max-width: 600px;
    background-color: #fff;
    padding: 30px;
    border-radius: 8px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    text-align: center;

    .form-step {
      display: none;
    }

    .form-step.slide-in {
      display: block;
      animation: slideIn 0.5s ease-out;
    }

    @keyframes slideIn {
      from {
        transform: translateX(100%);
      }

      to {
        transform: translateX(0);
      }
    }

    input {
      width: 100%;
      padding: 12px;
      margin-bottom: 20px;
      border: 1px solid #ddd;
      border-radius: 6px;
    }

    .plans {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: center;
    }

    .plan-card {
      padding: 20px;
      border: 1px solid #ddd;
      border-radius: 8px;
      cursor: pointer;
      width: 200px;
      text-align: center;
      transition: background-color 0.3s;
    }

    .plan-card:hover {
      background-color: #f0f0f0;
    }

    .plan-selected {
      background-color: #2980b9;
      color: white;
    }

    .button-container {
      display: flex;
      justify-content: space-between;
    }

    button {
      padding: 12px 20px;
      background-color: #2980b9;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    button:hover {
      background-color: #1f6392;
    }

    .confirmation {
      text-align: center;

      h3 {
        color: #333;
        margin-bottom: 20px;
      }
    }
  }
}
</style>