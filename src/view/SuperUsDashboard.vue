<template>
  <div class="superuser-dashboard-container">
    <!-- Barra lateral -->
    <aside class="sidebar">
      <div class="sidebar-header">
        <i class="fas fa-cogs"></i>
        <span class="sidebar-title">DASHBOARD DE SUPERVISOR</span>
      </div>
      <nav class="sidebar-nav">
        <ul>
          <li @click="changeSection('users')">Usuarios</li>
          <li @click="changeSection('microempresa')">Microempresas</li>
          <li @click="changeSection('subscriptions')">Planes y Suscripciones</li>
        </ul>
      </nav>
    </aside>

    <!-- Contenido principal -->
    <main class="main-content">
      <header class="main-header">
        <h1>BIENVENIDO, SUPERVISOR</h1>
        <p>Gestiona los usuarios, microempresas y suscripciones desde aquí.</p>
      </header>

      <!-- Sección de Usuarios -->
      <section v-if="currentSection === 'users'" class="section-users">
        <h2>Gestión de Usuarios</h2>
        <div class="table-container">
          <table>
            <thead>
              <tr>
                <th>Nombre</th>
                <th>Email</th>
                <th>Rol</th>
                <th>Estado</th>
                <th>Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="user in users" :key="user.id">
                <td>{{ user.name }}</td>
                <td>{{ user.email }}</td>
                <td>{{ user.role }}</td>
                <td>
                  <span :class="user.status === 'active' ? 'status-active' : 'status-inactive'">
                    {{ user.status }}
                  </span>
                </td>
                <td>
                  <button @click="toggleUserStatus(user)" class="btn-action">
                    <i class="fas" :class="user.status === 'active' ? 'fa-toggle-off' : 'fa-toggle-on'"></i>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>

      <!-- Sección de Microempresas -->
      <section v-if="currentSection === 'microempresa'" class="section-microempresa">
        <h2>Gestión de Microempresas</h2>
        <div class="table-container">
          <table>
            <thead>
              <tr>
                <th>Nombre de Empresa</th>
                <th>Email de Contacto</th>
                <th>Estado</th>
                <th>Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="empresa in empresas" :key="empresa.id">
                <td>{{ empresa.name }}</td>
                <td>{{ empresa.contactEmail }}</td>
                <td>
                  <span :class="empresa.status === 'active' ? 'status-active' : 'status-inactive'">
                    {{ empresa.status }}
                  </span>
                </td>
                <td>
                  <button @click="toggleEmpresaStatus(empresa)" class="btn-action">
                    <i class="fas" :class="empresa.status === 'active' ? 'fa-toggle-off' : 'fa-toggle-on'"></i>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>

      <!-- Sección de Planes y Suscripciones -->
      <section v-if="currentSection === 'subscriptions'" class="section-subscriptions">
        <h2>Gestión de Planes y Suscripciones</h2>
        <div class="table-container">
          <table>
            <thead>
              <tr>
                <th>Plan</th>
                <th>Precio</th>
                <th>Estado</th>
                <th>Acciones</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="plan in plans" :key="plan.id">
                <td>{{ plan.name }}</td>
                <td>{{ plan.price }} Bs</td>
                <td>
                  <span :class="plan.status === 'active' ? 'status-active' : 'status-inactive'">
                    {{ plan.status }}
                  </span>
                </td>
                <td>
                  <button @click="togglePlanStatus(plan)" class="btn-action">
                    <i class="fas" :class="plan.status === 'active' ? 'fa-toggle-off' : 'fa-toggle-on'"></i>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentSection: 'users', // Controla la sección actual
      users: [
        { id: 1, name: 'Juan Pérez', email: 'juan@example.com', role: 'Vendedor', status: 'active' },
        { id: 2, name: 'Ana Gómez', email: 'ana@example.com', role: 'Vendedor', status: 'inactive' }
      ],
      empresas: [
        { id: 1, name: 'Empresa 1', contactEmail: 'empresa1@example.com', status: 'active' },
        { id: 2, name: 'Empresa 2', contactEmail: 'empresa2@example.com', status: 'inactive' }
      ],
      plans: [
        { id: 1, name: 'Básico', price: 70, status: 'active' },
        { id: 2, name: 'Premium', price: 150, status: 'inactive' }
      ]
    };
  },
  methods: {
    changeSection(section) {
      this.currentSection = section;
    },
    toggleUserStatus(user) {
      user.status = user.status === 'active' ? 'inactive' : 'active';
    },
    toggleEmpresaStatus(empresa) {
      empresa.status = empresa.status === 'active' ? 'inactive' : 'active';
    },
    togglePlanStatus(plan) {
      plan.status = plan.status === 'active' ? 'inactive' : 'active';
    }
  }
};
</script>

<style scoped lang="scss">
.superuser-dashboard-container {
  display: flex;
  height: 100vh;

  .sidebar {
    width: 250px;
    background-color: #1f3a64;
    color: white;
    padding: 20px;
    font-size: 16px;

    .sidebar-header {
      display: flex;
      align-items: center;
      margin-bottom: 40px;
      .sidebar-title {
        margin-left: 10px;
        font-weight: bold;
        font-size: 22px;
        text-transform: uppercase;
      }
      i {
        font-size: 30px;
      }
    }

    .sidebar-nav ul {
      list-style: none;
      padding: 0;
      li {
        margin: 15px 0;
        cursor: pointer;
        &:hover {
          color: #3498db;
        }
      }
    }
  }

  .main-content {
    flex-grow: 1;
    padding: 20px;
    background-color: #fff;

    h1 {
      font-size: 36px;
      color: #333;
      margin-bottom: 20px;
    }

    .section-users, .section-microempresa, .section-subscriptions {
      margin-bottom: 40px;

      h2 {
        font-size: 28px;
        margin-bottom: 20px;
      }

      .table-container {
        overflow-x: auto;
        table {
          width: 100%;
          border-collapse: collapse;
          th, td {
            padding: 10px;
            border: 1px solid #ddd;
            text-align: left;
          }
        }
      }

      .status-active {
        color: green;
      }

      .status-inactive {
        color: red;
      }

      .btn-action {
        background-color: transparent;
        border: none;
        color: #3498db;
        cursor: pointer;
        font-size: 20px;
      }

      .btn-action:hover {
        color: #1f6392;
      }
    }
  }
}
</style>


