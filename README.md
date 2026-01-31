# MockPay - Payment Gateway Simulator

<div align="center">
  <img src="./public/mockpay-logo.svg" alt="MockPay Logo" width="80" height="80">
  
  **A developer-focused payment gateway simulation platform**
  
  Test payment flows, webhooks, and failure scenarios without using real money.
  
  [![Built with React](https://img.shields.io/badge/React-18-61DAFB?logo=react)](https://reactjs.org/)
  [![Vite](https://img.shields.io/badge/Vite-5-646CFF?logo=vite)](https://vitejs.dev/)
  [![GSAP](https://img.shields.io/badge/GSAP-Animations-88CE02)](https://greensock.com/gsap/)
  [![i18n](https://img.shields.io/badge/i18n-EN%20|%20AR%20|%20HI-blue)](https://react.i18next.com/)
</div>

---

## 🎯 Overview

MockPay simulates a complete payment gateway ecosystem, allowing developers to:
- Create merchant accounts and projects
- Generate test API keys
- Simulate payment transactions (success/failure/timeout)
- Test webhook integrations
- View analytics and reports

> **Note**: This is a frontend-only application designed for GitHub Pages deployment. All "backend" functionality is simulated using the Browser APIs.

---

## 🏗️ Architecture

### System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        MOCKPAY CLIENT                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐  ┌──────────────┐  ┌────────────────────────┐ │
│  │   React     │  │   React      │  │   React Router         │ │
│  │   Components│──│   Context    │──│   (HashRouter)         │ │
│  │             │  │   (State)    │  │                        │ │
│  └─────────────┘  └──────────────┘  └────────────────────────┘ │
│         │                │                      │               │
│         ▼                ▼                      ▼               │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                    Mock API Service                         ││
│  │  ┌─────────┐ ┌──────────┐ ┌─────────┐ ┌───────────────────┐││
│  │  │Projects │ │Payments  │ │Webhooks │ │    Analytics      │││
│  │  │   API   │ │   API    │ │   API   │ │       API         │││
│  │  └────┬────┘ └────┬─────┘ └────┬────┘ └────────┬──────────┘││
│  └───────┼───────────┼────────────┼───────────────┼───────────┘│
│          │           │            │               │             │
│          ▼           ▼            ▼               ▼             │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │                   localStorage                              ││
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────────────┐││
│  │  │mockpay_users │ │mockpay_      │ │mockpay_payments      │││
│  │  │              │ │projects      │ │mockpay_webhook_logs  │││
│  │  └──────────────┘ └──────────────┘ └──────────────────────┘││
│  └─────────────────────────────────────────────────────────────┘│
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Tech Stack

| Layer | Technology | Purpose |
|-------|------------|---------|
| **UI Framework** | React 18 | Component-based UI |
| **Build Tool** | Vite 5 | Fast development & bundling |
| **Routing** | React Router 6 | SPA navigation (HashRouter for GH Pages) |
| **State** | React Context | Global state management |
| **Styling** | Vanilla CSS | Custom design system |
| **Animations** | GSAP + ScrollTrigger | Premium animations & parallax |
| **Charts** | Recharts | Analytics visualizations |
| **i18n** | i18next | Multi-language support |
| **Icons** | Lucide React | SVG icon library |
| **Storage** | localStorage | Persistent data simulation |

---

## 🔧 Backend Simulation

Since this app runs on GitHub Pages (static hosting), all backend functionality is simulated on the client side.

### Data Models

```javascript
// User Model
User {
  id: string (UUID)
  email: string
  passwordHash: string (Base64 encoded)
  createdAt: ISO timestamp
}

// Project Model
Project {
  id: string (UUID)
  userId: string (FK → User)
  name: string
  apiKey: string (mk_test_...)
  webhookUrl: string
  webhookSecret: string (whsec_...)
  createdAt: ISO timestamp
}

// Payment Model
Payment {
  id: string (pay_...)
  projectId: string (FK → Project)
  amount: number
  currency: string (default: USD)
  customerEmail: string
  redirectUrl: string
  status: 'pending' | 'success' | 'failed' | 'timeout'
  createdAt: ISO timestamp
  updatedAt: ISO timestamp
}

// Webhook Log Model
WebhookLog {
  id: string (UUID)
  projectId: string (FK → Project)
  paymentId: string
  event: string (e.g., 'payment.success')
  url: string
  payload: JSON string
  status: 'sent' | 'failed'
  createdAt: ISO timestamp
}
```

### API Service Layer

The `src/services/mockApi.js` module provides a simulated REST API:

```javascript
// Projects API
projectsApi.getAll(userId)      // GET /projects
projectsApi.getById(projectId)  // GET /projects/:id
projectsApi.create(userId, name)// POST /projects
projectsApi.update(id, data)    // PUT /projects/:id
projectsApi.delete(projectId)   // DELETE /projects/:id

// Payments API
paymentsApi.getAll(projectId)         // GET /payments
paymentsApi.getAllForUser(userId)     // GET /users/:id/payments
paymentsApi.getById(paymentId)        // GET /payments/:id
paymentsApi.create(projectId, data)   // POST /payments
paymentsApi.updateStatus(id, status)  // PUT /payments/:id/status

// Webhooks API
webhookLogsApi.getAll(projectId)      // GET /webhooks
webhookLogsApi.create(projectId, data)// POST /webhooks

// Analytics API
analyticsApi.getStats(userId, days)   // GET /analytics
```

### Authentication Flow

```
┌──────────────┐     ┌───────────────┐     ┌─────────────────┐
│  Login Form  │────►│  AuthContext  │────►│  localStorage   │
│              │     │               │     │                 │
│  email       │     │  login()      │     │ mockpay_users   │
│  password    │     │  signup()     │     │ mockpay_current │
│              │     │  logout()     │     │     _user       │
└──────────────┘     └───────────────┘     └─────────────────┘
                            │
                            ▼
                     ┌──────────────┐
                     │ PrivateRoute │
                     │  Protected   │
                     │   Pages      │
                     └──────────────┘
```

**Security Notes:**
- Passwords are Base64 encoded (demo only - use bcrypt in production)
- Session persists in localStorage as `mockpay_current_user`
- JWT simulation via context-based authentication

### API Key Generation

```javascript
// Format: mk_test_[32 random alphanumeric chars]
generateApiKey() → "mk_test_xK7mN9pQ2rS4tU6vW8yA0bC3dE5fG7hJ"

// Webhook Secret Format: whsec_[24 random alphanumeric chars]  
generateWebhookSecret() → "whsec_aB3cD4eF5gH6iJ7kL8mN9o"
```

### Payment Flow Simulation

```
1. Create Payment
   ├── POST /payments/create
   ├── Generate payment_id (pay_...)
   ├── Store in localStorage
   └── Return payment_url: /#/pay/{payment_id}

2. Hosted Payment Page
   ├── User opens payment URL
   ├── Displays payment details
   └── User selects outcome: success | failure | timeout

3. Process Payment
   ├── Update payment status
   ├── Trigger webhook (if configured)
   │   ├── Create webhook log entry
   │   └── Payload: { payment, event: 'payment.{status}' }
   └── Redirect to merchant URL (if provided)
```

### Webhook System

```javascript
// Webhook payload structure
{
  payment: {
    id: "pay_abc123",
    projectId: "uuid",
    amount: 99.99,
    currency: "USD",
    status: "success",
    // ...
  },
  event: "payment.success"
}

// Webhook signature header (simulated)
X-MockPay-Signature: sha256=hmac(payload, webhookSecret)
```

---

## 🎨 UI/UX Features

### Design System

- **Typography**: Inter (body), Space Grotesk (headings), JetBrains Mono (code)
- **Colors**: Custom HSL palette with dark/light mode support
- **Effects**: Glassmorphism, mesh gradients, gradient icons
- **Animations**: GSAP-powered with ScrollTrigger for parallax

### Animations

```javascript
// Available animation hooks (src/hooks/useAnimations.js)
useScrollFadeIn()    // Fade in on scroll
useScrollStagger()   // Staggered children animation
useParallax(speed)   // Parallax scrolling effect
useTextReveal()      // Word-by-word text reveal
useMagneticHover()   // Magnetic button effect
useCountUp()         // Animated number counter
```

### Internationalization

| Language | Code | Direction |
|----------|------|-----------|
| English  | `en` | LTR       |
| Arabic   | `ar` | RTL       |
| Hindi    | `hi` | LTR       |

---

## 📁 Project Structure

```
mockpay/
├── public/
│   └── mockpay-logo.svg
├── src/
│   ├── components/
│   │   ├── Layout/
│   │   │   ├── Header.jsx
│   │   │   ├── Layout.jsx
│   │   │   ├── Layout.css
│   │   │   └── Sidebar.jsx
│   │   └── ShaderBackground.jsx
│   ├── context/
│   │   ├── AuthContext.jsx
│   │   └── ThemeContext.jsx
│   ├── hooks/
│   │   └── useAnimations.js
│   ├── i18n/
│   │   └── index.js
│   ├── pages/
│   │   ├── Analytics.jsx
│   │   ├── Dashboard.jsx
│   │   ├── Login.jsx
│   │   ├── PaymentPage.jsx
│   │   ├── ProjectDetail.jsx
│   │   ├── Projects.jsx
│   │   └── Signup.jsx
│   ├── services/
│   │   └── mockApi.js
│   ├── App.jsx
│   ├── main.jsx
│   └── index.css
├── index.html
├── package.json
├── vite.config.js
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- npm or yarn

### Installation

```bash
# Clone repository
git clone https://github.com/yourusername/mockpay.git
cd mockpay

# Install dependencies
npm install

# Start development server
npm run dev
```

### Available Scripts

| Script | Description |
|--------|-------------|
| `npm run dev` | Start development server |
| `npm run build` | Build for production |
| `npm run preview` | Preview production build |
| `npm run deploy` | Deploy to GitHub Pages |

---

## 🌐 Deployment

### GitHub Pages

1. Update `vite.config.js` with your repo name:
   ```javascript
   export default defineConfig({
     plugins: [react()],
     base: '/your-repo-name/', // Add this for subpath deployment
   })
   ```

2. Deploy:
   ```bash
   npm run deploy
   ```

3. Configure GitHub:
   - Go to Repository → Settings → Pages
   - Source: Deploy from branch
   - Branch: `gh-pages` / `root`

---

## 📊 Production Considerations

For a real production deployment, consider:

| Feature | Mock Implementation | Production Implementation |
|---------|-------------------|--------------------------|
| Database | localStorage | PostgreSQL / MongoDB |
| Auth | Base64 encoding | JWT + bcrypt |
| API | Client-side simulation | Node.js / Express backend |
| Webhooks | Log creation | HTTP POST to merchant URL |
| API Keys | Random string | Cryptographically secure |
| Hosting | GitHub Pages | AWS / GCP / Vercel |

---

## 📝 License

MIT © 2024 MockPay

---

<div align="center">
  <sub>Built with ❤️ for developers who love testing payment integrations</sub>
</div>
