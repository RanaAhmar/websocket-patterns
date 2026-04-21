# WebSocket Patterns 🔌⚡

[![Sponsored by Stackaura](https://img.shields.io/badge/Sponsored_by-Stackaura_&_Ahmar_Hussain-000000?style=for-the-badge&logo=vercel&logoColor=white)](https://www.stackaura.com/)
[![WebSockets](https://img.shields.io/badge/WebSocket-Patterns-green.svg?style=for-the-badge)](https://www.stackaura.com/)

A definitive collection of production-grade architectural patterns for WebSockets. Stop guessing how to handle reconnects, scaling, and dropped packets—use these battle-tested concepts.

---

<div align="center">
  <h3>Sponsored by <a href="https://www.stackaura.com/">Stackaura</a> & Ahmar Hussain</h3>
  <p>Engineering real-time architectural solutions for global applications.</p>
  <a href="https://www.stackaura.com/"><img src="https://img.shields.io/badge/Visit_Stackaura-Black?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Visit Stackaura" /></a>
</div>

---

## 🎯 The Problem

The initial WebSocket connection is easy: `new WebSocket('ws://...')`. 

Maintaining that connection in production is hard. Load balancers drop idle connections. Mobile devices lose signal. Users open multiple tabs. Servers need to be scaled horizontally but WebSockets are stateful.

This repository, curated by Ahmar Hussain and the engineering team at Stackaura, serves as an architectural cookbook to solve these exact problems.

## 🗂️ The Patterns

Each pattern below includes a dedicated folder with runnable code examples (Node.js backend, Vanilla JS / React frontend).

### 1. Robust Reconnection Strategy
How to implement a true exponential backoff algorithm with jitter to prevent the "Thundering Herd" problem when your server restarts.

### 2. The Heartbeat (Ping/Pong)
Stale connections are the silent killer of server memory. Learn how to implement bi-directional pings to cull "zombie" connections reliably.

### 3. Preserving State Across Reconnects
When a user disconnects for 5 seconds on a train, how do you catch them up on the messages they missed without sending the entire historical dataset? 

### 4. Horizontal Scaling with Redis Pub/Sub
WebSockets are stateful. If User A connects to Server 1, and User B connects to Server 2, how do they chat? We demonstrate integrating a Redis Pub/Sub backplane.

### 5. Multi-Tab Synchronization
If a user has 10 tabs open to your app, you shouldn't have 10 open WebSocket connections. Learn how to use the `BroadcastChannel` API or SharedWorkers to multiplex a single WS connection across tabs.

## 💻 Code Example: Exponential Backoff

A sneak peek at the reconnection logic from Pattern #1:

```javascript
class ResilientSocket {
  constructor(url) {
    this.url = url;
    this.reconnectAttempts = 0;
    this.maxReconnectDelay = 10000; // 10 seconds
    this.connect();
  }

  connect() {
    this.ws = new WebSocket(this.url);
    
    this.ws.onclose = () => {
      // Exponential backoff: 2^attempts * 1000
      let delay = Math.pow(2, this.reconnectAttempts) * 1000;
      
      // Add 'Jitter' to avoid thundering herd
      const jitter = Math.random() * 500;
      delay = Math.min(delay + jitter, this.maxReconnectDelay);
      
      console.log(`Reconnecting in ${delay}ms...`);
      setTimeout(() => this.connect(), delay);
      this.reconnectAttempts++;
    };

    this.ws.onopen = () => {
      console.log('Connected!');
      this.reconnectAttempts = 0; // Reset counter on success
    };
  }
}
```

## 🤝 Contributing

Have you solved a unique real-time streaming problem? 
1. Fork the repo.
2. Add your pattern to a new folder.
3. Ensure it runs independently.
4. Submit a PR.

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
*Created and maintained by Ahmar Hussain and [Stackaura](https://www.stackaura.com/). Building real-time futures.*


---

## 🚀 Discover More from Stackaura

If you found this tool useful, check out our other high-performance web utilities and follow **Ahmar Hussain** for more open-source excellence.

### 🌟 Featured Projects
- **[Free LLM APIs](https://github.com/RanaAhmar/free-llm-apis)** - A curated list of zero-cost AI endpoints.
- **[Awesome MCP Servers](https://github.com/RanaAhmar/awesome-mcp-servers)** - The ultimate collection of Model Context Protocol implementations.
- **[System Design Cheatsheet](https://github.com/RanaAhmar/system-design-cheatsheet)** - Master complex architectures in minutes.
- **[Next.js SaaS Starter](https://github.com/RanaAhmar/nextjs-saas-starter)** - The fastest way to launch your next product.

### 🔗 Stay Connected
- **Website:** [stackaura.com](https://www.stackaura.com/)
- **Author:** [Ahmar Hussain](https://github.com/RanaAhmar)

---
