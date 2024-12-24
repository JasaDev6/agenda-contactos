<template>
  <div class="app">
    <h1>Agenda de Contactos</h1>

    <!-- Filtro -->
    <div class="filter">
      <input
        type="text"
        v-model="searchName"
        placeholder="Buscar por nombre"
        @input="filterContacts"
      />
      <input
        type="text"
        v-model="searchEmail"
        placeholder="Buscar por correo electrónico"
        @input="filterContacts"
      />
    </div>

    <!-- Lista de contactos -->
    <ul>
      <li v-for="contact in filteredContacts" :key="contact.id">
        <table>
          <tr>
            <th>{{ contact.name }}</th>
            <th>{{ contact.email }}</th>
            <th>{{ contact.address }}</th>
            <th>{{ contact.phone }}</th>
            <th>{{ contact.country }}</th>
            <th>{{ contact.city }}</th>
          </tr>
        </table>
        <button @click="editContact(contact)">Editar</button>
        <button @click="deleteContact(contact.id)">Eliminar</button>
      </li>
    </ul>

    <!-- Formulario para agregar o editar contacto -->
    <div>
      <h2>{{ isEditing ? "Editar Contacto" : "Nuevo Contacto" }}</h2>
      <form @submit.prevent="saveContact">
        <input
          type="text"
          v-model="currentContact.name"
          placeholder="Nombre"
          required
        />
        <input
          type="email"
          v-model="currentContact.email"
          placeholder="Correo Electrónico"
          required
        />
        <input
          type="text"
          v-model="currentContact.address"
          placeholder="Dirección"
          required
        />
        <input
          type="tel"
          v-model="currentContact.phone"
          placeholder="Teléfono"
          required
        />
        <input
          type="text"
          v-model="currentContact.country"
          placeholder="País"
          required
        />
        <input
          type="text"
          v-model="currentContact.city"
          placeholder="Ciudad"
          required
        />
        <button type="submit">{{ isEditing ? "Guardar cambios" : "Agregar Contacto" }}</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Lista de contactos
      contacts: [
        {
          id: 1,
          name: "Alice Johnson",
          email: "alice.johnson@example.com",
          address: "123 Maple Street",
          phone: "123-456-7890",
          country: "USA",
          city: "New York"
        },
        {
          id: 2,
          name: "Bob Smith",
          email: "bob.smith@example.com",
          address: "456 Oak Avenue",
          phone: "987-654-3210",
          country: "Canada",
          city: "Toronto"
        },
        {
          id: 3,
          name: "Carol White",
          email: "carol.white@example.com",
          address: "789 Pine Road",
          phone: "555-123-4567",
          country: "UK",
          city: "London"
        },
        {
          id: 4,
          name: "David Brown",
          email: "david.brown@example.com",
          address: "321 Elm Street",
          phone: "444-555-6666",
          country: "Australia",
          city: "Sydney"
        },
        {
          id: 5,
          name: "Emily Davis",
          email: "emily.davis@example.com",
          address: "654 Spruce Lane",
          phone: "333-444-5555",
          country: "USA",
          city: "Los Angeles"
        }
      ],
      currentContact: {
        id: null,
        name: "",
        email: "",
        address: "",
        phone: "",
        country: "",
        city: ""
      },
      searchName: "",
      searchEmail: "",
      isEditing: false
    };
  },
  computed: {
    filteredContacts() {
      return this.contacts.filter(contact => {
        return (
          contact.name.toLowerCase().includes(this.searchName.toLowerCase()) &&
          contact.email.toLowerCase().includes(this.searchEmail.toLowerCase())
        );
      });
    }
  },
  methods: {
    // Guardar nuevo contacto o editar uno existente
    saveContact() {
      if (this.isEditing) {
        const index = this.contacts.findIndex(contact => contact.id === this.currentContact.id);
        if (index !== -1) {
          this.contacts[index] = { ...this.currentContact };
        }
      } else {
        this.currentContact.id = Date.now(); // Generar un ID único
        this.contacts.push({ ...this.currentContact });
      }

      // Limpiar formulario
      this.resetForm();
    },
    // Editar un contacto
    editContact(contact) {
      this.currentContact = { ...contact };
      this.isEditing = true;
    },
    // Eliminar un contacto
    deleteContact(id) {
      this.contacts = this.contacts.filter(contact => contact.id !== id);
    },
    // Restablecer el formulario
    resetForm() {
      this.currentContact = {
        id: null,
        name: "",
        email: "",
        address: "",
        phone: "",
        country: "",
        city: ""
      };
      this.isEditing = false;
    },
    // Filtrar contactos
    filterContacts() {
      // La computada `filteredContacts` ya realiza el filtrado
    }
  }
};
</script>

<style>
/* Agregar algunos estilos básicos */
.app {
  max-width: 600px;
  margin: auto;
  padding: 20px;
}

h1, h2 {
  text-align: center;
}

.filter {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

input {
  padding: 8px;
  margin-right: 10px;
  width: 45%;
}

button {
  padding: 5px 10px;
  margin-left: 10px;
  cursor: pointer;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 10px 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

table, th, td {
  border: 1px solid black;
}
</style>
