# MockPay - Payment Gateway Simulator

A developer-focused payment gateway simulation platform that allows developers to test payment flows, webhooks, and failure scenarios without using real money.

![MockPay](./public/mockpay-logo.svg)

## Features

- 🔐 **Authentication** - Signup/login with simulated JWT auth
- 📁 **Project Management** - Create and manage multiple projects
- 🔑 **API Keys** - Generate unique API keys per project
- 💳 **Payment Simulation** - Create payments and simulate success/failure/timeout
- 🪝 **Webhook System** - Configure webhook URLs with secret keys
- 📊 **Analytics Dashboard** - View payment trends and statistics
- 🌙 **Dark/Light Mode** - Toggle between themes
- 🌐 **Multi-language** - English, Arabic (RTL), and Hindi support

## Tech Stack

- **Frontend**: React 18 + Vite
- **Routing**: React Router DOM
- **Charts**: Recharts
- **Icons**: Lucide React
- **i18n**: i18next + react-i18next
- **Storage**: localStorage (simulated backend)

## Getting Started

### Prerequisites

- Node.js 18+
- npm or yarn

### Installation

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Deployment to GitHub Pages

1. Update `vite.config.js` if deploying to a subdirectory:
   ```js
   base: '/your-repo-name/'
   ```

2. Install gh-pages (already included):
   ```bash
   npm install gh-pages --save-dev
   ```

3. Deploy:
   ```bash
   npm run deploy
   ```

   Or manually:
   ```bash
   npm run build
   npx gh-pages -d dist
   ```

4. Go to your GitHub repo → Settings → Pages → Select "gh-pages" branch

## Project Structure

```
src/
├── components/
│   └── Layout/
│       ├── Header.jsx      # Theme toggle, language selector
│       ├── Layout.jsx      # Main layout wrapper
│       └── Sidebar.jsx     # Navigation sidebar
├── context/
│   ├── AuthContext.jsx     # Authentication state
│   └── ThemeContext.jsx    # Theme management
├── i18n/
│   └── index.js            # Translations (EN, AR, HI)
├── pages/
│   ├── Analytics.jsx       # Charts and metrics
│   ├── Dashboard.jsx       # Overview
│   ├── Login.jsx           # Login page
│   ├── PaymentPage.jsx     # Hosted payment simulation
│   ├── ProjectDetail.jsx   # Project management
│   ├── Projects.jsx        # Projects list
│   └── Signup.jsx          # Registration
├── services/
│   └── mockApi.js          # Simulated API layer
├── App.jsx                 # Routes
├── main.jsx                # Entry point
└── index.css               # Global styles
```

## Usage

1. **Sign up** with any email/password
2. **Create a project** from the Projects page
3. **View API keys** on the project detail page
4. **Create a payment** and click "Payment Page" to simulate
5. **Choose outcome**: Success, Failure, or Timeout
6. **View analytics** for payment trends

## License

MIT
