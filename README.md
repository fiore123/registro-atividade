# 🚛 Sistema de Controle de Pátio e Registro de Motoristas

Este projeto é uma **Single Page Application (SPA)** robusta desenvolvida para gerenciar o fluxo de entrada e saída de veículos de carga em centros de distribuição ou pátios logísticos. O sistema oferece interfaces distintas para motoristas (autoatendimento) e administradores (gestão e relatórios).

## 📋 Índice

1. [Visão Geral](#-visão-geral)
2. [Funcionalidades](#-funcionalidades)
3. [Tecnologias e Ferramentas](#-tecnologias-e-ferramentas)
4. [Pré-requisitos](#-pré-requisitos)
5. [Configuração e Instalação](#-configuração-e-instalação)
6. [Guia de Uso](#-guia-de-uso)
7. [Estrutura do Banco de Dados](#-estrutura-do-banco-de-dados)

---

## 🔭 Visão Geral

O sistema resolve o problema de controle de acesso manual, digitalizando o processo de portaria. Ele permite que motoristas registrem sua chegada utilizando seus próprios dispositivos (ou um tablet na portaria), validando a localização via GPS. Para a gestão, oferece um dashboard com KPIs em tempo real, gráficos e relatórios exportáveis.

---

## 🚀 Funcionalidades

### 👤 Para o Motorista (Visão Pública)
*   **Auto-Registro Inteligente:** Cadastro de Nome, CPF, Transportadora e Placas (Cavalo/Carreta).
*   **Busca Automática de Histórico:** Ao digitar o CPF, o sistema preenche automaticamente os dados se o motorista já tiver acessado o local anteriormente.
*   **Validação por Geolocalização:** O registro de entrada e saída exige permissão de GPS para garantir que o motorista está fisicamente no local.
*   **Cronômetro de Permanência:** Feedback visual do tempo decorrido desde a entrada.
*   **Modo Escuro/Claro:** Interface adaptável à preferência do usuário.
*   **Tutorial Interativo:** Guia passo-a-passo para novos usuários.

### 🛡️ Para o Administrador (Visão Privada)
*   **Dashboard Analítico:**
    *   KPIs em tempo real (Motoristas no pátio, Entradas do dia, Tempo médio de permanência).
    *   Gráfico de Pizza: Distribuição por tipo de veículo (Perfil).
    *   Gráfico de Barras: Top 5 Transportadoras mais frequentes.
*   **Histórico de Atividades (Log):**
    *   Tabela completa com filtros avançados (Nome, CPF, Placa, Data, Status).
    *   Links diretos para o Google Maps com as coordenadas de entrada e saída.
    *   **Exportação para CSV/Excel.**
*   **Gestão de Registros:**
    *   Ajuste manual de horários (caso o motorista esqueça de registrar).
    *   Funcionalidade de "Forçar Saída" ou "Reabrir Registro".
    *   Adição de observações em registros específicos.
*   **Gestão de Cadastros (CRUD):**
    *   Adicionar/Remover Transportadoras.
    *   Adicionar/Remover Perfis de Veículos (ex: VUC, Carreta, Toco).
    *   Gestão de usuários Administradores (com níveis Master e Admin).
*   **Sistema de Alertas:** Notificação visual para veículos que excederam 24 horas de permanência no pátio.

---

## 🛠 Tecnologias e Ferramentas

O projeto foi construído utilizando uma arquitetura *serverless* moderna e leve, sem necessidade de build tools complexos para execução imediata.

*   **Frontend:**
    *   **HTML5 & JavaScript (ES6+ Modules):** Lógica central da aplicação.
    *   **Tailwind CSS (via CDN):** Estilização utilitária, responsiva e suporte nativo a Dark Mode.
    *   **Chart.js:** Renderização de gráficos interativos no dashboard.
    *   **Google Fonts (Poppins):** Tipografia moderna.

*   **Backend & Persistência (BaaS):**
    *   **Firebase Authentication:** Gestão de login de administradores e autenticação anônima para motoristas.
    *   **Firebase Firestore:** Banco de dados NoSQL em tempo real para armazenar logs, perfis e configurações.

---

## 📦 Pré-requisitos

Para rodar este projeto, você precisará de:

1.  Um navegador moderno (Chrome, Firefox, Edge, Safari).
2.  Uma conta no **Google Firebase**.
3.  Um servidor local simples (Recomendado devido ao uso de `ES Modules`).

---

## ⚙️ Configuração e Instalação

### 1. Configurar o Firebase
1.  Crie um projeto no [Console do Firebase](https://console.firebase.google.com/).
2.  **Authentication:** Ative o provedor "Email/Password" e "Anonymous" (Anônimo).
3.  **Firestore Database:** Crie um banco de dados no modo de produção ou teste.
4.  **Regras de Segurança (Firestore):** Configure regras que permitam leitura pública para dados necessários, mas escrita restrita.

### 2. Atualizar o Código
No arquivo HTML principal, localize a constante `firebaseConfig` dentro da tag `<script type="module">` e substitua pelos dados do seu projeto:

```javascript
const firebaseConfig = {
  apiKey: "SUA_API_KEY",
  authDomain: "SEU_PROJETO.firebaseapp.com",
  projectId: "SEU_PROJECT_ID",
  storageBucket: "SEU_PROJETO.appspot.com",
  messagingSenderId: "SEU_SENDER_ID",
  appId: "SEU_APP_ID"
};
```

### 3. Criar o Primeiro Admin (Master)
Para o primeiro acesso administrativo, crie manualmente um documento na coleção `artifacts > [APP_ID] > public > data > admin_users` no Firestore com o ID do usuário (UID gerado no Authentication) e os campos:
*   `username`: (seu email)
*   `displayName`: (seu nome)
*   `role`: "master"

---

## 📖 Guia de Uso

### Fluxo do Motorista
1.  O motorista acessa o link da aplicação.
2.  Insere o CPF. O sistema busca dados anteriores automaticamente.
3.  Completa/Confirma Nome, Transportadora e Placa.
4.  Clica em **Registrar Entrada**.
5.  O navegador solicita permissão de GPS -> Motorista aceita.
6.  Tela de "Sessão Ativa" aparece com o cronômetro.
7.  Ao sair, o motorista clica em **Registrar Saída**.

### Fluxo do Administrador
1.  Clica no botão "Admin" no topo da página.
2.  Faz login com credenciais cadastradas.
3.  **Dashboard:** Visualiza o resumo do dia e gráficos.
4.  **Histórico:** Monitora atividades, ajusta registros manuais e exporta dados.
5.  **Cadastros:** Gerencia as listas de Perfis e Transportadoras.

---

## 🗂 Estrutura do Banco de Dados (Firestore)

O sistema utiliza uma estrutura aninhada (Pattern de "Artifacts") para organização:

Caminho Base: `artifacts/[FIREBASE_PROJECT_ID]/public/data/`

Coleções:
1.  **`arrivals`**: Logs de entrada e saída.
2.  **`active_cpfs`**: Controle de sessão para impedir duplicidade de entrada por CPF.
3.  **`driver_profiles`**: Cache dos dados do motorista para preenchimento automático.
4.  **`companies`**: Lista de transportadoras.
5.  **`profiles`**: Lista de perfis de veículos.
6.  **`admin_users`**: Perfis de permissão dos administradores.
