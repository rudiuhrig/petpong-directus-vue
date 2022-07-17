# PetPong

PetPong is a simple pet shop management application. This application is for learning purposes. 

# Run backend based on directus

## Directus Backend

This is a [Directus](https://directus.io) project bootstrapped with [`create-directus-project`](https://github.com/directus/directus/tree/main/packages/create-directus-project).

Admin credentials for this Directus instance:

- **Email** — `petpong@runxcorp.com`
- **Password** — `p3tp0ng`
  
### 📌 Prerequisites

- [Node.js](https://nodejs.dev) v12.20 and above

### 🚀 Getting Started

1. Clone this repo.

2. Install dependencies for this backend Directus project.

   ```shell
   cd directus
   npm install
   ```

3. Start the project locally.

   ```shell
   npx directus start
   ```

   Your Directus backend is now running at <http://localhost:8055>.

4. You can login with the admin credentials shown above to explore this Directus backend project.

### 🔗 Links

- [Official Site](https://directus.io)
- [Documentation](https://docs.directus.io)


# Run frontend based on vuejs

## Petgpong Directus Vue 

This is a [Vue 3](https://v3.vuejs.org) project bootstrapped with [`create-vue`](https://github.com/vuejs/create-vue).

### 📌 Prerequisites

You will need to have the provided [Directus project](../directus) running first before proceeding with this backend.

### 🚀 Getting Started

1. Clone this repo.

2. Install dependencies for this frontend.

   ```shell
   cd frontend
   npm install
   ```

3. Create a `.env` file by copying the provided `.env.example` file.

4. Start the development server.

   ```shell
   npm run dev
   ```

   Your Directus Vue frontend is now running at <http://localhost:3000>.

## 🔗 Links

#### Directus

- [Official Site](https://directus.io)
- [Documentation](https://docs.directus.io)

#### Vue

- [Official Site](https://v3.vuejs.org)
- [Documentation](https://v3.vuejs.org/guide/introduction.html)
