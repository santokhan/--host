# 📑 Uses of `--host` tag on `npm` script

The `--host` flag in Vite (and similar dev servers) changes **how the server binds to network interfaces** — crucial when working with Docker, reverse proxies like NGINX, or when you want your dev server accessible externally.

---

## 🚀 1. Adding `--host` **directly inside** the `package.json` script

If you want **`--host` to always apply**, update your `package.json` like this:

```json
{
  "scripts": {
    "dev": "vite --host"
  }
}
```

Then simply run:
```sh
npm run dev
```

✅ Vite will automatically bind to `0.0.0.0` (all network interfaces).  
✅ External services (like Docker containers) can now connect to your Vite dev server.

---

## 🚀 2. Keeping the script clean and **passing `--host` manually**

If you want a **cleaner `package.json`** like:
```json
{
  "scripts": {
    "dev": "vite"
  }
}
```
then, **when starting the server**, you must **pass `--host` manually**:

```sh
npm run dev -- --host
```

✅ The first `--` tells `npm` that everything following it should be passed **to Vite directly**.  
✅ This way, you have the flexibility to use or not use `--host` depending on your need.

You can also be explicit:
```sh
npm run dev -- --host 0.0.0.0
```

---

# 🧩 Why is `--host` important?

| Command                         | Server Bind Address     | Accessible From         | Docker/NGINX Access? |
|----------------------------------|--------------------------|--------------------------|----------------------|
| `vite`                           | 127.0.0.1 (localhost)     | Only the same machine    | ❌ No |
| `vite --host`                    | 0.0.0.0 (all interfaces) | Other machines, Docker   | ✅ Yes |
| `vite --host 192.168.x.x`         | Specific network IP       | Specific local devices   | ✅ Yes (same network) |

---

# 🎯 Best Practices

| Use Case                              | Recommendation                     |
|---------------------------------------|------------------------------------|
| Always behind Docker/NGINX            | Put `vite --host` inside `package.json` |
| Sometimes local-only, sometimes Docker | Keep a clean script and pass `--host` manually |

**Example with both options:**
```json
{
  "scripts": {
    "dev": "vite",
    "dev:docker": "vite --host"
  }
}
```
- Local development → `npm run dev`
- Docker environment → `npm run dev:docker`

---

# 🧠 Bonus Tip

You can also make it **smarter inside `vite.config.js`** by detecting if you're in a Docker environment, and setting `server.host = true` automatically.
